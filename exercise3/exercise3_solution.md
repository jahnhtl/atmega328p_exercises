# Lösung: Zwei LEDs – eine per Timer, eine per Tastendruck

## Beschreibung

**LED1** (an Pin 9 / PB1) blinkt alle **0,5 Sekunden** über **Timer1 im CTC-Modus** mit einem Prescaler von 1024.  
**LED2** (an Pin 10 / PB2) wird über einen **externen Taster** am **INT0 (Pin 2)** getoggelt. Eine einfache Software-Entprellung verhindert Mehrfachauslösungen.  
Die Steuerung erfolgt ausschließlich über **Interrupts**, der `loop()`-Teil bleibt leer.

---

## Vollständiger Lösungscode

```cpp

void setup()
{
  // Set LED-Pins as Output (PB1 = Pin 9, PB2 = Pin 10)
  DDRB |= (1 << PB1) | (1 << PB2);

  // Set Timer1 to CTC-Modus
  TCCR1B |= (1 << WGM12);

  // Compare Match Value for 0,5s at 16MHz and prescaler of 1024
  OCR1A = 7812;

  // Activate Compare Match A Interrupt
  TIMSK1 |= (1 << OCIE1A);

  // Set prescaler to 1024 setzen (and start timer by that)
  TCCR1B |= (1 << CS12) | (1 << CS10);

  // Set INT0 (PD2 / Pin 2) as input with pull-ups activated
  DDRD &= ~(1 << PD2);
  PORTD |= (1 << PD2);

  // Configure external interrupt INT0 with FALLING EDGE
  EICRA |= (1 << ISC01);
  EIMSK |= (1 << INT0);

  // Enable global interrupts
  sei();
}

ISR(TIMER1_COMPA_vect)
{
  // LED1 toggeln (PB1)
  PORTB ^= (1 << PB1);
}

ISR(INT0_vect)
{
  // LED2 toggeln (PB2)
  PORTB ^= (1 << PB2);

  // Debouncing is required - this is a BAD solution, but play around with increasing the delay
  // here to 1000 and see what happens.
  // Find a better solution with AI to solve the debouncing issue!
  delay(30);
}

void loop()
{
  // everything runs via ISR
}
