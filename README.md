## EDO'S HACKINTOSH PROJECT

![about image](/SCREENSHOTS/about.png?raw=true "System About Image")

#### 0. INTRO

This guide is a log of the steps I've taken to put my system running OSX Catalina 10.15.3.

DISCLAIMER: _Follow this guide at your own responsibilty. I'm not responsible for any damage you commit to your hardware_

If you try to just copy/paste the thing, you won't have any luck. On one hand there are personal files that need to be created from your target hardware. On the other hand each system has its own differences. So if you're trying to do this the fast way, my advice to you is just buy a new Mac from Apple.

I've followed the AMD OS X Vanilla Guide [here](https://vanilla.amd-osx.com)

I have no credits for this, besides this document that can help you a lot if your hardware is same as mine.

All credits to the guys at AMD OS X Vanilla Guide :heart

So the purpose here is to create an hackintosh from Catalina 10.15.3 with OpenCore bootloader.

**TL;DR Benchmarks**

[Geekbench 5 CPU](/BENCHMARKS/Geekbench_5_CPU.png) : scores are okay according to Benchmarks Charts (expected)

[Geekbench 5 Compute OpenCL/Metal](/BENCHMARKS/Geekbench_5_Compute.png) : scores are okay according to Benchmarks Charts, altough some Radeon RX 5700 XT behave way up above the average values

**TL;DR What Works**

    - icloud
    - time machine
    - location services
    - airdrop
    - bluetooth (w/fenvi t919)
    - wifi (w/fenvi t919)
    - sleep
    - sound
    - video conference (webcam, sound, screen sharing)

    - need to try sidecar

**TL;DR Shenaningans**

    1. [NOT SOLVED YET] Random system stuttering/lag from time to time. It's rare but it happens. For 1-3 secs everything stops and then gets back to normality.

    2. [SOLVED] Ducky Mecha Mini USB Keyboard sometimes blocks and only one RGB line is lit. After reading a bit this is Ducky's problem and can be solved with firmware. To get it back to life I just unplug/plug the cable and everything goes fine for hours.

    UPDATE 29th Feb: to solve the ducky mecha mini issue follow these steps:
        1. press Fn + Alt + P for 3 sec.
        2. keyboard lit scheme will change
        3. type 05AC024F (changes the VID and PID which Apple authorised)
        4. restart the keyboard by unplug and plug

        (after 24h of testing I'm assuming this problem is solved)

    3. [HAVE TO LIVE WITH] Although VT-D is enabled in BIOS, OSX doesn't recognizes it.

    NOTE: Parallels and VirtualBox work but Docker Desktop not. So if you're a Docker user prepare to use Docker Machine.

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
    - fenvi t919 (wifi + bluetooth PCIExpr Card), -> bought specifically to solve BT+WiFi problem
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

If you hardware is not exactly like mine please read the guide mentioned above in the section related to [Gathering Files](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/ktext)

#### 5. CREATE AN INSTALL USB PEN DRIVE

Create an USB with Catalina 10.15.3 install (done with macOS)

    1. Download Catalina from App Store

    2. Format your USB with
       - Name: Hackintosh
        - Format: Mac OS Extended(journaled)
       - Scheme: GUID Partition Map

    3. Run the createinstallmedia command


    sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Hackintosh

This will take some time but in the end you'll have a simple Catalina USB installer.

Nothing new till now.

#### 6. LET THE GAMES BEGIN (YOUR EFI)

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

In the image above you can see an example how it will be after all the tweaking needed.

#### 7. EFI FOLDER STRUCTURE

The first step is to copy the EFI Folder that I've created with the help of the tutorials found on the AMD OSX Vanilla Guide.

If your hardware is not similar to mine, please read the AMD OSX Vanilla Guide and check the reason of all the files and tweaks being there.

Tweak it according to your spec.

You can find my EFI in the repo's /EFI folder.

![EFI Folder](/SCREENSHOTS/EFIFolder.png?raw=true "Prepared EFI Folder")

Just copy my EFI prepared folder into your empty EFI partition (EFI parent folder included)

#### 8. ACPI

If your hardware is exactly mine you can use the files inside EFI/OC/ACPI. Otherwise you need to do your own files.

To do that follow the Getting started with ACPI guide provided by [Opencore Vanilla Guide](khronokernel.github.io/Getting-Started-With-ACPI/)

NOTE: Pay attention to this step because you can brick your machine with this...

If you need to create your specific ACPI files you'll find the SSDTime helper inside my Tools Folder.

Just unzip it in the target machine desktop (the one where you intend to install your macOS) and run it in a Windows installation, executing the SSDTime.bat as admin.

You can also run it on linux. More info on that in the last link provided. Just read it carefully and in doubt check other EFI folders or ask in reddit/forums about it.

#### 9. config.plist

Once again, if your hardware is exactly like mine, you can use the config.plist file provided in my EFI folder.

If it is not, please check the Opencore Vanilla Guide.

Even if it's equal to mine you'll need to run GenSMBIOS application to creating a MacSerial for your fake new mac.

To do that use the GenSMBIOS.zip in this repo's TOOLS folder and follow the steps in the Platforminfo section of this page [amd-config](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/amd-config.plist/amd-config)

With the fake data you get from GenSMBIOS you should open config.plist and fulfill it with this new info.

NOTE: I've removed my personal GenSMBIOS info from config.plist file.

![config.plist PlatformInfo](/SCREENSHOTS/PlatformInfo.png?raw=true "Platform Info")

#### 10. PREPARE YOUR BIOS FOR HACKINTOSH (GIGABTE AORUS MASTER)

These are the tweaks I've made to my motherboard BIOS.

**TL;DR If you have a Gigabyte Aorus Master X570 Mobo just do this flash BIOS to F11 version and use the profile I've prepared that's available in this repo**

[BIOS FIRMWARE F11](/BIOS_FIRMWARE_UPDATE/mb_bios_x570-aorus-master_f11.zip)

[Aorus Master X570 Saved Profile](/BIOS_PROFILE/MB_SETTINGS_PROFILE)

NOTE: this file already contains all the BIOS tweaks needed to run your hackintosh without problems.

    Tweaks made:

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

#### 11. LET'S BRICK IT OR OWN IT

At this point your pen drive and your system are ready to install macOS.

Boot from the UEFI pendrive option and install macOS as if it was an Apple Machine.

In the end you'll have to boot again from the Pendrive, this time selecting the macOS Installer option and continue with the install.

At the third boot, boot again from the Pendrive and you have your macOS boot entry to start the post-install tweaks on your system.

Test your system, check if it doesn't over heat, check if it seems graphically stable, install some programs, take a look around.

I wish you the best luck with this. :heart

For post install things check the POST INSTALL section of the [Opencore Vanilla Desktop Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/)

#### 12. SHENANIGANS

1. Random rare 1-3 sec. stuttering (not sure why, but system is okay). Trying to solve this.

**If you know anything about it please get in touch!**
