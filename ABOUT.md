# About AudioRoom

AudioRoom sends sound from one of your devices to another — your phone's audio to your
PC, your PC's audio to your phone, or one phone to another — over your own network. No
account, no cloud, no pairing ritual. You press share on one side, join on the other, and
you hear it.

It was built to solve a small, stubborn problem: getting audio from a device that has it
to a device that can play it, when the two are sitting right next to each other and every
existing option wants an account, a subscription, or a trip through the internet.

- **Author:** idkc7 — [Instagram](https://instagram.com/idkc7) · [GitHub](https://github.com/idkc71)
- **Downloads:** [Releases](https://github.com/idkc71/AudioRoom/releases)
- **Source:** private. This repository hosts the builds, the update manifests and the docs.

---

## What it does

AudioRoom has two halves that can each be either side of a stream:

- **The Windows app** — a portable program. It can play audio it receives, and it can
  share the sound of any output device on the PC.
- **The Android app** — it can listen, and it can share what the phone plays (with
  Android's screen-capture consent, which is how Android exposes system audio).

Either half can host; either half can listen. There is no server and no relay: the audio
goes directly from one device to the other.

### Features

- Automatic room discovery on the local network — no IP addresses to type.
- Manual connection by address, for when discovery is blocked.
- USB-cable connection via phone USB tethering, for unreliable Wi-Fi.
- Bluetooth, for sending phone audio to the PC.
- Optional 4-digit PIN on a room. The PIN is verified with a challenge-response, so it is
  never sent over the network.
- In-app updates from GitHub, with SHA-256 verification before anything is installed.
- English UI, no account, no telemetry.

---

## How it works (the short version)

1. A host opens a room and starts capturing audio: 48 kHz, stereo, 16-bit.
2. It announces the room on the local network a few times a second — a small broadcast
   with the room code, the name, the address, the port, and whether a PIN is required.
3. A listener sees the room in its list, connects to the host's audio port, optionally
   answers the PIN challenge, and then receives a stream of audio frames.
4. The listener plays those frames on its own output device.

The two ports involved are **5590/TCP** (the audio stream) and **5591/UDP** (room
discovery). If a firewall blocks those, discovery and playback stop — that is the single
most common setup problem.

Audio is never written to disk, never stored, and never sent anywhere except to the
device you connected.

---

## Install

### Windows

1. Download `AudioRoom-win64.zip` from [Releases](https://github.com/idkc71/AudioRoom/releases).
2. Unzip it anywhere — it is portable, there is no installer.
3. Run `AudioRoom.exe`. Allow it through the firewall on **private** networks when
   Windows asks; without that, other devices cannot reach it.

Requirements: 64-bit Windows 10 version 2004 (build 19041) or newer. Settings live in
`%LOCALAPPDATA%\AudioRoom`.

### Android

1. Download `AudioRoom.apk` from [Releases](https://github.com/idkc71/AudioRoom/releases).
2. Open it and allow installing from your browser or file manager.
3. Launch AudioRoom. For in-app updates, allow AudioRoom to install unknown apps when
   Android asks (Android requires this for any app that updates itself).

Requirements: Android 7.0 (API 24) or newer.

> If you installed a pre-1.0.0 test build, uninstall it first. It was signed with a
> different key, so Android will refuse to install over it ("App not installed").

---

## Connecting

| Route | When to use it |
|---|---|
| **Same Wi-Fi** | Both devices on one network. Rooms appear automatically. |
| **USB cable** | Wi-Fi is slow or flaky. Turn on USB tethering on the phone; the PC sees a new network. |
| **Bluetooth** | Sending phone audio to the PC. |
| **Manual address** | Discovery is blocked (guest Wi-Fi, AP isolation). Type the host's address and port 5590. |

---

## Updating

Both apps check GitHub when they start. If a newer version exists, the Update card offers
a button; the download is verified against the SHA-256 published in the manifest, then
installed in place. Windows swaps the app folder and relaunches; Android hands the APK to
the system installer.

Nothing is checked or downloaded if the update check fails — the app keeps working, and
the card simply says so.

---

## Privacy

- No account, no analytics, no telemetry, no crash reporting.
- No recording. Audio exists only as it flows between your devices.
- The only outbound request is the update check to `raw.githubusercontent.com`.
- Stored locally: preferences, and a downloaded update until it is installed.

Details: [PRIVACY.md](PRIVACY.md).

---

## Why Windows or your phone may warn about this app

This is expected for a small, independently built app, and it is worth understanding
rather than clicking through blindly.

- **SmartScreen ("Windows protected your PC" / unknown publisher).** The app is not
  code-signed yet, so Windows cannot attribute it to a publisher. SmartScreen is a
  reputation system: a new binary with no download history is treated as unknown. Click
  **More info → Run anyway** if you trust where you got the file.
- **Antivirus heuristics.** AudioRoom captures audio, streams it over the network, and
  updates itself by replacing its own files. Legitimate software does this; so does
  malware. Combined with a lack of signing, an unsigned build can be flagged as a
  false positive. The way to be sure is to download only from the Releases page and
  compare the SHA-256 in `version.json` / `android-version.json` with the file you got.
- **Play Protect on Android.** It warns about apps installed outside the Play Store that
  it has not seen before. Choose "Install anyway" if you trust the source.

If your antivirus reports a specific detection name, that is a false positive worth
reporting to the vendor — it helps every future user.

---

## Troubleshooting

| Problem | Likely cause |
|---|---|
| A room never appears in the list | Different network, guest network with AP isolation, VPN on either device, or firewall blocking UDP 5591 |
| The room appears but audio does not start | Firewall blocking TCP 5590 on the host |
| "App not installed" on Android | An older AudioRoom signed with a different key is still installed — uninstall it first |
| Audio stutters on Wi-Fi | Weak signal or a busy network; USB tethering is the reliable fallback |
| Update check always fails | No network, or the release manifest has not reached the CDN yet (it can lag a few minutes) |
| Windows app does not start | Check that the ZIP was fully extracted (Windows' "preview" mode runs from inside the archive) |

---

## Versioning

`MAJOR.MINOR.PATCH`. The Android build number is `100` for 1.0.0 and increases by one per
release; both platforms share the same version name. Update manifests live in this
repository and point at the assets attached to each Release.

---

## Licence

Proprietary, all rights reserved — free to use, not free to redistribute or modify. See
[LICENSE](LICENSE) and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

---

© 2026 idkc7. All rights reserved.
