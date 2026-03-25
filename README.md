# mac-tools

A collection of standalone macOS utilities for privacy, system maintenance, and development environment setup. Each tool lives in its own directory with its own documentation.

**Platform:** macOS (Apple Silicon, Sequoia/Tahoe)

## Tools

| Tool | Description | Install |
|------|-------------|---------|
| [tor-proxy](tor-proxy/) | Route system traffic through the Tor network via SOCKS proxy | `brew install davidwhittington/mac-tools/tor-proxy` |
| [chromium-browse](chromium-browse/) | Simulated browsing traffic generator for any Chromium-based browser | `brew install davidwhittington/mac-tools/chromium-browse` |
| [brew-upgrade](brew-upgrade/) | Automated Homebrew package updater with change logging | `brew install davidwhittington/mac-tools/brew-upgrade` |
| [setup-claude](setup-claude/) | Claude Code installation and configuration for new Macs | `brew install davidwhittington/mac-tools/setup-claude` |

## Quick install

```bash
brew tap davidwhittington/mac-tools

# Install any tool individually
brew install davidwhittington/mac-tools/tor-proxy
brew install davidwhittington/mac-tools/chromium-browse
brew install davidwhittington/mac-tools/brew-upgrade
brew install davidwhittington/mac-tools/setup-claude
```

## Structure

Each tool is self-contained with its own README covering installation, usage, and any relevant context.

```
mac-tools/
├── tor-proxy/           Tor SOCKS proxy anonymizer
├── chromium-browse/     Headless browsing traffic generator
├── brew-upgrade/        Homebrew update automation with logging
└── setup-claude/        Claude Code setup and configuration
```

## Related

- [mac-security](https://github.com/davidwhittington/mac-security) — macOS security hardening and auditing
- [linux-security](https://github.com/davidwhittington/linux-security) — VPS/server security hardening

## License

MIT
