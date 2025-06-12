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

### Watchdog

In this case, the Watchdog is used as a safeguard during the button press detection, to cover a specific scenario:
 
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
Este es el eje X pero yo lo usaré como si fuera el Y
if (Xvalue < 400) {
    direction += "Abajo ";  
  } else if (Xvalue > 600) {
    direction += "Arriba ";  
  }

Este es el eje Y pero yo lo usaré como si fuera el X
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

