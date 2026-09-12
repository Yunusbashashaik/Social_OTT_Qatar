---
published: false
---

# Social_OTT_Qatar

> **Open the website (iPad / phone):** [https://yunusbashashaik.github.io/Social_OTT_Qatar/](https://yunusbashashaik.github.io/Social_OTT_Qatar/)  
> Do **not** use `yunusbashashaik.github.io` alone — that is not your store URL.

Premium Store — bilingual digital subscription marketplace for Qatar (QAR).

## Development

Requirements: Node.js 20+.

```bash
npm install
npm run dev
```

- **Client:** http://localhost:5173 (Vite dev server; proxies `/api` to the backend)
- **API:** http://localhost:3001 (`GET /api/health`, `GET /api/services`, `GET /api/settings`, `POST /api/complaints`, `POST /api/admin/login`)

```bash
npm run lint
npm run test
npm run build
npm start   # serves built client + API on port 3001
```

### Dynamic database (SQLite)

Admin edits and public catalog/settings are stored in **`server/data/globalstore.db`** (not GitHub-tracked static files). Every visitor hitting the Node API sees the same live data.

Optional env:

- `DATABASE_PATH` — custom SQLite file path
- `ADMIN_USERNAME` (default: `admin`)
- `ADMIN_PASSWORD` (default: `Go$StQ821`)
- `ADMIN_SESSION_SECRET` — signs admin session tokens

### Admin panel

Click the **Admin** icon in the header. A modal prompts for credentials, then opens the Admin Dashboard:

- **Add Services** — JPEG image, name, EN/AR descriptions, 1-month and 1-year prices
- **Edit Services** — dropdown for Services, Complaint Email ID, Contact Details (WhatsApp), and About Us / social links

Default credentials: `admin` / `Go$StQ821` (override with `ADMIN_USERNAME` / `ADMIN_PASSWORD`).

Out-of-stock services use price `0`, show an **Out of Stock** note, and disable Add to Cart.

### Deploy on GoDaddy (Node.js)

Admin login needs a **running Node app**. If `https://YOUR-DOMAIN/api/health` does not return `{"ok":true}`, login cannot work.

**cPanel Application Manager (Passenger)**

1. Setup → Application Manager → Register Application  
2. Application root = this repo folder  
3. Application URL = your domain (or subdomain) **root**, not a `/public_html` static copy  
4. Application startup file: `app.js`  
5. Node.js version: 20+  
6. In the app directory:
   ```bash
   npm install
   npm run build
   ```
7. Restart the application  
8. Visit `https://YOUR-DOMAIN/api/health` — you must see JSON `ok: true`  
9. Then sign in with `admin` / `Go$StQ821`

Do **not** FTP only `client/dist` into `public_html`. That is static hosting and `/api/health` will 404.

If Apache serves static files and Node is on port 3001, proxy `/api` to the Node process (requires `mod_proxy`).

If the website and API use different URLs, edit `client/public/runtime-config.js` after build:

```js
window.__GLOBALSTORE_CONFIG__ = { apiUrl: "https://your-node-api-url" };
```

Keep `server/data/` on a persistent disk so SQLite and uploads survive restarts.

### Complaint email

Complaints are sent by **email only** (not WhatsApp). The destination address is stored in the database (default `global2stor2@gmail.com`) and can be changed from the admin panel.

- **Static hosting (GitHub Pages):** FormSubmit classic multipart POST fallback
- **Node API + SMTP:** screenshot embedded + attached

Optional env: `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS`, `SMTP_FROM`, `COMPLAINT_EMAIL` / `VITE_COMPLAINT_EMAIL`

See `Tech. Document` for full product requirements.

## Deployment (GitHub Pages) — free account OK

You **do not need a paid GitHub plan** for a **public** repository. GitHub Pages is included on free accounts. This repo is public.

Pushes to **`main`** build the site into the **repository root** on the same branch. This repo stays on **`main` only**.

### One-time setup (iPhone, iPad, or computer)

1. Open **https://github.com/Yunusbashashaik/Social_OTT_Qatar/settings/pages**
2. Under **Build and deployment** → **Source**, choose **Deploy from a branch**
3. **Branch:** `main` · **Folder:** `/ (root)` · **Save**
4. Wait 1–2 minutes, then open on your iPad:

   **https://yunusbashashaik.github.io/Social_OTT_Qatar/**

The homepage has **no bundled catalog**. If the API is unavailable it stays empty until Admin adds services on the Node server (`npm start` on a host such as Render or GoDaddy Node). Point that host at a persistent disk so `server/data/globalstore.db` survives restarts.
