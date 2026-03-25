# chromium-browse

Simulated browsing traffic generator for any Chromium-based browser. Visits URLs from a list in randomized order, follows links on each page, scrolls, and dwells to simulate organic browsing patterns. Controls the browser via Chrome DevTools Protocol (CDP) in headless mode.

## Install

```bash
brew tap davidwhittington/mac-tools
brew install davidwhittington/mac-tools/chromium-browse
```

Or run directly from the repo:

```bash
chromium-browse/chromium-browse --browser chrome --urls urls.txt
```

## Usage

```bash
# Browse with a specific browser
chromium-browse --browser island --urls urls.txt

# Multiple rounds through the URL list
chromium-browse --browser chrome --rounds 3

# Use a custom Chromium binary
chromium-browse --browser /path/to/binary --urls urls.txt

# See which browsers are installed
chromium-browse --list-browsers
```

## Options

| Flag | Default | Description |
|------|---------|-------------|
| `--browser` | chrome | Browser name or path to binary |
| `--urls` | urls.txt | Path to URL list file |
| `--max-depth` | 2 | Max link-follow depth per site |
| `--min-delay` | 2.0 | Min dwell time in seconds |
| `--max-delay` | 8.0 | Max dwell time in seconds |
| `--max-links` | 5 | Max links to follow per page |
| `--rounds` | 1 | Full passes through the URL list |
| `--cdp-port` | 9222 | Chrome DevTools Protocol port |
| `--list-browsers` | | List known browsers and install status |

## Supported browsers

Chrome, Chromium, Edge, Brave, Island, Arc, Vivaldi, Opera, Comet, Atlas, Sidekick, Wavebox, Thorium, Ungoogled Chromium, and any custom Chromium binary.

## URL file format

One URL per line. Blank lines and `#` comments are ignored. URLs without a scheme get `https://` prepended.

```
# News sites
https://news.ycombinator.com
https://example.com
reuters.com
```

An example file is included at `urls.txt.example`.

## How it works

1. Launches the browser in headless mode with CDP enabled
2. Connects via WebSocket to the DevTools Protocol
3. For each URL: navigates, scrolls randomly, dwells for a random duration
4. Follows random same-domain links up to `max-depth`
5. Pauses between sites to simulate natural pacing
6. Cleans up the temporary user profile on exit

## Requirements

- macOS or Linux
- Python 3.10+
- A Chromium-based browser installed
- `websockets` and `aiohttp` Python packages (auto-installed by the wrapper, or via formula)
