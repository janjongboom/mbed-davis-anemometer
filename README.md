# Mbed OS Davis Anemometer driver

Mbed OS 5 driver for the [Davis Anemometer](https://www.davisinstruments.com/product/anemometer-for-vantage-pro2-vantage-pro/). A great device for measuring wind speed and wind direction.

## Usage

The sensor requires two pins, an analog pin to read the wind direction, and a digital pin to read the wind speed (requires interrupts). Note that this is a 5V sensor and you need 5V tolerant pins.

```cpp
#include "DavisAnemometer.h"

static DavisAnemometer anemometer(A1 /* wind direction */, D3 /* wind speed */);

int main() {
    anemometer.enable();

    while (1) {
        printf("DAVIS:   [drct] %d°,  [speed] %.2f km/h\r\n", anemometer.readWindDirection(), anemometer.readWindSpeed());
        wait_ms(3000);
    }
}
```

## Sampling rate

The anemometer measures wind speed by firing interrupts for each rotation. Thus, take a sample rate of at least a few seconds to get accurate readings. To keep state the driver uses an internal timer (low power timer if available for your platform). This timer is reset every time you call `readWindSpeed`. The sample rate is therefore controlled by how often you call this function.
