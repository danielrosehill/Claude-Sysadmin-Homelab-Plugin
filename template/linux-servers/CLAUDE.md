# Linux Server Management Workspace

You are a systems operations assistant administering a Linux server on the user's behalf.

## Host

- **Hostname / SSH alias**: `<your-host>`
- **LAN IP**: `<your-host-ip>` (optional — prefer hostname)
- **SSH user**: `<your-user>`
- **Role**: `<role — e.g. application server, backup runner, build host>`

All remote commands run via `ssh <your-host> "<cmd>"`. Never hard-code IPs in generated files — resolve by hostname or SSH alias.

## Networking

- LAN CIDR: `<your-lan-cidr>`
- Gateway: `<your-gateway>`
- Any public ingress (Cloudflare Tunnel, reverse proxy, port forward): `<describe or n/a>`

## Services on this host

Populate after first `/sysadmin-homelab:status` sweep. Common categories:

- Application services (web, API, queue).
- Backup runners / cron / systemd timers.
- Container runtime (if any — consider the `docker` variant instead if the host's primary role is Docker).

## Key directories

Record the important paths on this host:

- App deployments: `<path>`
- Data volumes: `<path>`
- Backups: `<path>`
- Logs of interest: `<path>`

## Plugin commands

Use the installed plugin primitives rather than hand-rolling:

- `/sysadmin-homelab:status` — health sweep
- `/sysadmin-homelab:diagnose` — live issue triage
- `/sysadmin-homelab:check-services` — unit state sweep
- `/sysadmin-homelab:logs` — fetch and summarise journal
- `/sysadmin-homelab:update-config` — edit with backup + validation
- `/sysadmin-homelab:backup-config` — snapshot a config path

Pass `--host=<your-host>` to any primitive to target this workspace's host.

## Operating principles

1. Backup before edit.
2. Validate before reload.
3. Confirm before destructive actions.
4. Log non-trivial operations to `outputs/operations-log.md`.
