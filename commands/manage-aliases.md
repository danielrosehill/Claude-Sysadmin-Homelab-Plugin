---
description: Manage bash aliases — add, edit, delete, prune, or ideate new aliases in ~/.bash_aliases
argument-hint: "[add|edit|delete|prune|ideate] [description or alias name]"
allowed-tools: Bash, Read, Edit, Write
---

Your task is to help the user manage their bash aliases located at `~/.bash_aliases`.

The user may ask you to:
- **Add** a new alias
- **Edit** an existing alias
- **Delete** or remove an alias by name
- **Prune** duplicate, conflicting, or redundant aliases
- **Ideate** — suggest useful aliases based on the user's workflow or community best practices

## Working with the aliases file

Always read `~/.bash_aliases` before making changes so you understand the current state.

When adding or editing aliases:
1. Research common conventions for the requested task (use web search if available) and prefer established patterns unless the user specifies otherwise.
2. Follow the format already in use in the file (comment style, grouping, spacing).
3. Apply the change to `~/.bash_aliases`.

## After every change

1. Source bash to test the change does not cause errors:
   ```bash
   source ~/.bash_aliases 2>&1 || echo "Error sourcing aliases"
   ```
2. Confirm the alias works as expected.
3. If the user uses a dotfiles manager (e.g. YADM, chezmoi, GNU Stow), remind them to sync/push their dotfiles after the session. Do not automatically push unless the user asks you to.

## Example requests

- "Add an alias to navigate to the root of a git project"
- "Show me all aliases related to git"
- "Remove duplicate or conflicting aliases"
- "What are common aliases for Docker commands?"
- "Rename the alias `ll` to `la`"
