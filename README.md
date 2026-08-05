# Web App Launcher

A single-page launcher / front page for the local dev servers you keep coming back to. Add the ports you run, see which are live at a glance, and jump to any of them in a click. Everything is stored in your browser — no backend, no build step.

## Files

| File | What it is |
| --- | --- |
| `launcher.html` | **The app.** Open this to use it; it starts empty and remembers your servers in this browser. |
| `modernist.css` | Modernist theme (default) — Archivo, hard edges, red accent. |
| `broadsheet.css` | Broadsheet theme — Source Serif, newspaper masthead, cyan accent. |

## Using it

Open **`launcher.html`** in a browser.

- **Hosts** — check the hosts you want to show. The list gets a section per selected host, and the same ports are reached across every one (e.g. `localhost` and a Tailscale IP). Each host carries its own `http`/`https` scheme. **Select all** toggles them together.
- **Live status** — every host×server pair is pinged on a schedule; live servers rise to the top of each host's section, offline ones drop below. Re-checks keep the last known status until a fresh result lands, so the list doesn't flicker.
- **Scan** — sweeps a port range (default `3000→9000` step `100`) plus extras like Vite `5173`, Headroom `8787`, claude-mem `37777` across **every selected host**, and lists anything that answers but isn't on your list yet.
- **Settings** — edit the scan range, extra ports, and how often live status re-checks (in seconds).

### Themes

Modernist is the default. Append `?theme=broadsheet` to switch:

```
launcher.html?theme=broadsheet
```

## Notes

- Pure static HTML/CSS/JS. Serve the folder with any static server, or open the files directly.
- Icons (Lucide / Phosphor) and fonts (Google Fonts) load from CDNs; without a connection the app still works, just with fallback glyphs and system fonts.
