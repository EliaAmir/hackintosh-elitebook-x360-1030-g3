# Build Notes — HP EliteBook x360 1030 G3 Hackintosh

## EFI Creation Process

The EFI was built following Dortania's OpenCore Install Guide from the beginning — not adapted from an existing EFI for a different machine. Starting from scratch ensures you understand every component in your config and can diagnose problems when they occur.

The process:
1. Download the OpenCore package from the official release
2. Follow the Dortania guide for Coffee Lake (and Whiskey Lake, which shares the same process)
3. Add kexts one at a time, testing at each step
4. Build ACPI SSDTs using SSDTTime for the specific machine
5. Configure config.plist according to the Dortania checklist
6. Test, fix, iterate

---

## Kext Selection Methodology

Kexts were added individually, not as a bundle. The method:

1. Start with the absolute minimum required to boot: Lilu + VirtualSMC + WhateverGreen + AppleALC
2. Boot. If it fails, diagnose before adding more.
3. Add one kext at a time, boot, and verify the feature it enables actually works.
4. Document what each kext is doing before moving to the next.

This means you can isolate which kext causes a problem. If you add five kexts at once and something breaks, you have no way to know which one is responsible.

---

## RTC BIOS Error Diagnosis and Patch

**Symptom:** On every boot, the HP BIOS displayed an "CMOS checksum error" or "Real-Time Clock Error" message, and sleep/wake was broken — the machine would kernel panic or hang on wake.

**Root cause:** macOS writes to RTC memory regions that HP's BIOS firmware has marked as reserved. When macOS writes to these regions, the BIOS detects the checksum mismatch on next boot and throws an error.

**Solution:** RTCMemoryFixup.kext with custom exclusion masks in boot-args. This tells macOS to avoid writing to the specific RTC memory offsets that HP's BIOS protects.

The specific offsets were identified using the `rtcfx_exclude` boot argument in a diagnostic mode, then a fixed exclusion range was set once the problematic range was confirmed.

---

## ACPI Patching

The following custom SSDTs were required for this machine:

| SSDT | Purpose |
|------|---------|
| SSDT-AWAC | Disables the AWAC clock system and enables the legacy RTC, required for all 300-series chipsets and newer |
| SSDT-EC-USBX-LAPTOP | Creates a fake Embedded Controller device for macOS and sets USB power properties appropriate for a laptop |
| SSDT-PLUG-DRTNIA | Injects the plugin-type property into the CPU device, enabling Apple's CPU power management (XCPM) |
| SSDT-PNLF | Enables display backlight control — required for brightness keys and smooth dimming |
| SSDT-XOSI | Patches the _OSI method to report Windows compatibility, required for touchscreen and some I2C devices that query _OSI before initializing |

All SSDTs were generated using SSDTTime on the actual machine.

---

## Key Lessons

**On touchscreen:** The touchscreen requires both VoodooI2C + VoodooI2CHID (for the HID protocol) + VoodooI2CSynaptics (for the Synaptics controller). It also requires SSDT-XOSI to report Windows compatibility — without it, the I2C controller doesn't initialize under macOS.

**On Bluetooth:** Intel Bluetooth on this generation is partially supported. IntelBluetoothFirmware + IntelBTPatcher + BlueToolFixup are all present in this EFI, but Bluetooth remains non-functional on this specific hardware revision. The kexts are kept in the EFI as placeholders for future fixes.

**On CPU power management:** CPUFriend + CPUFriendDataProvider are required to inject the correct frequency vectors for this CPU. Without them, macOS uses default Apple power management curves that are not optimal for this Intel mobile chip.

**On battery:** ECEnabler is essential for accurate battery percentage on HP laptops. Without it, the battery status shows as absent or stuck at a fixed percentage.
