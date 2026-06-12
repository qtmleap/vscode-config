# vscode-config

Shared VS Code workspace config for qtmleap projects. Polyglot baseline with one
standard tool per language:

- **JS / TS / JSON** -> Biome (format + organize imports on save)
- **Python** -> Ruff (format + fix-all + organize imports on save)
- **Rust** -> rust-analyzer (rustfmt on save, `cargo clippy` for checks)
- **C / C++** -> clangd (clang-format on save); CodeLLDB for debugging (shared with Rust)

Plus a few common editor-behavior defaults and the recommended extension set.
Kept in one place so every project formats and lints consistently. Each language
block only applies to its own files, so a single-language project is unaffected
by the others (trim what you don't use).

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
`biome.json` (JS/TS), a Ruff config (`ruff.toml` or `[tool.ruff]` in
`pyproject.toml`) for Python, `rustfmt.toml`/Cargo defaults for Rust, and a
`.clang-format` for C/C++. Drop the language blocks and extensions you don't use.

C/C++ uses clangd (LSP + clang-format) rather than Microsoft C/C++ Tools; if you
prefer `ms-vscode.cpptools`, swap the `[cpp]`/`[c]` defaultFormatter and the
extension. Don't enable both clangd and cpptools IntelliSense at once.
