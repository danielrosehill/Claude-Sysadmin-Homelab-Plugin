---
description: Comprehensive health snapshot of a managed host — system, disks, services, containers.
argument-hint: "[--host=<ssh-alias>] [--variant=linux|docker|proxmox|nas|sbc]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Status

Generate a health snapshot. Prefix commands with `ssh <host>` when operating remotely. Adapt to the variant hint (or the workspace CLAUDE.md).

## Core (always)

```bash
hostnamectl
uptime
free -h
df -hT --exclude-type=tmpfs --exclude-type=devtmpfs
systemctl --failed --no-pager
journalctl -p err --since "1 hour ago" --no-pager | tail -30
```

## Variant add-ons

### linux-servers / remote-admin

```bash
ss -tuln | head
who
last -n 5
```

### docker

```bash
docker version
docker ps --format 'table {{.Names}}\t{{.Status}}\t{{.Image}}'
docker stats --no-stream
docker system df
```

### proxmox

```bash
pveversion
pvesh get /cluster/status 2>/dev/null || true
qm list 2>/dev/null || true
pct list 2>/dev/null || true
pvesm status
```

### nas-synology (SSH into DSM)

```bash
cat /etc/synoinfo.conf | grep -E "unique|model"
df -hT
cat /proc/mdstat 2>/dev/null
synoservicectl --list 2>/dev/null | head -20 || true
```

### single-board-computers

```bash
cat /sys/class/thermal/thermal_zone0/temp 2>/dev/null | awk '{print $1/1000"°C"}'
vcgencmd measure_temp 2>/dev/null || true
vcgencmd get_throttled 2>/dev/null || true
```

## Output Format

Produce a single markdown report with sections for **System**, **Storage**, **Services**, **Containers/VMs** (if relevant), **Errors**. Save a dated copy to `outputs/status-<YYYYMMDD-HHMM>.md` if run from a workspace.
