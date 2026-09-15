# PDF Compressor — Secure, Local, Open Source

Compress PDF files entirely in your browser. No uploads, no server, no accounts, no tracking. Files never leave your device.

Two builds are included:

| Build | Folder | Use it when |
| --- | --- | --- |
| **Single file** | `pdf-compressor-single-file/` | You want one `index.html` you can open or host anywhere |
| **PWA** | `pdf-compressor-pwa/` | You want an installable app that works offline |

---

## Features

- Secure and private: 100% client-side PDF compression
- Works offline, no internet connection required
- Four levels: High, Medium, Low and Lossless
- Size estimates shown before you compress
- Text, fonts, links, annotations and form fields stay searchable
- Every result verified — no image, page or text is lost
- Output is never larger than the input
- Light and dark themes, English and Arabic
- Zero dependencies, MIT licensed

---

## Option 1 — Run locally

### Single file build

Open `pdf-compressor-single-file/index.html` in Chrome, Edge, Firefox or Safari. That's it — no server needed.

### PWA build

The PWA needs a local server for its service worker.

```bash
cd pdf-compressor-pwa
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

---

## Option 2 — Deploy to GitHub Pages

1. Sign in to <https://github.com> and select **+ → New repository**.
2. Name it `pdf-compressor`, choose **Public**, then **Create repository**.
3. Select **uploading an existing file**.
4. Open the build folder you want and drag **the files inside it** onto the upload area — not the folder itself. `index.html` must sit at the repository root.
5. Select **Commit changes**.
6. Go to **Settings → Pages**.
7. Under **Source**, choose **Deploy from a branch**.
8. Set branch to `main` and folder to `/ (root)`, then **Save**.
9. Wait one to two minutes, reload the page, and open the URL shown:
   `https://<username>.github.io/pdf-compressor/`

---

## Option 3 — Deploy to any hosting server

Upload the contents of your chosen build folder to your web root, for example `public_html/` or `/var/www/html/`.

```bash
scp -r pdf-compressor-pwa/* user@yourserver:/var/www/html/
```

No build step, no Node.js, no database. Any static host works: Netlify, Vercel, Cloudflare Pages, Apache, Nginx, IIS or an intranet share.

**Serve the PWA over HTTPS.** Offline mode and installation only work on a secure origin.

---

## Install the PWA on a device

- **Windows / macOS (Chrome or Edge):** open the site, select the install icon in the address bar, then **Install**.
- **Android (Chrome):** browser menu → **Install app**.
- **iPhone / iPad (Safari):** **Share** → **Add to Home Screen**.

---

## Updating the PWA

After changing any file, bump the version on the first line of `service-worker.js` (for example `pdf-compressor-v3` → `pdf-compressor-v4`) so installed copies pick up the update.

---

## Requirements

A current version of Chrome, Edge, Firefox or Safari. Password-protected PDFs cannot be opened.

---

## License

MIT. Bundled libraries retain their original notices inside `index.html`.

---

## Keywords

compress pdf, pdf compressor, secure pdf compressor, local pdf compressor, open source pdf compressor, offline pdf compressor, private pdf compression, client side pdf compression, browser pdf compressor, compress pdf without uploading, reduce pdf file size, shrink pdf, pdf size reducer, optimize pdf, pdf optimizer, lossless pdf compression, free pdf compression tool, javascript pdf compressor, progressive web app, installable web app, offline first, single file web app, static site pdf tool, github pages pdf compressor, no upload pdf tool, privacy first pdf tool
