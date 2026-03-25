# tor-proxy

Route macOS system traffic through the Tor network. Configures your Mac's SOCKS proxy to point at a local Tor instance, with automatic network interface detection.

## Install

```bash
brew tap davidwhittington/mac-tools
brew install davidwhittington/mac-tools/tor-proxy
```

Installs `tor-proxy` and its dependency (`tor`) in one step.

## Usage

```bash
tor-proxy enable     # Start Tor, configure system SOCKS proxy
tor-proxy disable    # Stop Tor, restore direct connection
tor-proxy status     # Show Tor state, proxy config, external IP
tor-proxy newid      # Request a new Tor circuit (new exit IP)
```

`enable` and `disable` prompt for your system password (`networksetup` requires admin).

## How it works

1. Detects your active network interface by checking the default route
2. Starts Tor via `brew services`, which opens a SOCKS5 proxy on port 9050
3. Configures macOS to use that proxy via `networksetup`
4. Local addresses (`.local`, `127.0.0.1`, `localhost`, `169.254/16`) bypass the proxy

Traffic flow:

```
Your Mac (apps) -> Tor SOCKS 127.0.0.1:9050 -> Tor Network (3 relays) -> Internet
```

## What's covered

Apps that honor macOS system proxy settings (Safari, Chrome, most GUI apps) route through Tor automatically. Some apps manage their own connections and will bypass.

| App | Honors system proxy? |
|-----|---------------------|
| Safari | Yes |
| Chrome | Yes |
| Firefox | No (has its own proxy settings) |
| curl | With `--proxy socks5h://127.0.0.1:9050` |
| git (HTTPS) | Set `http.proxy` in git config |
| ssh | No (use `ProxyCommand` with `nc -X 5`) |

## Faster identity switching

By default, `newid` restarts Tor to get new circuits. For faster switching, enable the control port:

```bash
echo -e "ControlPort 9051\nCookieAuthentication 0" >> $(brew --prefix)/etc/tor/torrc
brew services restart tor
```

Then `newid` sends a `NEWNYM` signal over the control port instead of restarting.

## Limitations

- **Not a VPN.** This is a proxy. The distinction matters for threat modeling.
- **DNS leaks.** Compliant apps resolve DNS through Tor (SOCKS5). Non-compliant apps may leak DNS to your ISP.
- **Performance.** Tor adds 200-800ms latency per request. This is normal.
- **Exit node blocking.** Some sites block known Tor exit IPs. Use `tor-proxy newid` to try a different exit.

## Optional hardening

Block non-Tor traffic at the firewall level with `pf` rules:

```
block out quick on egress proto { tcp, udp } from any to any
pass out quick on egress proto tcp from any to 127.0.0.1 port 9050
pass out quick on lo0 all
```

Reload: `sudo pfctl -f /etc/pf.conf && sudo pfctl -e`. Revert: `sudo pfctl -d`.

## Requirements

- macOS
- Homebrew (tor installed as formula dependency)
