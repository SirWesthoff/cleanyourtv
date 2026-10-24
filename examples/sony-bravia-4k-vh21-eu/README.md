# Sony BRAVIA 4K VH21, EU (XR-55A80K, XR-65A80K)

**Model:** Sony XR-65A80K (2022 OLED, A80K series), the second unit of this list; the first was an XR-55A80K
**Platform:** `BRAVIA_VH21_EU` / device `BRAVIA_VH21`, MediaTek mt5895
**OS:** Google TV 12 (SDK 31), build `STT2.230505.001.S136 682241`, security patch 2026-05-01
**RAM / storage:** 3 GB, 4 GB `/data`

The notes below are from the XR-65A80K. The list's header says which lines are whose choice.

## What was tested

- 46 packages: the lines of the first owner's list this owner also wanted off, plus the section at the end of
  [`debloat.txt`](debloat.txt), applied with `tools/disable.sh` in nine batches with a Home screenshot after
  each. Rebooted three times: everything stayed disabled, nothing was re-enabled by the firmware, and all
  HDMI and tuner inputs stayed registered.
- Launcher from `launcher/`, installed with `launcher/build-and-install.sh`, dock from
  [`config.json`](config.json). Home confirmed with `mFocusedApp` before and after reboots. D-pad checked from
  every edge. The Inputs menu lists HDMI 1 to 4 and Live TV, and a row switches through `com.sony.dtv.tvx`.
- On the real remote, with launcherx disabled and no key helper: Home, Back, Input, the gear button and the
  Netflix, YouTube, Prime Video and Disney+ buttons.

## Casting with setupwraith disabled

On Google's own Cast hardware the receiver loads its device credentials from setupwraith, so disabling the
wizard breaks casting (docs/TROUBLESHOOTING.md). This set is a third-party Cast device: after a reboot with
setupwraith disabled, logcat shows

```
cast_ClientAuthCredsProvisioning: Loading Widevine creds for 3P device.
cast_shell: Intermediate cert: Sony TV BRAVIA_VH21 Mediatek MT5835 Cast ICA
cast_shell: Created singleton instance of ClientAuthSigner.
```

and casting YouTube from a phone worked. The `Failed to find provider info for ...CastInfoProvider` lines
that appear too come from the stock home's UI, not from the Cast signer.

## Voice

The mic button and app commands ("open YouTube") work with launcherx disabled. Voice searches for content
answer "Google Assistant can't process the request" (`Failed to execute provider.OPEN with status:
UNKNOWN_ERROR`): Google TV shows those results in launcherx. Enabling launcherx again brings them back, and it
then takes Home.

## Dock

[`config.json`](config.json) is the dock used on the XR-65A80K: Netflix, YouTube, Prime Video, Disney+ and
HBO Max in five columns, the Live TV app hidden as a tile (it is under Inputs) and the tile menu on (hold OK
on a tile, Move). Push it with `TV_HOST=<tv ip> tools/theme.sh examples/sony-bravia-4k-vh21-eu`.

## Notes from this firmware

- `/data` was 99% full on the 4 GB partition; `pm trim-caches 4G` freed about 630 MB before the launcher
  install.
- The UI runs at 1920×1080 on the 4K panel (`wm size`: Override size 1920x1080, density 320). That is the
  firmware default, not a setting to fix.
- Living Decor was the screensaver. Set `screensaver_components` to the launcher's `ClockDream` before
  disabling it, as the list's comment says.
- `adb pull` of the `/vendor/app` APKs is refused by SELinux on this firmware although the shell can read
  them (`md5sum` works); copying through the shell (`base64`) does.
