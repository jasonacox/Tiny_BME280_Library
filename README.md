# Tiny BME280 Library
This is a library for the  BME280 Humidity, Barometric Pressure + Temp sensor

## Description
Tiny library for BME280 sensor for gathering temperature, humidity and pressure.  Based on Adafruit_BME280_Library but with SPI and other features removed to reduce PROGMEM space.  Ideal for smaller controllers like the ATtiny85.

Designed to work with the Adafruit BME280 Breakout 
 * http://www.adafruit.com/products/2652

## Setup
These sensors use I2C to communicate, using 2 pins.

Use of this library also requires [Adafruit_Sensor](https://github.com/adafruit/Adafruit_Sensor)
to be installed on your local system.

Place this Tiny_BME280 library folder your ~/Documents/Arduino/libraries/ folder and restart
the IDE.

## Example Usage
```cpp
#include <Adafruit_Sensor.h>
#include <Tiny_BME280.h>

// Initialize BME280 Class
Tiny_BME280 bme; 

// Setup
void setup() {
    Serial.begin(9600);
    while(!bme.begin()) {
        Serial.println("BME280 not responding... waiting.");
        delay(1000);
    }
    Serial.println("BME280 activated");
}

// Loop
void loop() { 
    int temperature, pressure, humidity;

    temperature = bme.readTemperature();
    pressure = bme.readPressure() / 100.0;
    humidity = bme.readHumidity();

    Serial.print("Temperature = ");
    Serial.print(temperature);
    Serial.print("'C [");
    Serial.print((temperature * 1.8) + 32);
    Serial.println("'F]");
    
    Serial.print("Relative Humidity = ");
    Serial.print(humidity);
    Serial.println("%");

    Serial.print("Pressure = ");
    Serial.print(pressure);
    Serial.println(" hPa");
    
    Serial.println();

    delay(1000);
}
```

