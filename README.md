# Nigel's Potty Timer

A touchscreen timer application for the ESP32-32E LCD display to track our dog Nigel's potty breaks throughout the night.

## Purpose

Nigel wakes us up multiple times at night to go potty. This device sits by the door (visible from bed) and helps us track:
- How long since his last potty break
- Whether it's time to take him out again (via color-coded display)
- A log of all potty break durations

## Hardware

**Device:** 2.8" ESP32-32E Display Module
- **Display:** 240×320 ILI9341 TFT LCD (SPI)
- **Touch:** XPT2046 resistive touchscreen (SPI)
- **CPU:** ESP32-WROOM-32E (Xtensa dual-core @ 240MHz)
- **Memory:** 4MB Flash, 520KB SRAM
- **Link:** https://www.lcdwiki.com/2.8inch_ESP32-32E_Display

### Pin Configuration

**LCD Display:**
- CS: GPIO15
- DC: GPIO2
- SCK: GPIO14
- MOSI: GPIO13
- MISO: GPIO12
- Backlight: GPIO21

**Touch Controller:**
- T_CS: GPIO33
- T_CLK: GPIO25
- T_DIN: GPIO32
- T_DO: GPIO39
- T_IRQ: GPIO36

## How It Works

### On Boot
1. Device logs a "Boot" entry to flash storage
2. Display shows "Touch to Start" with timer at 0:00:00
3. Background is red (not started)

### Operation
1. **First touch:** Timer starts counting up from 0:00:00
2. **Subsequent touches:** 
   - Log entry written with duration since last touch
   - Timer resets to 0:00:00
   - Timer immediately restarts

### Color Coding (Visible from Bed)

The background color indicates time since last potty break:

- 🔴 **Red:** 0-30 minutes (too soon to go again)
- 🟡 **Yellow:** 30 minutes - 2 hours (borderline)
- 🟢 **Green:** 2+ hours (definitely time to go)

### Timer Display

- **Format:** hh:mm:ss (hours:minutes:seconds)
- **Updates:** Every second
- **Touch debounce:** Prevents accidental double-touches

### Data Logging

- **Storage:** LittleFS (ESP32 flash memory)
- **File:** `/logs.txt`
- **Format:** Simple text entries with relative timestamps
  ```
  Boot at +0ms
  Duration: 01:23:45
  Duration: 00:45:12
  ```
- **Persistence:** Logs survive power cycles

## Configuration

### Thresholds (constants in code)

```cpp
const int THRESHOLD_YELLOW = 1800;  // 30 minutes (seconds)
const int THRESHOLD_GREEN = 7200;   // 2 hours (seconds)
```

### Touch Debouncing

Configurable debounce delay to prevent accidental double-touches (default: 2 seconds).

## Development

### Platform

- **Framework:** PlatformIO
- **Build System:** Arduino framework for ESP32
- **IDE:** VS Code with PlatformIO extension

### Dependencies

- `bodmer/TFT_eSPI` - Display driver library
- `paulstoffregen/XPT2046_Touchscreen` - Touch controller library
- Built-in ESP32 libraries: `LittleFS.h`, `Preferences.h`

### Setup

1. Install PlatformIO in VS Code
2. Clone this repository
3. Open folder in VS Code
4. Build and upload to ESP32-32E device

```bash
pio run --target upload
```

## Future Enhancements (v2+)

- ⚙️ Settings screen (gear icon) to:
  - Configure thresholds
  - View complete log history
  - Clear logs
  - Adjust display brightness
- 📶 WiFi + NTP for real timestamps (date/time)
- 🔄 Log rotation/management (auto-delete old entries)
- 📊 Statistics view (average duration, frequency patterns)
- 🔊 Audio alerts via DAC (GPIO26) when green threshold reached
- 🌙 Night mode (dimmed display after certain hours)

## Version

**v1.0** - Initial release with basic timer, color coding, and duration logging

## License

Personal project for household use.

---

*Built with ❤️ for Nigel the dog* 🐕
