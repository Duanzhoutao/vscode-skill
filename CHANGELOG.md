# Changelog

All notable changes to this skill are documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-06-20

### Added

- **Cross-platform Python gateway** `scripts/vscode_skill.py`. On Windows it forwards to the PowerShell driver; on macOS and Linux it implements `info`, `ext list`, `settings path/get/set/smoke`, and `python set-interpreter` natively in Python. All other areas (SSH, WSL, install, etc.) shell out to the local `code` binary.
- **New areas**: `tasks`, `launch`, `extensions`, `settings-sync`, `workspace`, `profile`. Each area works on both Windows and macOS/Linux.
- **Bundled templates**: 10 ready-made `tasks.json` and 9 ready-made `launch.json` under `references/templates/{tasks,launch}/`. Use `tasks init --Template <id>` / `launch init --Template <id>` to copy them into a project.
- **Extension packs**: 8 curated stacks (frontend-web, backend-python, backend-node, data-science, systems, devops, polyglot-baseline, ai-coding). Use `extensions recommend/install/pin --Stack <name>`.
- **Settings reference index**: `references/settings-index.md` with 80 most-used settings (key, default, type, scope, description).
- **Settings precedence explainer**: `references/settings-precedence.md` covering the 11-layer precedence model and multi-root workspace behavior.
- **Keybindings cheatsheet**: `references/keybindings-cheatsheet.md` with 50 most-remapped shortcuts.
- **Multi-root workspace helpers**: `workspace info / folder-settings-get / folder-settings-set` read and edit `.code-workspace` files safely.
- **Profiles**: `profile list / show` reads the on-disk profile structure (read-only).
- **Settings Sync**: `settings-sync status` reports whether sync storage exists locally without reading any payload. **Settings Sync is never enabled or disabled programmatically.**

### Changed

- `openai.yaml` `default_prompt` now describes the cross-platform gateway and the new areas.
- `SKILL.md` rewritten with cross-platform Quick Start (Python gateway) and a Windows-only PowerShell alternative section.
- Test runner extended from 8 to 16 basic checks; all 16 pass on Windows.

## [1.0.0] - 2026-06-20

### Added

- Initial public release.
- Commands: `info`, `ext list/locate/install`, `settings path/get/set/smoke`, `python make-venvs/set-interpreter/verify`, `ssh open/smoke`, `wsl list/open/smoke`.
- PowerShell driver `scripts/vscode-cli.ps1` with environment-variable and config-file-based path resolution.
- Safe test runner `scripts/run_vscode_skill_tests.py` with `--basic` and `--include-python-venv` groups.
- Reference document `references/vscode-config-files.md` for project-level config file formats.