# Tiny BME280 Library #
[![Build Status](https://travis-ci.org/jasonacox/Tiny_BME280_Library.svg?branch=master)](https://travis-ci.org/jasonacox/Tiny_BME280_Library)

This is a Arduino library for the BME280 Humidity, Barometric Pressure + Temp sensor

## Description
This is a Arduino library for BME280 sensor for gathering temperature, humidity and pressure using I2C communication.  This library is a minimized fork of the more robust [Adafruit_BME280_Library](https://github.com/adafruit/Adafruit_BME280_Library) but with SPI and other features removed to reduce PROGMEM space.  This library is ideal for smaller controllers like the ATtiny85.

This library was designed to work with the Adafruit BME280 sensor in I2C mode.
 * http://www.adafruit.com/products/2652

## Setup
These sensors use I2C to communicate, using the I2C Logic pins:
* SCK - this is also the I2C clock pin, connect to the microcontrollers I2C clock line. On the ATtiny85 this is PB2 (physical pin 7)
* SDI or SDA - this is also the I2C data pin, connect to the microcontrollers I2C data line. On the ATtiny85 this is PB0 (physical pin 5)

Use of this library also requires the [Adafruit_Sensor](https://github.com/adafruit/Adafruit_Sensor) library to be installed on your system.

Download or clone this Tiny_BME280 library repo into your local Arduino libraries fold (e.g. `~/Documents/Arduino/libraries/` for MacOS) and restart the IDE.
```bash
cd ~/Documents/Arduino/libraries
git clone https://github.com/jasonacox/Tiny_BME280_Library.git 
```
This has been tested to work with ATtiny85 microcontrollers using the [ATTinyCore](https://github.com/SpenceKonde/ATTinyCore) core.

## Example Usage
```cpp
#include <Adafruit_Sensor.h>    // https://github.com/adafruit/Adafruit_Sensor
#include <Tiny_BME280.h>        // https://github.com/jasonacox/Tiny_BME280_Library

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

## API reference


### Constructors

*   `Tiny_BME280()`
    *   Description: Default constructor.
*   `~Tiny_BME280(void)`
    *   Description: Destructor.

### Methods

*   `bool begin(uint8_t addr = BME280_ADDRESS, TwoWire *theWire = &Wire)`
    *   Description: Initializes the BME280 sensor.
    *   Parameters:
        *   `addr` (optional): I2C address of the sensor. Defaults to `BME280_ADDRESS` (`0x77`).
        *   `theWire` (optional): Pointer to the `TwoWire` object. Defaults to `Wire`.
    *   Returns: `true` if initialization is successful, `false` otherwise.
*   `bool init()`
    *   Description: Initializes the BME280 sensor.
    *   Returns: `true` if initialization is successful, `false` otherwise.
*   `void setSampling(sensor_mode mode, sensor_sampling tempSampling, sensor_sampling pressSampling, sensor_sampling humSampling, sensor_filter filter, standby_duration duration)`
    *   Description: Sets the sampling settings for temperature, pressure, humidity, filter, and standby duration.
    *   Parameters:
        *   `mode`: Power mode (`MODE_SLEEP`, `MODE_FORCED`, or `MODE_NORMAL`).
        *   `tempSampling`: Temperature sampling rate (`SAMPLING_NONE`, `SAMPLING_X1`, `SAMPLING_X2`, `SAMPLING_X4`, `SAMPLING_X8`, or `SAMPLING_X16`).
        *   `pressSampling`: Pressure sampling rate (`SAMPLING_NONE`, `SAMPLING_X1`, `SAMPLING_X2`, `SAMPLING_X4`, `SAMPLING_X8`, or `SAMPLING_X16`).
        *   `humSampling`: Humidity sampling rate (`SAMPLING_NONE`, `SAMPLING_X1`, `SAMPLING_X2`, `SAMPLING_X4`, `SAMPLING_X8`, or `SAMPLING_X16`).
        *   `filter`: Filter setting (`FILTER_OFF`, `FILTER_X2`, `FILTER_X4`, `FILTER_X8`, or `FILTER_X16`).
        *   `duration`: Standby duration (`STANDBY_MS_0_5`, `STANDBY_MS_10`, `STANDBY_MS_20`, `STANDBY_MS_62_5`, `STANDBY_MS_125`, `STANDBY_MS_250`, `STANDBY_MS_500`, or `STANDBY_MS_1000`).
*   `void takeForcedMeasurement()`
    *   Description: Initiates a forced measurement and waits for the results to be available.
*   `float readTemperature()`
    *   Description: Reads the temperature from the sensor.
    *   Returns: Temperature value in degrees Celsius.
*   `float readPressure()`
    *   Description: Reads the pressure from the sensor.
    *   Returns: Pressure value in Pascals.
*   `float readHumidity()`
    *   Description: Reads the humidity from the sensor.
    *   Returns: Humidity value as a percentage.
*   `float readAltitude(float seaLevel)`
    *   Description: Calculates the altitude based on the specified sea level pressure.
    *   Parameters:
        *   `seaLevel`: Sea level pressure in Pascals.
    *   Returns: Altitude value in meters.
*   `float seaLevelForAltitude(float altitude, float pressure)`
    *   Description: Calculates the sea level pressure based on the specified altitude.
    *   Parameters:
        *   `altitude`: Altitude value in meters. 
        *   `pressure`: Pressure value in Pascals.
