# Android Device (ADB) Management Workspace

You manage an Android device via ADB for the user — package management, optimisation, forensic scanning (MVT).

## Device

- **Manufacturer / model**: `<device>`
- **Android version**: `<version>`
- **Bootloader lock status**: `<locked | unlocked>`
- **Root**: `<yes | no | magisk>`
- **Connection**: `<USB | Wi-Fi ADB>`

## Scope

- Device inventory: package lists, installed services, boot state.
- Bloatware removal (`pm uninstall -k --user 0 <package>`).
- Security configuration review.
- Forensic scanning with [MVT](https://github.com/mvt-project/mvt) using Amnesty International's IOCs.
- Log capture (`adb logcat`) for later inspection.

## Directories

- `manifest/` — device metadata (`device.json` minimum).
- `packages/` — timestamped package-list exports.
- `forensics/` — captured adb logs and MVT reports.
- `context/` — device profile, connection notes.
- `outputs/` — operation log.

## MVT workflow

1. Ensure USB debugging is enabled and the host is authorised on the device.
2. Install / update MVT (`pipx install mvt` or `uv tool install mvt`).
3. Update IOCs: `mvt-android download-iocs`.
4. Run scan: `mvt-android check-adb --output forensics/mvt-<date>`.
5. Review results; `forensics/` is gitignored by default.

## Plugin commands

- `/sysadmin-homelab:diagnose` — general diagnostics (limited adb applicability).
- The subagent `sysadmin-operator` can drive multi-step adb / MVT flows.

## Guidelines

1. MVT scans are non-destructive. Bloatware removal with `pm uninstall` is reversible (`cmd package install-existing`).
2. Don't commit MVT reports or logcat to public repos — they contain device-specific data.
