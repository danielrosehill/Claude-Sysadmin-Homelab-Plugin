---
description: Diagnose a process or service crash on a managed host — journal, coredumps, segfault logs.
argument-hint: "<optional: application or unit name> [--host=<ssh-alias>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Crash Diagnosis

A process or service has just crashed. Pull fresh logs and diagnose. Application/unit (if provided): $ARGUMENTS

Prefix commands with `ssh <host>` when operating against a remote managed host.

## Log Collection

```bash
# Recent journal
journalctl --since "5 minutes ago" --no-pager

# Segfaults, aborts, OOM kills
journalctl --since "5 minutes ago" | grep -iE "segfault|sigsegv|killed|core dump|abort|oom" || true

# systemd-coredump entries
coredumpctl list 2>/dev/null | tail -10 || true

# Kernel messages
dmesg | tail -40

# If app/unit provided, filter
APP="${ARGUMENTS}"
if [ -n "$APP" ]; then
  journalctl --since "5 minutes ago" -u "$APP" --no-pager 2>/dev/null || \
    journalctl --since "5 minutes ago" | grep -i "$APP" --no-pager || true
fi
```

## Analysis

Look for:
- Signal that killed the process (SIGSEGV, SIGABRT, SIGBUS, SIGKILL from OOM).
- Library / shared object loading failures.
- OOM kill events.
- Permission or resource limit violations.
- Any stack trace hints.

## Output Format

1. **Diagnosis** — most likely cause.
2. **Evidence** — relevant log lines and coredump info.
3. **Immediate action** — e.g. `coredumpctl debug <pid>`.
4. **Fix or workaround** — library reinstall, resource limit adjustment, unit restart policy.
