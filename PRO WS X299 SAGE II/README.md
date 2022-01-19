# PRO WS X299 SAGE II

![](/PRO%20WS%20X299%20SAGE%20II/Images/ProWSX299SageII.png)

## Specifications
### Components

| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS Pro WS X299 Sage II | BIOS 0901 due to Thunderbolt issues on newer BIOS revisions |
| Processor | Intel i9-10980XE | |
| CPU Cooler | Fractal Design Celsius+ S36 Dynamic | |
| RAM | 4x16 Corsair Vengeance LPX 3200 Mhz | |
| Boot Drive | Sabrent Rocket 1 TB | |
| Graphics Card | AMD Radeon Pro W5500 | |
| Wifi/Bluetooth Card | Broadcom BCM943602CDP |  |
| Power Supply | Corsair RM 850x | |
| Case | Lian Li PC 011 Dynamic | |

### PCIe Slot Layout
| Slot | PCIe Gen | Speed | Device | Notes |
| ----- | ----- |----- | ---------------------------------------|-------------------|
| 1 | 3 | x16 | | |
| 2 | 3 | x4 | Gigabyte GC-Titan Ridge V2.0 | Thunderbolt settings enabled in BIOS |
| 3 | 3 | x8 | | |
| 4 | 3 | x8 | | |
| 5 | 3 | x8 | AMD Radeon Pro W5500 | |
| 6 | 3 | x8 | Intel X550-T2 10G Ethernet Card | |
| 7 | 3 | x8 | | |

### M.2/U.2 Layout
| Slot | PCIe Gen | Device | Notes |
| ----- | ----- | ---------------------------------------|-------------------|
| U.2_1 | 3 | | |
| U.2_2 | 3 | | |
| U.2_3 | 3 | | |
| M.2_1 | 3 | Broadcom BCM943602CDP | Using [M.2 NGFF Adapter](https://www.amazon.com/gp/product/B07R3XVD54/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1) |
| M.2_2 | 3 | Sabrent Rocket 1 TB | |

## What Works / What Doesn't Work
- [x] Sleep / Wake
- [x] Wifi and Bluetooth
- [x] Handoff, Continuity, AirDrop, Continuity Camera, Universal Control, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 2.5G Ethernet
- [x] 10G Ethernet
    * Modified EEPROM to work with SmallTree drivers. Refer to section [Intel 10 Gigabit NICs with Small Tree macOS Drivers](https://github.com/shinoki7/ASUS-X299-Hackintosh/tree/main/Intel%2010G%20SmallTree) for more info.
- [x] HEVC, H.264
- [x] Onboard audio
- [x] TRIM
- [x] USB 2.0
- [x] USB 3.2 Gen 1
- [x] USB 3.2 Gen 2
- [x] DRM
- [x] Native NVRAM
- [x] CPU Power Management
- [x] USB Power
- [x] Thunderbolt 3/4 hot-plug
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS

## Screenshots

### System Report
![](/PRO%20WS%20X299%20SAGE%20II/Images/aboutthismac.png)
![](/PRO%20WS%20X299%20SAGE%20II/Images/overview.png)

### Memory
![](/PRO%20WS%20X299%20SAGE%20II/Images/memory1.png)
![](/PRO%20WS%20X299%20SAGE%20II/Images/memory2.png)

### Audio
![](/PRO%20WS%20X299%20SAGE%20II/Images/audio.png)

### Ethernet
![](/PRO%20WS%20X299%20SAGE%20II/Images/ethernet.png)

### GPU
![](/PRO%20WS%20X299%20SAGE%20II/Images/graphics.png)

### NVME
![](/PRO%20WS%20X299%20SAGE%20II/Images/NVMExpress.png)

### USB
![](/PRO%20WS%20X299%20SAGE%20II/Images/usb.png)

### PCI
![](/PRO%20WS%20X299%20SAGE%20II/Images/pci.png)

### Thunderbolt
![](/PRO%20WS%20X299%20SAGE%20II/Images/tbbus.png)
