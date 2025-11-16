# QuackPack-DuckGPS

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![ClusterDuck Protocol](https://img.shields.io/badge/ClusterDuck-Protocol-yellow.svg)](https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol)

## Overview

This QuackPack makes it easy to work with ublox GPS chips on devices running the ClusterDuck Protocol firmware. This library handles GPS initialization, chip configuration and data parsing. This QuackPack is designed to work with popular ublox modules such as the NEO-6M, NEO-7M, and NEO-8M series and log messages are handled by the ClusterDuck Protocol's logging framework.

## What is a QuackPack?

A **QuackPack** is a modular library that extends Duck firmware with application-specific functionality. QuackPacks are **not standalone projects** - they are included in your Duck firmware project and configured through your main sketch.

This QuackPack adds [describe your specific functionality] to the Duck mesh network.

## Hardware Requirements

### Supported Boards
- **Primary**: TTGO T-Beam v1.1 with SX1262
- **Also Supported**: TTGO T-Beam v1.0 with SX1276

### Additional Hardware
May require a more sensitive GPS antenna for optimal performance.

**Example**:
- GPS Module (if not built-in)

### Pinout Configuration

| Component     | GPIO Pin | Notes        |
|---------------|----------|--------------|
| CDPCFG_GPS_RX | 34       | Receive Pin  |
| CDPCFG_GPS_TX | 12       | Transfer Pin |
| CDPCFG_PIN_LED1  | 25       | TBeam LED    |

## Installation

### Prerequisites

- [PlatformIO](https://platformio.org/)
- An existing Duck firmware project based on [ClusterDuck Protocol](https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol) 

### Option 1: Manual Integration with PlatformIO

Add this QuackPack to your Duck firmware's `platformio.ini`:

```ini
[env:ttgo-t-beam-sx1262]
platform = espressif32
board = ttgo-t-beam
framework = arduino

lib_deps = 
    https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol.git
    https://github.com/Project-Owl/QuackPack-DuckGPS.git

monitor_speed = 115200
```

Then build and upload your firmware:
```bash
pio run --target upload
pio device monitor
```

## Quick Start

Include this QuackPack in your Duck firmware's main sketch:

### Basic Integration

```cpp
#include <Ducks/MamaDuck.h>
#include <DuckGPS.h>
#include <utils/DuckUtils.h>

MamaDuck duck;
DuckGPS dgps(34, 12); // RX, TX pins

void setup() {
    Serial.begin(115200);
    
    // Initialize the Duck
    duck.begin();
    duck.setDeviceId("DUCK001");
    duck.setupWithDefaults(); // or setupMamaDuck() / setupPapaDuck()
    
    // Initialize and configure your QuackPack
    dgps.setup();
    
    
    Serial.println("Duck ready!");
}

void loop() {
    // Run Duck mesh networking
    duck.run();
    
    tgps.readData(10000);
    
    std::cout << "Latitude: " << dgps.lat() << ", Longitude: " << dgps.lng() << '\n';
    std::cout << "Altitude: " << dgps.altitude(DuckGPS::AltitudeUnit::meter) << " meters" << '\n';
    std::cout << "Satellites: " << dgps.satellites() << '\n';
    std::cout << "Speed: " << dgps.speed(DuckGPS::SpeedUnit::kmph) << " km/h" << '\n';
    std::cout << "Time: " << tgps.epoch() << " epoch seconds" << '\n';
    std::cout << "GeoJSON Point: " << dgps.geoJsonPoint() << '\n';
    
    sleep(5000); // Delay between readings
    
    
}
```

## Configuration

### Method 1: Programmatic Configuration (Recommended)

Configure directly in your `setup()`:

```cpp
void setup() {
    duck.begin();
    duck.setDeviceId("DUCKGPS1");
    duck.setupWithDefaults();
}
```

## API Reference

### Main Class: `DuckGPS`

#### Constructor
```cpp
DuckGPS(RXPin, TXPin);
```
[Description]

Based on the `DuckGPS` QuackPack template, here are the methods and their documentation:

## Core Methods

### Constructor
```cpp
DuckGPS(uint8_t rxPin, uint8_t txPin);
```
Initializes the GPS module with specified RX and TX pins for serial communication.

**Parameters:**
- `rxPin`: GPIO pin for receiving GPS data
- `txPin`: GPIO pin for transmitting to GPS module

### Initialization
```cpp
void setup();
```
Initializes the GPS module and configures the ublox chip for operation.

### Data Acquisition
```cpp
void readData(unsigned long timeout_ms);
```
Reads and parses GPS data from the module.

**Parameters:**
- `timeout_ms`: Maximum time in milliseconds to wait for GPS data

### Position Methods
```cpp
double lat();
double lng();
```
Returns the current latitude and longitude in decimal degrees.

```cpp
double altitude(DuckGPS::AltitudeUnit unit);
```
Returns altitude in the specified unit.

**Parameters:**
- `unit`: `DuckGPS::AltitudeUnit::meter` or `DuckGPS::AltitudeUnit::feet`

### Status Methods
```cpp
uint8_t satellites();
```
Returns the number of satellites currently in view/used.

```cpp
double speed(DuckGPS::SpeedUnit unit);
```
Returns current speed in the specified unit.

**Parameters:**
- `unit`: `DuckGPS::SpeedUnit::kmph`, `DuckGPS::SpeedUnit::mph`, or `DuckGPS::SpeedUnit::knots`

### Time Methods
```cpp
unsigned long epoch();
```
Returns current GPS time as Unix epoch timestamp (seconds since Jan 1, 1970).

### Format Methods
```cpp
String geoJsonPoint();
```
Returns current position formatted as a GeoJSON Point string for easy integration with mapping systems.

## Dependencies

This QuackPack requires:
- [ClusterDuck Protocol](https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol.git)
- adafruit/Adafruit uBlox@^1.0.0
- mikalhart/TinyGPSPlus@~1.0.2

## Deployment

### PlatformIO
Manually integrate into your Duck firmware project using the instructions above.

### OWL DMS
[Describe DMS availability status]

To submit this QuackPack for inclusion in the OWL DMS community library, contact **support@owlintegrations.com** with:
- Repository URL
- Supported hardware
- Use case description
- Example configurations

## Compatibility

- **ClusterDuck Protocol Version**: v5.0+
- **PlatformIO**: 6.0+
- **ESP32 Core**: 2.0.0+
- **Development Environment**: PlatformIO only (Arduino IDE not supported)

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## Changelog

### 1.0.0 - [Initial Release Date]
- Initial release
- [Feature 1]
- [Feature 2]

## Roadmap

- [ ] [Planned feature 1]
- [ ] [Planned feature 2]
- [ ] [Planned feature 3]

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built on the [ClusterDuck Protocol](https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol)
- Created by [OWL Integrations](https://www.owlintegrations.com/)

## Resources

- [OWL Integrations](https://www.owlintegrations.com/)
- [OWL DMS](https://www.owlintegrations.com/) - Device Management System for mesh networking
- [ClusterDuck Protocol](https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol)
- [Discord Community](https://discord.gg/clusterduck)
- **DMS Support**: support@owlintegrations.com

## Support

For questions and support:
- Join the [ClusterDuck Protocol Discord](https://discord.gg/clusterduck)
- Open an [Issue](https://github.com/Project-Owl/QuackPack-DuckGPS/issues)

## Authors

- **Brenton Poke** - *Initial work* - [@BrentonPoke](https://github.com/brentonpoke)



