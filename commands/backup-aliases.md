---
description: Create a timestamped backup of ~/.bash_aliases
allowed-tools: Bash
---

Create a timestamped backup of `~/.bash_aliases`.

Steps:
1. Determine the backup directory. Default: `~/.bash_aliases_backups/`. Create it if it does not exist.
2. Copy `~/.bash_aliases` to `~/.bash_aliases_backups/bash_aliases_<YYYYMMDD_HHMMSS>.backup`.
3. Report the backup file path, its size, and the number of aliases it contains.
4. If there are more than 10 backup files in the directory, offer to delete the oldest ones to keep the directory tidy (but do not delete without confirmation).

Example backup command:
```bash
mkdir -p ~/.bash_aliases_backups
cp ~/.bash_aliases ~/.bash_aliases_backups/bash_aliases_$(date +%Y%m%d_%H%M%S).backup
```
