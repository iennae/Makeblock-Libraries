# Changelog

## `iennae/Makeblock-Libraries` (Original mBot / `mCore` Fork)

### Key Updates in This Fork
- Restored `arduino:avr:uno` (`ATmega328P` / `MeMCore.h`) compilation compatibility.
- Fixed an out-of-bounds buffer read in `sendShort()` inside `examples/Firmware_For_mBlock/mbot_firmware/mbot_firmware.ino`.
- Resolved all 46 `avr-gcc` compiler warnings across the library modules and mBot firmwares.
- Added automated GitHub Actions CI (`arduino-lint` and `arduino/compile-sketches` targeting `arduino:avr:uno`).
- Scoped documentation and setup guides to the original Makeblock mBot (`mCore`) for use with [`iennae/python-for-mbot`](https://github.com/iennae/python-for-mbot).

---

## Upstream Revision History

| Author | Time | Version | Description |
| :--- | :---: | :---: | :--- |
| Mark Yan | 2015/07/24 | 3.0.0 | Rebuild the old lib. |
| Rafael Lee | 2015/09/02 | 3.1.0 | Added some comments and macros. |
| Lawrence | 2015/09/09 | 3.2.0 | Include some Arduino's official headfiles which path specified. |
| Mark Yan | 2015/11/02 | 3.2.1 | Fix bug on macOS. |
| Mark Yan | 2016/01/21 | 3.2.2 | Fix some library bugs. |
| Mark Yan | 2016/05/17 | 3.2.3 | Add support for MegaPi and Auriga Board. |
| Mark Yan | 2016/07/27 | 3.2.4 | Fix some JIRA issues, add PID motion for MegaPi/Auriga on board encoder motor. |
| Mark Yan | 2018/05/16 | 3.2.5 | Correct copyright information. |
| Vincent He | 2019/01/04 | 3.2.6 | 1. mBot/Ranger adds the function of communication variables. 2. Solve the blocking problem of 9g steering gear. 3. Solve the problem that the intelligent steering gear cannot read back the parameters. 4. Add version number function. 5. High power code motor reinforcement version query function. 6. Solve the problem of `setColor(uint8_t index, long value)` function error in `MeRGBLed.cpp`. 7. Fix mBot board RGB extinguish behavior in `MeRGBLed.cpp`. 8. In the MegaPi firmware, `SLOT1` is changed to `slot_num` in the command processing stepper motor. |
| Vincent He | 2019/09/02 | 3.2.7 | 1. Fix the problem that the electronic compass `MeCompass` hung on the Orion mainboard 7 or 8 ports. 2. Fix `getPointFast()` in `MeHumitureSensor.cpp`. 3. Fix `smartservo_test.ino` compile error with Arduino 1.6.5 and mBlock V3.4.12. 4. Remove `MeSuperVariable.cpp`/`MeSuperVariable.h`. 5. Fix ultrasonic module maximum range to 400cm. |
| Nicolas Bourré | 2023/10/16 | 3.28 | 1. Added missing `gyro.getGyroZ`. 2. Compliant semver.org version number. 3. Added `gyro.resetData` function. 4. Modified the gyro address if it is an Auriga board. |
| Nicolas Bourré | 2025/04/30 | 3.28.1 | Added the `MeRGBLineFollower` class. |
| Nick B | 2025/09/24 | 3.29.0 | 1. Added `getTemperature()` method to `MeGyro` class for MPU-6050 internal temperature sensor. 2. Added full accelerometer functionality to `MeGyro` class with `getAccX()`, `getAccY()`, `getAccZ()`, and `getAcc(index)` methods. 3. Fixed multiple definition linker errors by moving `MeAuriga` global array initializations to `.cpp` file. 4. Fixed accelerometer initialization by properly configuring register `0x1c` for ±2g range. 5. Added `AurigaTempSensors` example. 6. Updated `Auriga_MeGyroTest` example. 7. Enhanced `MeGyro` class with proper accelerometer sensitivity handling and raw data processing. |
| koekjesbakker | 2026/05/07 | 3.30.0 | 1. Added the `MeAudioPlayer` class back to the library. 2. Restored the `SimplePlayback` example. 3. Kept the Auriga-related integration changes included with the audio player update. |
| Nick B | 2026/10/05 | 3.31.0 | Added opt-in cascaded P/I/D position control while preserving legacy behavior by default, configurable position deadband, and an Auriga serial PID tuning example. |

---

## Upstream v3.29.0 Release Notes — Enhanced `MeGyro` Class

In upstream v3.29.0, the `MeGyro` class was enhanced with accelerometer and temperature sensor support for the MPU-6050 chip:

- **Accelerometer Methods**:
  - `double getAccX()` — Get X-axis acceleration in g units (gravity = 1.0g)
  - `double getAccY()` — Get Y-axis acceleration in g units
  - `double getAccZ()` — Get Z-axis acceleration in g units
  - `double getAcc(uint8_t index)` — Get acceleration for specified axis (`1`=X, `2`=Y, `3`=Z)
- **Temperature Method**:
  - `double getTemperature()` — Get temperature from MPU-6050 internal sensor in degrees Celsius
