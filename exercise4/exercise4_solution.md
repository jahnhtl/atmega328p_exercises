# Lösung: LED-Frequenzsteuerung per Potentiometer – Timer1 und ADC

## Beschreibung

Die **LED an Pin 9 (PB1)** blinkt mit einer Frequenz, die durch ein **Potentiometer an A2** gesteuert wird.  
Der **ADC-Wert (0–1023)** wird regelmäßig eingelesen und auf eine Frequenz zwischen **1Hz und 10Hz** abgebildet.  
Der **Timer1** läuft im **CTC-Modus** mit **Prescaler 256**, und das Register **OCR1A** wird entsprechend angepasst.  
Die LED wird über den Compare-Match-A Interrupt getoggelt.

---

## Vollständiger Lösungscode

```cpp

void setup()
{
  // Define LED-Pin as output (PB1 = Pin 9)
  DDRB |= (1 << PB1);

  // ADC-Configuration
  ADMUX = (ADMUX & 0xF0) | 0x02;       // Channel A2
  ADMUX |= (1 << REFS0);               // AVcc Reference voltage

  ADCSRA |= (1 << ADEN);               // Enable ADC
  ADCSRA |= (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0); // Prescaler 128

  // Configure Timer1 in CTC-Modus
  TCCR1B |= (1 << WGM12);

  // Enable Compare Match A Interrupt
  TIMSK1 |= (1 << OCIE1A);

  // Set prescaler to 256
  TCCR1B |= (1 << CS12);

  // Enable globale interrupts
  sei(); 
}

ISR(TIMER1_COMPA_vect)
{
  // Toggle LED
  PORTB ^= (1 << PB1);
}

void loop()
{
  // Start ADC conversion
  ADCSRA |= (1 << ADSC);
  while (ADCSRA & (1 << ADSC)); // Wait till completed

  uint16_t adc_value = ADC;

  // Convert ADC value (0–1023) to frequency range 1–10 Hz
  // -> Calculate fitting OCR1A-Werte:
  // Formula: OCR1A = (F_CPU / (2 * Prescaler * freq)) - 1
  // With prescaler 256: freq 1–10 Hz -> OCR1A about 31249 to 3124
  uint16_t freq = map(adc_value, 0, 1023, 1, 10);
  uint16_t ocr_val = (16000000UL / (2UL * 256UL * freq)) - 1;

  OCR1A = ocr_val;

  delay(100); // short delay to stabilize ADC
}
