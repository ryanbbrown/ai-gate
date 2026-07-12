# Changelog

All notable changes to AI Gate are documented here.

---

## [4.7.1] — 2026-07-12

### Fixed
- **Google OAuth / "Continue with Google" sign-in** — `allowpopups` was passed as a JSX boolean shorthand on the `<webview>` element. React 19 silently drops boolean shorthands on custom elements, so the attribute never reached the DOM and Chromium blocked every pop-up before Electron's handlers could run. Passing the string `"true"` makes the attribute actually render.

### Added
- **Pop-up confirmation dialog** — instead of a hard-coded Google-only exception, any `http`/`https` pop-up from a webview now opens hidden and waits for a native **Allow / Always Allow / Decline** dialog naming the opener and destination before being shown.
- **Persistent pop-up allowlist** — "Always Allow" persists the `opener → destination` domain pair to `popup-allowlist.json` in the app data directory. Matching pairs skip the dialog on subsequent opens.
- **Purge allowlist** — Settings → General → Advanced → **Purge** clears all saved pop-up permissions and shows how many entries were removed.

### Security
- Replaced the single `accounts.google.com` hard-coded exception with a user-consent dialog that covers any site, removing the need to hard-code trusted domains as more providers are added.
- New pop-up windows retain hardened defaults: `nodeIntegration: false`, `contextIsolation: true`, no preload, web security on.

### Contributors
- @n3yne — all changes in this release

---

## [4.7.0] — 2026-07-03

### Added
- **Drag-to-reorder tools in the sidebar** — drag tools to any position; order persists across restarts. Built on `@dnd-kit/sortable` with a `PointerSensor`, `restrictToVerticalAxis`, and `restrictToParentElement`. Expanded sidebar shows a grip handle on hover; collapsed sidebar uses a full-item drag overlay.
- **Ctrl+scroll zoom in webviews** — scrolling with Ctrl held steps through the same zoom levels as `Ctrl++` / `Ctrl+-`. Routes through the existing `webviewZoom` system with no new IPC surface.

### Fixed
- **Tab drag causing a black window** — two root causes resolved: `translate3d()` in `Tab.tsx` promoted elements to a GPU compositing layer conflicting with Electron's webview GPU rendering on Windows (switched to 2D `translate()`); and `reorderTabInPanel()` received indices computed against the full global list but resolved them against a panel-local list, producing out-of-bounds splices that crashed the renderer in synced-tabs mode.
- **Tab reorder had no effect in synced-tabs mode** — `TabBar.handleDragEnd` now calls `reorderInstances()` in synced mode and `reorderTabInPanel()` in per-panel mode.
- **Sidebar drag, tab overflow, and auto-collapse layout edge cases.**
- **Redundant `UpdateService` polling interval** — removed the internal `setInterval` that fired network requests every 12 h and discarded results; `UpdateContext` already owns the polling lifecycle.
- **Dead `window.openExternal` assignment in preload** — unreachable under `contextIsolation: true`.

### Security
- **`shell.openExternal` now uses an allowlist** — only `https:` and `http:` are permitted (previously a blocklist of dangerous schemes). Any scheme not explicitly listed is rejected by default.
- **Content-Security-Policy tightened to per-directive rules** — replaced the broad `default-src 'self' 'unsafe-inline' 'unsafe-eval' *` fallback with explicit `script-src`, `style-src`, `font-src`, `img-src *`, and `connect-src *` directives.

### Contributors
- @n3yne — all changes in this release

---

## [4.6.0] — 2026-05-23

### Added
- **Customizable keyboard shortcuts** — configure shortcuts for tab switching, pane navigation, tab creation, close, and rename. Three built-in presets: Original (defaults), iTerm (`Ctrl/Cmd+1…9`), and tmux (prefix-key). Includes prefix-mode with a 2-second timeout and `Escape` to cancel, plus a shortcut recording UI.
- **Browser zoom controls** — `Ctrl/Cmd +/-/0` zoom in, out, and reset inside provider webviews.
- **Tab numbers** — shown in the tab bar for quick reference.
- **Provider context menus** — right-click copy, inspect, etc. inside webviews.
- **Google auth popups** — Google sign-in popups from webviews now open in-app with the shared session.

### Fixed
- **Wayland: black window on launch** — switched from `--ozone-platform=wayland` to `--ozone-platform-hint=auto`.
- **Wayland: silent feature flag drops** — flags are now merged into a single `--enable-features` switch before launch.
- **Wayland: `ELECTRON_ARGS` parser** — flags of the form `--enable-features=A,B` now correctly split by comma.
- **Vite dev server crashing with `ELOOP`** when `flatpak/build-dir` contained mounted filesystems with circular symlinks.
- **Flatpak crash on launch** — added `--in-process-gpu` flag.
- **Provider icons** now fall back to favicons before the generic placeholder.
- **macOS icon** inset to 824×824 inside a 1024×1024 canvas for correct native proportions.

### Contributors
- @ryanbbrown — keyboard shortcuts, browser zoom, tab numbering, E2E test suite, and numerous bug fixes.

---

## [4.5.0] — 2026-03-05

See the [v4.5.0 release](https://github.com/inulute/ai-gate/releases/tag/v4.5.0) for details.

---

## [4.0.1] — 2025-10-06

See the [v4.0.1 release](https://github.com/inulute/ai-gate/releases/tag/v4.0.1) for details.

---

## [3.0.0] — 2024-10-27

See the [v3.0.0 release](https://github.com/inulute/ai-gate/releases/tag/v3.0.0) for details.

---

## [2.0.0] — 2024-03-16

See the [v2.0.0 release](https://github.com/inulute/ai-gate/releases/tag/v2.0.0) for details.

---

## [1.0.0] — 2023-08-04

Initial release.
