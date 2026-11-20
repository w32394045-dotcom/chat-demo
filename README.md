# DS Chat — Installable PWA with Simple / Geek modes

> **[Open the app →](https://chat.xszx.cc.cd/)**
>
> A static build of [`deepseek-ai/chat-demo`](https://github.com/deepseek-ai/chat-demo)
> extended into an installable PWA with two interface modes, Cloudflare Workers + KV hosting
> and a GitHub Pages mirror. Unofficial; for learning / personal use only.
> Upstream code is MIT licensed, Copyright (c) 2026 DeepSeek.
>
> 中文说明：[`README.zh-CN.md`](README.zh-CN.md)

## Why

Chatting with an AI on a phone should not start with *open browser → type URL → find bookmark*.
Install it like an app: one icon, full screen, no browser chrome.

## What this build adds (v0.3.5)

- **Two modes** — *Simple* keeps the original UI; *Geek* unlocks generation parameters,
  reasoning effort, safety level, stats and shortcuts.
- **Per-chat settings** — model, system prompt, streaming, effort, safety, context limit,
  background image (stored per conversation).
- **Context compression** — over threshold, older rounds are summarised by the model and the
  summary is injected as `latest_reminder`.
- **Usage statistics & limits** (Geek) — tokens by day / month / year and by conversation;
  daily / monthly token quotas block sending when reached.
- **Message display** — layout (classic / bubble), info chips (model, time, words, tokens,
  first-token delay, token throughput), avatar upload with cropper (IndexedDB), hide system prompt.
- **Thinking box** — the reasoning is shown in a light rounded card that mirrors the official
  app (grey, compact, no timeline rails). The header reads “已深度思考（用时 N 秒）” /
  “Thought for N seconds”, scrolls up with the content and then sticks at the top so it can be
  collapsed at any time; it auto-collapses when the answer starts.
- **Scroll behaviour** — the message list follows streaming output without fighting your
  finger: auto-follow pauses on touch, resumes only at the bottom, writes are batched per frame,
  browser scroll anchoring is disabled, and the touch handler no longer reads layout styles per
  move event. A floating scroll-to-bottom button (pulsing while streaming) jumps back instantly.
- **Mobile settings** — labels and controls stack vertically instead of squeezing the label to
  one character per line; the section tabs wrap instead of being clipped; slider rows no longer
  show the same “reset” label twice.
- **Mobile** — attachment sheet (camera / photos / files), bottom-sheet message editor with
  removable attachments; desktop inline editor also supports removing attachments.

## Live URLs

| Host | URL |
|---|---|
| Custom domain | <https://chat.xszx.cc.cd/> |
| workers.dev | <https://chat-demo.w32394045.workers.dev/> |
| GitHub Pages | <https://w32394045-dotcom.github.io/chat-demo/> |

## Install

- Android Chrome/Edge: address-bar *Install*.
- iOS Safari: Share → *Add to Home Screen*.
- Desktop Chrome/Edge: address-bar *Install*.

First run: Settings → Data → paste **your own** DeepSeek API key.

## Official API fields used

`model`, `system`, `messages`, `max_tokens`, `stream`, `thinking.type`,
`output_config.effort` (`low` / `high` / `max`), `safety_check.level` (`low` / `high`),
`tools`, `title`. Reference: <https://api-docs.deepseek.com/api/create-message>

## Security fixes from upstream v0.1.2

1. Attachment previews no longer create `blob:` URLs for arbitrary types (XSS in the app origin
   could read the API key); type comes from the extension, SVG uses a `data:` URL.
2. Reply links only allow `http(s)`.

## Updates

The app shell uses a service worker. Page navigations are **network-first** (offline falls back
to cache), so opening the app after a release shows the new build right away; hashed JS/CSS keep
cache-first for speed. The version currently running is printed at the bottom of Settings.

| Version | Highlights |
|---|---|
| 0.3.4 | Official-style thinking card, mobile scroll fixes, mobile settings layout, network-first SW navigations, version label |
| 0.3.5 | Legacy-browser compat (Win7 / Chrome 80–109): polyfills for Array.prototype.at, 	oReversed/	oSorted, Object.hasOwn, AbortSignal.throwIfAborted; render error boundary; mobile settings rows stack vertically |
| 0.3.3 | Unversioned IndexedDB open + 8 s timeout + auto fresh-DB fallback (fixes the “blocked by another tab” deadlock) |
| 0.3.2 | Usage statistics / quotas, per-chat settings, edit sheet, context compression |
| 0.3.0 | Simple / Geek modes, deepseek-flash & deepseek-v4-pro models, PWA packaging |
| 0.1.x | Upstream `deepseek-ai/chat-demo` |

## Verification

`tsc` clean · `oxlint` clean · 45 unit tests passing · all hosts serve identical bytes
(HTML differs only by the Cloudflare analytics snippet).

## License

MIT (upstream), Copyright (c) 2026 DeepSeek. Trademark and artwork belong to their owners.
Unofficial deployment — see [`NOTICE.md`](NOTICE.md).
