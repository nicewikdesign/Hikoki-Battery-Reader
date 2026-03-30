# Hikoki-Battery-Reader
[![Buy Me A Coffee](https://img.shields.shields.shields.shields.io/badge/Buy%20Me%20A%20Coffee-Donate-orange?style=for-the-badge&logo=buy-me-a-coffee)](https://buymeacoffee.com/nicewikdesign)

Hikoki MultiVolt Battery Diagnostic Tool (ESP32-S3)
This project provides an advanced diagnostic interface for Hikoki / Metabo HPT MultiVolt (36V/18V) batteries using the ESP32-S3. It allows you to monitor cell health, cycle counts, and internal BMS error logs.

    Note: This version is specifically optimized for the ESP32-S3 hardware timers and high clock speeds.
    
Features
Real-time Cell Voltage: Monitor all 10 cells (2 banks of 5 cells).
Health Metrics: Check charge cycle counts and total runtime (18V vs 36V modes).
BMS Error Logs: Access internal error history (Unbalance, Overcharge, Over-temp).
Hardware Info: Production date, serial number, and exact battery model.
Lockout Reset: Includes a function to clear software-based lockout flags (use with extreme caution).

Hardware Setup
To communicate with the battery, you need to interface with its proprietary inverted UART protocol (9600 baud).
Wiring Diagram
### Wiring Diagram
| Battery Pin | ESP32-S3 Pin | Note |
| :--- | :--- | :--- |
| **GND (-)** | **GND** | Common Ground |
| **Signal (T)** | **GPIO 16** | Main Communication Line |
| **Signal (V;LS)** | **GPIO 17 & 18** | TX / Inverted TX lines |

Critical Component: The Pull-Up Resistor
Because the battery uses an open-collector style communication, you MUST use a pull-up resistor for the signal to be readable by the ESP32:
  4.7kΩ Resistor connected between GPIO 16 and 3.3V.

##  How to use the Diagnostic Menu

After uploading the code, open the **Serial Monitor** in Arduino IDE and set the baud rate to **115200**. 

The program waits for your input. Type one of the following keys into the command line and press Enter to switch between screens:

| Key | Screen Name | What you will see |
| :--- | :--- | :--- |
| **`0`** | **General Info** | Battery Model, Serial Number, Mfg Date, Charge Cycles, and Capacity % |
| **`1`** | **Voltages & Temp** | Individual voltages for all 10 cells, Bank totals, and Temperature sensors |
| **`2`** | **Error Logs** | History of BMS errors (e.g., "Bank Unbalance", "Over Charge") |
| **`3`** | **Tool Usage** | List of Hikoki tools used with this battery and how many times |
| **`4`** | **Safety Flags** | Critical flags: Fuse Status, Error Flag (Yes/No), and Protection states |
| **`5`** | **Stress Stats** | Statistics on Over-discharge and Over-current events (18V/36V) |
| **`6`** | **Reset Lockout** | **DANGEROUS:** Tries to clear the software lock. Use only after balancing cells! |
| **`P`** | **Deep Sleep** | Puts the ESP32-S3 into low power mode to save energy |

> **💡 Pro Tip:** If the battery does not respond, press the **LED battery indicator button** on the battery housing to wake up the internal BMS before sending a command.


Safety Warning
Lithium-ion batteries can be dangerous. This tool allows access to internal BMS functions. Resetting a lockout or attempting to charge an unbalanced battery can lead to fire, explosion, or permanent damage. Always verify cell voltages manually if the BMS reports an unbalance. Use this software at your own risk.
