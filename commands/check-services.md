---
description: Sweep systemd unit state on a managed host — active, failed, flapping.
argument-hint: "[--host=<ssh-alias>] [unit-glob]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Check Services

Inspect systemd service state. Prefix with `ssh <host>` for remote.

```bash
# Failed units
systemctl --failed --no-pager

# All units matching glob (if provided)
GLOB="${ARGUMENTS:-*}"
systemctl list-units --type=service --all "$GLOB" --no-pager | head -60

# Recently restarted units (flapping detection)
journalctl --since "1 hour ago" | grep -E "Started|Stopped|Failed" | tail -40

# User units too
systemctl --user --failed --no-pager 2>/dev/null || true
```

## Output

- **Failed** — units in failed state, with last error from `journalctl -u <unit> -n 20`.
- **Flapping** — units restarted more than twice in the last hour.
- **Healthy** — count of active units.
- **Recommended actions** — restart / mask / investigate per unit.
