# ROG RAMPAGE VI EXTREME ENCORE

![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/ROGRampageVIExtremeEncore.png)

## Specifications
### Components

| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS ROG Rampage VI Extreme Encore | BIOS 1301 |
| Processor | Intel i9-7900X | |
| CPU Cooler | Fractal Design Celsius+ S36 Dynamic | |
| RAM | 4x16 Corsair Dominator Platinum RGB 3200 Mhz | |
| Boot Drive | Sabrent Rocket 1 TB | |
| Graphics Card | AMD Radeon Pro W5500 | |
| Wifi/Bluetooth Card | Broadcom BCM943602CDP |  |
| Power Supply | Corsair RM 850x | |
| Case | Lian Li PC 011 Dynamic | |

### PCIe Slot Layout
| Slot | PCIe Gen | Speed | Device | Notes |
| ----- | ----- | ----- |---------------------------------------|-------------------|
| 1 | 3 | x16 | AMD Radeon Pro W5500 | |
| 2 | 3 | x4 | | PCH (Enabling disables M.2_1) |
| 3 | 3 | x16 | | (Running at x16 disables DIMM.2_2) |
| 4 | 3 | x4 | Broadcom BCM943602CDP | |

### M.2/DIMM.2 Layout
| Slot | PCIe Gen | Device | Notes |
| ----- | ----- |---------------------------------------|-------------------|
| M.2_1 | 3 | | Disabled |
| M.2_2 | 3 | | |
| DIMM.2_1 | 3 | Sabrent Rocket 1 TB | |
| DIMM.2_2 | 3 | | Disabled |

## What Works / What Doesn't Work
- [x] Sleep / Wake
- [x] Wifi and Bluetooth
  * Disabled onboard Wifi/Bluetooth since using Broadcom BCM943602CDP card.
- [x] Handoff, Continuity, AirDrop, Continuity Camera, Universal Control, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 1G Ethernet
- [x] 10G Ethernet
- [x] HEVC, H.264
- [x] Onboard audio
- [x] TRIM
- [x] USB 2.0
- [x] USB 3.2 Gen 1
- [x] USB 3.2 Gen 2
- [x] USB 3.2 Gen 2x2
- [x] DRM
- [x] Native NVRAM
- [x] CPU Power Management
- [x] USB Power
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS

## Screenshots

### System Report
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/aboutthismac.png)
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/overview.png)

### Memory
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/memory1.png)
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/memory2.png)

### Audio
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/audio.png)

### Ethernet
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/ethernet.png)

### GPU
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/graphics.png)

### NVME
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/NVMExpress.png)

### SATA
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/sata.png)

### USB
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/usb.png)

### PCI
![](/ROG%20RAMPAGE%20VI%20EXTREME%20ENCORE/Images/pci.png)

# Changelog:
## Initial EFI upload, OpenCore 0.7.7 (2022.01.14)
