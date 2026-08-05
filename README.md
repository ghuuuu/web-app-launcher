# Web App Launcher

![version](https://img.shields.io/badge/version-0.1.0-ec3013) ![no build](https://img.shields.io/badge/build-none-232019) ![storage](https://img.shields.io/badge/storage-localStorage-6b655c)

A front page for the local dev servers you keep coming back to. Add the ports you run, see which are live at a glance, and jump to any of them in a click. One HTML file, no backend, no build step — everything is stored in your browser.

## Features

- **A launcher list** of your dev servers, each a click away in a new tab, with the site's own favicon.
- **Live status** — every server is pinged on a schedule; live ones rise to the top of each host, offline ones drop below. Re-checks keep the last known status until a fresh result lands, so the list never flickers.
- **Multiple hosts** — reach the *same* ports across different bases (e.g. `localhost` and a Tailscale IP from another device). Tick the hosts you want in the **Hosts** panel — each carries its own `http`/`https` scheme, and the list gets a section per selected host. **Select all** toggles them together.
- **Scan** — sweeps a port range plus known extras across **every selected host** and lists anything that answers but isn't on your list yet, one click to add.
- **Configurable** — set the scan range, extra ports, and how often live status re-checks (in seconds), all under **Settings**.
- **Drag to reorder**, filter by name or port, and two visual themes.
- **Private by design** — no accounts, no server, no telemetry. State lives in `localStorage` in your browser.

## Getting started

Open **`launcher.html`** in a browser — that's the whole app. It starts empty; add your first server (or **Scan** for running ones) and it remembers them.

To serve the folder instead of opening the file directly, any static server works:

```bash
python -m http.server 4173
# then open http://localhost:4173/launcher.html
```

## Using it

| Control | What it does |
| --- | --- |
| **Add server** | Add a port (name optional — type just a port to quick-add). |
| **Hosts** | Check which hosts to show and scan across; add/remove hosts; set each one's `http`/`https`. |
| **Scan** | Ping a range of ports on every selected host and add the ones that answer. |
| **Settings** | Scan range (`from` / `to` / `step`), extra ports, and the live re-check interval. |
| **Filter** | Narrow the list by server name or port. |
| **Re-check now** | Ping everything immediately instead of waiting for the next interval. |

### Themes

**Modernist** (Archivo, hard edges, red accent) is the default. Append `?theme=broadsheet` for the **Broadsheet** look (Source Serif, newspaper masthead, cyan accent):

```
launcher.html?theme=broadsheet
```

## Files

| File | What it is |
| --- | --- |
| `launcher.html` | **The app** — markup, logic, and layout in one file. |
| `modernist.css` | Modernist theme (default). |
| `broadsheet.css` | Broadsheet theme (`?theme=broadsheet`). |

## Notes

- Pure static HTML / CSS / JS — nothing to install.
- Icons (Lucide / Phosphor) and fonts (Google Fonts) load from CDNs; offline, the app still works with fallback glyphs and system fonts.
- Live checks and scanning use no-CORS `fetch` pings, so a server counts as "up" if it answers at all — the launcher never reads its response.

## License

[MIT](LICENSE) © Gerald Untario

## Changelog

### 0.1.0
- First release: launcher list with live status, multi-host selection (checkboxes + Select all) with a section per host, cross-host scanning, configurable re-check interval, no-flicker status updates, drag-to-reorder, filter, and Modernist / Broadsheet themes.
