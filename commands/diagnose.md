---
description: Diagnose a live system issue on a managed host from fresh logs. Accepts a free-form description.
argument-hint: "<brief description of what is happening right now> [--host=<ssh-alias-or-host>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Diagnose

The user is reporting a system issue that is happening **right now** (or has just occurred). Logs are fresh. Move quickly.

**Report:** $ARGUMENTS

If the user passed `--host=<ssh-alias>` or the current workspace CLAUDE.md names a managed host, prefix log commands with `ssh <host>` instead of running locally. If no host is specified and the workspace is LAN-scoped, ask which host.

## Immediate Actions

1. Recent journal (last 5 minutes, all priorities):
   ```bash
   journalctl --since "5 minutes ago" --no-pager
   ```
2. Warnings and above:
   ```bash
   journalctl --since "5 minutes ago" -p warning --no-pager
   ```
3. Kernel ring buffer:
   ```bash
   dmesg | tail -50
   ```
4. Failed systemd units:
   ```bash
   systemctl --failed --no-pager
   ```

## Analysis

- Identify the most likely root cause from user description and log output.
- Highlight relevant error / warning lines.
- Propose a concrete fix preferring commands the user can run immediately.
- Note whether this looks like a one-off or a recurring pattern (check for repeated occurrences in the journal).

## Output Format

1. **Diagnosis** — one or two sentences stating the most likely cause.
2. **Evidence** — specific log lines that support the diagnosis.
3. **Recommended fix** — runnable commands or config changes.
4. **Follow-up** — monitoring or permanent mitigations if the issue recurs.
