# Privacy

AudioRoom is built to need nothing from you and to send nothing about you.

## No account, no analytics, no telemetry

There is no sign-up, no login, no user profile, and no identifier sent anywhere.
AudioRoom does not contain an analytics or crash-reporting SDK.

## Audio is never recorded or uploaded

Your sound is captured, sent directly to the device you connect, and played. It is
never written to a file, never stored, and never sent to any server. Audio travels only
between your own devices — over your local network, a USB cable, or Bluetooth.

## The only outbound request

When the app starts it checks for a newer version by fetching a small JSON file from
`raw.githubusercontent.com`. That request carries nothing but the download itself: no
device name, no room name, no PIN, no audio. If you block it, the app works normally and
only the update notice is missing.

## What is stored, and where

| What | Where |
|---|---|
| Preferences (last device, auto-start, whether the welcome was shown) | `%LOCALAPPDATA%\AudioRoom\settings.json` on Windows; app-private storage on Android |
| A downloaded update, until it is installed | The system temp folder (Windows) or app-private cache (Android) |
| Room PIN | Never stored by the peer; only used on the device that hosts the room |

Nothing in that list leaves your device.

## Discovery

AudioRoom announces its rooms with a UDP broadcast on the local network so other
devices can find them. The announcement carries the room code, room name, a local IP
address, the ports in use, and whether the room needs a PIN. The PIN itself is never
broadcast. These broadcasts do not leave your local network.

## Children

AudioRoom has no accounts and collects nothing, so there is no personal data to manage.

## Contact

Questions about privacy: idkc7 on Instagram ([@idkc7](https://instagram.com/idkc7)) or
[github.com/idkc71](https://github.com/idkc71).

---

© 2026 idkc7. All rights reserved.
