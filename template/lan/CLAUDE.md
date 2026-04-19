# LAN Management Workspace

You are the user's base for operating across their local network — discovery, multi-host admin, inventory.

## LAN profile

- **CIDR**: `<your-lan-cidr>` (e.g. `192.168.1.0/24`)
- **Gateway**: `<your-gateway>`
- **Router / firewall**: `<OPNsense | pfSense | UniFi | ISP router | …>`
- **DNS**: `<local DNS / Pi-hole / upstream>`

## Hosts

Maintain a registry in `context/hosts.md` — hostname, role, SSH alias, notes. Prefer hostnames over IPs in any generated config.

## Discovery

Common scans:

```bash
# ARP neighbours
ip neigh

# ICMP sweep (requires nmap or fping)
nmap -sn <your-lan-cidr>

# Service scan (top ports)
nmap -sT --top-ports 50 <your-lan-cidr>

# mDNS services
avahi-browse -a -t 2>/dev/null | head -50
```

Save discovery snapshots to `outputs/scans/scan-<YYYYMMDD-HHMM>.txt` and reconcile changes into `context/hosts.md`.

## Plugin commands

- `/sysadmin-homelab:status --host=<your-host>` per host.
- `/sysadmin-homelab:diagnose-network` for network-level issues.

## Guidelines

1. Be explicit about scope when scanning — don't spray probes beyond the documented CIDR.
2. Respect IoT devices: some misbehave on aggressive scans.
3. Re-run discovery when hardware changes or hosts move.
