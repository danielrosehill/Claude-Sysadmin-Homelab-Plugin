# Single Board Computer (SBC) Management Workspace

You administer a single-board computer — Raspberry Pi, Orange Pi, Rock Pi, Odroid, Pine64, or similar.

## SBC profile

- **Model**: `<e.g. Raspberry Pi 5 8GB>`
- **OS**: `<Raspberry Pi OS | DietPi | Armbian | Ubuntu | …>`
- **Hostname / SSH alias**: `<your-host>`
- **SSH user**: `<your-user>`
- **Role**: `<e.g. Pi-hole, media player, Home Assistant, print server>`
- **Storage**: `<SD | eMMC | USB SSD | NVMe>`
- **Cooling**: `<passive | active>`

## Resource constraints

SBCs have limited resources. Be conservative:

- ARM CPU — avoid CPU-hungry services.
- Limited RAM (often 1–8 GB) — watch for swap.
- SD / eMMC wear — avoid write-heavy logging, prefer log2ram or journald volatile.
- Thermal throttling — monitor temps (`vcgencmd measure_temp` on Pi).

## Typical use cases

- Network services (Pi-hole, WireGuard, reverse proxy).
- Home automation (Home Assistant, Zigbee / Z-Wave bridge).
- Media (Plex, Jellyfin, Kodi, music server).
- Monitoring / dashboards.

## Plugin commands

- `/sysadmin-homelab:status --variant=sbc` — SBC-flavoured status (temp, throttling, storage).
- `/sysadmin-homelab:diagnose` — live triage.
- `/sysadmin-homelab:hw-probe` — record the hardware profile.

## Directories

- `context/hw-profile/` — hardware probe output.
- `context/env.md` — OS, kernel, networking notes.
- `outputs/` — status reports, operations log.

## Guidelines

1. Back up the SD / eMMC image periodically — easiest disaster recovery.
2. Use `unattended-upgrades` cautiously on low-memory boards.
3. Avoid running a second workload that conflicts with the primary role.
