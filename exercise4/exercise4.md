# Übungsblatt: LED-Frequenzsteuerung per Potentiometer – Timer1 und ADC

## Aufgabe

Eine **LED an Pin 9** soll blinken. Die **Frequenz** wird über ein **Potentiometer an A2** eingestellt.  
Der ADC-Wert wird regelmäßig gelesen und in eine Blinkfrequenz von ca. **1 Hz bis 10 Hz** umgesetzt.

**Zeichne zunächst ein Flussdiagramm, wie du die Aufgabe angehen möchtest.**


Ergänze den folgenden Code, damit

- **ADC** liest regelmäßig den Wert des Potentiometers (0–1023)
- Der ADC-Wert wird in eine **OCR1A**-Vergleichszeit übersetzt
- **Timer1 im CTC-Modus**, 16 MHz, z.B. mit Prescaler 256 oder 1024
- **LED toggelt per Timer1 im Compare-Match-A Interrupt**

Verwende **Bitshift-Operationen** (z.B. `_BV()` oder `<<`) und das Datenblatt, um die richtigen Register und Einstellungen zu finden. 

---

## Unvollständiger Code

```cpp

void setup()
{
  // Define LED-Pin as output
  

  // ADC-Configuration






  // Timer1 Configuration
  






  // Enable globale interrupts

}

ISR(TIMER1_COMPA_vect)
{
  // Toggle LED

}

void loop()
{
  // Start ADC conversion



  // Convert ADC value (0–1023) to frequency range 1–10 Hz




  // Update Timer1


}