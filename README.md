# Re-rootAVD
### A fork of [rootAVD](https://gitlab.com/newbit/rootAVD) by [newbit @ xda-developers](https://forum.xda-developers.com/m/newbit.1350876), updated for Android 17 support
A Script to...
* root your Android Studio Virtual Device (AVD), with Magisk (Stable, Canary or Alpha)
* patch its fstab
* download and install the USB HOST Permissions Module for Magisk
* install custom build Kernel and its Modules
* download and install AOSP prebuilt Kernel and its Modules

...within seconds.

## Install Magisk
### Download Re-rootAVD via
* `git clone https://github.com/Ko-kn3t/Re-rootAVD.git`

### Preconditions
* the AVD is running
* a working Internet connection for the Menu
* a command prompt / terminal is opened
* `adb shell` will connect to the running AVD

### How to Install ADB (Android SDK Platform-Tools)
* Open Android Studio -> SDK Manager -> Android SDK -> SDK Tools -> Check on **Android SDK Platform-Tools** -> Apply
<img src="https://user-images.githubusercontent.com/37043777/140064719-ea2dd704-1aea-4c38-9725-3edbdafe7924.png" width="200" height="200" />

## rootAVD Help Menu
```
rootAVD A Script to root AVD by NewBit XDA

Usage:  rootAVD [DIR/ramdisk.img] [OPTIONS] | [EXTRA ARGUMENTS]
or:     rootAVD [ARGUMENTS]

Arguments:
        ListAllAVDs                     Lists Command Examples for ALL installed AVDs

        InstallApps                     Just install all APKs placed in the Apps folder

Main operation mode:
        DIR                             a path to an AVD system-image
                                        - must always be the 1st Argument after rootAVD

ADB Path | Ramdisk DIR| ANDROID_HOME:
        [M]ac/Darwin:                   export PATH=~/Library/Android/sdk/platform-tools:$PATH
                                        export PATH=$ANDROID_HOME/platform-tools:$PATH
                                        system-images/android-$API/google_apis_playstore/x86_64/

        [L]inux:                        export PATH=~/Android/Sdk/platform-tools:$PATH
                                        export PATH=$ANDROID_HOME/platform-tools:$PATH
                                        system-images/android-$API/google_apis_playstore/x86_64/

        [W]indows:                      set PATH=%LOCALAPPDATA%\Android\Sdk\platform-tools;%PATH%
                                        system-images\android-$API\google_apis_playstore\x86_64\

        ANDROID_HOME:                   By default, the script uses %LOCALAPPDATA%, to set its Android Home
                                        directory, search for AVD system-images and ADB binarys. This behaviour
                                        can be overwritten by setting the ANDROID_HOME variable.
                                        e.g. set ANDROID_HOME=%USERPROFILE%\Downloads\sdk

        $API:                           25,29,30,31,32,33,34,UpsideDownCake,etc.

Options:
        restore                         restore all existing .backup files, but doesn't delete them
                                        - the AVD doesn't need to be running
                                        - no other Argument after will be processed

        InstallKernelModules            install custom build kernel and its modules into ramdisk.img
                                        - kernel (bzImage) and its modules (initramfs.img) are inside rootAVD
                                        - both files will be deleted after installation

        InstallPrebuiltKernelModules    download and install an AOSP prebuilt kernel and its modules into ramdisk.img
                                        - similar to InstallKernelModules, but the AVD needs to be online

Options are exclusive, only one at the time will be processed.

Extra Arguments:
        DEBUG                           Debugging Mode, prevents rootAVD to pull back any patched file

        PATCHFSTAB                      fstab.ranchu will get patched to automount Block Devices like /dev/block/sda1
                                        - other entries can be added in the script as well
                                        - a custom build Kernel might be necessary

        GetUSBHPmodZ                    The USB HOST Permissions Module Zip will be downloaded into /sdcard/Download

        FAKEBOOTIMG                     Creates a fake Boot.img file that can directly be patched from the Magisk APP
                                        - Magisk will be launched to patch the fake Boot.img within 60s
                                        - the fake Boot.img will be placed under /sdcard/Download/fakeboot.img

Extra Arguments can be combined, there is no particular order.

Notes: rootAVD will
- always create .backup files of ramdisk*.img and kernel-ranchu
- replace both when done patching
- show a Menu, to choose the Magisk Version (Stable || Canary || Alpha), if the AVD is online
- make the choosen Magisk Version to its local
- install all APKs placed in the Apps folder
- use %LOCALAPPDATA%\Android\Sdk to search for AVD system images
```
### Linux & MacOS
```
Command Examples:
./rootAVD.sh
./rootAVD.sh ListAllAVDs
./rootAVD.sh InstallApps

./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img FAKEBOOTIMG
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img DEBUG PATCHFSTAB GetUSBHPmodZ
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img restore
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img InstallKernelModules
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img InstallPrebuiltKernelModules
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img InstallPrebuiltKernelModules GetUSBHPmodZ PATCHFSTAB DEBUG
./rootAVD.sh system-images/android-33/google_apis_playstore/x86_64/ramdisk.img AddRCscripts
```

### Windows
```
Command Examples:
rootAVD.bat
rootAVD.bat ListAllAVDs
rootAVD.bat InstallApps

rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img FAKEBOOTIMG
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img DEBUG PATCHFSTAB GetUSBHPmodZ
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img restore
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img InstallKernelModules
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img InstallPrebuiltKernelModules
rootAVD.bat system-images\android-33\google_apis_playstore\x86_64\ramdisk.img InstallPrebuiltKernelModules GetUSBHPmodZ PATCHFSTAB DEBUG
```

### ANDROID_HOME
* Default location can be overwritten by setting the `ANDROID_HOME` variable
* In both cases, the script will search in it for AVD system-images and adb binarys
* `ANDROID_HOME` Sets the path to the SDK installation directory -> [AOSP Variables reference](https://developer.android.com/tools/variables#envar)

### Compatibility Notes
* 64 Bit Only Systems needs Magisk 23.x
* API 28 (Pie) is **not supported** at all -> [because](https://source.android.com/devices/bootloader/partitions/system-as-root#sar-partitioning)
* Magisk Versions >= 26.x can only be proper installed with the FAKEBOOTIMG argument
	* due to the [New sepolicy.rule Implementation](https://github.com/topjohnwu/Magisk/releases/tag/v26.1)
* Android 14 needs Magisk Version >= 26.x to be rooted
* Android 17 needs Magisk Version >= 30.x to be rooted (bundled Magisk.zip updated to v30.7)
