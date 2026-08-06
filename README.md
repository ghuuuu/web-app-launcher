# Web App Launcher

![version](https://img.shields.io/badge/version-0.3.2-ec3013) ![no build](https://img.shields.io/badge/build-none-232019) ![PWA](https://img.shields.io/badge/PWA-installable-2d6cdf) ![storage](https://img.shields.io/badge/storage-localStorage-6b655c)

A front page for the local dev servers you keep coming back to. Add the ports you run, see which are live at a glance, and jump to any of them in a click. One HTML file, no backend, no build step — everything is stored in your browser.

## Features

- **A launcher list** of your dev servers, each a click away in a new tab, with the site's own favicon.
- **Live status** — every server is pinged on a schedule; live ones rise to the top of each host. Re-checks keep the last known status until a fresh result lands, so the list never flickers.
- **Multiple hosts, side by side** — reach the *same* ports across different bases (e.g. `localhost` and a Tailscale IP). Tick the hosts you want in the **Hosts** panel; each becomes its own full-width column (hosts with live servers sort left), each carries its own `http`/`https`, and **Select all** toggles them together. Only what's live on a host shows there — a **Show offline** toggle reveals the rest.
- **Scan, three ways** — **Smart** (70+ well-known dev & self-hosted ports: Vite, Next, Immich, Nextcloud, NocoBase, Ollama, Jellyfin…), **Custom** (your configured range + extras), or **Full** (every port `1–65535`). Scans run in the **background** with a progress bar showing what's being checked; anything found that isn't on your list is one click to add, across **every selected host**.
- **Installable (PWA)** — add it to your home screen / dock and it opens like a native app, and loads offline.
- **Mobile-friendly** — on small screens hosts collapse to tabs (swipe or tap between them); everything stays touch-sized.
- **Export / import** — copy a short code and paste it into the app in another browser to carry your hosts and settings across.
- **Tags** — label servers (frontend / api / db…), see them as chips, and filter the list by tag.
- **Dark mode** — follows your system light/dark preference automatically.
- **Accessible** — reduced-motion support, keyboard focus trapping in dialogs, and text labels alongside every status dot.
- **Configurable** — set the scan range, extra ports, and how often live status re-checks (in seconds), under **Settings**.
- **Drag to reorder** (mouse or touch), filter by name or port, and two visual themes.
- **Private by design** — no accounts, no server, no telemetry. State lives in `localStorage` in your browser.

## Getting started

Open **`index.html`** in a browser — that's the whole app. It starts empty; add your first server (or **Scan** for running ones) and it remembers them.

To serve the folder instead of opening the file directly, any static server works:

```bash
python -m http.server 4173
# then open http://localhost:4173/index.html
```

> **Heads up:** the launcher checks `http://localhost` servers from your browser, so it must run from an **http** origin (open the file locally, or serve it over http on your machine). If you host it over **HTTPS** (e.g. Cloudflare Pages), the browser blocks its requests to `http://localhost` as mixed content, and every server shows offline.

## Portable single file (best for real use)

The launcher's live checks and scans reach your servers from **your browser**, and browsers block requests from an HTTPS page to `http://localhost` (mixed content). So a copy **hosted online over HTTPS can display the UI but can't actually check or scan your machine** — every server shows offline and scans find nothing.

The fix is to **run it locally**. The easiest way is the portable build: download **[`web-app-launcher.html`](web-app-launcher.html)** — the entire app in one file, with the CSS inlined (Modernist theme, no external files). Save it anywhere and open it directly (or serve it over http on your machine), and its checks/scans can reach your `http://localhost` servers.

*(`web-app-launcher.html` is generated from `index.html` + `modernist.css` by `node build-portable.js`; edit those and rebuild.)*

## Install as an app

Open the app in a browser and choose **Install** (desktop: the install icon in the address bar; iOS Safari: Share → *Add to Home Screen*; Android Chrome: menu → *Install app*). It then opens in its own window and works offline. Installing needs an `https` or `localhost` origin (service workers don't run from `file://`).

## Move it to another browser

Open **Export / Import** (the ⤒ button in the toolbar), **Copy code**, then paste that code into the Import box on the other browser and press **Import**. The code is a compressed, URL-safe string — short and not human-readable — that carries all your hosts, selected hosts, services, and scan settings. Importing replaces the current setup.

## Using it

| Control | What it does |
| --- | --- |
| **Add server** | Add a port (name optional — type just a port to quick-add). |
| **Hosts** | Check which hosts to show and scan across; add/remove hosts; set each one's `http`/`https`. |
| **Scan** | Smart / Custom / Full port scan across every selected host, in the background with a progress bar. |
| **Show offline** | Reveal servers that aren't live on a host (dimmed, at the bottom); off by default. |
| **Settings** | Scan range (`from` / `to` / `step`), extra ports, and the live re-check interval. |
| **Tags** | Add comma-separated tags when editing a server; filter by them from the tag bar. |
| **Export / Import** | Copy a code to move hosts + settings to another browser, or paste one to import. |
| **Filter** | Narrow the list by server name or port. |
| **Re-check now** | Ping everything immediately instead of waiting for the next interval. |

### Themes

**Modernist** (Archivo, hard edges, red accent) is the default. Append `?theme=broadsheet` for the **Broadsheet** look (Source Serif, newspaper masthead, cyan accent):

```
index.html?theme=broadsheet
```

## Files

| File | What it is |
| --- | --- |
| `index.html` | **The app** — markup, logic, and layout in one file. |
| `modernist.css` | Modernist theme (default), with a dark variant. |
| `broadsheet.css` | Broadsheet theme (`?theme=broadsheet`), with a dark variant. |
| `web-app-launcher.html` | **Portable single-file build** — CSS inlined, no external files. Download and open locally. |
| `build-portable.js` | Node script that generates the portable file from `index.html` + `modernist.css`. |
| `manifest.webmanifest` | PWA manifest (install metadata). |
| `sw.js` | Service worker — caches the app shell for offline / instant loads. |
| `icon.svg` | App icon (vector) — source for the favicon and PWA icons. |
| `icon-192.png`, `icon-512.png` | Raster app icons (generated from `icon.svg`). |

## Notes

- Pure static HTML / CSS / JS — nothing to install.
- Icons (Lucide / Phosphor) and fonts (Google Fonts) load from CDNs; offline, the app still works with fallback glyphs and system fonts.
- Live checks and scanning use no-CORS `fetch` pings, so a server counts as "up" if it answers at all — the launcher never reads its response.

## License

[MIT](LICENSE) © Gerald Untario

## Changelog

### Unreleased
- Fix: long server names/URLs no longer make a row spill past its host column.
- Row actions (rename / delete) are now a compact vertical stack pinned to the right, and server names wrap onto multiple lines — leaving much more room for the name.

### 0.3.2
- **Portable single-file build** (`web-app-launcher.html`) with the CSS inlined — download and open it locally so localhost checks and scans actually work.
- A bit more spacing between the live-status label and a row's edit/delete buttons.

### 0.3.1
- **Edit hosts** inline (rename / change address) in the Hosts panel.
- **Tags** — give servers tags, see them as chips, and filter by tag from a tag bar.
- **Accessibility** — modal focus trap + focus return, tab roles/labels, status dots always paired with text, and `prefers-reduced-motion` support.
- **Smarter Full scan** — ports generated lazily (no memory spike) with shorter timeouts on localhost and a heads-up for slow remote hosts.
- The background **scan panel** is isolated from re-renders, so long scans stay smooth.

### 0.3.0
- **Scan modes** — Smart (70+ known dev/self-hosted ports), Custom, and Full (1–65535), each running in the background with a live progress bar and current-port readout.
- In-app **HTTPS warning** when the page is served over HTTPS but points at `http://localhost` hosts (mixed content blocks the checks).
- New **app icon** (`icon.svg`) used for the favicon and PWA/home-screen icons.

### 0.2.0
- Full-width layout: one column per host (live hosts sort left); each host shows only what's live, with a **Show offline** toggle.
- **PWA** — installable, with a service worker for offline / instant loads and app icons.
- **Dark mode** via `prefers-color-scheme` in both themes.
- **Export / Import** hosts + settings as a compressed, copy-paste code.
- **Mobile** — hosts collapse to swipeable/tappable tabs, touch-friendly row actions, pointer-based drag reorder (works on touch), long names wrap, safe-area insets. The active tab defaults to the left-most (most-online) host.

### 0.1.0
- First release: launcher list with live status, multi-host selection (checkboxes + Select all) with a section per host, cross-host scanning, configurable re-check interval, no-flicker status updates, drag-to-reorder, filter, and Modernist / Broadsheet themes.
