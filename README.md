# ASUS X299 Hackintosh

# Introduction
The ASUS X299 Hackintosh repository contains OpenCore EFI distributions and related files that can be used as a reference when setting up or updating your X299 Hackintosh with the OpenCore bootloader.  While the EFIs can be used as a starting point for all ASUS X299 boards, it is still highly recommended to review the [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Install-Guide/) and [Skylake-X section](https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/skylake-x.html) for a full guide.  These files were created using the ASUS ROG Rampave VI Extreme Encore as a baseline.

# Repository Folders
| Folder | Description |
| :------------- | :---------- |
| BASE-EFI | OpenCore EFI distributions that should be valid for all ASUS X299 boards |
| Custom BIOS Collection | Modified BIOS .CAP files that have custom boot logos and/or other modifications |
| Intel 10G SmallTree | Information about configuring Intel 10G ports for macOS support |
| Kexts | Additional kexts used in BASE-EFI |
| PRO WS X299 SAGE II | Information about personal build using the ASUS Pro WS X299 Sage II running macOS Monterey |
| ROG RAMPAGE VI EXTREME ENCORE | Information about personal build using the ASUS ROG Rampage VI Extreme Encore running macOS Monterey |
| SSDTs | Precompiled SSDTs used in BASE-EFI |
| Thunderbolt | Information about configuring Thunderbolt 3/4 for macOS |
| USB Kexts | Mapped motherboard USB kexts |

# Optional USB Peripherals for Internal USB 2.0 Devices
* 1. [USB 3.0 20 Pin Female to USB 2.0 Pin Male Adapter](https://www.amazon.com/gp/product/B01MFB04JP/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * This adapter converts the internal USB 3.0 19 pin header to a USB 2.0 9 pin.
* 2. [USB 2.0 9 Pin Header 1 to 4 Extension Hub Splitter](https://www.amazon.com/gp/product/B085KVH16T/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
* 3. [USB 2.0 IDC 5 Male to USB A Male adapter](https://www.amazon.com/gp/product/B000V6WD8A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Uses one of the USB ports on the back of the motherboard to connect internal devices in the case.  You can use two of these together and remove 1 of the pins to make a "9 pin internal adapter".  If the cable is too short to reach inside the case, you can extend with a USB A extension cable.
* 4. PCIe USB 3.0 Card with Internal USB Connector
    * [Inateck USB 3.0 Card](https://www.amazon.com/Inateck-Express-Controller-Internal-Connector/dp/B00JFR2H64/ref=sr_1_3?dchild=1&keywords=inateck+pcie+card&qid=1592455853&s=electronics&sr=1-3)
    * [StarTech USB 3.1 PCIe Card](https://www.amazon.com/gp/product/B01I39D15A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Can use the internal header on the card for the case USB ports or to connect internal devices.
    * **NOTE**: Wake from Bluetooth devices does not work with this so it's best to connect Bluetooth to one of the motherboard ports.

# Credits
* [Apple](https://www.apple.com) : macOS
* [Acidanthera](https://github.com/acidanthera) : OpencorePkg, kexts, etc.
* [Dortania](https://github.com/dortania) : Opencore guide
