# beacond

Lightweight heartbeat & endpoint monitor. Single static binary, no
dependencies, no database — just a config file and a place to send alerts.

`beacond` polls a list of HTTP(S) and TCP endpoints on an interval you
define, and pushes state changes to a webhook, a Prometheus textfile,
or both. Runs comfortably on a $5 VPS next to whatever else you have
going on.

## Why

Most monitoring stacks are heavier than the thing they're monitoring.
`beacond` is meant for the "I have six servers and want to know when
one falls over" case — not a replacement for Prometheus + Alertmanager
at scale, just a small always-on process that notices before you do.

## Install

Grab a binary from [Releases](../../releases) — no build step, no
runtime dependencies.

```bash
curl -L -o beacond https://github.com/USER/beacond/releases/latest/download/beacond-linux-amd64
chmod +x beacond
./beacond -config beacond.yml
```

Supported targets: `linux-amd64`, `linux-arm64`, `darwin-amd64`,
`darwin-arm64`.

## Config

```yaml
interval: 30s

targets:
  - name: api
    type: http
    url: https://api.example.com/health
    expect_status: 200
    timeout: 5s

  - name: db-tcp
    type: tcp
    address: 10.0.0.5:5432
    timeout: 3s

alerts:
  webhook: https://hooks.example.com/beacond
  prometheus_textfile: /var/lib/node_exporter/textfile_collector/beacond.prom
```

Each target is checked independently on its own goroutine; a slow
target never blocks the others. State transitions (up → down, down →
up) are what trigger alerts — not every poll, so you don't get spammed
on a flapping check.

## Prometheus

When `prometheus_textfile` is set, `beacond` writes metrics in
node_exporter textfile format on every check cycle:

```
beacond_target_up{name="api"} 1
beacond_target_latency_seconds{name="api"} 0.084
beacond_target_last_change_timestamp{name="api"} 1780531200
```

Point `node_exporter --collector.textfile.directory` at the same path
and scrape as usual.

## Webhook payload

```json
{
  "target": "api",
  "state": "down",
  "previous_state": "up",
  "since": "2026-08-30T14:02:11Z",
  "latency_ms": null,
  "error": "context deadline exceeded"
}
```

## Status

Personal project, used on my own infra. Stable enough that I haven't
had to think about it in months, which is the goal. Issues and PRs are
welcome but responses may be slow — this isn't anyone's day job.

## License

MIT — see [LICENSE](LICENSE).
