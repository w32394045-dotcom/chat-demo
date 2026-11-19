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

## What this build adds (v0.3.0)

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
- **Thinking UI** — auto-collapse when the answer starts, sticky header while scrolling,
  floating scroll-to-bottom button with a pulsing ring while streaming.
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

## Verification

`tsc` clean · `oxlint` clean · 45 unit tests passing · all hosts serve identical bytes
(HTML differs only by the Cloudflare analytics snippet).

## License

MIT (upstream), Copyright (c) 2026 DeepSeek. Trademark and artwork belong to their owners.
Unofficial deployment — see [`NOTICE.md`](NOTICE.md).
