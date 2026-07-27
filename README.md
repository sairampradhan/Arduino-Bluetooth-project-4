# RemoteXY Bluetooth Potentiometer Visualizer

An Arduino automation script that reads analog data streams from a local potentiometer, maps the raw electrical voltage level to a clean percentage scale, and mirrors the results across two distinct dashboard tracking bars on a mobile screen.

## 🛠️ Hardware Requirements

* **Microcontroller:** Arduino Uno, Nano, or any ATmega328P variant.
* **Analog Input:** 1x 10kΩ Rotary Potentiometer (or any linear analog voltage sensor).
* **Wireless Transceiver:** 1x HC-05 / HC-06 Bluetooth Module.
* **Hookups:** Breadboard and standard jumper wires.

### 📌 Physical Wiring Diagram

#### 1. Bluetooth Transceiver Connections

| Module Pin | Target MCU Pin | Function / Mode |
| :--- | :--- | :--- |
| **TX** | `D2` | SoftwareSerial Receive (RX) |
| **RX** | `D3` | SoftwareSerial Transmit (TX) |
| **VCC** | `5V` | System 5V Power |
| **GND** | `GND` | Common Ground |

#### 2. Rotary Potentiometer Layout

| Potentiometer Terminal | Arduino Pin | Description |
| :--- | :--- | :--- |
| **Left Pin (Pin 1)** | `GND` | Ground Reference |
| **Wiper Pin (Pin 2)** | `A0` | Analog Read Voltage Input (`POT_PIN`) |
| **Right Pin (Pin 3)** | `5V` | 5V Power Supply |

---

## 📱 Mobile App UI Structure

The hardcoded dashboard layout chunk (`RemoteXY_CONF_PROGMEM`) configures two separate visual level displays driven by the same variable:

* **`RemoteXY.circularBar_01`**: An integer output (`0` to `100`) populating a radial/circular completion ring.
* **`RemoteXY.linearbar_01`**: A floating-point output (`0.0` to `100.0`) populating a linear progress bar.
* **`RemoteXY.connect_flag`**: System connection status monitor (`1` if connected, `0` if disconnected).

### ⚙️ Signal Processing Logic
The microprocessor samples raw data from the wiper terminal using its 10-bit analog-to-digital converter (ADC), converting it to digital steps. The code processes it using the following logic:
1. Natively samples the physical terminal via `analogRead(A0)`, returning a number from **`0` to `1023`**.
2. Scales that resolution down via `map(potValue, 0, 1023, 0, 100)` to output a standard percentage value (**`0%` to `100%`**).
3. Assigns this calculated scale to both UI tracking bars simultaneously for matched visualization.

---

## 📦 Software Setup & Deployment

### 1. Library Installation
The firmware links directly to the official RemoteXY framework core:
* Open the Arduino IDE.
* Navigate to **Sketch** -> **Include Library** -> **Manage Libraries...**
* Search for **RemoteXY** and click **Install** to add the latest driver stack.

### 2. Flashing the Controller
* Open this file sketch directly inside your local development interface.
* *Troubleshooting Tip:* If you run into upload errors or timed-out connections, temporarily unplug the hardware wires from pins `D2` and `D3`.
* Compile and load the program by pressing **Upload** (`Ctrl + U`).

---

## 🚀 Pairing & Live Operation

1. Download the **RemoteXY** configuration app from your mobile marketplace.
2. Turn on your mobile device's **Bluetooth** system antenna.
3. Power up your microcontroller framework over USB or an external cell pack.
4. Tap the **`+` (Add Device)** icon inside the app window, pick **Bluetooth**, and pair with your **HC-05/HC-06** module name.
5. Rotate the dial on your potentiometer to watch both the linear status line and the circular progress meter change instantly on your smartphone screen.

## 📄 License
This interactive telemetry monitor uses the standard communication stack and is shared globally under the open [MIT License](https://github.com).
