---
name: new-workspace
description: Provision a new sysadmin-homelab workspace on disk. Use when the user wants to start managing a new server, container host, NAS, SBC, or LAN. Accepts a workspace name and optional variant (linux-servers | docker | conda | proxmox | nas-synology | android-debug | single-board-computers | remote-admin | lan). Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, and (by default) creates a GitHub repo.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Read
---

# Provision Sysadmin-Homelab Workspace

Creates a new workspace for administering a remote machine (server, container host, NAS, SBC, or LAN). This plugin's commands (`/sysadmin-homelab:status`, `/sysadmin-homelab:diagnose`, etc.) are globally available once installed — this skill only provisions the **data scaffold** (CLAUDE.md, context/, outputs/) that those commands read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (kebab-case, used as directory and GitHub repo name). Required.
- **Second positional** (optional): target parent path. Defaults to `~/repos/github/my-repos`.
- **`--variant=<name>`** (optional): which scaffold to copy. Default: `linux-servers`. Options: `linux-servers`, `docker`, `conda`, `proxmox`, `nas-synology`, `android-debug`, `single-board-computers`, `remote-admin`, `lan`.
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a public GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private.

### Examples

```
/sysadmin-homelab:new-workspace home-proxmox --variant=proxmox
/sysadmin-homelab:new-workspace office-nas --variant=nas-synology --private
/sysadmin-homelab:new-workspace rpi-kitchen --variant=single-board-computers --local-only
/sysadmin-homelab:new-workspace docker-host --variant=docker
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user for it before proceeding. If variant is missing, default to `linux-servers`.

### 2. Resolve the scaffold path

The bundled scaffold lives at `${CLAUDE_SKILL_DIR}/../../template/<variant>/`. Confirm it exists. If the variant is not one of the supported names, tell the user which variants are available.

### 3. Read ambient facts

Read `~/.claude/CLAUDE.md` if it exists. Extract OS, locale, timezone, and user identity facts. These will personalise the workspace's CLAUDE.md at step 5.

### 4. Create the workspace directory

```bash
mkdir -p <target-parent>/<workspace-name>
cp -r ${CLAUDE_SKILL_DIR}/../../template/<variant>/. <target-parent>/<workspace-name>/
```

Do **not** copy any `.claude/` tree. The plugin's primitives are global.

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace any `<your-host>`, `<your-user>`, `<your-lan-cidr>` placeholders with the user's values, prompting if not inferable.
- Add a short header noting the workspace name and variant.
- If ambient facts include OS/locale, embed them so downstream commands can skip re-asking.

### 6. Prompt for workspace-specific facts

Ask the user only for facts the plugin cannot infer. Typical prompts by variant:

- **linux-servers / proxmox / remote-admin / docker / sbc**: target host (hostname or IP), SSH alias, SSH user.
- **nas-synology**: NAS hostname/IP, DSM username, whether DSM API access is needed.
- **android-debug**: device model, connection method (USB / Wi-Fi ADB), whether MVT forensic scanning is desired.
- **conda**: whether the host is local or remote, hardware accelerator (CUDA / ROCm / CPU).
- **lan**: LAN CIDR, gateway IP, whether there is a router API (OPNsense / pfSense / UniFi) to talk to.

Write answers to the appropriate file in the workspace (`context/host-profile.md`, `context/nas-profile.md`, `context/device.md`, etc.).

### 7. Initialise git and (optionally) publish

```bash
cd <target-parent>/<workspace-name>
git init
git add .
git commit -m "Initial workspace from sysadmin-homelab plugin"
```

Unless `--local-only` is set:

```bash
gh repo create <workspace-name> --<public|private> --source=. --push
```

Use `--public` by default, `--private` if flag was passed. Private is a reasonable default for workspaces that may record internal hostnames; suggest it if the user hasn't chosen.

### 8. Print next steps

Tell the user:

- Workspace path.
- Variant chosen.
- Which plugin commands apply — for example:
  - All variants: `/sysadmin-homelab:status`, `/sysadmin-homelab:diagnose`, `/sysadmin-homelab:logs`, `/sysadmin-homelab:check-services`.
  - `docker`: also `/sysadmin-homelab:status` focuses on compose stacks.
  - `android-debug`: adb / MVT scans are driven through the agent.
  - `lan`: start with `/sysadmin-homelab:status` then scan from within the workspace.
- Reminder that the workspace is **data** — it can be deleted or moved without losing plugin commands.

## Notes

- The scaffold path must be resolved via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}` — that variable isn't exported in skill bash injection, only in hooks/MCP).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace. If the user wants workspace-local overrides, they can add them manually later.
- Don't hard-code any personal paths, hostnames, or IPs in the scaffolds — everything specific comes from user memory or prompts at provision time.
