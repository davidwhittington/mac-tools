# brew-upgrade

Automated Homebrew package updater with change logging. Updates the formula index, shows what's outdated, upgrades everything, cleans up old versions, and logs what changed.

## Install

```bash
brew tap davidwhittington/mac-tools
brew install davidwhittington/mac-tools/brew-upgrade
```

Or run directly:

```bash
bash brew-upgrade/brew-upgrade.sh
```

## Usage

```bash
# Interactive mode (shows outdated, asks for confirmation)
brew-upgrade

# Preview what would be upgraded
brew-upgrade --dry-run

# Skip casks, formulae only
brew-upgrade --no-casks

# Non-interactive (skip confirmation prompt)
brew-upgrade --confirm
```

## What it does

1. `brew update` to fetch the latest formula index
2. Lists all outdated formulae and casks
3. Prompts for confirmation (unless `--confirm` or `--dry-run`)
4. `brew upgrade --formula` and `brew upgrade --cask --greedy`
5. `brew cleanup` to remove old versions
6. Logs a dated Markdown summary to `private/machines/<hostname>/brew-upgrades.log` (if the private submodule exists)

## Logging

When a `private/machines/<hostname>/` directory exists (from the mac-security private submodule), upgrade summaries are appended to `brew-upgrades.log` with timestamps. This creates an audit trail of package changes per machine.

If no private submodule is present, upgrades still run normally without logging.

## Requirements

- macOS
- Homebrew
