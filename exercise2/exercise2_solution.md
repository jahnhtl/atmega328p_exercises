# Lösung: PWM – Helligkeitssteuerung mit Potentiometer

## Beschreibung

Die LED an Pin PD6 (OC0A) wird über Timer0 im **Phase Correct PWM Mode** mit einer variablen Helligkeit betrieben.  
Ein Potentiometer an A3 liefert über den ADC-Wert den Helligkeitswert (0–1023), der auf den 8-Bit-Wert (0–255) für PWM skaliert wird.

---

## Vollständiger Lösungscode

```cpp
void setup()
{
  // Timer Configuration
  // Set OC0A (PD6) to Output
  DDRD |= (1 << PD6);

  // Select Phase Correct PWM Mode (WGM00 = 1, WGM01 = 0, WGM02 = 0)
  TCCR0A |= (1 << WGM00);

  // Clear OC0A on compare match (non-inverting mode)
  TCCR0A |= (1 << COM0A1);

  // start timer with prescaler 64
  TCCR0B |= (1 << CS01) | (1 << CS00);

  // set duty cycle to 0
  OCR0A = 0;

  // ADC Configuration
  // enable ADC and set prescaler to 128 -> 125kHz @ 16MHz
  ADCSRA |= (1 << ADEN) | (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0);

  // set ADC channel A3 (MUX3 = 0b0011)
  ADMUX = (ADMUX & 0xF0) | 0x03;

  // set internal 5V reference (AVcc with external capacitor at AREF)
  ADMUX |= (1 << REFS0);
}

void loop() {
  // start ADC conversion
  ADCSRA |= (1 << ADSC);                

  // wait for conversion to complete
  while((ADCSRA & (1 << ADSC)) != 0);   

  // read value and scale to 8-bit PWM
  uint16_t value = ADC;
  OCR0A = value >> 2;  // scale 10-bit to 8-bit -> shift left by 2 is the same as divided by 4
}
