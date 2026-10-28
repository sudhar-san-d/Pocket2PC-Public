# Pocket-to-PC

Pocket-to-PC turns your Android phone into an encrypted local companion and remote control for your Windows PC over Wi-Fi.

Enjoy smooth touchpad navigation, live screen preview, high-speed local file sharing, keyboard input, and one-tap everyday workflows—all communicating strictly over your local network using mutual TLS (mTLS) with zero cloud relays.

---

## Features

- **Touchpad & Remote Keyboard**: Seamless mouse control with multi-touch gestures, live typing, and dedicated media keys.
- **Live Screen Preview**: Low-latency screen mirror right on your phone with Direct Touch controls.
- **High-Speed File Sharing**: Transfer photos, videos, documents, and clipboard snippets directly between phone and PC with one tap.
- **One-Tap Workflows**: Convenient quick actions like locking your PC, turning off monitors, adjusting volume/brightness, and opening favorite sites.
- **System Telemetry**: Check PC CPU, RAM, and battery levels at a glance.
- **Private & Cloud-Free**: 100% peer-to-peer over your local Wi-Fi. No account required, no third-party servers, and no data collection.

---

## Requirements

- **Windows PC**: Windows 10 or Windows 11 (64-bit).
- **Android Phone**: Android 8.0 (Oreo) or newer.
- **Network**: Both devices connected to the same local Wi-Fi / LAN network.

---

## Installation

### Windows Installation
1. Go to the [Releases](https://github.com/sudhar-san-d/Pocket2PC-Public/releases) page.
2. Download the latest installer: PocketToPC-Setup-2.0.1.exe.
3. Run the installer and follow the setup wizard.
4. **SmartScreen Notice**: Because this installer is open-source and not signed with an expensive enterprise certificate, Windows SmartScreen may show a warning ("Windows protected your PC"). Click **More info** and then **Run anyway** to proceed.

### Android Installation
1. Go to the [Releases](https://github.com/sudhar-san-d/Pocket2PC-Public/releases) page on your phone.
2. Download the latest Android package: PocketToPC-2.0.1.apk.
3. Open the downloaded APK. If prompted, enable **Install unknown apps** for your browser or file manager, then tap **Install**.

---

## How Pairing Works

Pocket-to-PC connects devices securely using hardware-backed mutual TLS (mTLS) certificates:

1. **Start the PC App**: Launch Pocket-to-PC on Windows. A QR code pairing dialog will appear (or click the tray icon and select **Show Pairing QR**).
2. **Scan from Phone**: Open Pocket-to-PC on your phone and tap **Scan QR Code**.
3. **Approve Pairing**: Point your phone camera at the QR code on your PC screen. A prompt will appear on your PC asking to authorize the device.
4. **Connected!**: Once approved, your phone and PC establish an encrypted link and reconnect automatically whenever they are on the same Wi-Fi.

---

## Verification (Checksums)

You can verify the integrity of downloaded release assets using SHA256SUMS.txt:

`powershell
# Windows PowerShell
Get-FileHash PocketToPC-Setup-2.0.1.exe -Algorithm SHA256
Get-FileHash PocketToPC-2.0.1.apk -Algorithm SHA256
`

Compare the output hash against the corresponding hash in SHA256SUMS.txt.

---

## Reporting Issues

If you run into issues or have questions, please report them on GitHub:
👉 [https://github.com/sudhar-san-d/Pocket2PC-Public/issues](https://github.com/sudhar-san-d/Pocket2PC-Public/issues)

---

## License

All rights reserved. Free to use, no redistribution, no modification, no reverse engineering.  
Credit: Sudharsan
