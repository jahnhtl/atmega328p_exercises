# Übungsblatt: ADC-Messung per Timer – Ausgabe über Serielle Schnittstelle

## Aufgabe

Ein Mikrocontroller soll in regelmäßigen Abständen **alle 0,5 Sekunden** eine **ADC-Messung** auf A0 durchführen und den Wert über die **serielle Schnittstelle** ausgeben.

**Zeichne zunächst ein Flussdiagramm, wie du die Aufgabe angehen möchtest.**


Ergänze den folgenden Code, damit

- **Timer0** im **CTC-Modus** läuft,
- ein **Interrupt** alle 62,5ms ausgelöst wird (nutze geeignete Prescaler Einstellungen),
- in der **Interrupt Service Routine (ISR)** eine **ADC-Messung** getriggert wird (aber nur alle 0,5 Sekunden),
- der Messwert über `Serial.println()` ausgegeben wird.

Verwende **Bitshift-Operationen** (z.B. `_BV()` oder `<<`) und das Datenblatt, um die richtigen Register und Einstellungen zu finden. 

---

## Unvollständiger Code

```cpp
volatile bool adc_trigger = false;

void setup()
{
  Serial.begin(9600);

  // Timer Configuration
  // Set CTC Mode


  // Set Compare Match value for 0.5s @ 16MHz


  // Enable Compare Match Interrupt


  // Set Prescaler and start timer



  // ADC Configuration
  // Enable ADC and set Prescaler (e.g. 128 for 125kHz @ 16MHz)


  // Set voltage reference


  // Select ADC channel


  // Enable global interrupts

}

ISR(TIMER0_COMPA_vect)
{
  // trigger adc every 500ms




}

void loop()
{
  if (adc_trigger)
  {
    adc_trigger = false;

    // Start ADC conversion


    // Wait for conversion to complete


    // Read ADC result


    // Print result to serial

    
  }
}
