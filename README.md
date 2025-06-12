# Vending Machine Controller

## Introduction

In this practice you must recreate a vending machine with an Arduino Uno and its respective sensors and actuators.

### Materials

- Arduino UNO
- LCD
- Joystick
- Sensor Temperatura/Humedad DHT11
- Sensor Ultrasonidos
- Botón
- 2 LEDS normales (LED1, LED2)

### Fritzing diagram

This is my circuit diagram

![circuito_p3_bb](https://github.com/user-attachments/assets/446b7d07-a40a-4f77-ba1c-e55b19301fd5)

## Explanation

### Interruptions

Interrupts allow the microcontroller to immediately respond to external events, without constantly checking (polling) its state inside the main loop().

```c
attachInterrupt(digitalPinToInterrupt(bot), button_pressed, CHANGE);
```
This line sets up an interrupt on the bot pin that calls the button_pressed() function whenever the pin state changes 


```c
if (digitalRead(bot) == HIGH) {
  // Button released
  if (pressing_time >= 5) {
    // Long press → enter admin mode
  } else if (pressing_time >= 2 && pressing_time < 3 && state == 1) {
    // Medium press → restart normal flow
  }
  pressing_time = 0;
} else {
  // Button pressed
  Timer1.start(); // Start counting how long the button is held
}
```
This detects if the button was released (HIGH) or pressed (LOW) and responds accordingly.

```c
Timer1.initialize(1000000); // Every 1 second
Timer1.attachInterrupt(timerCallback);
Timer1.stop(); // It only starts when button is pressed
```
When the button is pressed, the timer is started (Timer1.start()).
Every second, the timerCallback() function is triggered:

```c
void timerCallback() {
  noInterrupts();
  pressing_time++;
}
```
This increases pressing_time each second the button is held down, allowing the code to determine whether it was a short, medium, or long press.

### Watchdog

The Watchdog is used as a safety mechanism to reset the microcontroller if something goes wrong while handling the physical button. 
 
```c
void button_pressed() {
  interrupts();
  wdt_enable(WDTO_8S);
```
The Watchdog is enabled when the button (bot) is pressed (detected via interrupt).
It acts as a safety net in case the system gets stuck while handling the button

```c
if (digitalRead(bot) == HIGH) {
    wdt_disable(); // Disables the Watchdog if everything went well
    Timer1.stop();
```

If the button was released (HIGH), it means the logic continued correctly.
Therefore, the Watchdog is disabled, since no reset is needed.

```c
} else {
    interrupts();
    wdt_enable(WDTO_8S);
    Timer1.start();
}
```
If the button is still pressed (LOW), Timer1 is started and the Watchdog is re-enabled as a precaution.

### Joystick

My kit did not include male-female cables. That's why I had to put it directly on the breadboard. For this reason the axes in the code are inverted, to be able to show it more intuitively in the video.
```c
// Este es el eje X pero yo lo usaré como si fuera el Y
if (Xvalue < 400) {
    direction += "Abajo ";  
  } else if (Xvalue > 600) {
    direction += "Arriba ";  
  }

// Este es el eje Y pero yo lo usaré como si fuera el X
if (Yvalue < 400) {
  direction += "Izquierda ";  
} else if (Yvalue > 600) {
  direction += "Derecha "; 
}
```

![WhatsApp Image 2025-06-12 at 17 02 28](https://github.com/user-attachments/assets/c4a7989f-8dbc-4c2f-b275-363c0427c1e6)


## Final result

In the first video I forgot to prepare a product

https://github.com/user-attachments/assets/5216f6ac-1397-48da-8f25-6720fe715858


https://github.com/user-attachments/assets/aef62759-9b04-4739-9a1a-6b60652cb7b8

