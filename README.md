# Hackintosh — HP EliteBook x360 1030 G3

**Status: Stable Daily Driver**

macOS Ventura on the HP EliteBook x360 1030 G3. Built from scratch using Dortania's OpenCore documentation. Fully stable — this is the machine this EFI was made on.

---

## Hardware

| Component | Details |
|-----------|---------|
| Device | HP EliteBook x360 1030 G3 |
| CPU | Intel Core i5/i7 8th Gen (Whiskey Lake) |
| GPU | Intel UHD 620 (integrated) |
| Display | 13.3" IPS touchscreen, 120Hz |
| RAM | 16 GB LPDDR3 |
| Storage | NVMe SSD |
| WiFi | Intel Wireless-AC 8265 |
| Bluetooth | Intel Bluetooth (same card) |
| Bootloader | OpenCore |
| macOS | Ventura (13.x) |

---

## What Works

| Feature | Status |
|---------|--------|
| Graphics (resolution + brightness) | Working |
| 120Hz refresh rate | Working |
| HDMI output | Working |
| Touchscreen | Working |
| Keyboard | Working |
| Touchpad | Working |
| WiFi | Working |
| Thunderbolt | Working |
| iMessage / iCloud | Working |
| Battery management | Working |
| Audio (speakers + microphone) | Working |
| CPU power management | Working |
| Tablet mode (bending beyond 180°) | Not working |

## What Does Not Work

| Feature | Status |
|---------|--------|
| Bluetooth | Not working |

---

## Build Story

This EFI was built from scratch using Dortania's OpenCore documentation. Kexts were selected and tested individually — not pulled from someone else's EFI. The process involved overcoming RTC BIOS errors that prevented sleep/wake from working correctly, hardware incompatibilities that required custom ACPI patches, and several system-level bugs that took significant debugging to isolate.

The result is a machine that runs macOS Ventura as a full daily driver. It boots cleanly, sleeps and wakes reliably, and handles a full workload.

---

## EFI Structure

```
EFI/
├── BOOT/
│   └── BOOTx64.efi
└── OC/
    ├── ACPI/          — custom ACPI patches (SSDT files)
    ├── Drivers/       — OpenCore drivers
    ├── Kexts/         — hardware enablement kexts
    ├── Resources/     — OpenCore UI resources
    ├── Tools/         — EFI shell tools
    ├── config.plist   — main OpenCore configuration
    └── OpenCore.efi
```

See [docs/kext-reference.md](docs/kext-reference.md) for a full table of every kext, what it does, and why it is included.

---

## Docs

- [docs/build-notes.md](docs/build-notes.md) — EFI creation process, kext selection methodology, RTC error diagnosis
- [docs/kext-reference.md](docs/kext-reference.md) — every kext in this EFI with purpose and reasoning

---

## Disclaimer

This EFI is provided as-is. It was built for a specific machine. Your HP EliteBook x360 1030 G3 may have different hardware revisions (different WiFi card, different SSD, different display panel). Always test on your hardware before using in production. Read the Dortania guide to understand what each component does before deploying.
