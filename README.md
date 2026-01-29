# POD Generator

Generate Proof of Delivery (POD) PDFs from an Excel file. Auto-splits into ZIPs of 3000 PDFs each.

## Deploy on Railway (recommended for 1000+ orders)

Railway supports long-running requests and enough memory for Puppeteer, so you can process **1000–26k+ orders** per upload.

### Option A: Deploy with Docker (recommended)

1. Push the repo to GitHub.
2. Go to [Railway](https://railway.app) → **New Project** → **Deploy from GitHub repo**.
3. Select this repo. Railway will detect the **Dockerfile** and deploy.
4. Add a **public domain** in the service → **Settings** → **Networking** (e.g. `your-app.up.railway.app`).
5. Deploy. No extra env vars needed; the Dockerfile sets `PUPPETEER_EXECUTABLE_PATH` and installs Chromium.

### Option B: Deploy with Nixpacks (no Dockerfile)

1. In Railway, create a new project from GitHub and select this repo.
2. In **Settings** → **Build**, set **Builder** to **Nixpacks** (if not already).
3. Add these **Environment Variables**:
   - `PUPPETEER_EXECUTABLE_PATH` = `/usr/bin/chromium` (or the path Nixpacks provides; check Railway docs for Node + Chromium).
4. Optionally add a **nixpacks.toml** or use a **Nixpacks** config that installs Chromium. Easiest is to use the **Dockerfile** (Option A).

### Env vars (optional on Railway)

| Variable | Default | Use |
|----------|---------|-----|
| `CONCURRENT_PDFS` | 1 (Railway) | Parallel PDFs (1 = stable; try 2–3 if you have more RAM). |
| `BROWSER_RESTART_EVERY` | 600 | Restart browser every N PDFs to avoid memory issues. |
| `PDF_TIMEOUT_MS` | 45000 | Timeout per PDF (ms). |

---

## Run locally

```bash
npm install
npm start
```

Uses full Puppeteer and defaults to concurrency 5. Open `http://localhost:3000`, upload an Excel file, get a ZIP of POD PDFs.

---

## Deploy on Vercel (small batches only)

Vercel has a **60s** execution limit per request (Pro). The app limits to **50 orders per upload** on Vercel by default.

- Good for: quick tests, small files (e.g. &lt; 50 orders).
- For **1000+ orders**, use **Railway** instead.

To deploy: import the repo in [Vercel](https://vercel.com); no extra config. Optionally set `VERCEL_MAX_ORDERS` (e.g. `30` or `50`).
