# Stats contract

The device writes these files and pushes them to this repo. A small script can build both from AdGuard Home's `GET /control/stats` API: the query total, the blocked total, and the top blocked domains.

Dates and timestamps are UTC.

## Daily stats

`stats/YYYY-MM-DD.json` is one file per UTC day.

- `queries` (int): DNS queries seen that day.
- `blocked` (int): queries blocked that day.
- `top_blocked` (array): objects with `domain` (string) and `count` (int), highest count first.

Example `stats/2026-11-07.json`:

```json
{
  "queries": 12053,
  "blocked": 842,
  "top_blocked": [
    { "domain": "doubleclick.net", "count": 120 },
    { "domain": "googleads.g.doubleclick.net", "count": 86 },
    { "domain": "pagead2.googlesyndication.com", "count": 41 }
  ]
}
```

## Heartbeat

`heartbeat.json` at the repo root is overwritten on each push.

- `last_seen` (string): ISO 8601 UTC time of the push, such as `2026-11-07T12:00:00Z`.
- `blocked_today` (int): queries blocked so far today.
- `queries_today` (int): queries seen so far today.
- `blocker` (string): product and version, such as `AdGuard Home v0.107.52`.
- `blocklist_commit` (string): full git SHA of the blocklist the device last loaded. Empty string if that SHA is not known.

Example `heartbeat.json`:

```json
{
  "last_seen": "2026-11-07T12:00:00Z",
  "blocked_today": 842,
  "queries_today": 12053,
  "blocker": "AdGuard Home v0.107.52",
  "blocklist_commit": "0123456789abcdef0123456789abcdef01234567"
}
```
