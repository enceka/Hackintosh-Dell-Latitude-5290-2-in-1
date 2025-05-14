#  Dell Latitude 5290 2-in-1 UHD620 iGPU OpenCore

## Specifics

- CPU : Intel® Core™ i5-8350U Processor (6M Cache, up to 3.60 GHz)
- Graphics : Intel® UHD Graphics 620
- Sound : Realtek ALC3253 (ALC225)
- Display : 12.3 Inch 1920 X 1280 (WUXGA+) 3:2 10 Points Multi Touch
- Memory : Samsung LPDDR3 8GB 1867MHZ (4GB * 2 Dual Channel)
- SSD : IM2P33F8-512GD
- Wireless : Intel 8265
- Battery : 42WHr


## OpenCore/macOS Version

- OpenCore : 1.0.4
- macOS : 13.6


## BIOS Setup

- Load Optimized Defaults


## SSDT

- SSDT-ALC225.aml [Sleep Headphone Output Fix]
- SSDT-EC-USBX.aml [USB Power Control]
- SSDT-PLUG.aml [PluginType=1]
- SSDT-PNLF.aml [Brightness Control]
- SSDT-PRTSC-F13.aml [PrtScr Key to F13 Key]
- SSDT-RMNE.aml [Null Ethernet]
- SSDT-UPRW.aml [Prevent wake from USB, Fix some USB issues]
- SSDT-XOSI.aml [OS Check Fix for Brightness Control Key, Power Button, 
Touch Screen]


## CLOVER ACPI Hotpatch

- change GFX0 to IGPU [IGPU Fix]
- change HDAS to HDEF [Audio Fix]
- change HECI to IMEI
- change MEI to IMEI
- change ECDV to EC [USB Fix]
- change OSID to XSID [OS Check Fix for Brightness Control Key]
- change \_OSI to XOSI [OS Check Fix for Brightness Control Key]
- change UPRW to XPRW [Prevent wake from USB]
- change GPRW to YPRW [Prevent wake from USB]
- change \_RMV to XRMV [Type C Hot Swap Fix for ***Thunderbolt 3 Model***]


## Drivers64UEFI

- OpenRuntime.efi (neccessary)
- OpenCanopy.efi (neccessary)
- OpenHfsPlus.efi (neccessary)
- OpenLinuxBoot.efi
- OpenLegacyBoot.efi
- OpenNetworkBoot.efi
- ResetNvramEntry.efi
- ToggleSipEntry.efi


## Kexts

- Lilu.kext
- HibernationFixup.kext (Seemed unuseful)
- VirtualSMC.kext
- WhateverGreen.kext
- AppleALC.kext
- IntelBTPatcher.kext
- IntelBluetoothFirmware.kext
- BlueToolFixup.kext
- SMCBatteryManager.kext
- SMCDellSensor.kext
- SMCLightSensor.kext
- SMCProcessor.kext
- SMCSuperIO.kext
- USBPorts.kext
- CPUFriend.kext
- CPUFriendDataProvider.kext
- VoodooPS2Controller.kext
- VoodooPS2Keyboard.kext
- Airportltlwm.kext
- VoodooI2C/VoodooInput.kext
- VoodooI2C/VoodooGPIO.kext
- VoodooI2C/VoodooI2CServices.kext
- VoodooI2C.kext
- VoodooI2CHID.kext
- BrightnessKeys.kext (Seemd unuseful,brightness can only be set with 
F11/F12(FN+S/B),cannot enable FN+key,need to further research)

***CPUFriend.kext, CPUFriendDataProvider.kext are not mandatory kext  
But creating it for your system will help you manage power***

## Devices-Properties

- Set Audio Layout-ID, Enable Display Audio
```
      <key>PciRoot(0x0)/Pci(0x1F,0x3)</key>
      <dict>
        <key>layout-id</key>
        <integer>30</integer>
      </dict>
```
- Intel® UHD Graphics 620
```
      <key>PciRoot(0x0)/Pci(0x2,0x0)</key>
      <dict>
				<key>AAPL,GfxYTile</key>
				<data>AQAAAA==</data>
				<key>AAPL,ig-platform-id</key>
				<data>AADAhw==</data>
				<key>device-id</key>
				<data>FlkAAA==</data>
				<key>dpcd-max-link-rate</key>
				<data>CgAAAA==</data>
				<key>enable-dpcd-max-link-rate-fix</key>
				<data>AQAAAA==</data>
				<key>enable-hdmi20</key>
				<data>AQAAAA==</data>
				<key>framebuffer-con0-alldata</key>
				<data>AAASAAIAAACYAAAAAQUSAAAEAADHAwAAAgQSAAAEAADHAwAA</data>
				<key>framebuffer-con0-enable</key>
				<data>AQAAAA==</data>
				<key>framebuffer-fbmem</key>
				<data>AACQAA==</data>
				<key>framebuffer-patch-enable</key>
				<data>AQAAAA==</data>
				<key>framebuffer-stolenmem</key>
				<data>AAAwAQ==</data>
				<key>framebuffer-unifiedmem</key>
				<data>AAAAgA==</data>
			</dict>
```


## ETC

***After Installation***
- Remove these Boot Arguments  
    -v  
    debug=0x100  
    keepsyms=1
- Install Karabiner-Elements.app to bind media control to function key
- HiDPI 1920 * 1280 (3840 * 2560) can be added, but it requires more 
resources,recommend 1200x800

***Intel® Core™ i5-8350U Processor***
- CPUFriendDataProvider.kext has been modified to manage the operation of 
the 'Intel® Core ™ i5-8350U Processor'  
  If your CPU is not 'Intel® Core™ i5-8350U Processor', remove or 
regenerate the CPUFriendDataProvider.kext

***NullEthernet.kext & SSDT-RMNE.aml***
- Null Ethernet is a way to prevent a Mac address-based license for some 
software from being broken when a wireless card is absent or replaced 
(including iCloud)  
  If you do not need to consider blocking software licenses by changing 
your Mac address, you can remove it

***Fn Key***
- Seemed that not working perfectly

## What Works(Have tested)

***Graphics/Display***
- Intel® UHD Graphics 620 QE/CI, 2048MB vRam
- Brightness control

***Audio***
- Built-in speaker
- Built-in microphone

***Input***
- I2C touch screen Up to 10 points Gesture action (recognized as Magic 
Trackpad 2)
- PS2 Keyboard (Dell Latitude 2-in-1 Travel Keyboard) with Backlight
- Touchpad Up to 5 point Gesture action(Travel Keyboard, Work as a normal 
touchpad)
- Volume button, power button(Power Key)

***Power Management***
- CPU/Speed Step
- Battery
- Type C PD 2 Ports Charging, PowerShare

***USB, Storage***
- Full Size/Type C USB 2.0, 3.0 Hot Swap
- m.2 NVME 2280/ m.2 SATA 2280 1 Slot

## Issues 
- Seemed USBPorts and CPUFriendDataProvider havent been loaded properly?Espeacially USBPorts.kext
- Sleep/Wake/Lid Close Sleep with Travel Keyboard (Sometime can with 
mode25?IDK)
- Bluetooth can scan but cannot connect
- 3.5mm audio output(headphone jack)
- MicroSD slot havent tested,because i do not have a SDcard,maybe work 
after install a kernel kext?
- I2C front and rear camera (AVStream2500, OV5670, OV8858) not 
recognized,also not planned

## Screenshots

<img width="701" alt="截屏2025-05-14 20 18 05" 
src="https://github.com/user-attachments/assets/4c5d3a37-36fc-4044-a084-021fe15d5c4b" 
/>

<img width="693" alt="截屏2025-05-14 23 26 29" 
src="https://github.com/user-attachments/assets/e8d3d3bf-c6b1-4a66-9467-f56e5ea5addb" 
/>

<img width="867" alt="截屏2025-05-14 20 16 53" 
src="https://github.com/user-attachments/assets/e53c7419-390a-4373-898a-aac5a9d4739a" 
/>



