## EDO'S HACKINTOSH PROJECT

#### 0. INTRO

This guide is a log of the steps I've taken to put my system running OSX Catalina 10.15.3.

DISCLAIMER: *Follow this guide at your own responsibilty. I'm not responsible for any damage  you commit to your hardware*

If you try to just copy/paste the thing, you won't have any luck. On one hand there are personal files that need to be created from your target hardware. On the other hand each system has its own differences. So if you're trying to do this the fast way, my advice to you is just buy a new Mac from Apple.

I've followed the AMD OS X Vanilla Guide [here](https://vanilla.amd-osx.com)

I have no credits for this, besides this document that can help you a lot if your hardware is same as mine.

All credits to the guys at AMD OS X Vanilla Guide :heart

So the purpose here is to create an hackintosh from Catalina 10.15.3 with OpenCore bootloader.


#### 1. HARDWARE

List of the hardware I'm using:

    - Case Extended ATX NZXT Ht700i w/Black Window
    - Power Supply: NZXT E650 Digital 650W 80 PLUS Gold Full Modular Black
    - AMD Ryzen 9 3900X 12-Core 3.8 GHz Turbo 4.6GHz 70MB Socket AM4
    - CPU Water Cooler NZXT Kraken X52 v2
    - Motherboard ATX Gigabyte X570 Aorus Master
    - RAM GSkill Trident Z RGB (for AMD) 4x16GB DDR-4-3200MHz CL16 Black
    - Graphics Card RADEON RX5700 XT 8GB
    - 2x SSD M.2 2280 Samsung 970 Evo Plus 500GB
    - Razer Seiren X Condenser Streaming Microphone
    - Razer Kiyo FHD Webcam
    - Mouse Logitech G-903
    - Keyboard Ducky MECHA MINI 60%

#### 2. THINGS YOU'LL NEED

    - PEN DRIVE 16GB
    - XCode installed (in order to edif *.plist files)
    - A macOS Catalina 10.15.3 install (Install macOS Catalina.app)
        NOTE: download it directly from the App Store using your own account
    - iCloud account (usually people already have one)
    - lots of patience
    - lots of google-fu


#### 3. GATHERING RESOURCES

Things you'll need to download before starting:

    - OpenCore-0.5.5
    - GenSMBIOS
    - MasciASL-1.5.7
    - MountEFI
    - ProperTree
    - SSDTTime

    You can find it inside the RESOURCES folder of this repo.


#### 4. KEXTs (Kernel Extensions)

    - Lilu.kext
    - VirtualSMC.kext
    - WhateverGreen.kext
    - AppleALC.kext
    - NVMeFix.kext
    - SmallTreeIntel82576.kext
    - AppleMCEReporterDisabler.kext
    - BrcmBluetoothInjector.kext
    - BrcmFirmwareData.kext
    - BrcmPatchRAM3.kext
    - AirportBrcmFixup.kext

    You can find it in the KEXTS folder of this repo.


#### 5. INSTALLATION

Create an USB with Catalina 10.15.3 install (done with macOS)

    1. Download Catalina from App Store

    2. Format your USB with
       - Name: Hackintosh
       - Format: APFS
       - Scheme: GUID Partition Map

    3. Run the createinstallmedia command


    sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Hackintosh



This will take some time but in the end you'll have a simple Catalina USB installer.

Nothing new till now.


#### 6. LET THE GAMES BEGIN (YOUR SAMPLE EFI)

So the challenge starts now.

You need to mount your USB EFI partition using the MountEFI tool and copy there the OpenCore EFI Folder (EFI folder included)

No worries, let me guide you on this...

    - You'll find MountEFI in this repo's TOOLS folder
    - You'll find OpenCore-0.5.5-RELEASE.ZIP in this repo's OPENCORE-0.5.5 folder

Steps to do it:

    1. mount your USB EFI partition

        - unzip MountEFI.zip
        - cd into MountEFI directory
        - run ./MountEFI.command
        - choose your pen drive

![logo](/SCREENSHOTS/MountEFI001.png?raw=true "Mount EFI")


        - at this point you can close MountEFI with CMD+Q, you should already have EFI partition mounted in your finder

![logo](/SCREENSHOTS/MountEFI002.png?raw=true "Mounted EFI")


In your pen there won't be anything into the EFI partition. You'll find it empty.

In the image below you can see an example how it will be after all the tweaking needed.

#### 7. STANDARD EFI STRUCTURE

The first step is to copy the standard EFI folder, provided in the open core package, to your EFI partition on the pen drive.

In the /OPENCORE-0.5.5 of this repo unzip the OpenCore-0.5.5-RELEASE.zip file.

![Standard EFI Folder](/SCREENSHOTS/StandardEFIFolder.png?raw=true "Standard EFI Folder")

Just copy the complete folder, including the EFI parent partition into your EFI partition on the pen drive.





#### 10. PREPARE YOUR BIOS FOR HACKINTOSH (GIGABTE AORUS MASTER)

    1. UPDATE BIOS TO THE LATEST ONE (in my case, F11 BIOS version)

    2. LOAD OPTIMIZED DEFAULT


    BIOS

    4. FAST BOOT: disabled

    5. CSM Support: disabled

    6. LAN PXE Boot Option ROM: set to disabled

    7. Storage Boot Option Control: UEFI

    8. Other PCI Devices: UEFI

    PERIPHERALS

    9. Security Device Support: Disabled

    10. USB Configuration

        - XHCI Hand-off: Enabled

        - Legacy USB Support: Enabled

    11. Network Stack: Disabled

    CHIPSET

    12. VT-d: Enabled

    POWER

    13. Platform Power Management: Disabled
