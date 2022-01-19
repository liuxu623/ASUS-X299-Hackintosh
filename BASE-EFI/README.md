# BASE-EFI

# Introduction
The Base EFI folder contains a pre-built EFI with the OpenCanopy GUI using the MacPro7,1 SMBIOS that should be valid for all ASUS X299 motherboards.  It is built using my personal build as a baseline.

**NOTE**: Some config settings are different than the Skylake-X section of the Dortania OpenCore Vanilla Guide to be compatible with ASUS motherboards.

# 1. Recommended BIOS Settings
* Originally based off kgp's original X299 Clover guide [section B1) ASUS BIOS Configuration](https://www.tonymacx86.com/threads/imac-pro-x299-live-the-future-now-with-macos-10-14-mojave-successful-build-extended-guide.255082/) but modified for the newest BIOS revisions.
* Reset to Default Settings before adjusting to these settings.  It is recommended to use one of the more recent BIOS revisions.

      AI Tweaker
        * AI Overclock Tuner - XMP
        * CPU SVID Support - Enabled

      Advanced
        CPU Configuration
          * MSR Lock Control - Disabled
          CPU Power Management Configuration
            * Enhanced Intel SpeedStep Technology - Enabled
            * Turbo Mode - Enabled
            * Autonomous Core C-State - Enabled
            * Enhanced Halt State (C1E) - Enabled
            * CPU C6 Report - Enabled
            * Package C State - C6(non Retention) state
            * Intel(R) Speed Shift Technology - Enabled
            * MFC Mode Override - OS Native Support

        System Agent (SA) Configuration
          * Intel VT for Directed I/O (VT-d) - Enabled

        PCI Subsystem Settings
          * Above 4G Decoding - Enabled
          * Re-Size BAR Support - Auto

        PCH Storage Configuration
          * SATA Mode Selection - AHCI

      Boot
        CSM (Compatability Support Module)
          * Launch CSM - Disabled

        Secure Boot
          * OS Type - Other OS

# 2. config.plist Configuration
The BASE-EFI is currently configured using the MacPro7,1 SMBIOS and OpenCore 0.7.7.  Please review the configuration below to make adjustments as necessary.

**NOTE**: MacPro7,1 SMBIOS only works on macOS Catalina and higher.  Adjust to iMacPro1,1 for macOS Mojave or lower.

## Kernel
### Add
1. Ethernet:
    * IntelMausi.kext - Enabled for onboard Ethernet
    * [SmallTreeIntel8259x.kext](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) - Disabled by default but required for Intel X550-AT2 10G Ethernet to work on the WS X299 Sage/10G.  If you need this, please enable this entry otherwise you can delete it.
      * **NOTE**: Ubuntu EEPROM modding outlined [here](https://github.com/shinoki7/ASUS-X299-Hackintosh/tree/main/Intel%2010G%20SmallTree#intel-10-gigabit-nics-with-small-tree-macos-drivers) is required for this kext to work
    * [SmallTreeIntel82576.kext](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases) - Disabled by default but required for Intel I211 NICs like on the X299 Deluxe.  If you need this, please enable this entry otherwise you can delete it.
3. RestrictEvents:
    * If you are using the iMacPro1,1 SMBIOS, you can delete this entry.

### Patch
1. Aquantia AQC107 patch
    * Disabled by default but required for Aquantia 10G support in macOS Big Sur and Monterey.  If you need this, please enable this entry otherwise you can delete it.

## Misc
SecureBootModel is currently configured for macOS Monterey, For proper configuration on prior macOS releases, refer to the [SecureBootModel section](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#securebootmodel).

## PlatformInfo
You will need to create your own Serial Number and SMUUID.  Instructions can be found [here](https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/skylake-x.html#platforminfo).
Remember to adjust the Type depending on which SMBIOS you are using.  Either iMacPro1,1 or MacPro7,1

  1. Using your results from GenSMBIOS, adjust the following (replace '[Removed]') under `Generic`.
      * MLB: Board Serial
      * ROM: ROM
      * SystemSerialNumber: Serial
      * SystemUUID: SmUUID
  2. If you are using the iMacPro1,1 SMBIOS, you can delete the Memory Dictionaries `#Memory - 2 DIMMS`, `#Memory - 4 DIMMS`, `#Memory - 8 DIMMS`.

## UEFI
As of OpenCore 0.7.2, APFS drivers only load for macOS Big Sur and above.  The BASE-EFI currently has `UEFI-APFS-MinDate` and `UEFI-APFS-MinVersion` set to `0` for APFS support on macOS Monterey.  Refer to [OcApfsLib](https://github.com/acidanthera/OpenCorePkg/blob/master/Include/Acidanthera/Library/OcApfsLib.h) to adjust these for prior macOS releases.

# 3. Post-Install
1. USB:
    * Please use [this](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html) as a proper guide to map your USB ports.
    * Once mapped, make sure to replace the `USBInjectAll.kext` entry under `Kernel-Add` with `USBMap.kext`.  Also disable `XhciPortLimit` under `Kernel-Quirks`.
    * **NOTE**: The `XhciPortLimit` quirk may not work on macOS 11.3+ so it's recommended to install an older version of macOS to map the ports then update.
2. Custom Memory (SMBIOS MacPro7,1 only):
    * Depending on how many memory DIMM slots on your motherboard are filled, rename the Memory Dictionary under `PlatformInfo` and remove the other one.  (I.E. I only have 4 DIMM slots filled, so I renamed `#Memory - 4 DIMMS` to `Memory` and deleted `#Memory - 8 DIMMS` and `#Memory - 2 DIMMS`).
    * Expand `Devices` under `PlatformInfo-Memory-Devices` and adjust the following properties that match your Memory.
      * Manufacturer
      * Part Number
      * Serial Number
      * Size
      * Speed

    For 2 DIMMS, this would be BANK 5/3.

    For 4 DIMMS, this would be BANK 7/9/6/4.

    For 8 DIMMS, this would be BANK 7/8/9/10/6/5/4/3.
    * Once mapped make sure to remove `RestrictEvents.kext` under `Kernel-Add` and also delete the kext in your `Kexts` folder under `OC-Kexts`.

# Changelog:
## OpenCore 0.7.7 (2022.01.13)
Bootloader / Kexts:
* CpuTscSync 1.0.5
* WhateverGreen 1.5.6
* RestrictEvents 1.0.6
* AppleALC 1.6.8
* Lilu 1.5.9
* VirtualSMC 1.2.8
* Removed AdvancedMap

config.plist Changes:
* `Booter-Quirks-ResizeAppleGpuBars` to 0 for Resize Bar support

## OpenCore 0.7.4 (2021.10.13)
Bootloader / Kexts:
* CpuTscSync 1.0.5
* WhateverGreen 1.5.4
* RestrictEvents 1.0.5
* AppleALC 1.6.5
* AdvancedMap 1.0.0 (Modern maps on non Apple Silicon: [github](https://github.com/notjosh/AdvancedMap))

## OpenCore 0.7.3 (2021.09.06)
Bootloader / Kexts:
* CpuTscSync 1.0.4
  * Switched back since it is now macOS Monterey compatible.  If you are still having issues, revert back to TSCAdjustReset.kext
* WhateverGreen 1.5.3
* RestrictEvents 1.0.4
* AppleALC 1.6.4
* VirtualSMC 1.2.7
* Lilu 1.5.6

## OpenCore 0.7.2 (2021.08.02)
Bootloader / Kexts:
* Lilu 1.5.5
* WhateverGreen 1.5.2
* AppleALC 1.6.3
* VirtualSMC 1.2.6
* SmallTreeIntel8259x.kext
  * Added but disabled by default
* SmallTreeIntel82576.kext
  * Added but disabled by default

config.plist Changes:
* SSDT - Combined all required SSDTs into SSDT-BASE.aml
* `ACPI-Quirks-ResetLogoStatus` to False
* `Kernel-Quirks-DisableLinkedJettison` to True
* `Misc-Boot-PollAppleHotKeys` to True
* `Misc-Security-AllowToogleSip` to True
* `Misc-Security-SecureBootModel` to Disabled
  * OpenCore 0.7.2 now defaults to `x86legacy` which only loads on macOS Big Sur and up.
  * Refer to the [SecureBootModel section](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#securebootmodel) for more detail on proper configuration.
* `UEFI-APFS-MinDate` and `UEFI-APFS-MinVersion` to `-1` for APFS support on lower macOS versions.
  * Refer to [OcApfsLib](https://github.com/acidanthera/OpenCorePkg/blob/master/Include/Acidanthera/Library/OcApfsLib.h) to adjust these to proper value.


## Updated required SSDTs (2021.07.07)
* Reverted required SSDTs to the ones by khronokernel since was getting boot errors with BIOS 3405.

## OpenCore 0.7.1 (2021.07.05)
Bootloader / Kexts:
* Lilu 1.5.4
* AppleALC 1.6.2
* VirtualSMC 1.2.5
* WhateverGreen 1.5.1
* RestrictEvents 1.0.3
* NVMeFix 1.0.9
* IntelMausi 1.0.7
* Replaced CpuTscSync.kext with TSCAdjustReset.kext for macOS Monterey support (CpuTscSync.kext is currently incompatible)

config.plist Changes:
* Uncheck DisableIoMapper for AppleVTD support
* Removed iMacPro1,1 config.plist

## OpenCore 0.7.0 (2021.06.07)
Bootloader / Kexts:
* NVMeFix 1.0.8
* AppleALC 1.6.1
* VirtualSMC 1.2.4
* RestrictEvents 1.0.2
* WhateverGreen 1.5.0
* Updated "Resources" files for OpenCanopy GUI

config.plist Changes:
* Adjusted PickerAttributes to `17` and PickerVariant to `Acidanthera\GoldenGate`

## OpenCore 0.6.9 (2021.05.03)
Bootloader / Kexts:
* Lilu 1.5.3
* NVMeFix 1.0.7
* AppleALC 1.6.0
* VirtualSMC 1.2.3
* IntelMausi 1.0.6
* RestrictEvents 1.0.1

config.plist Changes:
* Added Custom Memory examples for SMBIOS MacPro7,1.  For more info, refer to [post](https://www.tonymacx86.com/threads/gigabyte-b550-vision-d-thunderbolt-3-amd-ryzen-7-3700x-amd-rx-5600-xt.304553/post-2246642).

## OpenCore 0.6.8 (2021.04.05)
Bootloader / Kexts:
* Lilu 1.5.2
* NVMeFix 1.0.6
* WhateverGreen 1.4.9
* AppleALC 1.5.9
* VirtualSMC 1.2.2
* Updated "Resources" files for OpenCanopy GUI

## Updated required SSDTs (2021.03.19)
* Replaced base SSDTs to the ones from the Dortania Guide

## Recommended BIOS Settings (2021.03.11)
* Moved BIOS Settings back to BASE-EFI README.

## OpenCore 0.6.7 (2021.03.01)
Bootloader / Kexts:
* WhateverGreen 1.4.8
* AppleALC 1.5.8
* VirtualSMC 1.2.1

## OpenCore 0.6.6 (2021.02.04)
Bootloader / Kexts:
* Lilu 1.5.1
* WhateverGreen 1.4.7
* AppleALC 1.5.7
* VirtualSMC 1.2.0
* Updated "Resources" files for OpenCanopy GUI

config.plist Changes:
* Misc -> Boot -> PickerAttributes - 25 (Mouse control in Picker)
* Misc -> Boot -> BootProtect -> OC 0.6.6 replaces with LauncherOption and is defaulted to Full
If you were previously using Bootstrap refer to this [link](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#prerequisites/) for instructions on how to upgrade. My previous EFis had it disabled.
* UEFI -> Drivers -> Added OpenHfsPlus.efi (Note: Commented out)
Note that OpenHfsPlus.efi is now bundled with OpenCore but HfsPlus.efi is still enabled for compatibility reasons and is still faster than OpenHfsPlus.efi.
