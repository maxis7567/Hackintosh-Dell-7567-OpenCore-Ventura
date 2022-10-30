# Personal Hackintosh Opencore Ventura Guide/Instructions for Dell 7567

### Notes
 - HDMI doesn't work on this hack because it is connected to nvidia card and we disabled it
 - Any other issues? Just open an issue in this repo!

### Known Bugs
 - 2.1 audio (2.0 works)

### Specs
 - Intel i7-7700HQ CPU
 - Intel HD Graphics 630 / nVidia GTX 1050 Ti
 - 16GB 2400MHz DDR4 RAM
 - 15.6” 1080p IPS Display
 - 500GB Samsung 970 Evo Plus SSD (Nvme)
 - 1TB 5400RPM Toshiba HDD

### Prerequisites
 - Set BIOS options properly:
   - Disable Legacy Option ROMs
   - Change SATA operation to AHCI (If already using windows, google how to)
   - Disable Secure Boot
   - Disable SGX

## Creating macOS USB With MacOS (you can create usb installer in windows just google it)

- Download MacOs Big Sur from app store and run this code in terminal

```bash
# Figure out identifier for of your usb (/dev/diskX, for example)
diskutil list

# Partition your usb in GPT
diskutil partitionDisk /dev/diskX 1 GPT HFS+J "install_osx" R

# Copy installer imge 
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/install_osx

# Rename 
sudo diskutil rename "Install macOS Ventura" install_osx
```
- Download [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/)
- Use the app to mount USB EFI partition
- Copy EFI from repo USB Efi partition


## Booting USB and Installing macOS

  - Press F12 repeatedly for one-time boot-menu and select your usb (if your usb not appear on boot menu you must add manualy from bios)
  - Choose *install_osx* in OpenCore
  - Open *diskutility* and format the partition into apfs and label the partition (eg. macOS)
  - Continue to install
  - System now automatically reboots, boot again into opencore, but now select 'Install macOS Ventura' instead of 'install_osx'
  

## Post Installation

- Use Clover Configurator to mount APFS EFI partition
- Copy EFI from repo USB EFI partition


**Configure ComboJack for Headphone Jack**
  - open terminal type *sudo* then drag install.sh from tools/comboJack_Installer then press 'Enter'
  - Reboot  
  
  
**To Do List and Things to Consider**
- Config file does not include SMBIOS parameters which is a must. One needs to provide own values. There are guides here and there. Your friend is google as always. For ROM adress you can use your builtin ethernet card MAC adress. MacSerial by Acidanthera is a good way to obtain proper serial and motherboard serial numbers. UUID can be generated with terminal command uuidgen. Make it produced at least five times to be sure it is unique enough. For working imessage and facetime all should be set in a sensible way and make sure that they are not used by someone else either hackintosh or real mac.
- USBMap.kext is set to Macbookpro14,1. If you want to use a different SMBIOS you should also change the correspond model name in the info.plist inside the kext. Fingerprint device is closed to save battery and avoid long waiting before root access. it does not work anyway for now because apple does not allow to use third party ones.

**Disable Hibernation**
```bash
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
sudo pmset -a powernap 0
```
