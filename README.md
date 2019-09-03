# Tarvos
macOS Mojave 10.14.4 on Kaby-Lake XPS9360

# Hardware Information
- Dell XPS 9360
    - Kabylake i7-7500U
    - 8GB RAM
    - Sharp `SHP144` `LQ133Z1` QHD+ (3200x1800) Touchscreen display
    - Samsung 960 Pro 512GB SSD
    - Dell DW1560 a.k.a BCM94532z Wireless Card
    - Sonix Technology Webcam, device ID [0c45:670c]
    - Disabled Device: SD Card reader
    - Thunderbolt 3
    - 2 USB 3.0 Ports

- Firmware Version
    - BIOS Version: 2.12.0

- Result
    - 11-stage CPU frequency
    - Touchscreen with multi-touch and perfect graphics
    - WiFi + Bluetooth with Airdrop, iMessage and Continuity supported
        - Wi-Fi device ID [14e4:43b1]
        - Bluetooth device ID [0a5c:216f], using BrcmPatchRAM2.kext
    - Touchpad (super precise, very close to original experience except force touch. Two-finger, three-finger, and four-finger gestures in-built and supported)
    - USB 3.0 + thunderbolt 3
    - External Monitor - Tested with `Dell S2417H 24'' 1080p`
    - Perfect sleep after banning hibernation
    - Support lid-close sleep, ->sleep
    - Support touchpad-click awake and lid-open awake
    - Sound and microphone jack
    - Perfect keyboard with in-built function keys like sound adjustment, media keys, brightness adjustment and keyboard backlight adjustment

# UEFI Adjustments

Launch grub from clover and enter the following commands:

```
setup_var 0x4de
setup_var 0x4de 0x00
setup_var 0x785
setup_var 0x785 0x06
setup_var 0x786
setup_var 0x786 0x03
```


| Variable              | Offset | Default value  | Desired value   | Comment                                                    |
|-----------------------|--------|----------------|-----------------|------------------------------------------------------------|
| CFG Lock              | 0x4de  | 0x01 (Enabled) | 0x00 (Disabled) | Disable CFG Lock to prevent MSR 0x02 errors on boot        |
| DVMT Pre-allocation   | 0x785  | 0x01 (32M)     | 0x06 (192M)     | Increase DVMT pre-allocated size to 192M for QHD+ displays |
| DVMT Total Gfx Memory | 0x786  | 0x01 (128M)    | 0x03 (MAX)      | Increase total gfx memory limit to maximum                 |

# BIOS Configurations
- Boot Sequence: Delete All (very important)
- Sata: AHCI
- Enable SMART Reporting
- Disable thunderbolt boot and pre-boot support
- USB security level: disabled
- Enable USB powershare
- Enable Unobstrusive mode
- Diable SD card reader (saves 0.5W of power)
- TPM off
- Deactivate COmputrace
- Enable CPU XD
- Disable Secure Boot
- Disable Intel SGX

- Disable Wake on USB-C Dell Dock
- Battery Charge profile: Standard
- Numlock Enable
- FN-lock mode: Disable/Standard
- Fastboot: minimal
- BIOS POST Time: 0s
- Enable VT
- Disable VT-D (important, otherwise BT not function)
- Wireless switch OFF for Wifi and BT
- Enable Wireless WIFI and BT
- Allow BIOS Downgrade
- Allow BIOS recovery from HD, disable Auto-recovery
- Auto-OS recovery threshold: OFF
- SupportAssist OS Recovery: OFF

# Post installation
Mount the EFI folder of the system, copy and paste the CLOVER folder of the boot USB drive to the system EFI.

Copy the files in the /L/E folder of this repo to /L/E and rebuild system kext cache with the following commands:

```
sudo cp -R *.kext /Library/Extensions
sudo touch /System/Library/Extensions && sudo kextcache -u /
```

This should make your bluetooth work.

```
sudo pmset -a hibernatemode 0
sudo pmset -a autopoweroff 0
sudo pmset -a standby 0
sudo rm /private/var/vm/sleepimage
sudo touch /private/var/vm/sleepimage
sudo chflags uchg /private/var/vm/sleepimage 
```

This should make your sleep perfect.

# Credits
- [XPS9360-macOS](https://github.com/the-darkvoid/XPS9360-macOS)
- All credits mentioned in the repo above