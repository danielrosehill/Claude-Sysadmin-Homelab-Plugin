---
description: Run the hardware probe tool (hw-probe) and save the report into the workspace.
argument-hint: "[--host=<ssh-alias>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Hardware Probe

Run [hw-probe](https://github.com/linuxhw/hw-probe) against the local machine or a remote managed host and stash the report.

## Procedure

1. **Check for tool**:
   ```bash
   which hw-probe || echo "not installed"
   ```
   If missing, offer install commands for Debian/Ubuntu (`apt install hw-probe`), Fedora (`dnf install hw-probe`), or the pip fallback.

2. **Run**:
   ```bash
   sudo -E hw-probe -all -save "outputs/hw-probe/$(date +%Y%m%d-%H%M%S)"
   ```
   For remote: `ssh <host> "sudo hw-probe -all -show"` then save stdout to `outputs/hw-probe/`.

3. **Summarise** the key facts to `context/hw-profile.md`: CPU model, RAM, storage devices and sizes, GPU, network interfaces, BIOS / firmware version, kernel.

4. **Optional upload** — if the user wants a shareable URL, offer `-upload` (privacy-aware; warn about what gets sent).
