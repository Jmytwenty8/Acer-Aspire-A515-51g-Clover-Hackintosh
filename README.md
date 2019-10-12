 # Acer-A515-51G-Hackintosh
#### Supports MacOS 10.15, 10.14.x and 10.13.x

<p align="center">
  <img src="https://imgur.com/a/ORRx4Ze" alt="Specs">
</p>


## Whats Working...
 - [x] Audio w/ headphone jack
 - [x] CPU Speedstep (XCPM)
 - [x] iGPU with disabled dGPU
 - [x] Battery Management
 - [x] Backlight
 - [x] Ethernet
 - [x] HDMI + Audio
 - [x] Sleep + Wake
 - [x] Smart Touchpad + Gestures (using I2C)
 - [x] WebCam
 - [x] Usb 3.0 + Type C
 - [x] Sleep From (Lid)
 - [x] Native hotkey support w/ Fn keys
 - [x] Internal SD Card reader

<p align="center">
  <img src="https://i.imgur.com/A0cKRrX.png" alt="Benchmarks">
</p>

  - Geekbench Score :
  - - Single-Core Score : 4219 [Link](https://browser.geekbench.com/v4/cpu/13793813).
  - - Multi-Core Score : 14837 [Link](https://browser.geekbench.com/v4/cpu/13793813).
  - - GPU OpenCL Score : 31191 [Link](https://browser.geekbench.com/v4/compute/4258348).
 
## Installation
 
 ### BIOS Settings
* *Security* → Set supervisor password (to disable secure boot)
* *Security* → Password on boot → **Disable**
* *Boot* → Secure Boot → **Disable**
* *Boot* → Boot Mode → **UEFI**
* *Main* → Touchpad → Set touchpad mode → **Advance**
* *Main* → Lid Open resume → **Enabled**


###  Basic Installation

- Create a Bootable USB for MacOS by using the Guide by RehabMan [[Guide] Booting the OS X installer on LAPTOPS with Clover](https://www.tonymacx86.com/el-capitan-laptop-support/148093-guide-booting-os-x-installer-laptops-clover.html).
- Install MacOS to SSD / Hard drive.
- Install [Clover Bootloader](https://sourceforge.net/projects/cloverefiboot) into SSD / hard drive.
- Copy **ALL** the Contains of this Repo into **CLOVER** Folder inside the EFI partition of SSD / Hard drive.
- **[IMPORTANT]** Make sure to Generate system definitions of MacBook pro 15.2 in config.plist file using [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/) or else MacOS will not Boot! You can find Tutorial about it [Here](https://www.tonymacx86.com/threads/guide-how-to-configure-your-systems-smbios-correctly.198155/).

### Post Installation

- Disable Hibernation : Hibernation (suspend to disk or S4 sleep) is not supported on hackintosh. it could cause problems if you don't disable it.

```sh

$ sudo pmset -a hibernatemode 0

$ sudo rm /var/vm/sleepimage

$ sudo mkdir /var/vm/sleepimage

$ sudo pmset -a standby 0

$ sudo pmset -a autopoweroff 0

$ sudo trimforce enable  ###For SSD only.

```

- OOB combojack ([FIX AVAILABLE](https://github.com/hackintosh-stuff/ComboJack)) Note: If you want to use combojack and only internal mic - use layout 3. If you want to use headset use layout 31. Layout 31 is yet not well supported and undefined behaviours occur (because combojack is designed for layout 71).



<p align="center">
<b>⭐ Please consider starring this repository if it helped you! ⭐</b>
</p>

---
### Laptop configuration


- **Audio** : The Sound Card is `Realtek ALC255`, which is driven by `AppleALC` on `layout-id 3`.

- **CPU** : The CPU model is `i5-8250U` and XCPM power management is natively supported.
 - - XCPM and HWP are recommended to work together to reach better power management, by injecting `plugin-type=1` with `SSDT-XCPM`, and using `CPUFriend.kext`.

- **Graphics** : The iGPU is `Intel UHD Graphics 620`, and its enabled using `Ig-Platform-id=0x191E0000`.
- - The discrete dGPU is `NVIDIA GeForce MX150`, and it is disabled because macOS doesn't support optimus technology. Plus battery life is much improved.
- - The colour branding or corrupted colour depth is fixed with Intel Skylake spoof and EDID fix.
- - Native brightness hotkey support; using `DSDT.aml` patched from RehabMan's [[Guide] Patching DSDT/SSDT for LAPTOP backlight control](https://www.tonymacx86.com/threads/guide-patching-dsdt-ssdt-for-laptop-backlight-control.152659/).

- **Battery** : Battery Management using `SMCBatteryManager.kext`.

- **Backlight** : Native Brightness control using `AppleBacklightFixup.kext` & `SSDT-PNLF.aml`.

- **Ethernet** : Gigabit Ethernet using `RealtekRTL8111.kext`.

- **HDMI** : Intel Framebuffer patches to change the connector-type to match the physical connector, using `WhateverGreen.kext`.
- - **HDMI Audio** : Injecting property `"hda-gfx" = "onboard-1"` on HDAU, IGPU, HDEF objects & also injecting `layout-id 3` on HDAU to match layout-id on HDEF.

- **Touchpad** : Native I2C touchpad support using `VoodooI2C.kext`; Set touchpad Mode to Advance from `BIOS` & All gestures will work fine.

- **USB** : Custom `SSDT-UIAC.aml` SSDT for `USBInjectAll.kext` that configures USB ports on XHC such that the port limit patch is not needed, and each UsbConnnector value is correct for each port.
- - You can also find USB port mapping for `SSDT-UIAC` [Here](https://github.com/SiddheshNan/Acer-A515-51G-Hackintosh/blob/master/USB%20port%20mapping%20for%20SSDT-UIAC.md).


- **Wi-Fi** : Stock WiFi Card is `Atheros QCA9377` It is not supported on MacOS.
- - Best Choice will be to replace current Card with `BCM94352Z` which has WiFi+BT, or `BCM943602BAED`. You can find it on AliExpress for like $20-30. I Have already changed my current WiFi card with `BCM94352Z`.
- - Config.plist is already patched for `BCM94352Z` and `BCM943602BAED` & added kexts for BT as well.
- - Keep in mind, this laptop uses **M.2(NGFF)** Socket with **A+E Key**. Half size card won't work.

- **Bluetooth** : Stock `Atheros QCA9377` BT will work out-of-the box, But you can't turn it off becouse BT power management is not supported;
- - To Fix this you'll have to get a MacOS compatible WiFi+BT card. The best choice will be `BCM94352Z` which has WiFi+BT.


## Credits

- **Special Thanks** to [Acidanthera](https://github.com/acidanthera) for most of the Kexts.
- **Special Thanks** to [RehabMan](https://github.com/RehabMan).
- Thanks to [Clover Bootloader](https://sourceforge.net/projects/cloverefiboot).
- Thanks to [goodwin](https://github.com/goodwin/) for [ALCPlugfix](https://github.com/goodwin/ALCPlugFix).
- Thanks to [alexandred](https://github.com/alexandred/) for [VoodooI2C](https://github.com/alexandred/VoodooI2C).
- Thanks to [daliansky](https://github.com/daliansky/) for Some Patches which I used here from [XiaoMi-Pro](https://github.com/daliansky/XiaoMi-Pro/).
- Thanks to [SiddheshNan](https://github.com/SiddheshNan) for his [CLOVER/EFI](https://github.com/SiddheshNan/Acer-A515-51G-Hackintosh). All credit goes to him. This EFI is adapted for my system and for personal use only. Please refer to his repo for info and enquiries.
- Thanks to [Sinetek](https://github.com/sinetek) for [Sinetek-rtsx](https://github.com/sinetek/Sinetek-rtsx).
- Thanks to [al3xtjames](https://github.com/al3xtjames) for [NoTouchID](https://github.com/al3xtjames/NoTouchID).
