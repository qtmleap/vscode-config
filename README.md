# vscode-config

Shared VS Code workspace config for qtmleap projects. Polyglot baseline:
**Biome** for JS/TS/JSON and **Ruff** for Python — format on save plus
organize-imports / fix-all on save — a few common editor-behavior defaults, and
the recommended extension set. Kept in one place so every project formats and
lints consistently. Each language block only applies to its own files, so a
single-language project is unaffected by the others (trim what you don't use).

## Use

Copy the two files into your project's `.vscode/` and commit them:

```sh
mkdir -p .vscode
curl -fsSL https://raw.githubusercontent.com/qtmleap/vscode-config/main/settings.json   -o .vscode/settings.json
curl -fsSL https://raw.githubusercontent.com/qtmleap/vscode-config/main/extensions.json -o .vscode/extensions.json
```

Then add project-specific bits to your own `.vscode/settings.json` (extra
`files.exclude` / `search.exclude` paths for the stack, e.g. `.tanstack`,
`.intlayer`, `.playwright-mcp`) and a `.vscode/tasks.json` if the project needs
one. Re-run the curl commands to resync when this repo changes.

## Why copy, not a submodule

VS Code reads a single `.vscode/settings.json` and has no shared+local merge, so
a submodule would be all-or-nothing with no room for per-project tweaks. A
missing/uninitialized submodule also fails *silently* (VS Code just stops
formatting), unlike a tool that errors loudly. Copying keeps per-project edits
trivial and avoids that footgun.

## Requires

The recommended extensions (in `extensions.json`) — VS Code prompts to install
them on first open. Formatting assumes the relevant config exists in the project:
`biome.json` for JS/TS, and a Ruff config (`ruff.toml` or `[tool.ruff]` in
`pyproject.toml`) for Python. Pure-JS projects can drop `ms-python.python` /
`charliermarsh.ruff` and the `[python]` block; pure-Python projects can drop the
Biome entries.
