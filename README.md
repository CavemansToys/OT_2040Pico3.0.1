# OpenTrickler Controller (RP2040) — OpenTrickler 2.9.0

Firmware for the [OpenTrickler](https://github.com/eamars/OpenTrickler-RP2040-Controller) automated powder trickler, used in precision ammunition reloading. Runs on a **Raspberry Pi Pico W (RP2040)** with FreeRTOS SMP, WiFi, and multiple hardware peripherals.

Developed by Gadgets — [www.cavemanstoys.com](https://www.cavemanstoys.com)

Based on [OpenTrickler-RP2040-Controller](https://github.com/eamars/OpenTrickler-RP2040-Controller).

## Supported Hardware

| Board | SoC | Status |
|---|---|---|
| Raspberry Pi Pico W | RP2040 | NOT Tested |

## Features

- Coarse tube reverse with configurable settle time --- I dont see this helping, but its there to try.
- AI-assisted PID auto-tuning
- Simulation mode for testing without the Scale or powder.
- PWM servo powder gate
- WiFi access point + station mode with web portal

## Prerequisites

- ARM embedded toolchain (`arm-none-eabi-gcc`)
- CMake >= 3.25
- Python 3 (for build scripts)
- Git

## Building

```bash
# Clone with submodules
git clone --recursive https://github.com/<your-org>/OpenTrickler.git
cd OpenTrickler

# If you forgot --recursive:
git submodule update --init --recursive

# Build for Pico W (RP2040, default) with Mini 12864 display
mkdir build && cd build
cmake -G Ninja ..
ninja

## Flashing

1. Hold **BOOTSEL** while plugging the Pico into USB
2. Copy `build/OpenTrickler_2.9.0.uf2` to the Pico mass storage device
3. The Pico reboots automatically

## Output Artifacts

| File | Description |
|---|---|
| `build/OpenTrickler_2.9.0.uf2` | Flashable firmware (drag-and-drop) |
| `build/OpenTrickler_2.9.0.elf` | For GDB debugging |
| `build/OpenTrickler_2.9.0.bin` | Binary image |

## Debugging

GDB scripts are provided in `scripts/`:
- `scripts/debug.gdb` — attach to running target
- `scripts/program.gdb` — flash and debug

Serial debug output is available via USB CDC at 115200 baud.

## Project Structure

```
src/                   Application source (C/C++)
  html/                Web portal pages (built into C headers at compile time)
  targets/             Board pin definitions
  generated/           Auto-generated files (do not edit, created at build time)
scripts/               Build helper scripts (html2header, version gen, GDB)
targets/               Board pin definitions (root-level include path)
library/               Git submodule dependencies
  pico-sdk/            Raspberry Pi Pico SDK
  FreeRTOS-Kernel/     FreeRTOS with RP2040 SMP port
  u8g2/                Monochrome display library
  Trinamic-library/    TMC stepper driver library
  lvgl/                LVGL for TFT displays (TFT builds only)
```

## Configuration

All persistent settings are stored in CAT24C256 EEPROM (32KB) with CRC32 validation. Configuration can be changed via:
- The web portal (connect to the Pico's WiFi AP or join its STA network)
- REST API endpoints under `/rest/*`
- The on-device menu (Mini 12864 display + rotary encoder)

## License

GPLv3 — see [LICENSE](LICENSE).
