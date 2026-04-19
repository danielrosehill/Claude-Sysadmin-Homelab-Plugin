# Synology NAS Management Workspace

You manage a Synology NAS on the user's behalf — storage, shares, backups, services.

## NAS profile

| Field | Value |
|---|---|
| Hostname / SSH alias | `<your-nas-host>` |
| LAN IP | `<your-nas-ip>` (prefer hostname) |
| Model | `<model>` |
| DSM version | `<version>` |
| SSH user | `<your-user>` |
| DSM API base | `http://<your-nas-host>:5000/webapi/` |

## Capabilities

1. **SSH** — direct shell for file operations, volume management, system commands.
2. **DSM API** — programmatic access (auth via `SYNO.API.Auth` → session id, then other endpoints).
3. **MCP** — if a Synology MCP server is configured, prefer it.

## Common tasks

- Shared folders: list, create, modify, delete.
- NFS / SMB mounts on the client side.
- Storage: disk usage, RAID/SHR status, volume health.
- Backups: Hyper Backup jobs, snapshot replication.
- Users, groups, permissions.

## Directories

- `context/nas-profile.md` — detailed NAS spec (read every session).
- `context/volumes/` — volume snapshots.
- `context/mounts/` — mount configs on the client.
- `outputs/storage-report.md` — latest storage utilisation.
- `outputs/operations-log.md` — change log.

## Plugin commands

- `/sysadmin-homelab:status --variant=nas` — DSM-aware status.
- `/sysadmin-homelab:logs --host=<your-nas-host>` — pull DSM logs.
- `/sysadmin-homelab:backup-config <path>` — snapshot a config path.

## Guidelines

1. Never store DSM passwords or API keys in tracked files. Use `.env` (gitignored).
2. Prefer the DSM API over SSH-poking filesystems when an endpoint exists.
3. Snapshot before destructive share or volume operations.
