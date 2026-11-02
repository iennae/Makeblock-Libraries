# Makeblock Library (`iennae` Original mBot Fork)

Arduino C++ Library and Firmwares for the **Original Makeblock mBot (`mCore` / `ATmega328P`)**.

---

## What This Project Is & What This Fork Does Differently

This repository is a customized fork maintained specifically to support the **original Makeblock mBot (`mCore` / `Arduino Uno`)** and pair with **[`iennae/python-for-mbot`](https://github.com/iennae/python-for-mbot)**.

While newer robot kits like the mBot2 support Python natively at a significantly higher price point, the original mBot remains an affordable, approachable hardware kit. Once a builder has assembled the original mBot and explored its pre-built remote-control modes and block-based programming, this repository provides the **Online Serial Firmware (`mbot_firmware.ino`)** needed to bridge into introductory Python programming on a computer.

Unlike the upstream repository (which evolved to focus on the Makeblock Ranger / `MeAuriga`), this fork is focused on and tested exclusively against the **original mBot (`mCore`)**.

- For release notes and historical version changes, see **[`CHANGELOG.md`](CHANGELOG.md)**.
- For upstream author credit and open-source module attribution, see **[`ATTRIBUTION.md`](ATTRIBUTION.md)**.

---

## ⚠️ Use at Your Own Risk & Scope Disclaimer

> **IMPORTANT — USE AT YOUR OWN RISK:**
> This software and firmware are provided **"AS IS"**, without warranty of any kind, express or implied. By downloading, flashing, or using this software with physical hardware, you assume all risks associated with its operation.
>
> - **Hardware Scope**: This fork is only tested with the **original Makeblock mBot (`mCore` / `ATmega328P`)**. Other Makeblock boards are not tested in this fork and may not work as expected.
> - **Moving Parts & Pinch Hazards**: Keep fingers, hair, loose clothing, and cables clear of spinning wheels, gears, and motors.
> - **Drop & Collision Hazards**: Tethered or autonomous motor commands can drive the robot off desks or tables unexpectedly. Prop the robot's wheels off the surface on a test stand when testing new code, or operate on the floor.
> - **Electrical & Battery Safety**: Use only the manufacturer-recommended battery pack and observe proper polarity. Disconnect power if any component becomes hot or emits an unusual odor.

---

## Original mBot (`mCore`) Firmware Setup

To control an original mBot from Python using [`iennae/python-for-mbot`](https://github.com/iennae/python-for-mbot), flash `mbot_firmware.ino` onto the mBot **once** using the Arduino IDE:

### Step 1: Install Arduino IDE & USB Driver
1. Download and install the **[Arduino IDE](https://www.arduino.cc/en/software)**.
2. **USB Driver Note (`CH340`)**: The original mBot (`mCore`) uses a WCH `CH340` USB-to-serial chip. Linux includes this driver built-in; on Windows or macOS, if no serial port appears when the mBot is plugged in via USB, install the standard [WCH CH340 USB driver](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all).

### Step 2: Install This Library (`iennae/Makeblock-Libraries`)
Choose either the GUI `.zip` method or `git clone`:

- **Option A — Arduino IDE `.zip` Installer (Easiest)**:
  1. Download **[`iennae/Makeblock-Libraries` (`main.zip`)](https://github.com/iennae/Makeblock-Libraries/archive/refs/heads/main.zip)**.
  2. In Arduino IDE, click **`Sketch -> Include Library -> Add .ZIP Library...`** and select the downloaded `.zip` file.
- **Option B — Clone into Your Arduino Sketchbook `libraries/` Folder**:
  ```bash
  # macOS / Windows (Git Bash):
  git clone https://github.com/iennae/Makeblock-Libraries.git ~/Documents/Arduino/libraries/Makeblock-Libraries

  # Linux:
  git clone https://github.com/iennae/Makeblock-Libraries.git ~/Arduino/libraries/Makeblock-Libraries
  ```
  *(Restart the Arduino IDE if it was already open.)*

### Step 3: Upload the Online Serial Firmware (`mbot_firmware.ino`)
1. Connect your mBot to your computer with the USB cable.
2. In Arduino IDE, open **`File -> Examples -> MakeBlock Drive Updated -> Firmware_For_mBlock -> mbot_firmware`**.
3. Select the board: **`Tools -> Board -> Arduino AVR Boards -> Arduino Uno`**.
4. Select the port: **`Tools -> Port`** (`COM*` on Windows, `/dev/cu.usbserial-*` or `/dev/cu.wchusbserial*` on macOS, `/dev/ttyUSB0` on Linux).
5. Click **Upload (`→`)** and wait for **"Done uploading."**
6. **Close the Arduino IDE** before running Python scripts so the USB serial port is released.

### Which Firmware Should I Use?
- **`examples/Firmware_For_mBlock/mbot_firmware/mbot_firmware.ino`**: **Online Serial Firmware** (`115200` baud) required when controlling the mBot interactively from Python (`iennae/python-for-mbot`).
- **`examples/Firmware_For_mBlock/mbot_factory_firmware/mbot_factory_firmware.ino`**: **Factory Default Firmware** — flash this anytime you want to restore the mBot's out-of-the-box infrared remote control, button mode switching, obstacle avoidance, and line-following demo modes.

---

## Original mBot (`mCore`) Technical Reference

### Onboard Pin Mapping (`MeMCore.h`)
For custom Arduino sketches targeting the original mBot, include `#include <MeMCore.h>`:

- **Onboard RGB LEDs (2x WS2812)**: `PORT_7`, `SLOT_2` (Pin 13)
- **Onboard Light Sensor**: `PORT_6` (`A6`)
- **Onboard Button**: `A7`
- **Onboard Buzzer**: Pin 8
- **DC Motors**: `M1` (Left) and `M2` (Right)

### Troubleshooting

#### Ambiguous Reference to `MePort`
If the compiler reports an ambiguous reference to `MePort`, an older version of the Makeblock library is likely installed alongside this one. Remove the older Makeblock library from your `Arduino/libraries/` folder or uninstall it via the Arduino Library Manager.

#### IR Receiver (`MeIR`) / Tone (Buzzer) Timer Conflict
`MeIR` uses a hardware timer ISR on AVR. If a third-party `Tone` library conflicts with `__vector_13`, open `src/MeIR_config.h`, uncomment `#define ME_IR_DISABLE_ISR`, and restart the Arduino IDE.
