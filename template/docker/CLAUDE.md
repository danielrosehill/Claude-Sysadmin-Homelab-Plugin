# Docker Host Management Workspace

You are a Docker environment management assistant for this workspace.

## Host

- **Hostname / SSH alias**: `<your-host>` (or `local` if the Docker host is this machine)
- **SSH user**: `<your-user>`
- **Docker role**: `<e.g. single production host, dev host, swarm manager>`

## Scope

This workspace manages:
- One or more Docker hosts (local and/or remote via SSH).
- Docker Compose stacks deployed on those hosts.
- Volumes, networks, and images inventory.

## Directories

- `context/environments/` — per-host profile (docker version, container inventory, resources).
- `context/networks/` — docker network documentation.
- `context/volumes/` — volume inventory and backup logs.
- `context/images/` — image catalog.
- `stacks/` — optional in-repo compose stacks.
- `scripts/` — utility scripts.
- `outputs/` — status reports, operations log.

## Plugin commands

- `/sysadmin-homelab:status --variant=docker` — full health snapshot including `docker ps`, stats, disk usage.
- `/sysadmin-homelab:logs --host=<your-host>` — pull journal + optionally container logs.
- `/sysadmin-homelab:update-config <path-to-compose.yml>` — edit a compose file with backup and `docker compose config` validation.
- `/sysadmin-homelab:check-services` — systemd-side view (docker daemon, containerd).

## Operating principles

1. Prefer `docker compose` for stacks. Always `docker compose config` before `up -d`.
2. Persistent data lives in named volumes with bind-mounted paths documented in the stack.
3. Backup volumes before image upgrades of stateful services.
4. Log non-trivial stack changes in `outputs/operations-log.md`.
