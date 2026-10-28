# Updating AudioRoom

This folder is what lives at the root of the public repository `idkc71/AudioRoom`.
This file is the maintainer's guide: how to cut a version, publish it, and what users
see when it lands.

## 1. What each file is

| File | Purpose |
|---|---|
| `README.md` | The public front page. |
| `ABOUT.md` | The long explanation: how it works, ports, troubleshooting, warnings. |
| `UPDATING.md` | This guide. |
| `LICENSE` | Proprietary licence terms. |
| `PRIVACY.md` | What the app stores and sends. |
| `THIRD-PARTY-NOTICES.md` | Licences of bundled components. |
| `android-version.json` | The Android update manifest the app fetches. |
| `version.json` | The Windows update manifest the app fetches. |
| `.gitignore` | Keeps `dist/`, binaries and signing material out of the repo. |

The two manifests are the only files the apps read at runtime. Everything else is
documentation. The installers themselves are **not** in this folder in the repository:
they are attached to a GitHub Release, and the manifests point at them by absolute URL.

## 2. Build and bump a version

From the repository root of the private work tree:

```powershell
.\version.ps1 -Code 101 -Name 1.0.1
```

That updates the version in every place at once: the Android manifest, the Android
`Tuning.java`, `AppInfo.cs` for Windows, `version.json` and `android-version.json`.
Never hand-edit a version number — if the five places disagree, the updater will
either miss an update or offer the same one forever.

Then build the release artifacts and refresh the manifests:

```powershell
.\publish-release.ps1
```

`publish-release.ps1` builds the release-signed APK, builds the portable Windows ZIP,
hashes both, writes the two manifests in `release/` with absolute, tag-pinned URLs, and
copies both artifacts into `release/dist/`. If the GitHub CLI is installed it also
creates the Release; otherwise it prints the manual steps.

Two flags are useful while iterating: `-SkipWindows` builds only Android, and
`-App <path>` points at the Windows project if it is not in its usual place.

## 3. Publish on GitHub without the CLI

1. Create a repository named `AudioRoom` under `idkc71`, public.
2. Upload the files in this folder (not `dist/`) to the default branch (`main`).
   `README.md`, the two JSON manifests and the four documents.
3. Go to **Releases → Draft a new release**.
4. Tag: `v1.0.1` — the tag must match `versionName` exactly, because the manifest URLs
   embed it.
5. Title: `AudioRoom 1.0.1`.
6. Attach `AudioRoom.apk` and `AudioRoom-win64.zip` from `release/dist/`.
7. Publish.

**Order matters in one direction:** attach the assets first, then commit the manifests.
A manifest that points at a Release without its asset makes every user see a failed
download.

## 4. Publish with the GitHub CLI

If `gh` is installed and authenticated (`gh auth login`):

```powershell
gh release create v1.0.1 `
  "release/dist/AudioRoom.apk" `
  "release/dist/AudioRoom-win64.zip" `
  --title "AudioRoom 1.0.1" `
  --notes "What changed in this version."
```

`publish-release.ps1` does this step for you when `gh` is on `PATH`. It does not commit
or upload the manifests — that is still a deliberate act:

```powershell
git add release/README.md release/UPDATING.md release/LICENSE release/PRIVACY.md `
        release/THIRD-PARTY-NOTICES.md release/version.json release/android-version.json
git commit -m "release: 1.0.1"
git push
```

## 5. What users see

The app checks `version.json` (Windows) or `android-version.json` (Android) on launch.
If the manifest's `versionCode` is higher than the installed build, the Update card
shows the new version and an Update button. Pressing it downloads the asset, verifies
its SHA-256 against the manifest, and installs:

- **Windows:** `AudioRoom.Update.exe` swaps the app folder and relaunches.
- **Android:** the system installer takes over from the app's content provider.

If the check fails — offline, GitHub down, repository not created yet — the card shows
a short failure line. Nothing crashes and nothing blocks the rest of the app.

## 6. The one-time Android move

Phones that still carry a pre-1.0.0 test build cannot update in place: the old build is
signed with Android's throwaway debug key, so the installer refuses the new APK. Those
devices need the old app uninstalled and `AudioRoom.apk` installed once by hand. After
that, in-app updates work forever because the identity no longer changes.

The same applies to the author's own phones. Everyone else installs 1.0.0 fresh and
never sees this.

## 7. Two gotchas

- **CDN caching.** `raw.githubusercontent.com` caches for a few minutes, so a freshly
  pushed manifest can lag behind. If a user says "no update yet", wait five minutes
  before investigating.
- **Asset before manifest.** Publishing a manifest that points at a missing asset
  produces a download failure in every client. Upload first, commit second.

## 8. Worked example: shipping 1.0.1

```powershell
# 1. Bump everywhere.
.\version.ps1 -Code 101 -Name 1.0.1

# 2. Build, hash, write manifests, copy artifacts to release\dist.
.\publish-release.ps1

# 3. Create the Release and attach both assets.
gh release create v1.0.1 `
  "release/dist/AudioRoom.apk" `
  "release/dist/AudioRoom-win64.zip" `
  --title "AudioRoom 1.0.1" `
  --notes "Fixed the thing that needed fixing."

# 4. Commit and push the manifests last.
git add release/version.json release/android-version.json
git commit -m "release: 1.0.1"
git push
```

That is the whole cycle. Users on 1.0.0 get the update card on their next launch.

---

© 2026 idkc7. All rights reserved.
