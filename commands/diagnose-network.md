---
description: Diagnose a network issue on a managed host or across the LAN.
argument-hint: "[--host=<ssh-alias>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Network Diagnosis

A network problem is happening right now. Pull fresh data and diagnose. Prefix with `ssh <host>` for remote.

## Data Collection

```bash
# Interfaces and routes
ip -br addr
ip route
ip -s link

# DNS
resolvectl status 2>/dev/null || cat /etc/resolv.conf

# Connectivity
ping -c 3 -W 2 1.1.1.1 || true
ping -c 3 -W 2 8.8.8.8 || true

# Recent journal — network stack
journalctl --since "10 minutes ago" -u NetworkManager -u systemd-networkd -u systemd-resolved --no-pager 2>/dev/null || true

# Kernel network events
dmesg | grep -iE "eth|wlan|link|carrier|phy" | tail -30

# Listening services
ss -tulnp 2>/dev/null | head -30
```

## Analysis

Look for:
- Link down / carrier lost events.
- DHCP failures or address churn.
- DNS resolution failures.
- Firewall drops in journal.
- Upstream gateway unreachable vs local service bind issues.

## Output Format

1. **Diagnosis** — likely cause.
2. **Evidence** — relevant output.
3. **Immediate fix** — restart NetworkManager, flush DNS, etc.
4. **Permanent fix** — config changes, route / firewall corrections.
