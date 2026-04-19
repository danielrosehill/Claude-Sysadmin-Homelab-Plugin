# Remote Machine Admin Workspace

You administer a single remote machine via SSH on the user's behalf.

## Machine profile

| Field | Value |
|---|---|
| Hostname / SSH alias | `<your-host>` |
| SSH user | `<your-user>` |
| Role | `<purpose of the machine>` |
| OS / distro | `<e.g. Ubuntu 24.04>` |

Before performing any task:

1. Check `context/machine-profile.md` (create if missing).
2. Review the latest snapshot in `system-index/latest/` if present.
3. Understand what this machine does before changing anything.

## Available resources

- `system-index/` — timestamped system snapshots, with `latest/` symlink.
- `system-parameters/hardware|software|network|security/` — structured facts.
- `backups/` — configuration backups.
- `documentation/` — machine-specific runbooks.
- `logs/ssh-logs|application-logs|system-logs|audit-logs/` — fetched logs.

## Plugin commands

- `/sysadmin-homelab:status --host=<your-host>`
- `/sysadmin-homelab:diagnose --host=<your-host>`
- `/sysadmin-homelab:logs --host=<your-host>`
- `/sysadmin-homelab:backup-config <path> --host=<your-host>`
- `/sysadmin-homelab:update-config <path> --host=<your-host>`

## Workflow

- **Investigate an issue**: read latest index → fetch current logs → compare with previous index → document findings.
- **Make a change**: backup → apply via SSH → verify → document → re-index if warranted.
- **Troubleshoot**: read baseline, gather current state, diff.
