# AudioRoom

Share sound between your own devices — no account, no cloud, no pairing dance.

AudioRoom streams audio from one device to another on your own network: phone to PC,
PC to phone, or phone to phone. Audio never leaves the devices you connect.

- **Author:** idkc7
- **Instagram:** [@idkc7](https://instagram.com/idkc7)
- **GitHub:** [github.com/idkc71](https://github.com/idkc71)
- **Downloads:** [Releases](https://github.com/idkc71/AudioRoom/releases)

A longer explanation of how AudioRoom works, why antivirus tools may warn about it, and
what to check when a room does not appear: [ABOUT.md](ABOUT.md).

## Three ways to connect

| Route | Use it when |
|---|---|
| **Same Wi-Fi** | Both devices are on the same network. Rooms are discovered automatically. |
| **USB cable** | Wi-Fi is slow or unreliable. Turn on USB tethering on the phone; the PC finds it. |
| **Bluetooth** | Sending audio from a phone to the PC. |

## Install — Windows

1. Download `AudioRoom-win64.zip` from [Releases](https://github.com/idkc71/AudioRoom/releases).
2. Unzip it anywhere you like — it is portable, there is no installer.
3. Run `AudioRoom.exe`. Windows may ask to allow it through the firewall; allow it on
   private networks so other devices can reach it.

To remove it, delete the folder. Settings live in `%LOCALAPPDATA%\AudioRoom`.

## Install — Android

1. Download `AudioRoom.apk` from [Releases](https://github.com/idkc71/AudioRoom/releases).
2. Open it and allow installing from your browser or file manager when Android asks.
3. Launch AudioRoom.

In-app updates on Android need the "install unknown apps" permission for AudioRoom
itself — Android asks the first time the app downloads an update.

## Updating

Both apps check GitHub for a newer version when they start. When one is available the
Update card offers a button; nothing else to do.

## Requirements

- Windows 10 version 2004 (build 19041) or newer, 64-bit.
- Android 7.0 (API 24) or newer.
- Both devices on the same network (or joined by a USB cable / Bluetooth).

## Privacy

No account, no analytics, no telemetry, no recording, no upload. The only outbound
request is the update check to `raw.githubusercontent.com`. Details in
[PRIVACY.md](PRIVACY.md).

## License

Proprietary, all rights reserved. See [LICENSE](LICENSE) and
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

---

© 2026 idkc7. All rights reserved.
