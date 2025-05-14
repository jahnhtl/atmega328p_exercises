# Übungsblatt: Zwei LEDs – eine per Timer, eine per Tastendruck

## Aufgabe

Ein Mikrocontroller steuert zwei LEDs:

- **LED1 (an Pin 9)** soll regelmäßig im Abstand von **0,5 Sekunden** blinken mit Hilfe eines Timer Interrupts.  
- **LED2 (an Pin 10)** wird über einen **externen Taster** ein- und ausgeschaltet (Toggeln bei Tastendruck über INT0).

**Zeichne zunächst ein Flussdiagramm, wie du die Aufgabe angehen möchtest.**


Ergänze den folgenden Code, damit

- **Timer1** im CTC-Modus mit geeignetem **Prescaler** konfigurieren
- **OCR1A** so setzen, dass der Compare Match Interrupt alle **0,5 Sekunden** auftritt
- Der Taster muss **entprellt** werden

Verwende **Bitshift-Operationen** (z.B. `_BV()` oder `<<`) und das Datenblatt, um die richtigen Register und Einstellungen zu finden. 

---

## Unvollständiger Code

```cpp

void setup()
{
  // Set LED-Pins as Output


  // Set Timer1 to CTC-Modus


  // Compare Match Value for 0,5s at 16MHz and prescaler of 1024


  // Activate Compare Match A Interrupt


  // Set prescaler to 1024 setzen (and start timer by that)


  // Set INT0 as input with pull-ups activated



  // Configure external interrupt INT0 with FALLING EDGE



  // Enable global interrupts


}

ISR(TIMER1_COMPA_vect)
{
  // LED1 toggeln


}

ISR(INT0_vect)
{
  // LED2 toggeln


  // Debouncing is required
  
  
}

void loop()
{
  // everything runs via ISR
}
