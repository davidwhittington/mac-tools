# setup-claude

Automated installation and configuration of Claude Code on a new Mac. Handles the full setup: Homebrew cask install, directory structure, settings.json with template rendering, status line style selection, global CLAUDE.md deployment, and project management scripts.

## Install

```bash
brew tap davidwhittington/mac-tools
brew install davidwhittington/mac-tools/setup-claude
```

Or run directly from the repo:

```bash
bash setup-claude/setup-claude.sh
```

## Usage

```bash
# Interactive setup (prompts at each step)
setup-claude

# Fully automated with defaults
setup-claude --auto

# Choose a specific status line style
setup-claude --statusline minimal

# Already have Claude installed, just configure
setup-claude --skip-install
```

## Options

| Flag | Description |
|------|-------------|
| `--auto` | Skip all prompts, use defaults |
| `--statusline <name>` | Choose style: `default`, `minimal`, or `verbose` |
| `--skip-install` | Skip `brew install --cask claude` |
| `--help` | Show help |

## What it does

1. Installs Claude Code via `brew install --cask claude`
2. Creates `~/.claude/` directory and gallery structure
3. Deploys status line script (with style selection)
4. Renders `settings.json` from template with variable substitution
5. Deploys `~/.claude/CLAUDE.md` from template (first install only, never overwrites)
6. Copies project management scripts to `~/.claude/project-management/`

## Status line styles

- **default** — directory, git branch, model, context usage %
- **minimal** — directory and git branch only
- **verbose** — directory, git branch + dirty flag, model, context %, clock

Gallery variants are copied to `~/.claude/gallery/statuslines/` for switching later:

```bash
cp ~/.claude/gallery/statuslines/minimal.sh ~/.claude/statusline-command.sh
```

## Environment variables

| Variable | Default | Description |
|----------|---------|-------------|
| `CLAUDE_DEFAULT_MODE` | `acceptEdits` | Default Claude Code mode (`acceptEdits`, `auto`, `plan`) |
| `GIT_USER_NAME` | `Your Name` | Used in CLAUDE.md template |
| `GIT_NOREPLY_EMAIL` | `you@users.noreply.github.com` | Used in CLAUDE.md template |

## Requirements

- macOS
- Homebrew
- The config templates directory from the mac-security repo (for full template rendering)

## Note

This script references templates from the `configs/templates/claude/` directory in the mac-security repo. When installed via Homebrew formula, templates are bundled. When running from source, clone mac-security alongside mac-tools or point `REPO_ROOT` at it.
