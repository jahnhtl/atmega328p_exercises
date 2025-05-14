# Übungsblatt: PWM – Helligkeitssteuerung mit Potentiometer

## Aufgabe 2

Ein Mikrocontroller erzeugt ein **PWM-Signal** zur Ansteuerung der Helligkeit einer LED am **OC0A-Pin**.  
Die Helligkeit soll über ein **Potentiometer** geregelt werden, das mit dem **analogen Pin A3** verbunden ist.

**Zeichne zunächst ein Flussdiagramm, wie du die Aufgabe angehen möchtest.**


Ergänze den folgenden Code, damit

- Timer0 im **Phase Correct PWM Mode** läuft (TOP Wert = 0xFF),
- der **OC0A-Pin als Ausgang** gesetzt wird,
- das **PWM-Signal** erzeugt wird,
- die **Helligkeit** der LED regelmäßig über **ADC (A3)** eingelesen und umgesetzt wird.

Verwende **Bitshift-Operationen** (z.B. `_BV()` oder `<<`) und das Datenblatt, um die richtigen Register und Einstellungen zu finden.

---

## Unvollständiger Code

```cpp
void setup()
{
  // Timer Configuration
  // Set OC0A (PD6) to Output


  // Select Phase Correct PWM Mode


  // Clear OC0A on compare match


  // start timer


  // set duty cycle to 0



  // ADC Configuration
  // enable ADC and set prescaler to 128 -> 125kHz @ 16MHz (best conversion rate is between 50 - 200kHz)

  
  // set ADC channel of multiplexer

  
  // set internal 5V reference


}

void loop() {

  // start ADC conversion


  // wait for conversion to complete


  // read value and scale to 8-bit PWM




}
