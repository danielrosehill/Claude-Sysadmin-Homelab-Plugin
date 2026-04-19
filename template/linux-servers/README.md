# Linux Server Workspace

A Claude Code workspace for administering a Linux server via SSH. Provisioned by the `sysadmin-homelab` plugin.

## Layout

- `CLAUDE.md` — workspace instructions (host profile, services, directories).
- `context/` — cached host facts, profiles, inventories.
- `outputs/` — status reports, operation logs, fetched logs.
- `backups/` — timestamped config backups (created on first backup).

The plugin's commands live globally; this workspace is pure data.
