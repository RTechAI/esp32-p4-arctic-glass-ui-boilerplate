# Arctic Glass — ESP32-P4 LVGL 9 UI/HMI boilerplate

![Arctic Glass conceptual UI artwork](<Splash arctic glass boilerplate.png>)

Arctic Glass is a single-screen embedded UI/HMI boilerplate for the ESP32-P4, built with ESP-IDF and LVGL 9 for the Waveshare ESP32-P4-WIFI6-Touch-LCD-7B. Its Arctic Glass interface is a light, frosted-panel design with cyan accents and dashboard-style controls.

This repository provides a ForgeUI One runtime baseline and a historically exported Arctic Glass LVGL screen that developers can build, flash, and adapt for ESP32-P4 touchscreen work.

> The hero image is conceptual UI artwork. This repository does not include a physical-hardware photograph.

## ForgeUI Ecosystem

ForgeUI is developed by [RTechAI](https://github.com/RTechAI).

The [ForgeUI website](https://forgeui.co.nz) is the official home of the ForgeUI embedded UI/HMI development ecosystem. ForgeUI Studio is the current visual embedded UI/HMI development environment for supported ESP32 hardware. [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) is the hosted, browser-based ForgeUI Studio application and is available for public registration.

RTechAI's GitHub organization hosts ForgeUI public repositories, hardware references, framework baselines, examples, and related open development work. Arctic Glass is one such historical ForgeUI One baseline: a board-specific reference project with a self-contained UI export and runtime services.

## Overview

The project initializes the board display through its Waveshare BSP, creates the generated Arctic Glass screen, and enables the configured Wi-Fi, RTC, and SD-card services. It is intentionally a small starting point rather than a multi-screen application.

## Hardware Target

| Item | Verified configuration |
| --- | --- |
| Board | Waveshare ESP32-P4-WIFI6-Touch-LCD-7B |
| MCU target | ESP32-P4 (`esp32p4`) |
| Display | 7-inch, 1024×600 MIPI DSI panel using EK79007 |
| Touch | GT911 capacitive touch controller |
| Wireless architecture | ESP-Hosted over SDIO to the board's ESP32-C6 |
| Optional peripherals enabled by default | DS3231 RTC and SD card |

## Software Stack

- ESP-IDF 5.5.4
- LVGL 9.2.2, via `esp_lvgl_port` 2.7.2
- Waveshare `esp32_p4_wifi6_touch_lcd_7b` BSP 1.0.2
- Espressif managed components for EK79007, GT911, ESP-Hosted, and Wi-Fi Remote
- ForgeUI One application runtime modules in `main/`

## What This Project Demonstrates

- Board display and touch bring-up through the Waveshare BSP.
- An LVGL 9 Arctic Glass screen exported into `main/90_Studio_Export.c`.
- An ESP-Hosted Wi-Fi status path, including scan, connect, disconnect, and status reporting.
- A DS3231-backed RTC path with NVS fallback.
- SD-card mounting and a read/write smoke test, after Wi-Fi initialization.

The audio module is present but disabled in the default ForgeUI configuration.

## UI / Theme

Arctic Glass uses a bright, spacious HMI treatment: frosted cards, soft white surfaces, cyan/ice-blue accents, and gauge-like information panels. The checked-in screen is a single page; the `90_Studio_Export.c` source supplies its clock and Wi-Fi status updates.

## Hardware and Runtime Baseline

`main.c` owns board start-up and service order. It starts the display, constructs the ForgeUI runtime, initializes the RTC, starts ESP-Hosted Wi-Fi, then mounts the SD card. This ordering is documented in the source as the project's stable Hosted Wi-Fi plus SD sequence.

Feature switches live in `main/00_ForgeUI_Config.h`. The defaults enable RTC, Wi-Fi, and SD support and leave audio disabled.

## Project Structure

```text
.
├── main/
│   ├── main.c                  # Application boot and service order
│   ├── 00_ForgeUI_Config.h     # Runtime feature switches
│   ├── 01_FG_Runtime.c         # LVGL runtime entry point
│   ├── 20_RTC.c                # DS3231/NVS time service
│   ├── 30_WIFI.c               # ESP-Hosted Wi-Fi service
│   ├── 40_SD.c                 # SD-card service
│   └── 90_Studio_Export.c      # Arctic Glass LVGL screen export
├── components/bsp_extra/       # Project BSP extension component
├── docs/                       # Setup references and visual assets
├── dependencies.lock           # Pinned ESP-IDF component versions
└── sdkconfig.defaults          # ESP32-P4 build defaults
```

## Build and Flash

Use an ESP-IDF 5.5.4 environment, then from the repository root:

```bash
idf.py set-target esp32p4
idf.py build
idf.py -p PORT flash monitor
```

Replace `PORT` with the board's serial port. If the target is already set, the first command is unnecessary. The default configuration expects the Waveshare ESP32-P4-WIFI6-Touch-LCD-7B and 16 MB QIO flash.

## Historical ForgeUI Context

The source and prior project materials identify this boilerplate as built with the earlier [ESP32-P4 UI Studio](https://github.com/RTechAI/esp32p4-ui-studio) and powered by [ForgeUI One](https://github.com/RTechAI/ForgeUI-One). That is historical lineage for this checked-in export and runtime; it does not imply that the project was generated by the current Hosted Studio.

## Current ForgeUI Studio

[ForgeUI Studio](https://forgeui.co.nz) is the current visual embedded UI/HMI development environment for supported ESP32 hardware. [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) is its hosted, browser-based application and is available for public registration.

## Related ForgeUI Projects

- [ForgeUI One](https://github.com/RTechAI/ForgeUI-One) — historical runtime lineage.
- [ESP32-P4 UI Studio](https://github.com/RTechAI/esp32p4-ui-studio) — historical visual-design and export lineage.
- [ForgeUI P4](https://github.com/RTechAI/ForgeUI-P4) — related ESP32-P4 ForgeUI work.
- [ESP32-P4 LVGL Boilerplate 3](https://github.com/RTechAI/ESP32-P4-LVGL-Boilerplate-3) — related ESP32-P4 LVGL baseline.

## About ForgeUI

[ForgeUI](https://forgeui.co.nz) is developed by [RTechAI](https://github.com/RTechAI). ForgeUI Studio provides visual embedded UI/HMI development workflows for supported ESP32 hardware, while [ForgeUI Hosted Studio](https://studio.forgeui.co.nz) provides the hosted, browser-based Studio application. RTechAI is the GitHub home for ForgeUI public repositories and reference work.

## License and Third-Party Software

ForgeUI-owned code in this repository is governed by the [ForgeUI Source Available License](LICENSE). Third-party components retain their own licenses; see [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) and the relevant component sources, including the Apache-2.0-licensed `components/bsp_extra/` component.

The root project license is not MIT. LVGL is one of several third-party components and is MIT-licensed under its own terms.

## Support

For ForgeUI information and current Studio resources, visit [forgeui.co.nz](https://forgeui.co.nz). For public ForgeUI repositories and reference work, visit [RTechAI on GitHub](https://github.com/RTechAI).
