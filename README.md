# turn-counter
##Description 
This project is based on a turn counter which i have arduino uno,16X2 lcd and pir motion detection sensor.
##Components used
-Arduino uno
-16x2 I2C LCD
- PIR Motion Sensor
- Connecting Wires
## Wiring Diagram
- **Arduino Uno 5V** to **LCD VCC**
- **Arduino Uno GND** to **LCD GND**
- **Arduino Uno A4** to **LCD SDA**
- **Arduino Uno A5** to **LCD SCL**
- **Arduino Uno 5V** to **PIR VCC**
- **Arduino Uno GND** to **PIR GND**
- **Arduino Uno Pin 2** to **PIR OUT**
- ## Code
The code for this project is written in Arduino C++ 
```cpp
#include <Arduino.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

int x = 0;
const int inputPin = 2; // Set the digital input pin for the PIR sensor
int state = 0;

LiquidCrystal_I2C lcd(0x27, 16, 2); // Set the LCD address to 0x27 for a 16 chars and 2 line display

void setup() {
  lcd.init(); // Initialize the LCD
  lcd.backlight(); // Turn on the backlight

  // Print initial message to the LCD
  lcd.setCursor(0, 0);
  lcd.print("  Turn Counter  ");
  lcd.setCursor(0, 1);
  lcd.print("Count: ");
  lcd.print(x);

  pinMode(inputPin, INPUT); // Explicitly set the input pin mode
}

void loop() {
  int counterValue = digitalRead(inputPin); // Read the digital input value

  // Simple debounce and threshold logic for detecting motion
  if (counterValue == HIGH && state == 0 && x < 10) {
    x++;
    lcd.setCursor(7, 1); // Move cursor to start of count position
    lcd.print(x);
    lcd.print("   "); // Clear any previous digits
    state = 1;
    delay(200); // Debounce delay
  } else if (counterValue == LOW && state == 1) {
    state = 0;
    delay(200); // Debounce delay
  }

  delay(50); // Small delay to stabilize readings
}
