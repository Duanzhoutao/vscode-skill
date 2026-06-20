# Contributing

Thanks for your interest in improving this skill.

## Development Setup

1. Clone this repository.
2. Have Python 3.10+ available. On Windows, also have PowerShell 7 (`pwsh`) for the `vscode-cli.ps1` driver.
3. Run the test suite to confirm a clean baseline:

   ```powershell
   python -X utf8 scripts/run_vscode_skill_tests.py --basic
   ```

The bundled `scripts/quick_validate.py` is sourced from the Agent Skills `skill-creator` reference. The CI workflow validates the YAML frontmatter inline without depending on the external script, so it works offline.

## Pull Request Guidelines

- Keep `SKILL.md` and `agents/openai.yaml` aligned with the rest of the documentation; the frontmatter `name` is the stable slug and must stay lowercase ASCII.
- Use English Title Case for `agents/openai.yaml` `display_name`.
- Add Chinese trigger phrases to `description` if the new feature covers a Chinese-localized IDE.
- For VS Code-derived products such as Trae, Qoder, and CodeBuddy, keep the default contribution scope to the VS Code-compatible editor/configuration layer unless there is official product-specific documentation for a separate adapter.
- Preserve the read-only / project / user-scope discipline documented in `SKILL.md`; never relax the `--Apply` requirement for write operations.
- When adding new areas or commands, implement them in **both** `scripts/vscode_skill.py` and `scripts/vscode-cli.ps1`, and add a corresponding entry to the dispatch table in each.
- For macOS / Linux support, prefer implementing the operation natively in Python inside `vscode_skill.py`. Forwarding to the local `code` binary is the right move for operations the skill cannot implement portably (e.g., `ext install`, `ssh open`).
- Add or update a test case in `scripts/run_vscode_skill_tests.py` so CI stays green.
- Run `python -X utf8 scripts/run_vscode_skill_tests.py --basic --include-python-venv` before opening a PR.

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` for new commands or capabilities.
- `fix:` for bug fixes.
- `docs:` for documentation-only changes.
- `chore:` for housekeeping (CI, dependencies, structure).
- `refactor:` for changes that neither fix a bug nor add a feature.

## Reporting Issues

When filing an issue, include:

- The exact command you ran.
- Python version (`python --version`).
- On Windows, PowerShell version (`$PSVersionTable.PSVersion`).
- Whether `--Product` was explicit or auto-detected.
- Relevant contents of your config file (redact any personal paths).
- Output of `python scripts/run_vscode_skill_tests.py --basic`.
