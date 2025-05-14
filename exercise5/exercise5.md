# Übungsblatt: Zwei LEDs versetzt blinken – Timer0 und Timer1

## Aufgabe

Zwei **LEDs an Pin 11** und **PB2 an Pin 12** sollen unabhängig mit unterschiedlichen Frequenzen blinken:

- **LED1**: 1 Hz mit **Timer0**
- **LED2**: 2 Hz mit **Timer1**

**Zeichne zunächst ein Flussdiagramm, wie du die Aufgabe angehen möchtest.**


Ergänze den folgenden Code, damit
- **Timer0** im Normal-Modus für LED1 (OCR0A)
- **Timer1** im CTC-Modus für LED2 (OCR1A)

Verwende **Bitshift-Operationen** (z.B. `_BV()` oder `<<`) und das Datenblatt, um die richtigen Register und Einstellungen zu finden. 

---

## Unvollständiger Code

```cpp


void setup()
{
  // Set LED pins as output


  // --- Timer0: Normal mode for LED1 (1 Hz) ---





  // Enable overflow interrupt of Timer0


  // --- Timer1: CTC mode for LED2 (2 Hz) ---





  // Enable compare match interrupt of Timer1


  // Enable global interrupts

}

ISR(TIMER0_OVF_vect)
{






}

ISR(TIMER1_COMPA_vect)
{


}

void loop()
{
  // nothing to do – everything is in the ISR
}
