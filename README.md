## EDO'S HACKINTOSH PROJECT

#### 0. NOTAS:

    - fazer SSDTS
    - kernel patches ryzen/threadripper 17h

#### 0. INTRO

    - This guide is a log of the steps I've taken to put my system running OSX Catalina 10.15.3.

    - DISCLAIMER: Follow this guide at your own responsibilty. I'm not responsible for any damage
    you commit to your hardware


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


#### 2. GATHERING RESOURCES

    I've followed the AMD OS X Vanilla Guid [read it here](https://vanilla.amd-osx.com)

    Things you'll need to download before starting:
        - gibMacOS, an awesome tool from CorpNewt [LINK](https://github.com/corpnewt/gibMacOS)
        - The Vanilla AMD config courtesy of AlGrey [LINK](https://github.com/AMD-OSX/AMD_Vanilla)
        - A USB Drive (16GB)
        - Some patience... lots of it... more of it....

        Keep calm, with this quick guide you'll do it almost without struggle :heart

#### 3. INSTALLATION

    Requires Windows 10

    1. Extract gibMacOS and cd into its folder

    2. Run gibMacOS.bat as Admin

    It will download and install some requirements.
    You'll need Python to do this, if you don't have it and the script does not propmpt you to install it, please install Python 3.

    3. Next, choose the macOS version to download: I've chosen macOS Catailna 10.15.3

    4. Choose R to Toggle Recovery-Only to ON

    5. It will download and prepare some files


#### 4. CREATING MACOS INSTALL USB AND BOOTLOADER SETUP

    1. Run MakeInstall as Admin from within the gibMacOS folder.

    2. Select your USB drive number. If you choose to use OpenCore add an O to the number: i.e. 4O
    I've chosen OpenCore as it is advised from OSX 10.15.2 onwards.

#### 5. KEXT


#### 10. PREPARE YOUR BIOS FOR HACKINTOSH (GIGABTE AORUS MASTER)


    1. UPDATE BIOS TO THE LATEST ONE (not always the best, but in most cases it is...)

    2. LOAD OPTIMIZED DEFAULT

    OC Features

    3. EXTREME MEMORY PROFILE (X.M.P): set it to Profile 1

    BIOS

    4. FAST BOOT: disabled

    5. Windows 8/10 Features: set to Windows 8/10

    6. CSM Support: Enabled, if you have problems disable it

    7. LAN PXE Boot Option ROM: set to disabled

    8. Storage Boot Option Control: UEFI

    9. Other PCI Devices: UEFI

    PERIPHERALS

    10. PCIe 1 Slot

    11. Security Device Support: Disabled

    12. USB Configuration

        - XHCI Hando-off: Enabled

        - Legacy USB Support: Enabled

    13. Network Stack: Disabled

    14. SATA Mode: AHCI

    CHIPSET

    15. VT-d: Enabled

    16. Internal Graphics: Enabled (even if you have dedicated GPU)

    17. IOAPIC 24-119 Entries: Enabled

    POWER

    18. Platform Power Management: Disabled
