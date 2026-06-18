# The Romance Writing Companion App

A free, phone-friendly web companion for the **Romance Writing Festival**, Bournemouth (Saturday 3 October 2026). Single self-contained page, works offline after first load, stores everything privately in the browser (no accounts, no backend).

## What's in this repo

| Path | What it is |
|---|---|
| `public/index.html` | **The app** — the self-contained file that gets served. |
| `public/serve.json` | Caching / security headers for the static server. |
| `package.json`, `package-lock.json` | Declares the `serve` static server + start script. |
| `railway.toml` | Railway deployment config. |
| `src/app3.src.html` | Editable source. The app is precompiled from here (see HANDOFF.md). |
| `USER_OVERVIEW.md` | Plain-language overview of features and intended use. |

## Run it locally

```bash
npm install
npm start
```
Then open the URL it prints (default http://localhost:3000).

> You can also just double-click `public/index.html` — it needs internet on first load for fonts/libraries.

## Publish to GitHub

```bash
git init
git add .
git commit -m "The Romance Writing Companion App"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

## Deploy on Railway

1. Go to railway.com → **New Project → Deploy from GitHub repo** and pick this repo.
2. Railway auto-detects Node (NIXPACKS), runs `npm install`, then `npm start` (from `railway.toml`).
3. When the deploy is healthy, open **Settings → Networking → Generate Domain** to get a public URL.
4. Done. Every `git push` to `main` redeploys automatically.

No environment variables or secrets are needed.

## Updating the app

Edit `src/app3.src.html`, rebuild (see `HANDOFF.md`), copy the build to `public/index.html`, then commit and push — Railway redeploys on push.
