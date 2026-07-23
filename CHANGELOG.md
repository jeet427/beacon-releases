# Changelog

## v1.3.4 — 2026-07-23

A security and reliability release. A full audit of the app turned up a batch of
defects — several of them credential leaks — and this release fixes them.

**Security**

- **Cloud Sync no longer uploads your credentials in plaintext.** Auth tokens,
  passwords, OAuth client secrets, `Authorization` headers, and credentials
  embedded in a URL are now encrypted before they leave your machine, the same
  way secret variables already were. (Previously only secret *variables* were
  encrypted; everything else synced in the clear.) On your next unlock the app
  re-uploads existing data so the older plaintext copies are overwritten.
- **Shared collections no longer leak secrets.** Publishing a collection now
  also strips API keys placed in query parameters, credentials embedded in a
  URL (`https://user:pass@host`), usernames, and local file paths — not just the
  auth fields it stripped before.
- Credential headers are no longer replayed across a redirect to a different
  host, and shared/imported collections keep their scripts disabled until you
  explicitly trust them (closing a way a malicious import could run code).

**Requests**

- Path parameters (`/users/:id`) and auth headers are no longer dropped when a
  pre-request script runs.
- Requests now send a `Content-Length` instead of chunked encoding, fixing
  uploads to S3 presigned URLs, signed APIs, and stricter gateways.
- Cookies now honour `Secure`, `Path`, and `Expires`/`Max-Age` correctly, and a
  server setting several cookies at once no longer loses their expiry.
- The Collection Runner and Flows now send file uploads and binary bodies (they
  were silently sent empty), and generated code snippets and the GraphQL
  "Fetch schema" now match what the app actually sends (query params, auth,
  cookies, proxy, JSON comments).

**Reliability**

- **Cancel** now actually stops a streaming or long-running response instead of
  spinning forever, and a very large response no longer wedges the request.
- Load tests and WebSocket connections now stop when you close the window.
- **Watch** keeps polling when you switch tabs instead of silently pausing, and
  each tab keeps its own counter and baseline.
- Script errors (pre-request and test) are now surfaced instead of being
  silently swallowed, and one failing test no longer discards the rest.

**Fixes**

- Pinned tabs no longer come back after you close them, and edits to a pinned
  tab are no longer lost on restart; tab order is remembered.
- Saving a request no longer erases its response Schema, and open tabs refresh
  after Find & Replace or a cloud pull instead of overwriting the change.
- ⌘/ and ⌘K are no longer stolen from the code editor while you're typing.
- Removing a collection from a workspace now actually sticks, and deleting a
  request cleans up its monitors instead of leaving them silently dead.

## v1.3.1 — 2026-07-20

- Flows: a new **Input** node pauses a running flow and shows a small form — the
  values you type become flow variables for every node downstream. Use it for a
  value you only know at run time (an order id, an OTP, a one-off token) or one
  that depends on an earlier response; Cancel aborts the run. Fields can be marked
  secret to mask them, and default values pre-fill the form.
- New: a complete in-app **[user guide](user-guide/README.md)** covering every
  feature — from your first request through variables, scripts, collections, the
  runner, flows, load testing, mocks, monitors, and cloud sync.

## v1.3.0 — 2026-07-19

- **Living Response** — a new interactive way to read JSON responses. Explore the
  body as a tree, hover any field to copy, extract, filter, or pin it, and pin
  fields to a Pinboard that charts how each value changes across sends (with
  sparklines and up/down deltas). Values are rendered richly inline — relative
  time for timestamps, a swatch for colors, links for URLs, human sizes for byte
  counts, and one-click decode for JWTs — plus a table view for arrays of objects
  and ⌘F find across both keys and values.
- **Flows** — chain requests visually on a canvas. Wire together Request,
  Condition, Extract, Delay, and Script nodes so one response feeds the next, and
  drag a saved request straight onto the canvas to add it.
- **Load-test request capture** — a run can now capture individual request and
  response samples (all requests, errors only, or off), stored locally on your
  device, with a per-sample detail view and an errors-only filter.
- Helpful tooltips throughout the app explain what each control does.
- Shell: every sidebar item now opens as a docked list with its editor in a
  workbench tab, and collection settings open in their own tab.

Fixes:

- Request history now actually persists between sessions — it was silently not
  saving.
- Security: scripts in imported or synced collections no longer run until you
  explicitly trust the collection; credential headers (Authorization, Cookie,
  API keys) are stripped from public shares and from cross-origin redirects; and
  collection imports fail closed.
- Load tests now send cookies from the cookie jar; generated code snippets resolve
  inherited and OAuth 2.0 auth and build GraphQL bodies correctly; and the
  environment editor no longer drops keystrokes while you type.
- A range of correctness fixes across HTTP handling, sync, scripts, monitors, and
  cURL import.

## v1.2.1 — 2026-07-18

- Environments, Load Tests, and Monitors now open as tabs (like requests) instead
  of taking over the whole window: pick one from its sidebar list and it opens in
  a workbench tab you can edit alongside your requests. The sidebar list stays
  docked open beside the editor — clicking an item no longer collapses it, and
  its rail icon toggles the whole panel.
- The last tab can now be closed — you get a clean "No tab open" empty state
  (press ⌘T for a new request) instead of a blank request being forced open.
- New app icon: refreshed, larger (now the standard macOS size so it matches other
  apps in your dock), with a background that follows your chosen accent color for
  the built-in accent presets.
- The sync button now shows a subtle greyed-out state when you're signed out or
  sync is off, instead of disappearing.

Fixes:

- Following a "303 See Other" redirect now keeps your Authorization and other
  headers and drops the previous request body — it was previously doing the
  opposite (dropping headers, re-sending the body).
- Background cloud sync no longer jumps you to a different tab while you're
  editing.
- Monitors now run a request exactly as saved — path variables, folder-inherited
  auth/headers, and response-variable extraction rules are all applied — so you
  get fewer false "down" alerts.
- Scripts: variables a collection script sets are now visible to the request's
  own script, `pm.environment.unset()` / `pm.globals.unset()` actually remove a
  variable, and collection scripts run before request scripts.
- Import from curl keeps passwords that contain a colon and no longer mistakes a
  flag's value for the URL.
- Environments and global variables still load if the system keychain is briefly
  unavailable, and "compare with previous response" now works again.

## v1.2.0 — 2026-07-18

- Full-screen workspaces: Environments, Load Tests, Monitors, Connections
  (Mock/WebSocket), and Flows now open full-screen in the main area instead of a
  cramped sidebar. Collections and History stay in the quick side panel. Press
  the sidebar icon again, Esc, or click a request tab to return.
- Environments, rebuilt: a full-screen editor with the environment list beside
  its variables — a clear "Set Active" button and Active badge, inline rename,
  secret masking, and one-click Postman environment import (no more popup dialog).
- Load testing with real charts: every run now records and keeps a latency
  histogram and time series, so past runs show full graphs — latency over time
  (avg/p50/p95/p99), throughput and virtual users, latency distribution, and
  status breakdown. A new History tab browses runs across all your load tests,
  and you can compare two runs side by side.
- Separate sync button: the sync indicator is now its own button, distinct from
  your profile — clicking your name just opens the account menu. It clearly shows
  in-sync, out-of-sync (with a pending-change count), syncing, locked, offline,
  and error states.
- Collection & folder auth + headers: set authentication and default headers once
  on a collection or folder and let requests inherit them (choose "Inherit from
  parent" on a request's Auth tab).
- Saved assertions & response variables: the assertion builder's rules now persist
  with the request (and sync/share with it), and a new "Vars" tab extracts values
  from a response (body/header/status) into an environment or global variable — no
  scripting needed.
- Better Postman import: imported requests keep their descriptions, path variables,
  and file form-fields; anything unsupported is listed in an "imported with
  warnings" summary.
- Collection Runner: added a stop-on-failure option and the ability to run a single
  folder.
- Markdown docs preview: the Docs tab on requests, collections, and folders now
  renders Markdown with an Edit/Preview toggle.

## v1.1.3 — 2026-07-17

- Cloud Sync: collections now appear correctly across all your devices. Every
  device's "Default" workspace is the same synced workspace (existing duplicate
  "Default" workspaces are merged automatically on first launch).
- Check for Updates: Beacon menu → "Check for Updates…", plus a Settings row and
  a quiet once-a-day launch check. Updates download and install in place with
  one click — just restart to finish (no browser needed).
- Folder colors: assign a color to collection folders from the sidebar.
- Fixes: pinned tabs no longer duplicate after a sync; collection refreshes no
  longer flash other workspaces' collections; Monaco editor focus-ring cleanup.

## v1.1.2 — 2026-07-17

- First Windows build: `Beacon-Setup-<version>.exe` (unsigned NSIS installer —
  SmartScreen will ask for "More info → Run anyway"). No macOS changes.

## v1.1.1 — 2026-07-17

Patch release.

- Fix: enabling Cloud Sync could trigger a rapid pull loop that flickered the UI and made
  the app unresponsive (no data was lost or corrupted). Sync now settles to idle and only
  runs when there are actual changes to push or pull.

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
