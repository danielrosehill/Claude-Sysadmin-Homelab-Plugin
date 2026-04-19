---
description: Fetch and summarise logs from a managed host. Pulls journal, application logs, and writes to the workspace.
argument-hint: "[--host=<ssh-alias>] [--since=<time>] [--unit=<unit>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Logs

Fetch logs from the managed host and store a dated copy in the workspace's `outputs/logs/` directory. Prefix commands with `ssh <host>` for remote.

## Procedure

1. **Resolve time window** — default `--since="1 hour ago"`.
2. **Fetch**:
   ```bash
   journalctl --since "$SINCE" ${UNIT:+-u "$UNIT"} --no-pager
   ```
3. **Save** to `outputs/logs/journal-<host>-<YYYYMMDD-HHMM>.log`. For remote hosts, pipe the SSH output into a local file.
4. **Summarise**:
   - Total lines and unique units.
   - Error / warning counts.
   - Most frequent message patterns (group similar lines).
   - Any obvious anomalies.
5. **Return** a short markdown summary plus the saved log path.

If no workspace (running from `~`), write to `/tmp/sysadmin-homelab-logs/` and warn the user.
