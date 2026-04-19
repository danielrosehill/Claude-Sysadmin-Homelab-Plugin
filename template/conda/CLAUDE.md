# Conda Environment Management Workspace

You help the user manage their Conda environments — creation, health, cleanup, documentation.

## Host

- **Hostname**: `<your-host>` (or `local`)
- **Accelerator**: `<CUDA | ROCm | CPU-only | Metal>`
- **Primary workloads**: `<e.g. ML training, data science, audio/video ML, LLM inference>`

## Goals

- Maintain a small number of **base environments** that cover most needs, to avoid bespoke envs per project.
- Keep environments reproducible via `environment.yml` exports in `envs/`.
- Remove broken / duplicate / unused envs.
- Document env purpose and contents in `docs/`.

## Directories

- `envs/` — exported `environment.yml` files, one per env.
- `backups/` — timestamped env exports.
- `docs/` — env purpose, notes, troubleshooting.
- `logs/` — install / solve logs for tricky envs.
- `outputs/` — reports and operation logs.

## Plugin commands

- `/sysadmin-homelab:status` — picks up conda via `conda info --envs` when run in this workspace.
- `/sysadmin-homelab:diagnose` — for environment solve failures or broken installs.

## Guidelines

1. Prefer `conda-forge` + `pip` inside conda for packages conda-forge lacks.
2. For GPU workloads, pin CUDA / ROCm carefully; document the matrix in `docs/`.
3. Before deleting an env, export it to `backups/` first.
4. Teach the user when to use conda vs uv vs pip when asked.
