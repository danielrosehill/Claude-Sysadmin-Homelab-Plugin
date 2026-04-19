---
description: Diagnose memory pressure or slowdown on a managed host.
argument-hint: "[--host=<ssh-alias>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Memory / Slowdown

The host is running slow or under memory pressure. Pull fresh data and diagnose. Prefix with `ssh <host>` for remote.

## Data Collection

```bash
# Snapshot
free -h
uptime
vmstat 1 3
top -b -n 1 | head -20

# Top memory consumers
ps aux --sort=-%mem | head -15

# Swap usage per process
for p in $(ps -eo pid --no-headers | head -50); do
  awk '/VmSwap/{print '"$p"', $2}' /proc/$p/status 2>/dev/null
done | sort -k2 -n | tail -10

# OOM events
journalctl --since "15 minutes ago" | grep -iE "out of memory|oom" || true
dmesg | grep -iE "oom|killed process" | tail -20
```

## Analysis

- Is memory actually full, or is this cache pressure?
- Any process with runaway growth?
- OOM killer active?
- Swap thrashing?

## Output Format

1. **Diagnosis** — root cause.
2. **Evidence** — specific numbers and log lines.
3. **Immediate fix** — e.g. restart a leaky service.
4. **Permanent fix** — limits, zram/swap tuning, more RAM.
