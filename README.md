# ESP32 Analog Clock

A smooth, anti-aliased analog clock for ESP32 "Cheap Yellow Display" (CYD) boards with NTP time synchronization.

## Features

- **Beautiful analog clock face** with anti-aliased rendering for smooth hands and dial
- **NTP time sync** - automatically fetches accurate time from the internet
- **Automatic DST handling** - supports multiple time zones with daylight saving rules
- **Digital time/date overlay** - shows 12-hour time and date in the center of the clock
- **Smooth animation** - clock hands update every 100ms for fluid movement
- **Multi-board support** - works with both resistive and capacitive touch CYD variants

## Supported Hardware

This project supports two ESP32 "Cheap Yellow Display" variants:

### ESP32-2432S028R (Resistive Touch)
- **Display:** 2.8" 320×240 ILI9341 TFT LCD
- **Touch:** XPT2046 resistive touchscreen (SPI)
- **Backlight:** GPIO 21

### JC2432W328C (Capacitive Touch)
- **Display:** 2.8" 320×240 ST7789 TFT LCD
- **Touch:** CST816S capacitive touch (I2C)
- **Backlight:** GPIO 27

### Common Pin Configuration

| Function | GPIO |
|----------|------|
| TFT CS   | 15   |
| TFT DC   | 2    |
| TFT SCK  | 14   |
| TFT MOSI | 13   |
| TFT MISO | 12   |

## Configuration

### WiFi Credentials

Edit the credentials in `src/main.cpp`:

```cpp
#define WIFI_SSID      "YourSSID"
#define WIFI_PASSWORD  "YourPassword"
```

### Time Zone

The default time zone is US Central. To change it, edit `src/NTP_Time.h`:

```cpp
#define TIMEZONE usCT  // Change to UK, usET, usPT, usMT, euCET, ausET, etc.
```

Available time zones:
- `UK` - United Kingdom (London)
- `euCET` - Central European Time (Paris, Frankfurt)
- `usET` - US Eastern (New York)
- `usCT` - US Central (Chicago)
- `usMT` - US Mountain (Denver)
- `usAZ` - Arizona (no DST)
- `usPT` - US Pacific (Los Angeles)
- `ausET` - Australia Eastern (Sydney)

## Building & Flashing

### Prerequisites

- [PlatformIO](https://platformio.org/) (VS Code extension or CLI)

### Build Commands

For **resistive touch** boards (ESP32-2432S028R):
```bash
pio run -e cyd_resistive
pio run -e cyd_resistive -t upload
```

For **capacitive touch** boards (JC2432W328C):
```bash
pio run -e cyd_capacitive
pio run -e cyd_capacitive -t upload
```

### Monitor Serial Output

```bash
pio device monitor
```

The serial output shows detailed debug info including chip info, memory usage, and display configuration.

## Dependencies

Managed automatically by PlatformIO:

- [TFT_eSPI](https://github.com/Bodmer/TFT_eSPI) - TFT display driver with anti-aliased graphics
- [TJpg_Decoder](https://github.com/Bodmer/TJpg_Decoder) - JPEG decoder
- [Time](https://github.com/PaulStoffregen/Time) - Time keeping library
- [Timezone](https://github.com/JChristensen/Timezone) - Timezone and DST handling
- [XPT2046_Touchscreen](https://github.com/PaulStoffregen/XPT2046_Touchscreen) - Touch controller (resistive variant only)

## Project Structure

```
├── platformio.ini          # Build configuration for both board variants
├── src/
│   ├── main.cpp            # Main application code
│   ├── NTP_Time.h          # NTP sync and timezone definitions
│   ├── dial.h              # Clock dial graphics
│   ├── NotoSansBold15.h    # Font for clock labels
│   └── NotoSansBold36.h    # Large font
```

## Credits

Anti-aliased clock rendering based on a sketch by [DavyLandman](https://github.com/Bodmer/TFT_eSPI/issues/905).

## License

MIT License
