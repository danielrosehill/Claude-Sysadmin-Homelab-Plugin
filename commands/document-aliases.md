---
description: Generate a human-readable documentation file for all bash aliases in ~/.bash_aliases
allowed-tools: Bash, Read, Write
---

Read `~/.bash_aliases` and produce a well-formatted Markdown documentation file that describes every alias.

## Output format

Create (or overwrite) `~/bash-aliases-docs.md` with:

1. A header section with the date the documentation was generated.
2. A table of contents if there are more than 10 aliases.
3. Each alias listed with:
   - Alias name (in code formatting)
   - Full expansion command
   - Plain-English description of what it does (inferred from the command and any existing comments)
   - Example usage where helpful

Group aliases by their section headers from the file. If no headers exist, group by inferred category.

After writing the file, report its location and how many aliases were documented.
