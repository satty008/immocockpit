# Immo Cockpit

An English-language rebuild of the "immocation Kalkulationstool Cockpit - Pro" German
buy-to-let property calculator, plus additional features: scenarios, compare, bank-meeting
sheet, household budget, net worth, multi-unit/MFH mode, furnished-let mode, renovation
planner, special loan repayments, and verdict scoring.

Single-file interactive web app, in-browser only — no backend, no database. All state lives
in the browser (React component state via a small custom runtime, `support.js`); nothing is
persisted server-side. Scenarios and inputs reset if browser storage is cleared.

## Running locally

Open `web/index.html` directly in a browser — it's fully self-contained and works with no
build step or server.

## Docker

```
docker compose up -d --build
```

Serves the static app via `nginx:alpine` on port 8090.

## Deployment

Runs on the `productivity` host, reverse-proxied at
`https://immocockpit.bhavibhavan.duckdns.org` via the Nginx Proxy Manager instance on
`velocitail`. No auth in front of it — same as this box's other personal tools.

## Notes

- No persistence beyond browser session state. Adding saved scenarios across
  devices/sessions would need real persistence (localStorage at minimum, or a
  backend + DB).
- No PWA manifest/service worker yet — the UI is mobile-responsive but not
  installable as a PWA today.
