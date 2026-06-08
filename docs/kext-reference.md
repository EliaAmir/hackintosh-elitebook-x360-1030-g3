# Kext Reference — HP EliteBook x360 1030 G3

All kexts documented here are read directly from the EFI/OC/Kexts/ folder. No kext names are assumed or invented.

---

## Core Kexts

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **Lilu.kext** | Kernel patching framework | Required by virtually every other kext — AppleALC, WhateverGreen, CPUFriend, etc. all depend on Lilu at runtime |
| **VirtualSMC.kext** | SMC emulation | macOS requires an Apple SMC chip to boot. VirtualSMC emulates it in software. Without this, macOS refuses to start. |

---

## Graphics

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **WhateverGreen.kext** | Intel/AMD/NVIDIA GPU patching | Required for Intel UHD 620 framebuffer patching, display output, and resolution on Whiskey Lake hardware |

---

## Audio

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **AppleALC.kext** | Audio codec patching | Enables speakers and microphone via AppleHDA patching. Requires Lilu. |

---

## Input Devices

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **VoodooPS2Controller.kext** | PS/2 keyboard and trackpad | Drives the built-in keyboard and trackpad via PS/2 protocol. Includes sub-plugins: VoodooPS2Keyboard, VoodooPS2Mouse, VoodooPS2Trackpad, VoodooInput |
| **VoodooI2C.kext** | I2C HID controller | Required for the touchscreen. The EliteBook x360's touchscreen uses an I2C bus. Includes sub-plugins: VoodooGPIO, VoodooI2CServices, VoodooInput |
| **VoodooI2CHID.kext** | I2C HID satellite | Works with VoodooI2C to handle the HID protocol layer for I2C touchscreen devices |
| **VoodooI2CSynaptics.kext** | Synaptics I2C trackpad | Provides Synaptics-specific I2C protocol support for the trackpad |
| **BrightnessKeys.kext** | Keyboard brightness control | Maps the HP function key brightness shortcuts (F2/F3) to macOS brightness control. Without this, brightness keys do nothing. |

---

## Battery and Power

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **SMCBatteryManager.kext** | Battery percentage and status | Reports accurate battery level and charge state to macOS. Part of the VirtualSMC family. |
| **SMCProcessor.kext** | CPU temperature sensors | Reports CPU temperature data via SMC. Part of the VirtualSMC family. |
| **SMCLightSensor.kext** | Ambient light sensor | Enables the ambient light sensor (ALS) for automatic display brightness. Part of the VirtualSMC family. |
| **SMCSuperIO.kext** | SuperIO chip sensors | Reports fan speed and other SuperIO sensor data. Part of the VirtualSMC family. |
| **ECEnabler.kext** | Embedded Controller battery fix | Required for accurate battery percentage on HP laptops. Without it, the battery status is absent or stuck. |
| **CPUFriend.kext** | CPU frequency injection | Allows injecting custom CPU power management data. Required to properly tune power management for this Intel mobile CPU. |
| **CPUFriendDataProvider.kext** | CPUFriend data for this CPU | Contains the specific frequency vectors and power profile for this machine's CPU. Works together with CPUFriend. |
| **RTCMemoryFixup.kext** | RTC BIOS error fix | Prevents macOS from writing to RTC memory regions that HP's BIOS has marked reserved. Without this, every boot shows a CMOS error and sleep/wake is broken. |

---

## USB

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **USBPorts.kext** | USB port mapping | Custom USB port map for this specific laptop. macOS limits the number of active USB ports; this kext declares the correct ports and their types, enabling all physical ports to function. |

---

## WiFi

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **AirportItlwm.kext** | Intel WiFi driver | Drives the Intel Wireless-AC 8265 card. Provides native macOS WiFi integration via the AirportItlwm interface. |

---

## Bluetooth (Partially Working)

| Kext | Purpose | Why Included |
|------|---------|-------------|
| **IntelBluetoothFirmware.kext** | Intel Bluetooth firmware upload | Uploads firmware to the Intel Bluetooth chipset on macOS boot |
| **IntelBTPatcher.kext** | Intel Bluetooth protocol patch | Patches the Bluetooth stack for Intel hardware compatibility |
| **BlueToolFixup.kext** | Bluetooth stack fix (macOS 12+) | Required for Intel Bluetooth on Monterey and later. Patches the macOS Bluetooth framework. |

> Note: Bluetooth is not working on this hardware revision despite all three kexts being present. The kexts are retained for future fix attempts.
