### LiquidCrystal_I2C

Arduino library for HD44780-compatible LCDs driven via an I2C backpack (e.g. PCF8574).

This library mirrors the familiar LiquidCrystal API and lets you use common LCDs (16x2, 20x4, etc.) over I2C with just two wires.

- **Features**: clear/display, cursor/blink, scrolling, LTR/RTL, autoscroll, custom characters, backlight control.
- **Targets**: Arduino AVR, and commonly used on ESP8266/ESP32 as well.
- **Examples included**: `HelloWorld`, `SerialDisplay`, `CustomChars`.

Note: This repo historically referenced a migration. The original lineage continues at `https://gitlab.com/tandembyte/LCD_I2C`. This copy is arranged for typical Arduino/PlatformIO consumption and includes examples and headers consistent with common community usage.

---

### Hardware and Wiring

Most I2C LCD backpacks (PCF8574/PCF8574A) expose the following pins:

- `GND` → Arduino GND
- `VCC` → 5V (many also work at 3.3V)
- `SDA` → Arduino SDA (A4 on UNO/Nano; D21 on MEGA; on ESP boards use the board's SDA pin)
- `SCL` → Arduino SCL (A5 on UNO/Nano; D20 on MEGA; on ESP boards use the board's SCL pin)

Common I2C addresses are `0x27` and `0x3F`. If you're unsure, run an I2C scanner sketch to discover your module's address.

---

### Installation

- Arduino IDE (Manual):
  1) Download or clone this repository
  2) Place the folder into your `Documents/Arduino/libraries` directory as `LiquidCrystal_I2C`
  3) Restart Arduino IDE and open File → Examples → LiquidCrystal_I2C

- PlatformIO:
  - This repo includes `library.json`. You can add it to your project by referencing the Git URL or by placing the library in `lib/`.

---

### Quick Start

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Typical addresses: 0x27 or 0x3F. Adjust cols/rows to your display.
LiquidCrystal_I2C lcd(0x27, 20, 4);

void setup() {
  lcd.init();           // Initialize the display
  lcd.backlight();      // Turn on backlight
  lcd.setCursor(3, 0);
  lcd.print("Hello, world!");
}

void loop() {}
```

See more in `examples/HelloWorld/HelloWorld.pde`.

---

### Examples

- `examples/HelloWorld/HelloWorld.pde`: Minimal setup and text output
- `examples/SerialDisplay/SerialDisplay.pde`: Echo data from Serial Monitor to the LCD
- `examples/CustomChars/CustomChars.pde`: Define and display custom characters

Open these from your IDE or copy snippets into your sketch.

---

### API Overview

- Constructor: `LiquidCrystal_I2C(uint8_t address, uint8_t cols, uint8_t rows, TwoWire* wire = &Wire)`
  - Pass a custom `TwoWire` instance if needed.
- Initialization:
  - `init()` — standard HD44780 initialization via I2C
  - `oled_init()` — alternative path for certain OLED I2C modules (set `_oled` behavior)
- Display control:
  - `clear()`, `home()`
  - `display()`, `noDisplay()`
  - `cursor()`, `noCursor()`
  - `blink()`, `noBlink()`
  - `backlight()`, `noBacklight()`
- Positioning and text:
  - `setCursor(col, row)`
  - `write(byte)`, `print(...)`, `printstr(const char[])`
- Scrolling/direction:
  - `scrollDisplayLeft()`, `scrollDisplayRight()`
  - `leftToRight()`, `rightToLeft()`
  - `autoscroll()`, `noAutoscroll()`
- Custom characters:
  - `createChar(slot, uint8_t bitmap[8])`
  - `createChar(slot, const char* PROGMEM)`
- Aliases for compatibility:
  - `blink_on()/blink_off()`, `cursor_on()/cursor_off()`, `setBacklight(uint8_t)`, `load_custom_character(...)`

Unsupported placeholders are present for some functions (e.g., `setContrast`, graphs) and are no-ops; see source for details.

---

### Notes and Tips

- If your screen shows garbled characters or nothing at all:
  - Verify the I2C address (`0x27` vs `0x3F`), wiring, and contrast pot on the backpack
  - Confirm that `cols`/`rows` in the constructor match your LCD (e.g., `16,2` or `20,4`)
- For non-default I2C pins or multiple buses, pass your own `TwoWire` instance to the constructor.
- For certain OLED I2C displays that mimic HD44780, try `oled_init()` after constructing the object.

---

### Platform Support

Declared targets include AVR and ESP8266 (per `library.json`). Many users also report success on ESP32 and other Arduino-compatible boards using the appropriate `Wire` pins and voltage.

---

### Acknowledgements

Originally authored by Frank de Brabander and maintained by community contributors. Portions are based on DFRobot work and common HD44780 initialization sequences. See `library.properties` and commit history for attribution.

