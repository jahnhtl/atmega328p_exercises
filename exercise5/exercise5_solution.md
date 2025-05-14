# Lösung: Zwei LEDs versetzt blinken – Timer0 und Timer1

## Beschreibung

Zwei LEDs blinken unabhängig voneinander:

- **LED1 an Pin 11 (PB3)** blinkt mit **1 Hz** über **Timer0 im Normalmodus**, mit einem Overflow-Interrupt und interner Zählung zur Frequenzbildung.
- **LED2 an Pin 12 (PB4)** blinkt mit **2 Hz** über **Timer1 im CTC-Modus** mit einem Compare-Match-A Interrupt.

Timer0 ist ein **8-Bit-Timer**, daher wird der Overflow häufiger ausgelöst und intern mitgezählt, um ca. 1 Hz zu erreichen.  
Timer1 ist ein **16-Bit-Timer**, daher kann er die gewünschte Frequenz direkt erzeugen.

---

## Vollständiger Lösungscode

```cpp
volatile uint16_t ovf_counter = 0;

void setup()
{
  // Set LED pins as output
  DDRB |= (1 << PB3) | (1 << PB4);  // PB3 = Pin 11, PB4 = Pin 12

  // --- Timer0: Normal mode for LED1 (1 Hz) ---
  TCCR0A = 0x00;

  // Prescaler 1024 → (16 MHz / 1024) = 15625 ticks/sec
  TCCR0B |= (1 << CS02) | (1 << CS00);

  // Enable overflow interrupt
  TIMSK0 |= (1 << TOIE0);

  // --- Timer1: CTC mode for LED2 (2 Hz) ---
  TCCR1B |= (1 << WGM12);                   // CTC mode
  TCCR1B |= (1 << CS12) | (1 << CS10);      // Prescaler 1024
  OCR1A = 3905; 							// (16 MHz / 1024 / 2 Hz / 2) - 1

  // Enable compare match interrupt
  TIMSK1 |= (1 << OCIE1A);

  // Enable global interrupts
  sei();
}

// ISR for Timer0 Overflow (every 256 ticks → approx. 61.035 ms)
ISR(TIMER0_OVF_vect)
{
  ovf_counter++;
  if (ovf_counter >= 16) // ~16 * 61ms ≈ 1 second
  {
    PINB ^= (1 << PB3); // Toggle LED1 (PB3)
    ovf_counter = 0;
  }
}

// ISR for Timer1 Compare Match A → 2 Hz
ISR(TIMER1_COMPA_vect)
{
  PINB ^= (1 << PB4); // Toggle LED2 (PB4)
}

void loop()
{
  // nothing to do – everything is in the ISR
}
