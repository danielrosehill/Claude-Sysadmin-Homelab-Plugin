---
description: List and search all currently defined bash aliases in ~/.bash_aliases
argument-hint: "[optional search term or category]"
allowed-tools: Bash, Read
---

Read `~/.bash_aliases` and display a structured summary of all aliases currently defined.

If the user provides a search term or category (e.g. "git", "docker", "navigation"), filter the output to show only matching aliases.

For each alias, show:
- The alias name
- The command it expands to
- Any inline comment describing it (if present)

Group aliases by the comment headers already in the file where available. If no grouping exists in the file, group by inferred category (git, navigation, file management, etc.).

At the end, report the total alias count.
