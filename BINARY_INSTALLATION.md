# Binary Installation Guide (ESP32)

This guide explains how to flash prebuilt firmware binaries to an ESP32 without source code.

## 1. Scope

This procedure is for flashing the firmware binaries of this project:

- `bootloader.bin` (offset `0x1000`)
- `partition-table.bin` (offset `0x8000`)
- `ESP32_VZ1_Display_driver.bin` (offset `0x10000`)

Flash settings used by this firmware build:

- `--flash_mode dio`
- `--flash_freq 40m`
- `--flash_size 2MB`

## 2. Prerequisites

You need:

- an ESP32 board
- a stable USB cable with data lines (not charge-only)
- Python 3.8+ installed
- serial USB driver for your USB-UART chip (CP210x / CH340 / FTDI), if needed
- firmware binary files from a release package

Install `esptool`:

```bash
python -m pip install --upgrade esptool
```

If `python` is not available, try:

```bash
python3 -m pip install --upgrade esptool
```

## 3. Prepare Files

Create a local folder, for example:

```text
FW_vz_fz_hs-xe_vX/
  bootloader.bin
  partition-table.bin
  ESP32_VZ1_Display_driver.bin
```

Open a terminal in that folder before flashing.

## 4. Find Serial Port

### Windows

- Open Device Manager
- Look under "Ports (COM & LPT)"
- Example port: `COM5`

### macOS

```bash
ls /dev/cu.*
```

Typical port: `/dev/cu.SLAB_USBtoUART` or `/dev/cu.usbserial-*`

### Linux

```bash
ls /dev/ttyUSB* /dev/ttyACM* 2>/dev/null
```

Typical port: `/dev/ttyUSB0` or `/dev/ttyACM0`

## 5. Put ESP32 into Download Mode

Many boards auto-enter download mode. If flashing cannot connect:

1. Hold `BOOT`
2. Press and release `EN` (or `RST`)
3. Release `BOOT` after 1-2 seconds

## 6. Full Flash (Recommended)

Use this for first install, board change, or when unsure about previous flash content.

Optional full erase:

```bash
python -m esptool --chip esp32 --port <PORT> --baud 460800 erase_flash
```

Full write:

```bash
python -m esptool --chip esp32 --port COM3 --baud 460800 write-flash ^
  --flash-mode dio ^
  --flash-freq 40m ^
  --flash-size 2MB ^
  0x1000 bootloader.bin ^
  0x8000 partition-table.bin ^
  0x10000 ESP32_VZ1_Display_driver.bin
```

Example (Windows):

```powershell
python -m esptool --chip esp32 --port COM5 --baud 460800 write_flash --flash_mode dio --flash_freq 40m --flash_size 2MB 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 ESP32_VZ1_Display_driver.bin
```

Example (macOS/Linux):

```bash
python3 -m esptool --chip esp32 --port /dev/cu.SLAB_USBtoUART --baud 460800 write_flash \
  --flash_mode dio --flash_freq 40m --flash_size 2MB \
  0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 ESP32_VZ1_Display_driver.bin
```

## 7. App-Only Update (Advanced)

Only use this if bootloader and partition table are already correct on the target board.

```bash
python -m esptool --chip esp32 --port <PORT> --baud 460800 write_flash \
  --flash_mode dio --flash_freq 40m --flash_size 2MB \
  0x10000 ESP32_VZ1_Display_driver.bin
```

If boot issues occur after app-only update, return to **Full Flash**.

## 8. Verify Flash and Boot

After flashing:

1. Reset board (`EN` / `RST`)
2. Observe board behavior (display init / startup behavior)
3. Optional serial output monitor:

```bash
python -m serial.tools.miniterm <PORT> 115200
```

Exit miniterm with `Ctrl+]`.

## 9. Troubleshooting

### "Failed to connect" / timeout

- Use a data USB cable
- Re-enter download mode manually (BOOT + EN sequence)
- Lower baud rate to `115200`

### "Invalid head of packet" / write errors

- Lower baud rate: `--baud 115200`
- Use a shorter or better USB cable
- Avoid USB hubs during flashing

### Board does not boot after flash

- Ensure exact offsets: `0x1000`, `0x8000`, `0x10000`
- Ensure flash settings: `dio`, `40m`, `2MB`
- Perform `erase_flash` then full flash again

### No serial port visible

- Install correct USB-UART driver
- Try a different USB port/cable
- Verify board power LED and device manager/system log

 ## CP2102 / CP210x Driver Issue on Windows

Some ESP32 boards use a **Silicon Labs CP2102 / CP210x USB to UART bridge** for USB serial communication.

If Windows detects the board, but no COM port appears, check **Device Manager**.

A typical problem looks like this:

```text
CP2102 USB to UART Bridge Controller
```

with a yellow warning symbol.

In this case, flashing will not work yet. The ESP32 board is connected, but Windows has not created a valid virtual COM port. `esptool` can only flash the board after the CP2102 / CP210x device appears as a real COM port, for example:

```text
Silicon Labs CP210x USB to UART Bridge (COM3)
```

### How to fix it

1. Open **Device Manager**
2. Find the CP2102 / CP210x device with the yellow warning symbol
3. Right click the device
4. Select **Properties**
5. Check the **Device status** message

If the driver is missing or broken, do the following:

1. Right click the CP2102 / CP210x device

2. Select **Uninstall device**

3. If Windows asks to delete the driver software, enable that option

4. Unplug the ESP32 board from USB

5. Install the official Silicon Labs CP210x VCP driver:

   [Silicon Labs CP210x USB to UART Bridge VCP Drivers](https://www.silabs.com/software-and-tools/usb-to-uart-bridge-vcp-drivers)

6. Restart Windows

7. Plug the ESP32 board back in

After this, the board should appear in Device Manager under:

```text
Ports (COM & LPT)
```

Example:

```text
Silicon Labs CP210x USB to UART Bridge (COM3)
```

Use that COM port number in the flashing command.

### Check available COM ports

In Windows Command Prompt:

```cmd
mode
```

Or in PowerShell:

```powershell
[System.IO.Ports.SerialPort]::getportnames()
```

If no COM port appears, the USB serial driver is still not working correctly.

### Flash example after driver installation

If the board appears as `COM3`, use:

```cmd
python -m esptool --chip esp32 --port COM3 --baud 460800 write-flash --flash-mode dio --flash-freq 40m --flash-size 2MB 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 ESP32_VZ1_Display_driver.bin
```

If flashing fails at high speed, try a lower baud rate:

```cmd
python -m esptool --chip esp32 --port COM3 --baud 115200 write-flash --flash-mode dio --flash-freq 40m --flash-size 2MB 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 ESP32_VZ1_Display_driver.bin
```

### If esptool cannot connect

Some ESP32 boards need to be put into download mode manually:

1. Hold **BOOT**
2. Press and release **EN** or **RST**
3. Keep holding **BOOT** for one or two seconds
4. Release **BOOT** when `esptool` starts connecting

### Important note for Windows CMD

Do not use Linux style line continuation with `\` in Windows Command Prompt.

This will not work in Windows CMD:

```cmd
python -m esptool ... write-flash \
  --flash-mode dio \
  0x1000 bootloader.bin
```

Windows CMD will pass `\` as an argument to `esptool`, which can cause errors such as:

```text
Address "\" must be a number.
```

For Windows CMD, either use one single line, or use `^` as the line continuation character.

Correct Windows CMD multiline example:

```cmd
python -m esptool --chip esp32 --port COM3 --baud 460800 write-flash ^
  --flash-mode dio ^
  --flash-freq 40m ^
  --flash-size 2MB ^
  0x1000 bootloader.bin ^
  0x8000 partition-table.bin ^
  0x10000 ESP32_VZ1_Display_driver.bin
```

Make sure there is no space after the `^` character.


## 10. Legal Note

By using the firmware binaries, the user accepts the license terms in `EULA.md`.
