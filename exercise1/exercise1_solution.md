# Lösung: ADC-Messung per Timer – Ausgabe über Serielle Schnittstelle

## Beschreibung

Dieses Programm konfiguriert **Timer0** im **CTC-Modus**, um alle **0,5 Sekunden** eine **ADC-Messung** durchzuführen. Das Ergebnis wird über die serielle Schnittstelle ausgegeben. Die ADC-Konfiguration verwendet AVcc als Referenzspannung und liest vom Kanal A0.

---

## Vollständiger Code

```cpp
volatile bool adc_trigger = false;

void setup()
{
  Serial.begin(9600);

  // Timer Configuration
  // Set CTC Mode
  TCCR0A |= (1 << WGM01);

  // Set Compare Match value for 0.5s @ 16MHz
  // With Prescaler 1024: f_timer = 16MHz / 1024 = 15625 Hz
  // 0,5s = 7812,5 clock cycles -> OCR0A = 15625 * 0.5 - 1 = 7812 -> does not fit in 8-bit
  // -> Take: 62,5ms period -> every 8th ISR-call = 0,5s
  OCR0A = 97;                     // 62,5ms intervall (97 + 1 = 98 clock cycles)

  // Enable Compare Match Interrupt
  TIMSK0 |= (1 << OCIE0A);

  // Set Prescaler and start timer (Prescaler 1024)
  TCCR0B |= (1 << CS02) | (1 << CS00);

  // ADC Configuration
  // Enable ADC and set Prescaler (e.g. 128 for 125kHz @ 16MHz)
  ADCSRA |= (1 << ADEN);
  ADCSRA |= (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0);

  // Set voltage reference
  ADMUX |= (1 << REFS0);

  // Select ADC channel
  ADMUX &= 0xF0;

  // Enable global interrupts
  sei();
}

ISR(TIMER0_COMPA_vect)
{
  static uint8_t isr_counter = 0;
  
  isr_counter++;
  if (isr_counter >= 8) // 8 * 62,5ms = 500ms
  {
    isr_counter = 0;
    adc_trigger = true;
  }
}

void loop()
{
  if (adc_trigger)
  {
    adc_trigger = false;

    // Start ADC conversion
    ADCSRA |= (1 << ADSC);

    // Wait for conversion to complete
    while (ADCSRA & (1 << ADSC));

    // Read ADC result
    uint16_t adc_value = ADC;

    // Print result to serial
    Serial.println(adc_value);
  }
}
