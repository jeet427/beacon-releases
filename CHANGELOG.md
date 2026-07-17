# Changelog

## v1.1.0 — 2026-07-17

Cloud Accounts — optional sign-in, zero-knowledge sync, and collection sharing. Beacon
stays 100% functional local-only and signed-out; everything below is opt-in.

- Cloud Accounts: sign in with GitHub, Google, or email. Sign-in opens in your default
  browser and reuses your existing GitHub/Google session, instead of a separate in-app
  window.
- Cloud Sync: zero-knowledge, cross-device sync of collections, environments, globals,
  workspaces, app settings, and mock routes. Secret-flagged values are encrypted
  client-side (PBKDF2 → AES-256-GCM) with a sync passphrase before upload — the server
  never sees them. Online/offline and syncing indicators on the account button; manual
  sync on click.
- Collection Sharing: publish a collection as a read-only share link. Secret values are
  stripped before publishing, and recipients can import with no account required. Manage
  and revoke your shares.
- Variable pills: `{{variables}}` render inline as highlighted pills in the omnibox,
  key-value rows, and auth fields, with hover tooltips showing the resolved value.
- Cleaner collection actions: a compact "＋" and a labeled "⋯" menu (also on right-click)
  replace the dense icon cluster on collection, folder, and request rows.
- Quality-of-life: ⌘S / Save updates an already-saved request in place; the code-snippet
  generator resolves variables to their real values; tighter URL bar and header rows; a
  cleaner account avatar; JSON body editor without the line-number gutter.

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
