# Intel 10 Gigabit NICs with Small Tree macOS Drivers
Intel 10 gigabit NICs such as the X540 or X550 (found on the WS X299 Sage/10G) do not have official support in macOS.  However with modding the EEPROM, we can modify the Subsystem ID to 000a to be compatible with the SmallTree macOS Drivers.  Original method by Squuiid outlined in this [MacRumors thread](https://forums.macrumors.com/threads/modify-retail-intel-10gbe-nics-to-use-small-tree-macos-drivers.1968456/).  

**Disclaimer: The instructions below are outlined for the X550-AT2 found on the WS X299 Sage/10G.  Other Intel NICs may have different commands and/or offsets.  I assume no responsibility or liability for any damage that may occur so proceed at your own risk!**

**Before EEPROM modding: No Driver Installed**

![](/Intel%2010G%20SmallTree/Images/eeprombefore.png)

**After EEPROM modding and SmallTree drivers: Driver Installed**

![](/Intel%2010G%20SmallTree/Images/eepromafter.png)

## Requirements
1. [Ubuntu 16.04 LTS](https://releases.ubuntu.com/16.04/)
    * Download the 64-bit PC (AMD64) desktop image
    * Newer versions of Ubuntu do not work.
2. USB Flash drive 8GB+
3. Windows install for installing Ubuntu to USB drive using [Rufus](https://rufus.ie)
4. Ethernet Drivers (SmallTree is recommended but Sonnet also works)
    * [SmallTree macOS drivers](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/)
        * Download the macOS Catalina drivers for macOS Big Sur and macOS Monterey
    * [Sonnet macOS drivers](http://www.sonnettech.com/support/kb/kb.php?cat=513&expand=&action=a3#a3)

## Instructions
1. Install the Ubuntu 16.04 LTS ISO image to a flash drive using Rufus.
2. Boot Ubuntu and click "Try Ubuntu without Installing".
    * Once Ubuntu is loaded, make sure that you have an Internet connection.
3. Open Terminal and enter in the following commands to install "net-tools" and "ethtool"
    * `sudo apt-get install net-tools`
    * `sudo apt-get install ethtool`
4. To locate the address of your ethernet devices, enter in command: `ifconfig`.  In my case my X550 ports were `enp180s0f0` and `enp180s0f1`.

![](/Intel%2010G%20SmallTree/Images/ifconfig.png)

5. To locate the current Vendor/Device-ID of your ethernet devices, enter in command `lspci -nn -vvv | grep Ethernet`.

![](/Intel%2010G%20SmallTree/Images/lspcigrep.png)

6. Using command `sudo ethtool -e enp180s0f0 | less` , we can search for the offset of the Subsytem Vendor/Subsystem ID (X550 is offset 0x240 - 0x243).

![](/Intel%2010G%20SmallTree/Images/beforeoffset.png)

7. For SmallTree driver support, we will modify the Subsytem-ID from `8712` to `000a`.  
For the X550-AT2, the commands are:
    * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x242 value 0x0a`
    *  `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x243 value 0x00`

    * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x242 value 0x0a`
    * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x243 value 0x00`

![](/Intel%2010G%20SmallTree/Images/afteroffset.png)

8. For Sonnet driver support, we will modify the Subsystem-Vendor-ID from `1043` to `16b8` and the Subsystem ID from `8712` to `7212`.  
For the X550-AT2, the commands are:
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x240 value 0xb8`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x241 value 0x16`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x242 value 0x12`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x243 value 0x72`

    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x240 value 0xb8`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x241 value 0x16`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x242 value 0x12`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x243 value 0x72`
    * Note if you want to revert back to SmallTree or Intel drivers, we will modify the Subsystem-Vendor-ID back to `1043`.  For the X550-AT2, the commands are:
        * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x240 value 0x43`
        * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x241 value 0x10`

        * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x240 value 0x43`
        * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x241 value 0x10`
9.  Reboot back into macOS and install the appropriate drivers and your ethernet ports should be enabled!
