# Changelog

## v1.0.0 — 2026-07-15

First public build of Beacon — a keyboard-first desktop API client.

- Command-deck shell: omnibox (URL / commands / search / `{{variable}}` autocomplete /
  cURL import), pinnable side panels, side-by-side request/response workbench.
- Collections, environments, history, cookies, mock server, WebSocket client.
- Pre-request and test scripts (`pm.*` sandbox) at both the request and collection level,
  including `pm.crypto` (HMAC/digest) for signed APIs.
- Response diff/compare (previous run, pinned baseline, or cross-environment), send-and-
  watch polling, a full DNS → TCP → TLS → TTFB → download timing waterfall.
- JSONPath filter and auto-table view for JSON responses; image/PDF/HTML preview.
- No-code assertion builder that compiles to real, runnable test scripts.
- API Monitors — schedule a saved request on an interval with uptime/latency history and
  desktop notifications on status change.
- Saved response examples, one click from example → generated mock route.
- Native macOS look, light/dark/system theme.
