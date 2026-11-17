# NOTICE — 非官方静态构建

本仓库是 [deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo) 的 **fork**，
通过 GitHub Pages 托管其**已构建的静态产物**，仅供学习 / 演示使用，**非 DeepSeek 官方发布**。

## 上游
- 仓库：https://github.com/deepseek-ai/chat-demo
- 许可：MIT License，Copyright (c) 2026 DeepSeek
- 在线 Demo：https://deepseek-ai.github.io/chat-demo/

## 本仓库相对上游的改动
1. 新增 PWA 图标 `icons/`：192 / 512 / maskable-512 / apple-touch-icon(180) / favicon
2. `index.html`：补 `favicon`、`apple-touch-icon`、iOS `apple-mobile-web-app-*` meta，`theme-color` 改为 `#445CA4`
3. `manifest.json`：补 `id`、主题色、图标引用
4. `sw.js`：缓存版本升到 `ds-chat-v2`，预缓存图标
5. 前端 bundle 与上游同一次构建（`static/js/main.9820cd3a.js`），未改动业务代码

## 说明
- 代码按 MIT License 分发；DeepSeek 商标、插画与图标素材权利归各自所有者。
- 若上游更新，本仓库需重新构建后再推送 `gh-pages`。
