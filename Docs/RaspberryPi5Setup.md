# Raspberry Pi 5 Setup

## Image Creation with Raspberry Pi Imager

### Software Used
- **Raspberry Pi Imager** (latest version)

### Configuration

#### Operating System
- **OS**: Raspberry Pi OS (64-bit)
- **Version**: Debian Trixie
- **Release**: Raspberry Pi OS (64-bit)

#### Image Creation Process

1. **Launch Raspberry Pi Imager**
   - Download and install Raspberry Pi Imager
   - Open the application

2. **Select Operating System**
   - Select "Raspberry Pi Device": Raspberry Pi 5
   - Click "Next"
   - Choose "Raspberry Pi OS (64-bit)" based on Debian Trixie

3. **Select Storage Medium**
   - Click "Choose Storage"
   - Select target SD card, USB storage device or NVMe

4. **Configure Advanced Options** (optional)
   - Set hostname
   - Enable SSH
   - Configure username and password
   - Pre-configure WiFi settings
   - Set locale settings (timezone, keyboard layout)

5. **Write Image**
   - Click "Write"
   - Wait for confirmation
   - After successful write: safely remove SD card/USB storage

## System Configuration

After booting the Raspberry Pi 5 for the first time, perform the following configuration steps.

### 1. PCIe Gen 3 Configuration

Enable PCIe Gen 3 mode for NVMe SSDs:

```bash
sudo nano /boot/firmware/config.txt
```

Add the following line:
```
dtparam=pciex1_gen=3
```

Reboot to apply changes:
```bash
sudo reboot
```

### 2. EEPROM Configuration

Edit the EEPROM configuration:

```bash
sudo rpi-eeprom-config --edit
```

Add or modify the following parameters:

**Configuration Details:**
- `PCIE_PROBE=1`: Enable PCIe device detection
- `BOOT_UART=1`: Enable UART for boot diagnostics
- `POWER_OFF_ON_HALT=1`: Power off on halt for power savings
- `BOOT_ORDER=0xf416`: Set boot order (NVMe priority) with power savings

### 3. Phison Controller NVMe Setup (Required for WD Black SN850 and similar)

For NVMe SSDs with Phison controllers that don't boot by default, the following steps are **required**:

1. **Flash to SD card or USB first**
   - Follow the Image Creation steps above
   - Flash the image to an SD card or USB drive instead of NVMe

2. **Boot and configure system**
   - Insert SD card/USB into Raspberry Pi 5
   - Complete steps 1 and 2 (PCIe and EEPROM configuration)

3. **Update system**
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

4. **Copy to NVMe**
   - Open "SD Card Copier" application (found in Accessories menu)
   - Select source: SD card or USB drive
   - Select destination: NVMe SSD
   - Click "Start" to copy the system to NVMe
   - Wait for completion
   - Shutdown, remove SD card/USB, and boot from NVMe

> **Note**: See [Hardware.md](Hardware.md) for list of affected SSDs with Phison controllers.
