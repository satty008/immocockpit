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

Static app served via `nginx:alpine` on port 8090. `docker-compose.yml` pulls the prebuilt
image from GHCR (`ghcr.io/satty008/immocockpit:latest`, `pull_policy: always`) rather than
building locally:

```
docker compose pull && docker compose up -d
```

For local development against the `Dockerfile` directly (e.g. before a change has been
pushed/built), override the image with a local build: `docker compose build && docker compose up -d --no-deps`
or just run `docker build -t immocockpit-local . && docker run --rm -p 8090:80 immocockpit-local`.

## CI/CD

`.github/workflows/docker.yml` builds and pushes `ghcr.io/satty008/immocockpit` on every push
to `master` (and on tags), then smoke-tests the freshly pushed image via `docker compose`
before finishing. Mirrors the same pattern used for `satty008/fredy`.

## Deployment

Runs on the `productivity` host (`~/repos/immocockpit`), reverse-proxied at
`https://immocockpit.bhavibhavan.duckdns.org` via the Nginx Proxy Manager instance on
`velocitail`. No auth in front of it — same as this box's other personal tools. To pick up a
new push: `git pull && docker compose pull && docker compose up -d` on `productivity`.

## PWA

Installable as a home-screen app (manifest + service worker, `web/manifest.json` /
`web/sw.js`). The service worker precaches the app shell and serves it network-first
(falling back to cache when offline); bump `CACHE_NAME` in `sw.js` when shipping a
change so clients pick it up promptly.

## Notes

- No persistence beyond browser session state. Adding saved scenarios across
  devices/sessions would need real persistence (localStorage at minimum, or a
  backend + DB).
