# TerraPC - Gigabyte GA-H110M-D2P- Intel i3-6100 - MacOS Ventura - Hackintosh
Terra PC (German PC OEM) with Gigabyte Mainboard GA-H110-D2P and an Intel i3 6100 Processor with 8GB Ram

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.0-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOSver](https://img.shields.io/badge/macOS-13.6.9-brightgreen.svg)](https://support.apple.com/HT213843)

### ⚠️ Disclaimer:
- Use this EFI as a **starting point**, remember to always make your own EFI by following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- This EFI may not be constantly updated to the latest version of opencore especially if no major changes are made.
- Please read the whole README before proceeding
- This readme is not yet complete
- EFI is cleaned from all unnecessary stuff
##

![ScreenShot](/terrapc.jpg?raw=true "ScreenShot")

### 🖥️ Hardware
| Component      | Info                                                             |
|----------------|------------------------------------------------------------------|
| **MOBO**       | `Gigabyte GA-H110M-D2P`
| **CPU**        | `Intel Core i3-6100`                                             |
| **RAM**        | `Kingston 8GB 2133mhz`                                           |
| **Storage**    | `Hikvision 256GB SSD`                                            |
| **iGPU**       | `Intel UHD Graphics 530 spoofed as Intel UHD Graphics 630`       |
| **dGPU**       | `None`                                                           |
| **Audio**      | `ALC887 - alcid=1`                                               |
| **Ethernet**   | `Realtek Gigabit Ethernet RTL8111GR`                             |

### ✅️ What works</strong></summary>

- iGPU acceleration Intel UHD 530 1536 MB spoofed as Intel UHD 630
- Audio
- HDMI port
- USB ports (Using [USBInjectAll.kext](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) Usb ports should be mapped but they are working fine for now)
- Ethernet
- iMessage (to be activated via "original" Apple Serial Number)

### ❌️ What doesn't work

- Sleep
- Airdrop (No natively compatible wifi adapter)
- Hardware DRM (No compatible dGPU): [Fixing DRM support and iGPU performance](https://dortania.github.io/OpenCore-Post-Install/universal/drm.html)   



## ⚙️ Setup
<details>
<summary><strong>🔧 BIOS Settings (maybe some of them not needed, but never touch a running system ;)</strong></summary>
  <br>

1. Load `BIOS Defaults` under `Save & Exit`
2. Under `BIOS` TAB:
3. `Windows 8/10 Features:               Other OS`
4. `Storage Boot Device Option control:  UEFI`
5. `Other PCI devices :                  UEFI`
6. `Secure Boot / Attempt Secure Boot:   Disabled`
7. Under `Peripherals` TAB:
8. `Initial Display Output:              IGFX`
9. `Intel Platform Trust Technology:     Disabled`
10. `Trusted Computing/Security Dev.Sup:  Disabled`
11. `Intel Bios Guard Technology:         Disabled`
12. Under `USB Configuration`
13. `Legacy USB Support:              Enabled`
14. `XHCI Hand-Off:                   Enabled`
15. `USB Mass Storage Dev. Support:   Enabled`
16. `Port 60/40 Emulation:            Enabled`
17. Under `SATA and RST Configuration`
18. `SATA Mode:                       AHCI` 

</details>
<details>
<summary><strong>🔢 Generating SMBIOS</strong></summary>
  <br>

Used [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) from corpnewt, to generate a fake serial number, UUID and MLB.

**This step is mandatory to get the device booting and get iServices to work later on**
1. Download GenSMBIOS from the link above as .ZIP, then extract it.
2. Start GenSMBIOS and select option `1` to download and install MacSerial
3. Select option `3` and enter `iMac18,1`
4. **IMPORTANT:** reminder that you need an **invalid serial!** to check copy and paste the second part saying `Serial: XXXXX..` in [Apple's Check Coverage Page](https://checkcoverage.apple.com/), if you get a red message saying "We're sorry, we're unable to check coverage for this serial number."
 then you're good to go! Otherwise, go back and restart from step `2` (more info [here](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#serial-number-validity))
5. once you get the right serial number you can go and fill the generated data in the config.plist file under `PlatformInfo` section, and you are good to go! 
</details>
<details>
<summary><strong>🫣 iGPU Spoofing - Generating Patch:</strong></summary>
  <br>
  
**This step is done after you finish the installation**
1. Download [Hackintool](https://github.com/benbaker76/Hackintool) and open it.
2. Go to `Patch` Category 
3. Select `Kaby Lake` in the `Intel Generation` Dropdown.
4. Select `0x59120000` in the `Platform ID` Dropdown.
5. Go to the `Patch` Tab and the `Advanced` menu.
6. Enable `DP -> HDMI`, `Use Intel HDMI`, and `Enable HDMI20 (4K)`.
7. Enable `Spoof Video Device ID` and select `0x5912: Intel HD Graphics 630` from the dropdown menu.
8. Click `Generate Patch`
9. Now you got the patch, you need to copy the `PciRoot(0x0)/Pci(0x2,0x0)` Key .
10. Paste it under `DeviceProperties > Add` in your config.plist file.
11. Have fun!
</details>
