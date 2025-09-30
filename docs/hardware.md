# Hardware Setup Guide

## Required Components

### Core Hardware
- Raspberry Pi 4 (4GB RAM minimum)
- 32GB microSD card (Class 10)
- NFC reader module (PN532 or RC522)
- GPS module (NEO-8M)
- 4G/LTE modem (optional for remote locations)

### Enclosure & Power
- Weatherproof enclosure (IP65 rated)
- 12V power adapter or solar panel kit
- Ethernet cable (Cat6)
- Mounting hardware

## Assembly Instructions

### 1. Prepare Raspberry Pi
```bash
# Flash Raspberry Pi OS
sudo dd if=raspios-lite.img of=/dev/sdX bs=4M status=progress

# Enable SSH and I2C
sudo raspi-config
```

### 2. Connect NFC Module
- VCC → 3.3V (Pin 1)
- GND → Ground (Pin 6)
- SDA → GPIO 2 (Pin 3)
- SCL → GPIO 3 (Pin 5)

### 3. Connect GPS Module
- VCC → 5V (Pin 2)
- GND → Ground (Pin 14)
- TX → GPIO 14 (Pin 8)
- RX → GPIO 15 (Pin 10)

### 4. Install in Enclosure
1. Mount Pi on DIN rail
2. Secure antenna connections
3. Route power and ethernet cables
4. Seal all entry points

## Deployment Locations

### Site Requirements
- Stable internet connection (Ethernet preferred)
- Power source (110V/220V or solar)
- Public accessibility
- Weather protection
- Secure mounting point

### Installation Steps
1. Survey location and obtain permits
2. Install mounting bracket
3. Connect power and network
4. Configure node settings
5. Test all functions
6. Register with network

## Testing Checklist

- [ ] Power LED indicator
- [ ] Network connectivity
- [ ] NFC reader detection
- [ ] GPS signal acquisition
- [ ] Solana RPC connection
- [ ] Check-in transaction test