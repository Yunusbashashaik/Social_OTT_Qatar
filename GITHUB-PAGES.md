# GitHub Pages setup (free account)

## Your store URL

### **https://yunusbashashaik.github.io/Social_OTT_Qatar/**

---

## How deploy works

This repo keeps a **single `main` branch**. GitHub Pages should serve **`main` / (root)**.

The built homepage, `assets/`, `logo.png`, and wallpaper live at the **repository root** as well as in `docs/`. If those files are missing from root, the store URL is a blank white page (the HTML loads, the JS bundle 404s).

After changing logo, wallpaper, or copy, rebuild with:

```bash
VITE_BASE_PATH=/Social_OTT_Qatar/ npm run build -w client
cp -a client/dist/. docs/
cp client/dist/index.html docs/404.html
cp -a docs/.nojekyll docs/assets docs/*.html docs/*.png docs/*.ico docs/*.js docs/*.xml docs/*.JPG docs/*.htaccess . 2>/dev/null
```

Then commit the updated root + `docs/` files on `main`.

### One-time Pages setting

1. Open **https://github.com/Yunusbashashaik/Social_OTT_Qatar/settings/pages**
2. **Source:** Deploy from a branch
3. **Branch:** `main` · **Folder:** `/ (root)`
4. Save, wait 1–2 minutes, then open the store URL above

---

## Wrong URLs

| URL | Result |
|-----|--------|
| `yunusbashashaik.github.io` | Not your store |
| `yunusbashashaik.github.io/Social_OTT_Qatar/` | **Correct homepage** |
