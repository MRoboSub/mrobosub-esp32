# mrobosub-esp32
Motor driver code using ESP32

## How to install the Firmware onto the ESP32-S3
The general workflow goes as such.

1. Install `esptool` onto your laptop (preferably in a `virtual environment`).
2. Download the firmware onto your computer (`.bin` format)
3. Flash the firmware to the computer
4. (Optional) Verify that the firmware was successfully flashed.

### 1. Install `esptool`

`esptool` is a Python package. As such, first ensure that you have a recent version of python installed on your device and is on your path.

> NOTE: For Windows users, I have found it easier to use a Python environment on **WINDOWS** and not on **WSL** because you have to do some setup in order to make the Serial ports accessible via WSL.

#### Create a virtual environment on Mac or Linux
```bash
python3 -m venv .venv
```

You may need to install a package (via `brew` or `apt`) for virtual environments. On Ubuntu it's
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-venv -y
```

To load in (or to use the proper lingo, _source_) the virtual environment, run in the top level directory:
```bash
source .venv/bin/activate
```

#### Create a virtual environment on Windows
```powershell
python -m venv .venv
```

To source the virtual environment, run in the top level directory:
```powershell
.\.venv\Scripts\activate.bat
```

NOTE: I needed to expose the `<Python Path>\Scripts` directory to my path in order to actually be able to run `esptool.exe` from the command line.


#### Install the `esptool` PIP package
With the virtual environment sourced, run these lines on Mac/Linux:
```bash
python3 -m pip install esptool
```

On Windows:
```
python -m pip install esptool
```

> NOTE: On Windows, the `esptool` application will be accessible via `esptool.exe`.

#### Erase any existing firmware
Connect the ESP32 via USB.

Next, on a Linux environment (I can't speak to Mac for this one), check to see if you can see the Serial device. To do this, look at the `/dev/` folder.

Try plugging in and out the USB device and see if a new `/dev/ttyACM*` device or similar is loaded as a result of connecting the device to your machine.

On a Windows machine, you can use the Device Manager. Look under "Ports (COM & LPT)", you should see a new device once it is plugged in. It should be specified as a `COM` port.

Next, in a terminal, run the following line (use the `.exe` for a Windows machine).
```bash
esptool[.exe] erase_flash
```

You may need to specify the Serial port using `--port <PORT>` on the above line.

> NOTE: If you encounter an issue connecting to the Serial port, try these tips. First,ensure no Arduino IDE instances are open. Next, load the board in **BOOT** mode by holding down the "B" button before plugging in the device to your computer, and only after that releasing the "B" button.

### 2. Download the Firmware
Download the `.bin` file from [this site](https://micropython.org/download/ESP32_GENERIC_S3/). 

### 3. Flash the Firmware to the ESP

Next, we can use the `esptool` command from the terminal to flash the ESP as follows:
```bash
esptool[.exe] write_flash 0 <PATH TO ESP32_FIRMWARE_FILE>
```

Again you may need to specify a Serial (`/dev/ttyACM*` on Linux/Mac or `COM*` on Windows) port.

### (Optional) 4. Verify that the Firmware flashed correctly
To do this we will connect to the ESP device using Serial.

For a cross-platform experience, I recommend using [Thonny](https://thonny.org/). However, I used [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) since I am on a Windows device.

#### How to connect using PuTTY
1. Specify `Serial` under `Connection type`.
2. Specify the Serial port as whichever COM port you find in Device Manager.
3. Set the Speed (which is the BAUD rate) as `115200`.
4. Click open.

You should now be able to see a `MicroPython` terminal open.

Try running
```python
import machine
machine.freq()
```

If you see something, then this is a success!

## Authors
- [Zayn Baig](https://github.com/imzaynb)
- [Colten Germundson](https://github.com/ColtGerm)
