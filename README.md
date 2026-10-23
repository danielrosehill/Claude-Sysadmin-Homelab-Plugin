> **Superseded (2026-10-23).** This plugin has been merged into [`ubuntu-admin`](https://github.com/danielrosehill/Ubuntu-Admin) and removed from the marketplace. Its README maps every command here to its replacement. This repo is kept for reference only.

# Claude-Sysadmin-Homelab-Plugin

Claude Code plugin for sysadmin and homelab workflows — remote server administration, container hosts, NAS, SBCs, and LAN resources. Ships diagnose / status / config / services / logs / alias / hardware-probe primitives plus a provisioning skill for a fresh sysadmin-homelab workspace.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

**Desktop sysadmin is out of scope** — see the separate `desktop-manager` plugin for that.

## What you get

### Primitives (always available once the plugin is installed)

**Diagnostics** (`/sysadmin-homelab:*`):
- `diagnose` — free-form live diagnosis from fresh logs (journal, dmesg, failed units)
- `diagnose-audio` — audio stack (PipeWire, ALSA) issues on a managed host
- `diagnose-network` — network stack diagnosis
- `diagnose-gpu` — GPU / DRM / graphics freeze diagnosis
- `diagnose-memory` — memory pressure / slowdown diagnosis
- `diagnose-usb` — USB device issues
- `diagnose-crash` — application / service crash triage

**Status and config**:
- `status` — comprehensive health snapshot (system, disks, services, containers)
- `update-config` — edit a managed config file with backup
- `check-services` — systemd unit state sweep
- `logs` — fetch and summarise logs from the managed host
- `backup-config` — back up a configuration path to the workspace

**Shell aliases** (absorbed from `bash-alias-manager`):
- `manage-aliases` — add / edit / delete / prune / ideate bash aliases
- `list-aliases` — list and search aliases
- `document-aliases` — generate markdown docs for aliases
- `backup-aliases` — timestamped `~/.bash_aliases` backup

**Hardware**:
- `hw-probe` — run the hardware probe tool and record the result

**Agent**:
- `sysadmin-operator` — autonomous subagent for multi-step admin tasks

### Provisioning skill

- `/sysadmin-homelab:new-workspace <name> [--variant=<variant>] [--local-only]`

Scaffolds a new workspace (CLAUDE.md + context/outputs structure), personalises it from `~/.claude/CLAUDE.md`, and (by default) creates a public GitHub repo for it.

## Variants

| Variant | Use case |
|---|---|
| `linux-servers` (default) | Generic Linux server admin via SSH |
| `docker` | Docker host / compose stack management |
| `conda` | Conda environment management |
| `proxmox` | Proxmox VE host admin |
| `nas-synology` | Synology NAS admin (SSH + DSM API) |
| `android-debug` | Android device via ADB (incl. MVT forensic scans) |
| `single-board-computers` | Raspberry Pi / Orange Pi / Odroid / SBC admin |
| `remote-admin` | Generic remote machine admin over SSH |
| `lan` | LAN scanning, discovery, multi-host admin |

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install sysadmin-homelab
```

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffolds are provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## License

MIT.
