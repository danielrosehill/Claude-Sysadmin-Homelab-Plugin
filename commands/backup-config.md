---
description: Back up a configuration file or directory from the managed host into the workspace.
argument-hint: "<path> [--host=<ssh-alias>]"
allowed-tools: ["Bash"]
---

# Sysadmin-Homelab — Backup Config

Back up a configuration path to the workspace `backups/` directory with a timestamp.

## Procedure

```bash
TS=$(date +%Y%m%d-%H%M%S)
SRC="$ARGUMENTS"
DEST="backups/$(basename "$SRC").$TS"
mkdir -p backups

if [ -n "$HOST" ]; then
  ssh "$HOST" "tar -cz -C / $(echo "$SRC" | sed 's|^/||')" > "${DEST}.tgz"
else
  if [ -d "$SRC" ]; then
    tar -cz -C "$(dirname "$SRC")" "$(basename "$SRC")" > "${DEST}.tgz"
  else
    cp -a "$SRC" "$DEST"
  fi
fi
```

Report the destination path, size, and checksum. Append an entry to `outputs/backup-log.md`.
