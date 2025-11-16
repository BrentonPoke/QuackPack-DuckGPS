# Basic Duck Firmware Example

This directory shows how to structure a complete Duck firmware project that uses a QuackPack.

## Project Structure

Your Duck firmware project should look like:

```
my-duck-firmware/
├── src/
│   └── main.ino              # Your main firmware sketch
├── include/
│   └── quackpack_config.h    # Optional: QuackPack configuration
├── platformio.ini            # Project configuration
└── README.md
```

## Setup

1. **Create a new PlatformIO project**:
```bash
mkdir my-duck-firmware
cd my-duck-firmware
pio init --board ttgo-t-beam
```

2. **Copy the example `platformio.ini`** from this directory and modify the `lib_deps` section to include your QuackPack.

3. **Create your main sketch** (use `Basic.ino` or `Advanced.ino` as a starting point).

4. **Build and upload**:
```bash
pio run --target upload
pio device monitor
```

## Using a Local QuackPack for Development

During development, you can reference a local QuackPack:

```ini
lib_deps = 
    https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol#network-routing
    file://../path/to/your/QuackPack
```

## Using a Published QuackPack

Once published, reference it by URL:

```ini
lib_deps = 
    https://github.com/ClusterDuck-Protocol/ClusterDuck-Protocol#network-routing
    https://github.com/YOUR_ORG/YOUR_QUACKPACK.git
```

## Configuration

Create an optional `include/quackpack_config.h` file for your configuration:

```cpp
#ifndef QUACKPACK_CONFIG_H
#define QUACKPACK_CONFIG_H

// Device Configuration
#define DEVICE_ID "DUCK001"
#define DUCK_TYPE DuckType::MAMA

// QuackPack Configuration
#define QUACKPACK_PARAM1 "value1"
#define QUACKPACK_PARAM2 42

#endif
```

Then include it in your sketch:

```cpp
#include "quackpack_config.h"

void setup() {
    duck.setDeviceId(DEVICE_ID);
    duck.setupMamaDuck();
    quackPack.begin(&duck);
    quackPack.setParameter("param1", QUACKPACK_PARAM1);
}
```

