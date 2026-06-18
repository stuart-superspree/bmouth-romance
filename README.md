# The Romance Writing Companion App

A free, phone-friendly web companion for the **Romance Writing Festival**, Bournemouth (Saturday 3 October 2026). Single self-contained page, works offline after first load, stores everything privately in the browser (no accounts, no backend).

## What's in this repo

| Path | What it is |
|---|---|
| `index.html` | **The app** — the self-contained file served at the homepage. |
| `serve.json` | Static-server config (no directory listing; any path serves the app). |
| `package.json`, `package-lock.json` | Declares the `serve` static server + start script. |
| `railway.toml` | Railway deployment config. |
| `src/app3.src.html` | Editable source. The app is precompiled from here. |
| `USER_OVERVIEW.md` | Plain-language overview of features and intended use. |

## Run it locally

```bash
npm install
npm start
```
Then open the URL it prints (default http://localhost:3000). You can also just double-click `index.html`.

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

1. railway.com → **New Project → Deploy from GitHub repo** → pick this repo.
2. Railway auto-detects Node, runs `npm install`, then `npm start` (serves the repo root).
3. **Settings → Networking → Generate Domain** for a public URL.
4. Every push to `main` redeploys automatically. No env vars needed.

## Updating the app

Edit `src/app3.src.html`, rebuild, copy the build over `index.html`, then commit and push.
