# NOTICE — 非官方静态构建

本仓库是 [deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo) 的 **fork**，
通过 GitHub Pages 托管其**已构建的静态产物**，仅供学习 / 演示使用，**非 DeepSeek 官方发布**。

- 上游仓库：<https://github.com/deepseek-ai/chat-demo>
- 上游在线 Demo：<https://deepseek-ai.github.io/chat-demo/>
- 许可：MIT License，Copyright (c) 2026 DeepSeek
- 本仓库站点：<https://w32394045-dotcom.github.io/chat-demo/>

## 本仓库相对上游构建产物的改动

1. 新增整套 PWA 图标 `icons/`：`icon-192` / `icon-512` / `maskable-512` / `apple-touch-icon(180)` / `favicon`
2. `index.html`：补 `favicon`、`apple-touch-icon`、iOS `apple-mobile-web-app-*` meta，`theme-color` 改为 `#445CA4`
3. `manifest.json`：补 `id`、主题色与图标引用
4. `sw.js`：缓存版本升到 `ds-chat-v2`，预缓存图标
5. 前端 bundle 与上游同一次构建（`static/js/main.9820cd3a.js`），业务代码未改动

## 权利说明

- 页面与脚本按 MIT License 分发。
- DeepSeek 商标、名称及相关标识归其所有者，本仓库不主张任何商标权利。
- 图标所用插画素材权利归原作者，此处仅作演示用途。
- 本仓库为非官方部署，与 DeepSeek 官方无隶属或背书关系。
