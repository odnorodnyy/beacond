# Changelog

## v0.6.1 — 2026-07-14
- Fix: TCP checks with no explicit `timeout` fell back to 0s instead of
  the 5s default.
- Fix: textfile collector output was missing a trailing newline, which
  some node_exporter versions treated as a partial write.

## v0.6.0 — 2026-06-02
- Add: `darwin-arm64` release target.
- Add: `previous_state` field in webhook payload.
- Change: default poll interval lowered from 60s to 30s.

## v0.5.0 — 2026-04-18
- Add: Prometheus textfile output (`prometheus_textfile` config key).
- Add: per-target `timeout` override.
- Fix: config reload on SIGHUP no longer drops in-flight checks.

## v0.4.0 — 2026-02-09
- Add: TCP target type (previously HTTP-only).
- Add: `linux-arm64` release build.

## v0.3.1 — 2025-12-20
- Fix: webhook retries could duplicate alerts under sustained 5xx
  responses from the receiving end.

## v0.3.0 — 2025-11-30
- Add: webhook alerting.
- Change: state machine rewritten to avoid alert spam on flapping
  checks — only transitions fire, not every poll.

## v0.2.0 — 2025-10-11
- Add: YAML config file support (previously flags only).
- Add: multiple targets per run.

## v0.1.0 — 2025-09-02
- Initial release. Single HTTP target, stdout logging only.
