# Thunderbolt

# 1. Introduction
The Thunderbolt folder contains information on how to configure your Thunderbolt 3/4 cards for support in macOS.  While Alpine Ridge and Maple Ridge cards are supported, the Titan Ridge card (specifically the Gigabyte GC-Titan Ridge V1.0/2.0) is highly preferred as it has the most support.  Typically, one thunderbolt card is officially supported on most motherboards but it is possible to use more than one Thunderbolt PCIe card in a system.  However, additional cards may not be as stable as the one configured in the BIOS.

# 2. BIOS Settings
BIOS settings listed below are configured for the first add-in or built-in Thunderbolt card.  If your motherboard does not have a Thunderbolt header you can skip this section.

    Advanced
      Thunderbolt(TM) Configuration
        * TBT Root port Selector - PCIe slot Thunderbolt card is in
        * Thunderbolt Usb Support - Disabled
        * Thunderbolt Boot Support - Disabled
        * Wake From Thunderbolt(TM) Devices - Off
        * Thunderbolt(TM) PCIe Cache-line Size - 128
        * GPIO3 Force Pwr - On
        * Wait time in ms after applying Force Pwr - 200
        * Skip PCI OptionRom - Enabled
        * Security Level - SL0-No Security
        * Reserve mem per phy slot - 32
        * Reserve P mem per phy slot - 32
        * Reserve IO per phy slot - 20
        * Delay Before SX Exit - 300
        * GPIO filter - Enabled
        * Enable CLK REQ - Disabled
        * Enable ASPM - Disabled
        * Enable LTR - Disabled
        * Extra Bus Reserved - 106
        * Reserved Memory - 737
        * Memory Alignment - 26
        * Reserved PMemory - 1184
        * PMemory Alignment - 28
        * Reserve I/O - 0
        * Alpine Ridge XHCI WA - Disabled

# 3. OpenCore config.plist Settings
  * Uncheck `DisableIoMapper` under `Kernel-Quirks` for AppleVTD support.
    * AppleVTD allows support for devices like Antelope Audio Thunderbolt interfaces or the Apple Thunderbolt-to-Gigabit Ethernet adapter.
  * If present, remove `dart=0`, from boot-args located at `NVRAM-Add-7C436110-AB2A-4BBB-A880-FE41995C9F82-boot-args`

If configured correctly your IOReg should show that you have AppleVTD and DMAC (Direct Memory Access Controller).
![](/Thunderbolt/Images/applevtd.png)
![](/Thunderbolt/Images/dmac.png)

# 4. Thunderbolt 3
## GC-Titan Ridge Internal Connections
    * 2x 6-pin PCIe power (Optional)
      *  Plug in if your devices require more power
    * USB 2.0 (Optional)
      * Plug in if you want to plug USB 2 devices directly into your card
    * 5-pin Thunderbolt header
      * Plug into thunderbolt header or jump pins 3 and 5.  Pins 3 and 5 are the ones furthest away from the PCIe slot like shown below.
  ![](/Thunderbolt/Images/jump3-5.jpeg)

## Standard or ICM (Internal Connection Manager) Mode Configuration
1. Once BIOS settings and OpenCore settings are configured, you should be able to load macOS with initial Thunderbolt support.  The location of your Thunderbolt card will vary depending on which slot it is occupied in.  For the WS X299 Sage/10G or any other motherboard set as Built-In it should be at PC00.RP05.  You can verify by searching for 'Thunderbolt' in IOReg.
If you look in your IOReg the Thunderbolt device tree should look like this.  
![](/Thunderbolt/Images/Thunderbolt-PreSSDT.png)

2. For hot-plug support, you will need a Thunderbolt hot-plug SSDT and a DTPG SSDT.  
* Download kgp's SSDT that I modified for OpenCore located [here](https://github.com/shinoki7/ASUS-X299-Hackintosh/blob/main/SSDTs/SSDT-TB3HP.aml)
* If you don't have a DTPG, SSDT-SBUS-MCHC, or X299-BASE SSDT, you can download it [here](https://github.com/shinoki7/ASUS-X299-Hackintosh/blob/main/SSDTs/SSDT-SBUS-MCHC.aml).
* Add these SSDTs to your EFI folder under `OC-ACPI` and the appropriate entries in your config.plist under `ACPI-Add`.  It is preferred to add `SSDT-TB3HP.aml` at the very bottom since it relies on method DTGP.  
3.  Reboot your computer and you should have hot plug support!
  * System Report showing two new devices
  ![](/Thunderbolt/Images/pci-SSDT.png)
  * IOReg showing hot plug SSDT adjustments
  ![](/Thunderbolt/Images/Thunderbolt-AfterSSDT.png)

## Thunderbolt Bus and Local Node Activation (Extended Mode)
If you have Thunderbolt configured, your Thunderbolt card is running in Standard or ICM (Internal Connection Manager) mode.  Apple developed another mode called Extended which implements more capabilities compared to Windows and Linux.

Switching from Standard to Extended is optional, but it brings your system more in line with a real Mac and provides these additional capabilities:
* Thunderbolt Bus and Local Node
* Thunderbolt Ethernet bridge
* Target Disk mode
* eGPU Support
* Thunderbolt NAS Support
* More support for legacy Thunderbolt 1 devices
* Refer to [this post](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2095044) for more devices

**Disclaimer:** While this brings Thunderbolt capabilities more like a real Mac, it is not perfect.  This affects hot plug support in Windows which may require a warm reboot from Mac then to Windows.  There may also be some inconsistencies with how Thunderbolt devices behave, USB 3 devices not being able to connect or other side effects.  Generally if your Thunderbolt devices work in ICM mode, **do not** proceed with these steps as it involves flashing the Thunderbolt firmware chip located on the Thunderbolt card/motherboard.  **I assume no responsibility or liability for any damage that may occur so proceed at your own risk!**

### 1. Guides by CaseySJ for flashing SPI ROM chip
* Mini-Guides: Generally prefer to use a CH341A programmer as it is much cheaper than a Raspberyy Pi and you can flash the chip using macOS.
  * [Flashing SPI ROM Chips using Raspberry Pi 3B or 4](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2080585)
  * [Flashing SPI ROM Chips using 3.3V CH341A Programmer](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2161463)
* Patched Thunderbolt Firmware files
  * [Repository of Patched Thunderbolt Firmware Files](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2087524)
    * Currently using [TitanRidgeNVM23-Elias64Fr-Mod.bin](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2085225)
* Additional Information
  * [Gigabyte Designare Z390 (Thunderbolt 3) + i7-9700K + AMD RX 580](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/)

### 2. Configuration
Once you have successfully flashed a custom firmware file following the guide, you should see the Thunderbolt Bus pane populated under `System Report-Thunderbolt/USB4`.
In order to have hot plug support, follow this guide by CaseySJ and Inqnuam [Using HackinDROM to Create Thunderbolt SSDT with Custom DROM](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2186155).
  * Note that SSDT-DTPG.aml is not needed if you already have SSDT-SBUS-MCHC.aml or X299-BASE.aml.

Once you reboot, you should have the Thunderbolt Bus pane properly populated and hot plug support!
  * System Report showing two new devices
  ![](/Personal%20Build/Images/pci.png)
  * System Report showing populated Thunderbolt Bus
  ![](/Thunderbolt/Images/tbbus.png)
  * IOReg showing hot plug SSDT adjustments
  ![](/Thunderbolt/Images/Thunderbolt-tbbus-AfterSSDT.png)

### What Works
* Thunderbolt 3 hot-plug
* USB 2.0 hot-plug/cold-plug
* USB 3.2 Gen 2 cold-plug/warm
* USB 3.2 Gen 2 hot-plug
* Sleep

### What Doesn't Work
* USB 3.2 Gen 2 on macOS 11.3 or higher
  * Have had mixed results trying various NVM firmware versions.  The XHC Controller does not even load on the latest BIOS with Resizable Bar support.
  * Workaround: Running BIOS 0901 with jumped pins.

# 5. Thunderbolt 4
For Thunderbolt 4, there are 3 PCIe Thunderbolt cards available using the Maple Ridge controller.  The information below is based on my testing with the ASUS ThunderboltEX 4.

## Current Observations
Note these observations are based on the devices tested listed below.  Your mileage may vary and issues I ran into may just be device specific.
 * Thunderbolt header may not necessarily be needed or jumped.  macOS recognizes the card on boot without a header plugged in.
 * The 6 pin power and USB 2.0 internal cable have to be connected.  
 * Thunderbolt settings have to be enabled in BIOS.  The Thunderbolt devices load in a different slot with Thunderbolt BIOS settings disabled but the device showed 'No driver installed'.
 * SSDT is required for Thunderbolt devices to be recognized.

## What Works
 * Thunderbolt 3 hot-plug
 * USB 2.0 hot-plug/cold-plug
 * USB 3.2 Gen 2 hot-plug/cold-plug
 * Sleep

## What Doesn't Work
 * Thunderbolt 3 cold/warm boot

## Devices Tested
 * Sabrent Thunderbolt 3 M.2 NVMe SSD Enclosure (EC-T3NS)

## To-Do
 * Extract original firmware and try installing custom firmware for Thunderbolt Local Node support.
 * Try more Thunderbolt 3/4 devices.

# Credits
* kgp
* DSM2
* CaseySJ
* Elias64Fr
* Inqnuam
