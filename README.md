# Smart Home System Prototype

Arduino-based smart home prototype with password-controlled door access, motion security, fire/temperature warning, garage automation, water-level detection, and a dual power-source hardware setup using solar panels and an electric plug.

## Overview

This project demonstrates a basic smart home control system using an Arduino board and multiple sensors/actuators. The system is designed as a prototype for home automation and security. It includes keypad-based door access, LCD feedback, motion detection, temperature/fire warning, automatic garage door control, and water-level monitoring.

The hardware is powered using two possible sources:

* Solar panel power source
* Electric plug / mains adapter power source

The code controls the smart home features. Power-source switching is handled at the hardware/power circuit level, not directly inside the Arduino sketch.

## Features

* Password-protected main door using a keypad
* LCD screen for user instructions and system messages
* Ability to change the door password
* Servo-controlled door lock/opening mechanism
* PIR motion detection for security
* Alarm trigger when motion is detected while the door is locked
* LED activation when motion is detected while the door is open
* Temperature/fire warning using an analog temperature sensor
* Automatic door opening during high-temperature warning
* Garage door automation using an ultrasonic sensor
* Servo-controlled garage and road/barrier mechanism
* Buzzer warning for close-range garage obstacles
* Water-level detection using an ultrasonic sensor
* Dual power-source prototype design: solar and electric plug

## Hardware Components

* Arduino Mega 2560 or compatible board
* 16x2 LCD display
* 4x3 keypad
* Servo motor for main door
* Servo motor for garage door
* Servo motor for road/barrier gate
* PIR motion sensor
* IR sensor
* Ultrasonic sensor for garage distance detection
* Ultrasonic sensor for water-level detection
* Temperature sensor connected to analog input
* LED
* Buzzer / alarm
* Solar panel power circuit
* Electric plug / DC adapter power circuit
* Breadboard and jumper wires
* External power supply for servos, if required

## Required Arduino Libraries

Install the following libraries in the Arduino IDE:

```cpp
#include <LiquidCrystal.h>
#include <Keypad.h>
#include <Servo.h>
```

`LiquidCrystal` and `Servo` are usually included with the Arduino IDE.
`Keypad` may need to be installed from the Arduino Library Manager.

## Pin Configuration

| Component                      | Arduino Pin(s)         |
| ------------------------------ | ---------------------- |
| LCD                            | 24, 25, 26, 27, 28, 29 |
| Keypad rows                    | 53, 52, 51, 50         |
| Keypad columns                 | 49, 48, 13             |
| Main door servo                | 12                     |
| IR sensor                      | 22                     |
| PIR sensor                     | 7                      |
| LED                            | 40                     |
| Alarm / buzzer                 | 38                     |
| Temperature sensor             | A0                     |
| Garage ultrasonic trigger      | 9                      |
| Garage ultrasonic echo         | 8                      |
| Garage servo                   | 11                     |
| Road/barrier servo             | 2                      |
| Water-level ultrasonic trigger | 6                      |
| Water-level ultrasonic echo    | 5                      |

## Default Password

The default door password is:

```text
1234
```

The password is stored in the following array:

```cpp
int saved_password[4] = {1, 2, 3, 4};
```

## System Controls

### Door Access

1. Enter the 4-digit password using the keypad.
2. Press `#` to submit.
3. If the password is correct:

   * The LCD displays a welcome message.
   * The door servo opens.
   * The system marks the door as open.
4. If the password is incorrect:

   * The LCD displays an incorrect password message.
   * The door remains closed.

### Change Password

1. Press `*` to switch to password-change mode.
2. Enter the old password.
3. Press `#`.
4. Enter the new 4-digit password.
5. Press `#` again to save it.

## Main Functions

### `keypad1()`

Handles keypad input, menu switching, password entry, password change mode, and door-related LCD messages.

### `Enter_Password()`

Checks the entered password against the saved password and opens or closes the door servo depending on whether the password is correct.

### `Change_Password()`

Allows the user to change the saved password after entering the correct old password.

### `PIR()`

Reads the PIR motion sensor.

* If motion is detected and the door is open, the LED turns on.
* If motion is detected and the door is closed, the alarm activates and the LCD displays a security warning.

### `TEMP()`

Reads the temperature sensor from analog pin `A0`.

* If temperature is above the threshold, the system displays a fire warning.
* The buzzer activates.
* The door opens automatically for safety.

### `Garagedoor()`

Uses an ultrasonic sensor to detect distance near the garage.

* If an object is very close, the buzzer turns on.
* If the object is within the garage-opening range, the garage servo and road/barrier servo move.
* Otherwise, the garage returns to the closed/default position.

### `waterlevel()`

Uses a second ultrasonic sensor to detect water level.

* If the measured distance is very low, the buzzer turns on.
* Otherwise, the buzzer remains off.

## Power System

This prototype is designed to support two power sources:

1. **Solar panel source**
2. **Electric plug / adapter source**

Recommended hardware setup:

* Use a regulated 5V supply for the Arduino.
* Use a suitable external supply for servo motors if they draw more current than the Arduino can provide.
* Connect all grounds together.
* Use a charge controller, battery management circuit, relay module, or automatic power-switching circuit if switching between solar and electric plug power.
* Do not connect an unregulated solar panel directly to the Arduino.

The Arduino sketch does not include software-based power-source selection. The power switching should be handled safely through the hardware circuit.

## Installation and Upload

1. Open the project in the Arduino IDE.
2. Select the correct board. Arduino Mega 2560 is recommended because the sketch uses high-numbered digital pins.
3. Select the correct COM port.
4. Install the required libraries.
5. Connect the components according to the pin configuration.
6. Upload the sketch to the Arduino board.
7. Open the Serial Monitor at `9600` baud if serial debugging is needed.

## Notes and Known Issues

* The sketch contains Bluetooth-related variables, but the Bluetooth function is currently not active in the main loop.
* Pin `38` is shared by the alarm, temperature output, and buzzer logic. This may be intentional, but it should be reviewed during wiring.
* Pin `13` is used as a keypad column and is also configured as an output in `setup()`. This should be reviewed to avoid pin conflicts.
* In the `waterlevel()` function, `duration2` should be assigned using `pulseIn(echoPin2, HIGH)`. Currently, the function calculates `distance2` without updating `duration2`.
* Servo angles may need calibration depending on the physical door and garage mechanism.
* The temperature threshold is currently set to `23°C`, which is low for a fire-warning threshold and may need adjustment.

## Future Improvements

* Add Bluetooth or mobile app control.
* Add automatic power-source monitoring.
* Add battery level monitoring for the solar power system.
* Store the password in EEPROM so it remains saved after power loss.
* Add RFID access.
* Add Wi-Fi support using ESP8266 or ESP32.
* Add real-time sensor data logging.
* Add a safer relay-based circuit for high-current devices.
* Improve code structure by separating each module into its own file.

## License

This project is licensed under the MIT License.
