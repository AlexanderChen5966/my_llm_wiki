---
title: "mauriceboe/TREK: A self-hosted travel/trip planner with real-time collaboration, interactive maps, PWA support, SSO, budgets, packing lists, and more."
source: "https://github.com/mauriceboe/TREK"
author:
  - "[[mauriceboe]]"
published:
created: 2026-07-02
description: "A self-hosted travel/trip planner with real-time collaboration, interactive maps, PWA support, SSO, budgets, packing lists, and more. - mauriceboe/TREK"
tags:
  - "clippings"
status: processed
---
![TREK](https://github.com/mauriceboe/TREK/raw/main/docs/logo-trek-light.gif)

![](https://github.com/mauriceboe/TREK/raw/main/docs/subtitle-light.png)

A self-hosted, real-time collaborative travel planner — with maps, budgets, packing lists, a journal, and AI built in.

---

[![TREK — 60-second tour](https://github.com/mauriceboe/trek-media/releases/download/readme-assets/TREK1.gif)](https://github.com/mauriceboe/trek-media/releases/download/readme-assets/TREK1.gif)

[![Dashboard](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/dashboard.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/dashboard.png) [![Trip planner with 3D map](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/trip-planner.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/trip-planner.png) [![Journey journal](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/journey.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/journey.png) [![Costs · expense splitting](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/budget.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/budget.png) [![Atlas · visited countries](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/atlas.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/atlas.png) [![Vacay planner](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/vacay.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/vacay.png) [![Trip planner · day plan and route](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/trip-iceland.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/trip-iceland.png) [![Admin panel](https://github.com/mauriceboe/TREK/raw/main/docs/screenshots/admin.png)](https://github.com/mauriceboe/TREK/blob/main/docs/screenshots/admin.png)

---

## What you get

![TREK feature tiles](https://github.com/mauriceboe/TREK/raw/main/docs/tiles/grid-desktop.svg)

**See all features**

<table><tbody><tr><td width="50%"><h4>🧭 Trip planning</h4><ul><li><strong>Drag & drop planner</strong> — organise places into day plans with reordering and cross-day moves</li><li><strong>Interactive map</strong> — Leaflet or Mapbox GL with 3D buildings, terrain, photo markers, clustering, route visualization</li><li><strong>Place search</strong> — Google Places (photos, ratings, hours) or OpenStreetMap (free, no API key)</li><li><strong>Place import</strong> — shared Google Maps / Naver Maps lists, plus GPX and KML/KMZ/GeoJSON map files</li><li><strong>Day notes</strong> — timestamped, icon-tagged notes with drag-and-drop reordering</li><li><strong>Route optimisation</strong> — auto-sort places and export to Google Maps</li><li><strong>Weather forecasts</strong> — 16-day via Open-Meteo (no key) + historical climate fallback</li><li><strong>Category filter</strong> — show only matching pins on the map</li></ul></td><td width="50%"><h4>🧳 Travel management</h4><ul><li><strong>Reservations</strong> — flights, accommodations, restaurants with status, confirmation numbers, files; import from booking confirmation emails and PDFs (<a href="https://invent.kde.org/pim/kitinerary">KDE Itinerary</a>)</li><li><strong>Costs</strong> — track and split trip expenses (Splitwise-style): per-person / per-day breakdowns, settle-up, multi-currency</li><li><strong>Packing lists</strong> — categories, templates, user assignment, progress tracking</li><li><strong>Bag tracking</strong> — optional weight tracking with iOS-style distribution</li><li><strong>Document manager</strong> — attach docs, tickets, PDFs to trips / places / reservations (≤ 50 MB each)</li><li><strong>PDF export</strong> — full trip plan as PDF with cover page, images, notes</li></ul></td></tr><tr><td width="50%"><h4>👥 Collaboration</h4><ul><li><strong>Real-time sync</strong> — WebSocket. Changes appear instantly across all connected users</li><li><strong>Multi-user trips</strong> — invite members with role-based access</li><li><strong>Invite links</strong> — one-time or reusable links with expiry</li><li><strong>SSO (OIDC)</strong> — Google, Apple, Authentik, Keycloak, or any OIDC provider</li><li><strong>2FA</strong> — TOTP + backup codes</li><li><strong>Passkeys</strong> — passwordless WebAuthn login (fingerprint / face / PIN / security key), admin-toggleable</li><li><strong>Collab suite</strong> — group chat, shared notes, polls, day check-ins</li></ul></td><td width="50%"><h4>📱 Mobile & PWA</h4><ul><li><strong>Installable</strong> — iOS and Android, straight from the browser, no App Store needed</li><li><strong>Offline support</strong> — Service Worker caches tiles, API, uploads via Workbox</li><li><strong>Native feel</strong> — fullscreen standalone, themed status bar, splash screen</li><li><strong>Touch optimised</strong> — mobile-specific layouts with safe-area handling</li></ul></td></tr><tr><td width="50%"><h4>🧩 Addons (admin-toggleable)</h4><ul><li><strong>Lists</strong> — packing lists + to-dos with templates, member assignments, optional bag tracking</li><li><strong>Costs</strong> — expense tracker with splits and settle-up (who owes whom), multi-currency</li><li><strong>Documents</strong> — file attachments on trips, places, and reservations</li><li><strong>Collab</strong> — chat, notes, polls, day-by-day attendance</li><li><strong>Vacay</strong> — personal vacation planner with calendar, 100+ country holidays, carry-over tracking</li><li><strong>Atlas</strong> — world map of visited countries, bucket list, travel stats, streak tracking, liquid-glass UI</li><li><strong>Journey</strong> — magazine-style travel journal with entries, photos (Immich/Synology), maps, moods</li><li><strong>AirTrail</strong> — connect a self-hosted AirTrail instance to import and sync flights into reservations</li><li><strong>MCP</strong> — expose TREK to AI assistants via OAuth 2.1</li></ul></td><td width="50%"><h4>🤖 AI / MCP</h4><ul><li><strong>Built-in MCP server</strong> — OAuth 2.1 authenticated. 150+ tools, 30 resources</li><li><strong>Granular scopes</strong> — 27 OAuth scopes across 13 permission groups</li><li><strong>Full automation</strong> — AI can create trips, plan days, build packing lists, manage budgets, mark countries visited</li><li><strong>Pre-built prompts</strong> — <code>trip-summary</code>, <code>packing-list</code>, <code>budget-overview</code></li><li><strong>Addon-aware</strong> — exposes Atlas, Collab, Vacay when those addons are on</li></ul></td></tr><tr><td colspan="2"><h4>⚙️ Admin & customisation</h4><ul><li><strong>Dashboard views</strong> — card grid or compact list · <strong>Dark mode</strong> — full theme with matching status bar</li><li><strong>20 languages</strong> — EN, DE, ES, FR, IT, NL, HU, RU, ZH, ZH-TW, PL, CS, AR (RTL), BR, ID, TR, JA, KO, UK, GR</li><li><strong>Admin panel</strong> — users, invites, packing templates, categories, addons, API keys, backups, GitHub history</li><li><strong>Notifications</strong> — per-user preferences across email (SMTP), webhook, ntfy, and an in-app notification center</li><li><strong>Auto-backups</strong> — scheduled with configurable retention · <strong>Units</strong> — °C/°F, 12h/24h, map tile sources, default coordinates</li></ul></td></tr></tbody></table>

## Get started in 30 seconds

```
ENCRYPTION_KEY=$(openssl rand -hex 32) docker run -d -p 3000:3000 \
  -e ENCRYPTION_KEY=$ENCRYPTION_KEY \
  -v ./data:/app/data -v ./uploads:/app/uploads mauriceboe/trek
```

Open `http://localhost:3000`. On first boot TREK seeds an admin account — if you set `ADMIN_EMAIL` / `ADMIN_PASSWORD` those are used, otherwise the credentials are printed to the container log (`docker logs trek`).

## Tech stack

Real-time sync via WebSocket (`ws`). Backend on NestJS 11. State with Zustand. Auth via JWT + OAuth 2.1 + OIDC + Passkeys (WebAuthn) + TOTP MFA. Weather via Open-Meteo (no key required). Maps with Leaflet and Mapbox GL.

## Docker Compose (production)

Full compose example with secure defaults
```
services:
  app:
    image: mauriceboe/trek:latest
    container_name: trek
    read_only: true
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - SETUID
      - SETGID
    tmpfs:
      - /tmp:noexec,nosuid,size=64m
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - PORT=3000
      - ENCRYPTION_KEY=${ENCRYPTION_KEY:-}   # generate with: openssl rand -hex 32
      - TZ=${TZ:-UTC}
      - LOG_LEVEL=${LOG_LEVEL:-info}
      - ALLOWED_ORIGINS=${ALLOWED_ORIGINS:-}
      - APP_URL=${APP_URL:-}                 # required for OIDC + email links
      # - FORCE_HTTPS=true                   # behind a TLS-terminating proxy
      # - TRUST_PROXY=1
      # - OIDC_ISSUER=https://auth.example.com
      # - OIDC_CLIENT_ID=trek
      # - OIDC_CLIENT_SECRET=supersecret
      # - OIDC_DISPLAY_NAME=SSO
      # - OIDC_ADMIN_CLAIM=groups
      # - OIDC_ADMIN_VALUE=app-trek-admins
    volumes:
      - ./data:/app/data
      - ./uploads:/app/uploads
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:3000/api/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 15s
```

Then:

```
docker compose up -d
```

**HTTPS notes:** `FORCE_HTTPS=true` is optional — it adds a 301 redirect, HSTS, CSP upgrade-insecure-requests, and forces the `secure` cookie flag. Only use it behind a TLS-terminating reverse proxy. `TRUST_PROXY=1` tells the server how many proxies sit in front so real client IPs and `X-Forwarded-Proto` work.

## Helm (Kubernetes)

```
helm repo add trek https://mauriceboe.github.io/TREK
helm repo update
helm install trek trek/trek
```

See [`charts/README.md`](https://github.com/mauriceboe/TREK/blob/main/charts/README.md) for values.

## Install as App (PWA)

TREK works as a Progressive Web App — no App Store needed.

1. Open TREK in the browser (HTTPS required)
2. **iOS**: Share ▸ *Add to Home Screen*
3. **Android**: Menu ▸ *Install app* (or *Add to Home Screen*)

TREK then launches fullscreen with its own icon, just like a native app.

## Updating

**Docker Compose:**

```
docker compose pull && docker compose up -d
```

**Docker run** — reuse the original volume paths:

```
docker pull mauriceboe/trek
docker rm -f trek
docker run -d --name trek -p 3000:3000 -v ./data:/app/data -v ./uploads:/app/uploads --restart unless-stopped mauriceboe/trek
```

> Not sure which paths you used? `docker inspect trek --format '{{json .Mounts}}'` before removing the container.

Your data stays in the mounted `data` and `uploads` volumes — updates never touch it.

> [!important] Important
> Mount **only** the data and uploads directories — `-v ./data:/app/data -v ./uploads:/app/uploads`. **Never mount a volume at `/app`.** Doing so hides the application code shipped in the image and the container fails to start with `Cannot find module 'tsconfig-paths/register'`. If you previously mounted `/app`, switch to the two mounts above; your data in `data/` and `uploads/` is preserved.

### Rotating the Encryption Key

If you need to rotate `ENCRYPTION_KEY` (e.g. upgrading from a version that derived encryption from `JWT_SECRET`):

```
docker exec -it trek node --import tsx scripts/migrate-encryption.ts
```

The script creates a timestamped DB backup before making changes and prompts for old + new keys (input is not echoed).

## Reverse Proxy

For production, put TREK behind a TLS-terminating reverse proxy. TREK uses WebSockets for real-time sync, so the proxy **must** support WebSocket upgrades on `/ws`.

Nginx
```
server {
    listen 80;
    server_name trek.yourdomain.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name trek.yourdomain.com;

    ssl_certificate     /etc/ssl/fullchain.pem;
    ssl_certificate_key /etc/ssl/privkey.pem;

    # 500 MB covers backup-restore uploads (capped at 500 MB server-side).
    client_max_body_size 500m;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /ws {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_read_timeout 86400;
    }
}
```
Caddy
```
trek.yourdomain.com {
    reverse_proxy localhost:3000
}
```

Caddy handles TLS and WebSockets automatically.

## Environment variables

**Full reference**

| Variable | Description | Default |
| --- | --- | --- |
| **Core** |  |  |
| `PORT` | Server port | `3000` |
| `NODE_ENV` | Environment (`production` / `development`) | `production` |
| `ENCRYPTION_KEY` | At-rest encryption key for stored secrets (API keys, MFA, SMTP, OIDC). Recommended: generate with `openssl rand -hex 32`. If unset, falls back to `data/.jwt_secret` (existing installs) or auto-generates a key (fresh installs). | Auto |
| `TZ` | Timezone for logs, reminders and cron jobs (e.g. `Europe/Berlin`) | `UTC` |
| `LOG_LEVEL` | `info` = concise user actions, `debug` = verbose details | `info` |
| `DEFAULT_LANGUAGE` | Default language on the login page for users with no saved preference. Browser/OS language is auto-detected first; this is the fallback. Supported: `de`, `en`, `es`, `fr`, `hu`, `nl`, `br`, `cs`, `pl`, `ru`, `zh`, `zh-TW`, `it`, `ar`, `id`, `tr`, `ja`, `ko`, `uk`, `gr` | `en` |
| `ALLOWED_ORIGINS` | Comma-separated origins for CORS and email links | same-origin |
| `FORCE_HTTPS` | Optional. When `true`: 301-redirects HTTP to HTTPS, sends HSTS, adds CSP `upgrade-insecure-requests`, forces the session cookie `secure` flag. Useful behind a TLS-terminating reverse proxy. Requires `TRUST_PROXY`. | `false` |
| `HSTS_INCLUDE_SUBDOMAINS` | When `true`: adds the `includeSubDomains` directive to the HSTS header, extending HTTPS enforcement to all subdomains. Only effective when HSTS is active (`FORCE_HTTPS=true` or `NODE_ENV=production`). Leave `false` if you run other services on sibling subdomains over plain HTTP. | `false` |
| `COOKIE_SECURE` | Controls the `secure` flag on the `trek_session` cookie. Auto-derived: on when `NODE_ENV=production` or `FORCE_HTTPS=true`. Escape hatch: set `false` to allow session cookies over plain HTTP. Not recommended in production. | auto |
| `SESSION_DURATION` | How long a login session stays valid when **"Remember me" is unchecked** (the default): sets the `trek_session` JWT `exp` and issues a browser-session cookie (cleared when the browser closes). Accepts `ms` -style strings: `1h`, `12h`, `7d`, `30d`, `90d`. Invalid values warn at startup and fall back to the default. | `24h` |
| `SESSION_DURATION_REMEMBER` | Session length when **"Remember me" is ticked** at login: a longer-lived JWT plus a persistent `trek_session` cookie that survives browser restarts. Same format and startup-fallback behaviour as `SESSION_DURATION`. | `30d` |
| `TRUST_PROXY` | Number of trusted reverse proxies. Tells the server to read client IP from `X-Forwarded-For` and protocol from `X-Forwarded-Proto`. Defaults to `1` in production; off in dev unless set. | `1` |
| `ALLOW_INTERNAL_NETWORK` | Allow outbound requests to private/RFC-1918 IPs (e.g. Immich on your LAN). Loopback and link-local addresses remain blocked. | `false` |
| `APP_URL` | Public base URL of this instance (e.g. `https://trek.example.com`). Required when OIDC is enabled; used as base for email notification links. | — |
| **OIDC / SSO** |  |  |
| `OIDC_ISSUER` | OpenID Connect provider URL | — |
| `OIDC_CLIENT_ID` | OIDC client ID | — |
| `OIDC_CLIENT_SECRET` | OIDC client secret | — |
| `OIDC_DISPLAY_NAME` | Label shown on the SSO login button | `SSO` |
| `OIDC_ONLY` | Force SSO-only mode: disables password login + registration, regardless of Admin > Settings. The first SSO login becomes admin. | `false` |
| `OIDC_ADMIN_CLAIM` | OIDC claim used to identify admin users | — |
| `OIDC_ADMIN_VALUE` | Value of the OIDC claim that grants admin role | — |
| `OIDC_SCOPE` | Space-separated OIDC scopes. **Fully replaces** the default — always include `openid email profile`. | `openid email profile` |
| `OIDC_DISCOVERY_URL` | Override the auto-constructed OIDC discovery endpoint (e.g. Authentik: `.../application/o/trek/.well-known/openid-configuration`) | — |
| **Initial setup** |  |  |
| `ADMIN_EMAIL` | Email for the first admin on initial boot. Must be set together with `ADMIN_PASSWORD`. If either is omitted a random password is printed to the server log. No effect once a user exists. | `admin@trek.local` |
| `ADMIN_PASSWORD` | Password for the first admin on initial boot. Pairs with `ADMIN_EMAIL`. | random |
| **Other** |  |  |
| `DEMO_MODE` | Enable demo mode (hourly data resets) | `false` |
| `MCP_RATE_LIMIT` | Max MCP API requests per user per minute | `300` |
| `MCP_MAX_SESSION_PER_USER` | Max concurrent MCP sessions per user | `20` |

## Data & Backups

- **Database** — SQLite, stored in `./data/travel.db`
- **Uploads** — stored in `./uploads/`
- **Logs** — `./data/logs/trek.log` (auto-rotated)
- **Backups** — create and restore via Admin Panel
- **Auto-Backups** — configurable schedule and retention in Admin Panel

## Data sources

The Atlas map's country and sub-national (province/county) boundaries come from [**geoBoundaries**](https://www.geoboundaries.org/) (Runfola et al., 2020), licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). See [NOTICE.md](https://github.com/mauriceboe/TREK/blob/main/NOTICE.md) for full third-party attributions.

## License

TREK is [AGPL v3](https://github.com/mauriceboe/TREK/blob/main/LICENSE). Self-host freely for personal or internal company use. If you modify and offer TREK as a network service to third parties, your modifications must be open-sourced under the same licence.