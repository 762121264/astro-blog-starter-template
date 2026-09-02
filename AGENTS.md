# Base44 Dev Environment

## Project
Astro 5 blog starter template (static/markdown content) with the `@astrojs/cloudflare` adapter. No backend, no database, no external services — no secrets required.

## Running
```
docker compose -f docker-compose.base44.yml up -d
```
- Web service: `node:22` base image, source bind-mounted at `/app`, `node_modules` in a named volume.
- Dev command: `npx astro dev --host 0.0.0.0 --port 3000` (runs `npm install` first).
- Healthcheck: `GET /` on port 3000.
- Live reload is active — edits to `src/` appear in the preview without a restart.

## Notes / Quirks
- `astro.config.mjs` has `server.host: true` and `server.allowedHosts: true` so the preview's external hostname is accepted (Astro/Vite blocks unknown hosts by default).
- The Cloudflare adapter emits warnings during dev (no server-rendered pages, sharp not supported at runtime) — these are harmless for local dev.
- `worker-configuration.d.ts` and `wrangler.json` are for Cloudflare deployment only; not used by the dev server.

## Verify
- `curl -sf http://localhost:3000/` returns the blog homepage (`<title>Astro Blog</title>`).
- `curl -sf -H "Host: external-preview.example.com" http://localhost:3000/` must also succeed (external host check).
