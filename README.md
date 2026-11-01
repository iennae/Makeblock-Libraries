# Makeblock Library (`iennae` Original mBot / mCore Fork)

Arduino C++ Library and Firmwares for Makeblock Electronic Modules, maintained with first-class support for the **Original Makeblock mBot (`mCore` / `ATmega328P`)**.

---

## About This Fork (`iennae/Makeblock-Libraries`)

This repository is a customized fork maintained to ensure the **original Makeblock mBot (`mCore` / `Arduino Uno`)** compiles cleanly on modern Arduino toolchains and pairs seamlessly with **[`iennae/python-for-mbot`](https://github.com/iennae/python-for-mbot)**.

While newer robot kits like the mBot2 support Python natively at a significantly higher price point, the original mBot remains a widely available, affordable platform. Once a builder has assembled the original mBot and explored its pre-built remote-control modes and block-based programming, this library provides the **Online Serial Firmware (`mbot_firmware.ino`)** needed to bridge directly into writing real Python programs from a computer.

### Key Updates in This Fork
- Restored `arduino:avr:uno` (`ATmega328P` / `MeMCore.h`) compilation compatibility alongside `MeAuriga` and `MeMegaPi` boards.
- Fixed an out-of-bounds buffer read in `sendShort()` inside `examples/Firmware_For_mBlock/mbot_firmware/mbot_firmware.ino`.
- Resolved all 46 `avr-gcc` compiler warnings across the library modules and mBot firmwares.
- Added automated GitHub Actions CI (`arduino-lint` and `arduino/compile-sketches` targeting `arduino:avr:uno`).

---

## ⚠️ Use at Your Own Risk & Safety Disclaimer

> **IMPORTANT — USE AT YOUR OWN RISK:**
> This software and firmware are provided **"AS IS"**, without warranty of any kind, express or implied. By downloading, flashing, or using this software with physical hardware, you assume all risks associated with its operation.
>
> - **Moving Parts & Pinch Hazards**: Keep fingers, hair, loose clothing, and cables clear of spinning wheels, gears, and motors.
> - **Drop & Collision Hazards**: Autonomous or tethered motor commands can drive the robot off desks or tables unexpectedly. Prop the robot's wheels off the surface on a test stand when testing new code, or operate on the floor.
> - **Electrical & Battery Safety**: Use only the manufacturer-recommended battery pack and observe proper polarity. Disconnect power if any component becomes hot or emits an unusual odor.

---

## Original mBot (`mCore` / `ATmega328P`) Firmware & Python Setup

To control an original mBot from Python using [`iennae/python-for-mbot`](https://github.com/iennae/python-for-mbot), you only need to flash `mbot_firmware.ino` onto the mBot **once** using the Arduino IDE:

### Step 1: Install Arduino IDE & USB Driver
1. Download and install the **[Arduino IDE](https://www.arduino.cc/en/software)**.
2. **USB Driver Note (`CH340`)**: The original mBot (`mCore`) uses a WCH `CH340` USB-to-serial chip. Linux includes this driver built-in; on Windows or macOS, if no serial port appears when the mBot is plugged in via USB, install the standard [WCH CH340 USB driver](https://learn.sparkfun.com/tutorials/how-to-install-ch340-drivers/all).

### Step 2: Install This Library (`iennae/Makeblock-Libraries`)
Choose either the GUI `.zip` method or `git clone`:

- **Option A — Arduino IDE `.zip` Installer (Easiest)**:
  1. Download **[`iennae/Makeblock-Libraries` (main.zip)](https://github.com/iennae/Makeblock-Libraries/archive/refs/heads/main.zip)**.
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

### mCore Onboard Pin Mapping (`MeMCore.h`)
- **Onboard RGB LEDs (2x WS2812)**: `PORT_7`, `SLOT_2` (Pin 13)
- **Onboard Light Sensor**: `PORT_6` (`A6`)
- **Onboard Button**: `A7`
- **Onboard Buzzer**: Pin 8
- **DC Motors**: `M1` (Left) and `M2` (Right)

---

## Upstream History & Attribution

> *The sections below are preserved from the upstream [`Makeblock-official/Makeblock-Libraries`](https://github.com/Makeblock-official/Makeblock-Libraries) and community `Makeblock Drive Updated` (v3.31.0) repositories to retain full author context and attribution.*

### Upstream Maintainer Note (Makeblock Library v3.31.0 - Updated)
The original library from Makeblock is not maintained anymore. I have a fleet of Makeblock Ranger robots and I needed to fix some issues with the library. I will try to keep it up to date with the original library.

Feel free to open an issue if you have any problem with the library or create a pull request if you have a fix.

### Copyright notice

In makeblock's library, some of the modules are derived from other open source projects, and also part of some code is inspired by the algorithms of other individuals or organizations. We will retain the copyright of the original open source code.

These modules is derived from other open source projects:

- MeRGBLed
- MeHumitureSensor
- Me7SegmentDisplay
- MeOneWire
- MeStepper
- [MeRGBLineFollower](./doc/devices/MeRGBLineFollower.md)

and these modules is inspired by some projects:

- MeUSBHost

As an open source library, we respect all contributors to the open source community and thank you very much for everyone's supervision.

If you have a discussion about licensing issues, please contact me (myan@makeblock.com -- Mark Yan)

### Board Header Mapping

Depending on the type of board you're using in custom Arduino sketches, modify the header file to match:

- Orion <--------> `MeOrion.h`
- BaseBoard <----> `MeBaseBoard.h`
- mCore <--------> `MeMCore.h` *(Original mBot)*
- Shield <-------> `MeShield.h`
- Auriga <-------> `MeAuriga.h` *(mBot Ranger)*
- MegaPi <-------> `MeMegaPi.h` *(Ultimate 2.0)*

### Include guidance

For sketches and examples, using the board header is the simplest option:

```cpp
#include <MeMCore.h> // Or <MeAuriga.h> for mBot Ranger
```

For reusable classes and custom libraries, prefer including only the module headers you actually use, for example:

```cpp
#include <MeRGBLed.h>
#include <MeGyro.h>
#include <MeSoundSensor.h>
```

This keeps dependencies smaller and avoids pulling board-level globals into unrelated headers, which helps prevent linker redefinition issues such as `encoder_Port`.

### IR receiver / Tone (buzzer) conflict

The library includes an IR receiver implementation (`MeIR`) that uses a hardware timer ISR. Some buzzer/tone libraries (for example `Tone` or `ezBuzzer`) use the same AVR timer and will cause a linker error like:

```
multiple definition of `__vector_13`
collect2.exe: error: ld returned 1 exit status
```

To avoid this conflict you can disable the built-in IR interrupt handler. The library provides a small configuration header `src/MeIR_config.h` with a toggle:

1. Open `src/MeIR_config.h` in the library folder.
2. Uncomment the line `#define ME_IR_DISABLE_ISR` to disable MeIR's ISR.
3. Restart the Arduino IDE (or PlatformIO) to force a clean library rebuild.

When `ME_IR_DISABLE_ISR` is defined, the MeIR implementation will not define the timer interrupt vector and will not conflict with other libraries that need that timer. You can still use MeIR's send functions; however, receiving via the built-in ISR will be disabled — you'll need to poll `MeIR::loop()` or use another receiver approach if necessary.

If you prefer not to edit the library each time, create a local patch or a config file under your copy of the library (the repository includes `MeIR_config.h` for that purpose).

### New Features in v3.29.0 - Enhanced MeGyro Class

The MeGyro class has been significantly enhanced with full accelerometer and temperature sensor support for the MPU-6050 chip:

#### Accelerometer Methods:
- `double getAccX()` - Get X-axis acceleration in g units (gravity = 1.0g)
- `double getAccY()` - Get Y-axis acceleration in g units
- `double getAccZ()` - Get Z-axis acceleration in g units  
- `double getAcc(uint8_t index)` - Get acceleration for specified axis (1=X, 2=Y, 3=Z)

#### Temperature Method:
- `double getTemperature()` - Get temperature from MPU-6050 internal sensor in degrees Celsius

#### Usage Example:
```cpp
#include "MeAuriga.h"

MeGyro gyro(0, 0x69); // For Auriga onboard gyro

void setup() {
  Serial.begin(115200);
  gyro.begin();
}

void loop() {
  gyro.update();
  
  // Read gyroscope angles (degrees)
  Serial.print("Angles - X: "); Serial.print(gyro.getAngleX(), 2);
  Serial.print(" Y: "); Serial.print(gyro.getAngleY(), 2);
  Serial.print(" Z: "); Serial.println(gyro.getAngleZ(), 2);
  
  // Read accelerations (g units)
  Serial.print("Accel - X: "); Serial.print(gyro.getAccX(), 3);
  Serial.print(" Y: "); Serial.print(gyro.getAccY(), 3);
  Serial.print(" Z: "); Serial.println(gyro.getAccZ(), 3);
  
  // Read temperature
  Serial.print("Temperature: "); Serial.print(gyro.getTemperature(), 1);
  Serial.println(" °C");
  
  delay(100);
}
```

#### Example Programs:
- **Auriga_MeGyroTest**: Enhanced test program with Serial Plotter compatibility
- **AurigaTempSensors**: Demonstrates reading from both gyro temperature sensor and onboard NTC thermistor

### Revision of history:

|Author      |       Time      |   Version    |    Descr     |
|:--------   |      :-----:    |   :----:     |    :-----    |
|Mark Yan    |     2015/07/24  |   3.0.0      |    Rebuild the old lib.|
|Rafael Lee  |     2015/09/02  |   3.1.0      |    Added some comments and macros.|
|Lawrence    |     2015/09/09  |   3.2.0      |    Include some Arduino's official headfiles which path specified.|
|Mark Yan    |     2015/11/02  |   3.2.1      |    fix bug on MACOS.|
|Mark Yan    |     2016/01/21  |   3.2.2      |    fix some library bugs.|
|Mark Yan    |     2016/05/17  |   3.2.3      |    add support for MegaPi and Auriga Board.|
|Mark Yan    |     2016/07/27  |   3.2.4      |    fix some JIRA issue, add PID motion for Megapi/Auriga on board encoder motor.|
|Mark Yan    |     2018/05/16  |   3.2.5      |    Correct copyright information.|
|Vincent He  |     2019/01/04  |   3.2.6      |    1.Mbot /ranger adds the function of communication variables. 2.Solve the blocking problem of 9g steering gear. 3.Solve the problem that the intelligent steering gear cannot read back the parameters. 4.Add version number function. 5.High power code motor reinforcement version query function. 6.Solve the problem of SetColor (uint8_t index, long value) function error in mergharp.cpp. 7.The mBot board cannot extinguish the RGB. First upload the program with the RGB in any color, and then upload the program with the RGB in all colors. The RGB cannot extinguish (MeRGBLed bled. CPP file). 8.In the MegaPi firmware, SLOT1 is changed to slot_num instead of parameter transmission in the command processing stepper motor.|
|Vincent He  |     2019/09/02  |   3.2.7      |    1.fix the problem that the electronic compass Mecompass is hung on the Orion mainboard 7 or 8 ports and communication will be hung dead. 2.fix the problem that the function getPointFast() in MeHumitureSensor.cpp does not normally output the value. 3.fix the problem that compile smartservo_test.ino firmware error report using the arduino1.6.5 environment with mBlock V3.4.12. 4.remove MeSuperVariable.cpp/MeSuperVariable.h. 5.fix the problem that ultrasonic module can only measure the maximum range of 375cm,and the maximum range of normal requirements is 400cm.|
| Nicolas Bourré |  2023/10/16 | 3.28 | 1. Added missing `gyro.getGyroZ`. 2. Compliant semver.org version number. 3. Added `gyro.resetData` function. 4. Modified the gyro address if it is an Auriga board. |
| Nicolas Bourré |  2025/04/30 | 3.28.1 | Added the MeRGBLineFollower class. |
| Nick B |  2025/09/24 | 3.29.0 | 1. Added `getTemperature()` method to MeGyro class for MPU-6050 internal temperature sensor. 2. Added full accelerometer functionality to MeGyro class with `getAccX()`, `getAccY()`, `getAccZ()`, and `getAcc(index)` methods. 3. Fixed multiple definition linker errors by moving MeAuriga global array initializations to .cpp file. 4. Fixed accelerometer initialization by properly configuring register 0x1c for ±2g range. 5. Added AurigaTempSensors example demonstrating dual temperature sensor reading (gyro + onboard NTC). 6. Updated Auriga_MeGyroTest example with Serial Plotter compatibility and comprehensive sensor data display. 7. Enhanced MeGyro class with proper accelerometer sensitivity handling and raw data processing. |
| koekjesbakker |  2026/05/07 | 3.30.0 | 1. Added the `MeAudioPlayer` class back to the library. 2. Restored the `SimplePlayback` example. 3. Kept the Auriga-related integration changes included with the audio player update. |
| Nick B |  2026/10/05 | 3.31.0 | Added opt-in cascaded P/I/D position control while preserving legacy behavior by default, configurable position deadband (PWM cutoff inside the deadband in both modes, legacy P/D hold outside it), and an Auriga serial PID tuning example. |

## Issues

### Remove old MakeBlock library
If you have problem with the compiler complaining of ambiguous reference to `MePort`, you may have an old version of the MakeBlock library installed. You can remove it by uninstalling the official Makeblock library via the library manager.
