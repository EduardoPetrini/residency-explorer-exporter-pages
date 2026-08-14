# Program List Exporter for Residency Explorer

A Chrome extension (Manifest V3) that exports residency program data from
residencyexplorer.org as a CSV. You sign in yourself, in your own tab; the
extension reads the pages you already have access to and builds the file.

Not affiliated with or endorsed by AAMC.

## Status

Early. The parsing layer is complete and tested; the export flow is not wired up
yet.

| | |
|---|---|
| ✅ | Session probe — finds your Residency Explorer tab and reports whether the session is live |
| ✅ | Program page parsing — charts, salary, visa, letters, director, positions |
| ✅ | The sheet's vocabulary — `Com/Univ`, `J1 only`, `3-4`, placeholder detection |
| ✅ | Program list — pages `/Explore/GetData` until it holds the reported total |
| ✅ | Filters — region and visa, client-side, with visa as a real union |
| ✅ | Criteria — structured rule builder over the same fact table the CSV reads |
| ✅ | Columns — presets plus reorder/remove, header names written byte-for-byte |
| ✅ | Export — paced run, progress, cancel, CSV download, IndexedDB cache |
| ⛔ | Store packaging — icons, listing copy, privacy policy |

### Which specialty gets exported

Whichever one the site is currently showing. `/Explore/GetData` carries no
specialty parameter — the grid's transport is bare, and the `onAdditionalData`
hook that sends `medicalSpecialtyId` belongs to the program-search autocomplete,
not to the grid. The specialty is server-side session state.

So the panel reads the current one off the page and names it, and changing it
means changing it on the site. The alternative would be driving the site's
specialty selector and its Go button, which navigates — exactly the page-driving
this design avoids.

## How it works

The site is server-rendered, which makes this much simpler than it sounds. A
program page fires no JSON XHR: every number its charts plot is already in the
HTML as `const locals = {...}`, and the rest is plain `<th>/<td>`. So one
`fetch`, one `DOMParser`, one `JSON.parse` — no Kendo, no rendering, no
executing the page's scripts.

Three pieces:

- **content script** (`src/content/`) — the only part that touches the network.
  It runs inside your residencyexplorer.org tab, so every request is same-origin
  and carries your session.
- **side panel** (`src/sidepanel/`) — the UI and the run loop. A real document,
  so it survives a twenty-minute export; a service worker would not.
- **service worker** (`src/worker.ts`) — opens the panel. Nothing else.

The pure layer (`src/parse/`, `src/transform/`) is where every decision lives and
is the only half worth unit-testing. It never imports a `chrome.*` API.

## Privacy

Nothing is collected and nothing is transmitted anywhere. Settings and cached
program pages stay in your own browser profile, and the only site the extension
can reach is residencyexplorer.org. Full policy: [docs/privacy.html](docs/privacy.html).

## Develop

```bash
pnpm install
pnpm test         # 56 tests
pnpm typecheck
pnpm build        # -> dist/
```

Then `chrome://extensions` → Developer mode → **Load unpacked** → pick `dist/`.
After a rebuild, hit reload on the extension card; reload the Residency Explorer
tab too if the content script changed.

## Fixtures

`fixtures/synthetic/` holds invented pages, shaped like the real response, and is
committed. `fixtures/local/` is gitignored: drop pages saved from the live site
there and the tests that need them stop skipping. Never commit those — they are
login-walled content.

## Relationship to the Python crawler

The Playwright crawler in `residency-explorer/` still exists and still works. It
is the reference implementation: the pure layer here is a port of its
`charts.py`, `page_fields.py`, `field_values.py`, `facts.py`, `validation.py` and
`csv_store.py`, and the way to check this port is to run both over the same
programs and diff the rows.
