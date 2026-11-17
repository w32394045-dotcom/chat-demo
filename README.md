# DS Chat — Installable PWA Deployment

> **[Open the app →](https://chat.xszx.cc.cd/)**
>
> A static build of [`deepseek-ai/chat-demo`](https://github.com/deepseek-ai/chat-demo),
> deployed as an **installable PWA**. Unofficial; for learning / personal use only.
> Upstream code is MIT licensed, Copyright (c) 2026 DeepSeek.
>
> 中文说明：[`README.zh-CN.md`](README.zh-CN.md)

---

## Why this repo exists

Chatting with an AI on a phone is painful if every session starts with
*open a browser → type the URL → find the bookmark → wait for the page*.

The single goal of this repo:

> **Make this web app installable on phones and desktops, so users tap one icon
> and get a full-screen app — no address bar, no browser chrome.**

That is what a PWA gives us:

| Capability | Mechanism | Result |
|---|---|---|
| Icon on home screen / dock | `manifest.json` + icons | A "DS Chat" app icon, not a bookmark |
| Full-screen, no browser UI | `display: standalone` + iOS `apple-mobile-web-app-capable` | Looks and feels like a native app |
| Instant start / offline shell | `sw.js` Service Worker pre-cache | Shell renders on weak networks |
| Correct icons everywhere | `maskable-512` (Android), `apple-touch-icon` (iOS), `favicon` (tabs) | Sharp, not cropped or blurry |

> Note: **chatting still requires network access** (DeepSeek API). Offline mode only
> keeps the app shell open; it is not offline chat.

---

## Live URLs

| Host | URL | Notes |
|---|---|---|
| Custom domain (primary) | <https://chat.xszx.cc.cd/> | Cloudflare Worker + KV static hosting |
| Cloudflare workers.dev | <https://chat-demo.w32394045.workers.dev/> | Fallback URL |
| GitHub Pages | <https://w32394045-dotcom.github.io/chat-demo/> | Mirror of the same build |

All three serve the identical byte-for-byte build.

---

## Install on mobile

### Android · Chrome / Edge

1. Open <https://chat.xszx.cc.cd/>
2. Tap the **Install** icon in the address bar (or menu → *Add to Home screen*)
3. Launch from the home-screen icon — full screen, no address bar

### iOS · Safari (Safari only; in-app browsers such as WeChat cannot install)

1. Open the same URL in Safari
2. Tap **Share** → **Add to Home Screen**
3. Launch from the home-screen icon — full screen (`apple-mobile-web-app-capable`)

### Desktop · Chrome / Edge

Address-bar **Install** icon → runs in its own window.

---

## First run

After installing, open **Settings → General** and enter **your own** DeepSeek API key.
Usage is billed to your key; this deployment never sees or stores your key on a server.

---

## What is different from the upstream build

Upstream ships a web shell with placeholder icons (a solid block) and no favicon or
iOS icon. This repo fills the missing "installable" pieces:

| File | Change |
|---|---|
| `icons/icon-192.png`, `icon-512.png` | Real app icon (face close-up, 96-colour palette) |
| `icons/maskable-512.png` | Android adaptive icon (`purpose: maskable`) |
| `icons/apple-touch-icon.png` | iOS 180×180 icon |
| `icons/favicon.ico`, `favicon-32.png` | Browser tab icon (previously 404) |
| `index.html` | Added `favicon`, `apple-touch-icon`, iOS meta; `theme-color` → `#445CA4` |
| `manifest.json` | Added `id`, theme colour, icon entries |
| `sw.js` | Cache version `ds-chat-v1 → ds-chat-v2`; icons pre-cached |
| `static/**` | **Unchanged** — same build as upstream (`main.9820cd3a.js`) |
| `NOTICE.md` | Attribution and non-official disclaimer |

---

## Layout

```
.
├── index.html            # entry: favicon / iOS meta / SW registration
├── manifest.json         # PWA manifest: name, icons, standalone, theme
├── sw.js                 # Service Worker: pre-cache + network-first fallback
├── icons/                # 192 / 512 / maskable-512 / apple-touch-icon / favicon
├── static/               # front-end build output (JS / CSS / KaTeX fonts), untouched
├── LICENSE               # upstream MIT license
├── NOTICE.md             # attribution
└── .nojekyll             # disable Jekyll on GitHub Pages
```

---

## Deployment

### Cloudflare (primary)

Hosted as a **Cloudflare Worker** that reads static files from a **Workers KV**
namespace (`chat-demo-assets`) and serves them with correct MIME types, immutable
caching for `/static/*`, and SPA fallback to `index.html`.

| Piece | Value |
|---|---|
| Worker script | `chat-demo` |
| KV namespace | `chat-demo-assets` |
| Custom domain | `chat.xszx.cc.cd` (zone `xszx.cc.cd`) |
| Cache policy | `/static/*` → 1 year immutable, others → 5 minutes |

Deploy / update:

```powershell
python cf_workers_deploy.py     # upload all files to KV + deploy the Worker
python cf_worker_only.py        # only redeploy the Worker code
```

> Note: Cloudflare **Pages** direct upload was attempted first and rejected by the
> platform — every deployment, including a one-file test project, returned HTTP 500
> from the edge. Workers + KV was used instead; it is the same static content and
> supports custom domains and PWAs identically.

### GitHub Pages (mirror)

Served from the `gh-pages` branch of this fork:

```powershell
python deploy_ghpages_v2.py
```

---

## Verification

The deployment is verified automatically, not assumed:

- All three hosts return HTTP 200 for the shell, manifest, Service Worker, icons and bundles
- Every fetched file is compared **SHA-256** against the local build (`ALL MATCH`)
- Installability checks pass: HTTPS, manifest (`name`, `short_name`, `start_url`,
  `display: standalone`), 192 & 512 icons, `maskable` icon, `apple-touch-icon` 180×180,
  Service Worker with a `fetch` handler
- Android circular-crop preview confirms the face icon survives the mask

---

## License and attribution

- Code: [deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo), MIT License,
  Copyright (c) 2026 DeepSeek
- Trademark: "DeepSeek" and related marks belong to their owners; no trademark rights claimed
- Icon artwork: rights belong to the original illustrator; used here for demonstration only
- This is an **unofficial** deployment with no affiliation or endorsement by DeepSeek.
  Details: [`NOTICE.md`](NOTICE.md)
