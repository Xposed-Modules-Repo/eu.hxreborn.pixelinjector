# PixelInjector

[![Release](https://img.shields.io/github/v/release/hxreborn/PixelInjector?style=flat-square)](https://github.com/hxreborn/PixelInjector/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/hxreborn/PixelInjector/total?style=flat-square)](https://github.com/hxreborn/PixelInjector/releases)

Opinionated Xposed module that bundles the Pixel tweaks I would otherwise need twenty modules for. I built it for my own phone, but if a tweak is useful to you and breaks on your build, I can adapt or fix it on demand.

## Screenshots

<table>
  <tr>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/dashboard.webp" width="240" alt="Dashboard"></td>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/settings.webp" width="240" alt="Settings"></td>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/reports.webp" width="240" alt="Your reports"></td>
  </tr>
  <tr>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/report.webp" width="240" alt="Report a bug"></td>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/log.webp" width="240" alt="Module log"></td>
    <td><img src="https://raw.githubusercontent.com/hxreborn/PixelInjector/main/assets/screenshots/reload.webp" width="240" alt="Hot reload"></td>
  </tr>
</table>

## Install

Needs Android 14 or newer, arm64 and an Xposed framework with libxposed API 102 (LSPosed 2.x or Vector).

1. Download the APK from [Releases](https://github.com/hxreborn/PixelInjector/releases/latest) or the [Xposed Modules Repo](https://github.com/Xposed-Modules-Repo/eu.hxreborn.pixelinjector/releases).
2. Enable PixelInjector in your Xposed manager. The scope comes preset.
3. Reboot once. Later updates install from the app and reload without a reboot.

## Tweaks

### System UI

- Warm Tiles: raises the limit of [3](https://android.googlesource.com/platform/frameworks/base/+/d5a204f16e7c71ffdbc6c8307a4134dcc1efd60d/packages/SystemUI/src/com/android/systemui/qs/external/TileServices.java#37) bound tiles in Quick Settings. Same hook as [qs-boundless-tiles](https://github.com/hxreborn/qs-boundless-tiles).
- PillShot: draws the app name on screenshots. Skips the selected apps or stamps only them. Revived from eXtreames/PillShot after its repo disappeared.
- Mute screenshot sound: skips the screenshot shutter sound.
- Empty shade message: replaces the trophy and text of the empty notification shade with your own text and one of 141 icons or no icon. Android 17 only.
- Always expand notifications: opens notifications expanded unless you collapsed one by hand.
- Double tap status bar to sleep: double tap empty status bar space and the phone sleeps.
- Biometric Bypass: taps confirm on the biometric prompt after a face unlock and plays a system sound of your choice. Same hook as [biometric-bypass](https://github.com/hxreborn/biometric-bypass).

### System Framework

- Force NEW_TASK: adds NEW_TASK to activity starts that match a rule. Rules go one per line as `source:target[:ir,nd]`. `ir` also catches starts that expect a result and `nd` adds NEW_DOCUMENT.
- Clipboard in background: allows selected apps to read the clipboard in the background.

### Launcher

- Double tap to sleep: double tap empty home screen space and the phone sleeps. Pixel Launcher and Launcher3.
- Hide gesture pill: makes the navigation handle transparent and keeps a chosen percent of the bottom inset under apps. A percent change restarts the launcher.
- Hide search bar: hides the search bar above the hotseat and frees the row it reserves. Pixel Launcher and Launcher3.

### Gboard

- AMOLED black keyboard: makes the tinted grey keyboard surfaces pure black in dark mode. Stock Gboard and the Morphe build. Toggling restarts Gboard. Same hook as [gboard-material-expressive-black](https://github.com/hxreborn/gboard-material-expressive-black).

### Phone

- Remove feedback chip: removes the Help us improve chip from the call list. Same hook as [gdialer-dechip](https://github.com/hxreborn/gdialer-dechip).

### File picker

- Force sort by newest first: opens folders sorted by newest instead of by name in the system file picker and the storage browser. Both DocumentsUI builds. Same hook as [remember-my-sort](https://github.com/hxreborn/remember-my-sort).

### KDE Connect

- KDE Connect Quiet: stops KDE Connect from holding the Wi-Fi multicast lock. Toggling restarts KDE Connect.

### GitHub

- GH FastPass: closes the Verification request approved dialog of the GitHub app as soon as a 2FA push is approved. Same hook as [gh-fast-pass](https://github.com/hxreborn/gh-fast-pass).

## Report a bug

Report from the app itself, end-to-end encrypted with [HPKE](https://www.rfc-editor.org/rfc/rfc9180) ([X25519](https://www.rfc-editor.org/rfc/rfc7748) and [AES-256-GCM](https://csrc.nist.gov/pubs/sp/800/38/d/final)). Opt-in: nothing leaves the phone until you tap Send encrypted report.

1. Open Settings and tap Report a bug.
2. Say what happened and pick the tweak if it is about one. Tap Send encrypted report.
3. Follow it in Settings, Your reports. A question from me shows there with a Reply button, and the GitHub issue once there is one.

You can also [open an issue](https://github.com/hxreborn/PixelInjector/issues/new) with the module log from your Xposed manager, or reach me at [hxreborn@duck.com](mailto:hxreborn@duck.com) or on [Telegram](https://t.me/hxreb0rn). [Open bugs](https://github.com/hxreborn/PixelInjector/issues?q=is%3Aissue%20state%3Aopen%20label%3Abug) are tracked on GitHub.

<details>
<summary>Full specs</summary>

| | |
| --- | --- |
| Scheme | [HPKE](https://www.rfc-editor.org/rfc/rfc9180) RFC 9180 base mode |
| Key exchange | DHKEM [X25519](https://www.rfc-editor.org/rfc/rfc7748) HKDF-SHA256 |
| Cipher | [AES-256-GCM](https://csrc.nist.gov/pubs/sp/800/38/d/final) |
| Encrypted | Message, module log, app versions |
| Not encrypted | Phone model, build, framework. Bound to the ciphertext as [associated data](https://www.rfc-editor.org/rfc/rfc9180#section-5.2) |
| Developer key | `3d76ec6a` |
| Reply key | X25519 per report, wrapped by [Android Keystore](https://developer.android.com/privacy-and-security/keystore) |
| Server | `reports.hxreborn.eu` holds no private key |
| Hidden in the log | Emails, IP and MAC addresses, Wi-Fi names, device IDs, storage paths, URL and header tokens ([Redactor](https://github.com/hxreborn/PixelInjector/blob/main/app/src/main/kotlin/eu/hxreborn/pixelinjector/support/Redactor.kt)) |
| Retention | 90 days |
| Hook status | Opt-in in Settings. Sends which tweaks load per Android build, once per version |
| Update check | On by default in Settings. A plain request every 6 hours with no device data |

</details>

## Credits

- [5ec1cff/MyInjector](https://github.com/5ec1cff/MyInjector) for the module structure, the hot reload flow and the NEW_TASK rule grammar.
- [tehcneko/clipboardwhitelist](https://github.com/tehcneko/clipboardwhitelist) for the clipboard hook idea.
- [Mahmud0808/PixelLauncherEnhanced](https://github.com/Mahmud0808/PixelLauncherEnhanced) for the gesture pill and search bar hooks.
- [LuckyPray/DexKit](https://github.com/LuckyPray/DexKit) for the dex search library.
- [JingMatrix/Vector](https://github.com/JingMatrix/Vector) for the module log screen design.
- [Material Symbols](https://github.com/google/material-design-icons) for the empty shade icons (Apache 2.0).

## License

[![GPL-3.0-only](https://img.shields.io/badge/LICENSE-GPL--3.0--only-%23A42E2B?style=for-the-badge&logo=gnu&logoColor=white&logoPosition=right)](https://github.com/hxreborn/PixelInjector/blob/main/LICENSE)
