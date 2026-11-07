# guardian

Shared source of truth for a home DNS ad blocker running [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome). AdGuard Home subscribes to the raw `blocklist.txt` on `main` and refreshes it on its own schedule. A small script on the device pushes a heartbeat and daily stats back into this repo.

## Files

`blocklist.txt` is one domain per line. A `#` starts a comment. Each entry blocks that domain and all of its subdomains. AdGuard Home treats a plain domain list this way.

`allowlist.txt` uses the same format. It lists domains that must never be blocked. It has a header comment only until something needs an exception.

`heartbeat.json` (repo root) and `stats/YYYY-MM-DD.json` are written by the device. Field names and examples are in [stats/README.md](stats/README.md).

## Add the list in AdGuard Home

1. Open Filters > DNS blocklists > Add blocklist > Add a custom list.
2. Use this URL:

https://raw.githubusercontent.com/heemsec/guardian/main/blocklist.txt

AdGuard Home downloads that file on its filter update schedule. After this file changes on `main`, the next refresh picks it up.

## Heartbeat and stats

The script on the device reads AdGuard Home's `/control/stats` API and pushes:

- `heartbeat.json` with the latest check-in
- one `stats/YYYY-MM-DD.json` per UTC day

## Daily refresh

Daily refresh pull requests add domains from the StevenBlack unified hosts list and the OISD small list. Those PRs are reviewed before they merge to `main`. Core services stay off the blocklist: google.com, apple.com, microsoft.com, github.com, cloudflare.com, cursor.com, auth providers, and AI assistants. Anything on `allowlist.txt` stays off too.
