#  Dell Latitude 5290 2-in-1 UHD620 iGPU Clover

## Specifics

- CPU : Intel® Core™ i5-8350U Processor (6M Cache, up to 3.60 GHz)
- Graphics : Intel® UHD Graphics 620
- Sound : Realtek ALC3253 (ALC225)
- Display : 12.3 Inch 1920 X 1280 (WUXGA+) 3:2 10 Points Multi Touch
- Memory : Samsung LPDDR3 8GB 1867MHZ (4GB * 2 Dual Channel)
- SSD : IM2P33F8-512GD
- Wireless : Intel AC8265
- Battery : 42WHr


## Clover Bootloader/macOS Version

- Clover Bootloader : 5161
- macOS : 13.6


## BIOS Setup

- Load Optimized Defaults


## DSDT Patch

- Edit syntax error  

from
```
                If (LEqual (PM6H, One))
                {
                    CreateBitField (BUF0, \_SB.PCI0._Y0C._RW, ECRW)  // _RW_: Read-Write Status
                    Store (Zero, ECRW (If (PM0H)
                            {
                                CreateDWordField (BUF0, \_SB.PCI0._Y0D._LEN, F0LN)  // _LEN: Length
                                Store (Zero, F0LN)
                            }))
                }
```
to
```
                If (LEqual (PM6H, One))
                {
                    CreateBitField (BUF0, \_SB.PCI0._Y0C._RW, ECRW)  // _RW_: Read-Write Status
                    Store (Zero, ECRW)

                }

                If (PM0H)
                {
                    CreateDWordField (BUF0, \_SB.PCI0._Y0D._LEN, F0LN)  // _LEN: Length
                    Store (Zero, F0LN)
                }
```
- Rename ```XTBT (TBSE, CPGN)``` to ```XTB2 (TBSE, CPGN)```
- Add this just before ```Method (XTBT, 2, Serialized)```
```
        Method (XTB2, 2)
        {
            XTBT (Arg0, Arg1)
        }
```
- [misc] Remove _PRW from LID
- [sys] AC Adapter Fix
- [sys] Add IMEI
- [sys] Fix _WAK Arg0 v2
- [sys] Fix Mutex with non-zero SyncLevel
- [sys] Fix PNOT/PPNT
- [sys] HPET Fix
- [sys] IRQ Fix
- [sys] OS Check Fix (Windows 10)
- [sys] RTC Fix
- [sys] Shutdown Fix v2
- [sys] SMBUS Fix
- [GPIO] GPIO Controller Enable [SKL+]
- Rename ```HECI``` to ```IMEI```
- Edit BRT6 method - control brightness with 'Fn + F11, Fn + F12' keys  

from
```
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (LCD, 0x86)
            }

            If (And (Arg0, 0x02))
            {
                Notify (LCD, 0x87)
            }
        }
```
to
```
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (^^LPCB.PS2K, 0x0406)
            }

            If (And (Arg0, 0x02))
            {
                Notify (^^LPCB.PS2K, 0x0405)
            }
        }
```

***Need these MaciASL patch sources  
_RehabMan Laptop [http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master]  
_VoodooI2C-Patches [http://raw.github.com/alexandred/VoodooI2C-Patches/master]***  
***Modify DSDT on your system to prevent kernel panic***


## SSDT

- SSDT-ALC225.aml [Sleep Headphone Output Fix]
- SSDT-DEEPIDLE.aml ***[Only For Thunderbolt 3 Model],Havent tested***
- SSDT-EC-USBX.aml [USB Power Control]
- SSDT-PLUG.aml [PluginType=1]
- SSDT-PNLF.aml [Brightness Control]
- SSDT-PRTSC-F13.aml [PrtScr Key to F13 Key]
- SSDT-RMNE.aml [Null Ethernet]
- SSDT-TB3.aml ***[Only For Thunderbolt 3 Model],Havent tested**
- SSDT-TYPC.aml ***[Only For Thunderbolt 3 Model],Havent tested**
- SSDT-UPRW.aml [Prevent wake from USB, Fix some USB issues]
- SSDT-XOSI.aml [OS Check Fix for Brightness Control Key, Power Button, Touch Screen]


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
- change \_RMV to XRMV [Type C Hot Swap Fix for ***Thunderbolt 3 Model,Havent tested**]


## Drivers64UEFI

- ApfsDriverLoader.efi
- HFSPlus.efi
- OpenRuntime.efi
- VBoxHfs.efi
Other drivers seemed unuseful,you can remove if you want. 


## Kexts

- Airportltlwm.kext
- AppleALC.kext
- IntelBTPatcher.kext
- IntelBluetoothFirmware.kext
- BlueToolFixup.kext
- CPUFriend.kext
- CPUFriendDataProvider.kext    -    Generated with one-key-cpufriend by stevezhengshiqi
- Lilu.kext
- RestrictEvents.kext
- SMCBatteryManager.kext
- SMCDellSensor.kext
- SMCLightSensor.kext
- SMCProcessor.kext
- SMCSuperIO.kext
- USBPorts.kext    -    Generated with Hackintool, HS/SS port matching and realignment
- VirtualSMC.kext
- VoodooI2C.kext
- VoodooI2CHID.kext
- VoodooPS2Controller.kext    -    Remove PlugIns/VoodooPS2Trackpad/VoodooPS2Mouse/VoodooInput.kext
- WhateverGreen.kext
- BrightnessKeys.kext

***CPUFriend.kext, CPUFriendDataProvider.kext are not mandatory kext  
But creating it for your system will help you manage power***


## CLOVER Boot Arguments

- alcid=30 - To make alc225 work

## CLOVER Devices-Properties

- Intel® UHD Graphics 620
```
            <key>PciRoot(0x0)/Pci(0x2,0x0)</key>
            <dict>
                <key>AAPL,GfxYTile</key>
                <data>AQAAAA==</data>
                <key>AAPL,ig-platform-id</key>
                <data>AgAmWQ==</data>
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
platform-id can be set 0000C087 according to Document of OpenCore

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
- 'Fn' + 'r' || PrtScr = F13
- 'Fn' + 'F11' || 'Fn' + 's' = F14 (Brightness down)
- 'Fn' + 'F12' || 'Fn' + 'b' = F15 (Brightness up)
- 'Fn' + 'Esc', 'F1', 'F2', 'F3', 'F4', 'F5', 'F6', 'F7', 'F10', 'PrtScr', 'Arrows'
- Press and hold the 'Power Button' for a short time to enter sleep, long press to display the power menu


## What Works

***Graphics/Display***
- Intel® UHD Graphics 620 QE/CI, 2048MB vRam
- Type C DP 2 ports Video/Audio output Hot Swap
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

***Power Management***
- CPU/Speed Step
- Battery
- Type C PD 2 Ports Charging, PowerShare
- Sleep/Wake : *** For Thunderbolt 3 model, see 'SSDT-DEEPIDLE.aml' in [Issues] ,Havent tested***
- Lid Close Sleep with Travel Keyboard

***USB, Storage***
- Full Size/Type C USB 2.0, 3.0 Hot Swap
- m.2 NVME 2280/ m.2 SATA 2280 1 Slot (m.2 NVME 2230(2242)/m.2 SATA 2230(2242) 1 Slot Havent tested,maybe okay)

***Wireless Communication***
- Wi-Fi, Bluetooth


## Issues

- 3.5mm audio output(headphone jack) havent tested,the jack seemed 
physically damaged in my device,so need test!
  
- MicroSD slot havent tested,because i do not have a SDcard,maybe work after install a kernel kext?

- I2C front and rear camera (AVStream2500, OV5670, OV8858) not recognized,also not planned

## Screenshots

<img width="567" alt="截屏2025-05-15 15 57 55" src="https://github.com/user-attachments/assets/ff3c1b9e-4ab1-4737-98a8-9b4eb2fdefb1" />

<img width="497" alt="截屏2025-05-15 15 58 17" src="https://github.com/user-attachments/assets/b0d2caf6-ca22-4a43-ba53-61d8fc9d9ebd" />

<img width="850" alt="截屏2025-05-15 15 57 22" src="https://github.com/user-attachments/assets/2c8a9edc-0bb3-4b44-aa82-6aac4bad2539" />

**Original USB Ports**
![07USBOrigin](https://user-images.githubusercontent.com/46496967/60283654-a36a7680-9944-11e9-8ca0-efb77f46b023.png)

**Edited USB Ports**
![08USBEdit](https://user-images.githubusercontent.com/46496967/60283655-a49ba380-9944-11e9-98ad-48ce1f903b61.png)


README_KR
https://github.com/laelsirus/Dell-Latitude-5290-2-in-1-UHD620-iGPU-CLOVER/blob/master/README_KR.md
