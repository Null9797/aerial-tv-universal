# Aerial Universal 1.57

**Unofficial derivative build based on Aerial by pengchujin. This is a community modification, not original work or an upstream release.**

## APK variants

- `aerial-universal-1.57-standard.apk` — declares Android HOME launcher entry.
- `aerial-universal-1.57-guard.apk` — omits HOME entry for firmware that blocks launcher apps; requires in-app Guard setup and any permissions shown by the TV.

Both variants use package `app.orchard.tv`; install only one.

## Changes in 1.57

- Added 14 bundled, TV-sized app icon designs for recognized apps. Apps without a bundled match keep their original launcher icon.
- Refined adaptive home layout, five-item dock sizing and spacing, and remote-focus animation.
- Added display refresh preference: Auto, 30, 60, or 120 Hz. Actual modes depend on the TV panel, Android version, and vendor firmware; unsupported rates are not forced.
- Kept weather, date, and time on the home screen; added wallpaper and input access alongside network and Aerial settings.
- Retained flip-clock screensaver, inactivity timer, wallpaper and video wallpaper tools, and Aerial's upstream organization/screensaver features.
- Added/retained update checks against the Null9797 release repository. A newer matching flavor can be downloaded from Aerial and passed to the Android system installer; the user must confirm installation on the TV.

## Compatibility

Primary end-to-end test device: TCL Q10G Pro (MT5879, Android 11). The home screenshot is a real TCL capture; the flip-clock screenshot is a representative capture from the custom build. Other devices are not individually verified for this release. See `README.md`, `INSTALL.md`, and `FAQ.md` for conservative model guidance.

## Attribution

This is an unofficial derivative of the MIT-licensed [Aerial project](https://github.com/pengchujin/aerial-tv), not an original app and not endorsed by the upstream author or TV manufacturers. Upstream attribution and license are preserved in `LICENSE`. Bundled third-party marks/icons belong to their respective owners.
