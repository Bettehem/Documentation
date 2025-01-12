# Documentation
Building &amp; installation documentation for Halium 9 Ubuntu Touch & Droidian on the OnePlus 3/3T

```
 * We are not responsible for bricked devices, dead SD cards,
 * thermonuclear war, or you getting fired because the alarm app failed. Please
 * do some research if you have any concerns about features included in this modification
 * before flashing it! YOU are choosing to make these modifications, and if
 * you point the finger at us for messing up your device, we are not responsible.
```

To install Ubuntu Touch or Droidian on your OP3/3T, you don't necessarily need to compile halium source.\
If you want to use the pre-built image, you can go directly to [Treblelizing your OP3(T) & Firmware & TWRP](#treblelizing-your-op3t--firmware--twrp)


### Table of Contents
* [Install prerequisites for building](#install-prerequisites-for-building)
* [Initializing local repo](#initializing-local-repo)
* [Syncing local repository](#syncing-local-repository)
* [Applying patches](#applying-patches)
* [Building HAL parts](#building-hal-parts)
* [Treblelizing your OP3(T) & Firmware & TWRP](#treblelizing-your-op3t--firmware--twrp)
* [Installing UBports using Erfan's GSI](#installing-erfans-ubuntu-touch-gsi)
* [Installing Droidian](#installing-droidian)
* [Note](#note)
* [Source](#source)
* [Thanks](#thanks)

## Install prerequisites for building

### Debian based systems
```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install git gnupg flex bison gperf build-essential \
  zip bzr curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
  libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
  libgl1-mesa-dev g++-multilib mingw-w64-i686-dev tofrodos \
  python-markdown libxml2-utils xsltproc zlib1g-dev:i386 schedtool \
  repo liblz4-tool bc lzop imagemagick libncurses5 rsync
```

For ubuntu Focal, repo is not in the ubuntu repository, download it manually from https://packages.ubuntu.com/groovy/all/repo/download

## Initializing local repo
```
mkdir ~/Halium/ && cd ~/Halium/
repo init -u https://github.com/Halium/android -b halium-9.0 --depth=1
git clone https://github.com/OP3-Halium/local_manifests .repo/local_manifests/
git clone https://gitlab.com/JBBgameich/halium-install/ halium/scripts/ --depth 1
```

## Syncing local repository
```
cd ~/Halium/
repo sync --no-clone-bundle --no-tags -c -j`nproc`
```

## Applying patches
```
hybris-patches/apply-patches.sh --mb
```
**NOTE:** This will fail if you've already applied them; to revert the patches **and all other local changes** run `repo sync -l`

## Building HAL parts
Type the following to initilize the current environment for building:
```
. build/envsetup.sh
breakfast oneplus3         
export LANG=C LC_ALL=POSIX
```
To produce the required `halium-boot.img` kernel image for Ubuntu Touch, execute:
```
mka halium-boot
```
**NOTE:** If you've decided to install manually (without Erfan GSI) you also need to `mka systemimage`!

## Treblelizing your OP3(T) & Firmware & TWRP
### Files 
https://drive.google.com/drive/folders/1vnJEKkhO3xqH-fWWG55-yxwx5K1EeKq7?usp=sharing

For further information please read the subject on [XDA](https://forum.xda-developers.com/oneplus-3/oneplus-3--3t-cross-device-development/treble-lineageos-15-1-treble-oneplus-3-t3830455)
 - To sumup, GSI port requires a dedicated vendor partition.

Download the "[New][A/B] LineageOS 16.0 Treble system-as-root"
- Recovery : [twrp-op3treble-3.3.1-1.img](https://mega.nz/folder/UgdQRYSD#8s-_u2HJQZDEqNnFOnejxQ)
- LineageOS 16 image : [lineage-Ubuntu touch custom](https://drive.google.com/drive/folders/1vnJEKkhO3xqH-fWWG55-yxwx5K1EeKq7?usp=sharing)
- Firmware 9.0.6 : [oxygenos-9.0.6](https://github.com/nvertigo/firmware-3t) **be careful the firmwares are device specific**

Reboot into fastboot and install the TWRP recovery => Check the official link for installation [TWRP link](https://twrp.me/oneplus/oneplusthree.html)

Treblelize your OP3(t)
 - Follow the information there : https://forum.xda-developers.com/oneplus-3/oneplus-3--3t-cross-device-development/treble-lineageos-15-1-treble-oneplus-3-t3830455

Reboot into the TWRP Recovery
- Update the firmware
- Install lineage 16.0

Reboot into Lineage, confirm everything works fine.


## Installing Erfan's Ubuntu Touch GSI

Note that this version of Ubuntu Touch is old and no longer developed. OnePlus 3 development is currently focused on Droidian but once everything is working there, Ubuntu Touch will be revived on the OP3
1. Download the latest GSI zip from [here](https://mirrors.lolinet.com/firmware/halium/GSI)
2. Ensure your `/vendor` partition is populated (after mounting) with content from an Android 9 ROM (LineageOS or equivalent)
3. In TWRP, go to `Wipe` -> `Advanced Wipe` -> Select everything except Vendor and USB-OTG, then `Swipe to Wipe`. Then reboot back to recovery.
4. Flash the GSI zip file
5. Flash [halium-boot.img](https://github.com/OP3-Halium/Documentation/blob/master/halium-boot.img) to your boot partition (See instructions in the [note](#note) below)
6. Flash the [OP3_GSI_Fix_V1.X](https://drive.google.com/drive/folders/1vnJEKkhO3xqH-fWWG55-yxwx5K1EeKq7?usp=sharing)
7. Reboot
8. Enjoy!


## Installing Droidian

1. Download latest .zip from [here](https://github.com/droidian-images/droidian/releases) (look for api28-arm64 zip)
2. Ensure your '/vendor' partition is populated (after mounting) with content from an Android 9 ROM (LineageOS or equivalent)
3. In TWRP, go to `Wipe` -> `Advanced Wipe` -> Select everything except Vendor and USB-OTG, then `Swipe to Wipe`. Then reboot back to recovery.
4. Flash the droidian zip file
5. Flash [halium-boot.img](https://github.com/OP3-Halium/Documentation/blob/master/halium-boot.img) to your boot partition (See instructions in the [note](#note) below)
6. Flash [op3-gsi-fix-droidian.zip](https://gitlab.com/Bettehem/op3-gsi-fix-droidian/-/jobs/artifacts/main/browse?job=makezip)
7. Reboot. The phone will first show the droidian logo, then the screen will go black for 20-30 seconds. During this time when the screen is black, **do not** press the power button. Wait patiently, the screen will turn back on when the boot process is done.
8. Once you have booted for the first time, open a terminal on the phone and run this command:\
   `sudo apt update && sudo apt install adaptation-droidian-oneplus3`
10. Next run the `move-home` command, which will move your home folder to the /userdata partition so you can use all of the storage on your device. Your device will reboot when it's done.
11. Enjoy!


## Note

If you built your own halium-boot.img, it will be located at `out/target/product/oneplus3/halium-boot.img`.\
There are 3 ways of flashing the boot image to your device (choose one):
1. Using adb (Boot into TWRP, connect usb cable and then run from your pc):
```
adb push path/to/halium-boot.img /tmp/
adb shell "dd if=/tmp/halium-boot.img of=/dev/block/bootdevice/by-name/boot"
```
2. Using fastboot (Boot into fastboot/bootloader mode, connect usb cable and then run from your pc):
```
fastboot flash boot path/to/halium-boot.img
```
3. Using TWRP's own install method (Boot into TWRP, connect usb cable and then run from your pc):\
a) Connect your device to your pc and push halium-boot.img to the device:
`adb push path/to/halium-boot.img /sdcard/.`<br>
b) In TWRP main menu, press `Install` -> `Install Image` -> `halium-boot.img`<br>
c) Select `Boot` as the partiton where to install, then `Swipe to confirm Flash`<br>


## Thanks
- Documentation based on https://github.com/ubports-oneplus5
- Another 3.18 kernel working with ErfanGSI : https://github.com/MotoZ-2016/android_kernel_motorola_msm8996/tree/halium-9.0
- https://github.com/OP3Treble Project

## Source
https://github.com/OP3-Halium/

## Ubuntu Touch Known issues & temporary fix
- Webbrowser not reconized as a mobile 
  -  edit the file ```sudo nano /usr/lib/arm-linux-gnueabihf/qt5/qml/Morph/Web/UserAgent02.qml```
  -  chang line 68 to ``` return (screenDiagonal === 0) ? "unknown" : (screenDiagonal > 0 && screenDiagonal < 190) ? "small" : "small" ```
  
- Data doesn't work 
  ```cd /usr/share/ofono/scripts/ && ./activate-context```
- Wifi doesn't restart
  -  ```sudo nano /etc/udev/rules.d/90-oneplus3.rules```
  -  add   ```ACTION=="change|remove|create", DEVPATH=="/devices/soc/600000.qcom,pcie/pci0000:00/0000:00:00.0/0000:01:00.0/ieee80211/phy?", RUN+="/usr/bin/python3 /home/phablet/wlan_restart.py"```
  -  then   ```nano /home/phablet/wlan_restart.py```
  -  ```#!/usr/bin/python3
     import subprocess
     import os.path
     subprocess.Popen("echo sta > /sys/module/wlan/parameters/fwpath", shell=True)
     print("Wifi card set up for wlan activation")
```
 
