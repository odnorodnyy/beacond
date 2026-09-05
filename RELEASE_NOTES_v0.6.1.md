## v0.6.1

Small fix release.

**Fixed**
- TCP checks with no explicit `timeout` fell back to 0s instead of the
  5s default — could cause false "down" alerts on slower networks.
- Prometheus textfile output was missing a trailing newline, which
  some `node_exporter` versions treated as a partial write and
  silently dropped.

No config changes needed, drop-in replacement for v0.6.0.

**Checksums**
```
5b83925fddc17faeb94d3eddb12814a78bb7517ba8f0368cd35cb46ec914814c  beacond-darwin-amd64
46014e99a8808227cd2263b825e530761984187591e21691ef827f7c031a2d19  beacond-darwin-arm64
afe4d40702e247f95b27ed6d0d6322e09832c288f8b2a72704228e9aca03f4dd  beacond-linux-amd64
113f043ec3b4b4419a0cd7b275e1afe3a0495bc24f7e9238e28128e21afde19e  beacond-linux-arm64
```
