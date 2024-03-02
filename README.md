// LM35-temperature-sensor-for-onboard-LED-blinking
#include <TimerFreeTone.h>

const int lm35Pin = A0;  // LM35 temperature sensor connected to analog pin A0
const int ledPin = 13;   // Onboard LED connected to digital pin 13

// Variables for initialize & storing
int temperature = 0;
int previousTemperature = 0;

// Variables to control LED blinking intervals
unsigned long previousMillis = 0;
unsigned long interval = 250; // Initial interval for temperature below 30°C

// Function to read temperature from LM35 sensor
int readTemperature() {
  int rawValue = analogRead(lm35Pin); // Read analog value from LM35
  float milliVolts = (rawValue / 1024.0) * 5000; // Convert to millivolts
  return milliVolts / 10; // Convert to degrees Celsius
}

void setup() {
  pinMode(ledPin, OUTPUT); // Set LED pin as output
}

void loop() {
  temperature = readTemperature(); // Read temperature
  
  // Check if temperature has changed
  if (temperature != previousTemperature) {
    // Update previous temperature
    previousTemperature = temperature;
    
    // Adjust blinking interval based on temperature
    if (temperature < 30) {
      interval = 250; // Blink every 250 milliseconds
    } else {
      interval = 500; // Blink every 500 milliseconds
    }
  }
  
  unsigned long currentMillis = millis(); // Get current time
  
  // Check if it's time to blink the LED
  if (currentMillis - previousMillis >= interval) {
    // Save the last time the LED blinked
    previousMillis = currentMillis;
    
    // Toggle LED
    TimerFreeTone(ledPin, 1000 / 2); // Blink onboard LED
  }
}
