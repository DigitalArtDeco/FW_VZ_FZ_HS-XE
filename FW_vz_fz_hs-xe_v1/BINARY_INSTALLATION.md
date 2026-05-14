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
FW_vz_fz_hs-xe_v1/
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
python -m esptool --chip esp32 --port <PORT> --baud 460800 write_flash \
  --flash_mode dio --flash_freq 40m --flash_size 2MB \
  0x1000 bootloader.bin \
  0x8000 partition-table.bin \
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

## 10. Recommended Release Package Contents

When sharing binaries, include:

- `bootloader.bin`
- `partition-table.bin`
- `ESP32_VZ1_Display_driver.bin`
- this file: `BINARY_INSTALLATION.md`
- optional: checksums file (`SHA256SUMS.txt`)

## 11. Legal Note

By using the firmware binaries, the user accepts the license terms in `EULA.md`.
