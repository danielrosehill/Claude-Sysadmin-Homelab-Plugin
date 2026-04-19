---
description: Edit a managed configuration file safely — back up, edit, validate, report.
argument-hint: "<path-to-config> [--host=<ssh-alias>]"
allowed-tools: ["Bash", "Read", "Edit"]
---

# Sysadmin-Homelab — Update Config

Safely update a configuration file. Target: $ARGUMENTS

## Procedure

1. **Resolve the host** — if `--host=` is passed or the workspace CLAUDE.md names one, operate via `ssh <host>`. Otherwise local.
2. **Backup** — copy the current file to `backups/<basename>.<YYYYMMDD-HHMMSS>.bak` in the workspace (create `backups/` if absent). For remote files, `scp` the backup down first.
3. **Read and edit** — apply the user's requested change.
4. **Validate** — where possible, run the tool's own validator:
   - nginx: `nginx -t`
   - sshd: `sshd -t`
   - systemd unit: `systemd-analyze verify <unit>`
   - docker compose: `docker compose config`
   - YAML: `python -c "import yaml,sys; yaml.safe_load(open(sys.argv[1]))" <file>`
5. **Reload** — if validation passes, reload the affected service; ask before restarting if the change is risky.
6. **Log** — append a dated entry to `outputs/config-changes.md` with the file, reason, and diff summary.

Never edit a config file without a backup. Always validate before reload.
