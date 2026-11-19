# DS Chat · 可安装 PWA（极简 / 极客双模式）

> **[打开应用 →](https://chat.xszx.cc.cd/)**
>
> 基于 [`deepseek-ai/chat-demo`](https://github.com/deepseek-ai/chat-demo) 的静态构建，
> 扩展为可安装 PWA + 双界面模式，部署在 Cloudflare Workers + KV，并镜像到 GitHub Pages。
> 非官方，仅供学习 / 自用。上游代码 MIT License，Copyright (c) 2026 DeepSeek。
>
> English: [`README.md`](README.md)

## 为什么

手机上每次「开浏览器 → 输网址 → 翻书签」用 AI，体验很差。目标：像 App 一样装到桌面，
一个图标、全屏打开、没有地址栏。

## v0.3.0 新增

- **双模式**：小白/极简保持原界面；开发者/极客解锁生成参数、思考强度、安全审查、统计与快捷键
- **每对话独立设置**：模型、系统提示词、流式、思考强度、安全审查、上下文上限、背景图
- **上下文压缩**：超过阈值时由模型总结更早的轮次，摘要以 `latest_reminder` 注入请求
- **用量统计与限额**（极客）：按天/月/年、按对话统计 token；可设每日/每月额度，超限阻止发送
- **消息显示**：经典/气泡排版；模型、时间、字数、Token、首 Token 延迟、Token 吐速等资讯项；
  头像上传（带裁剪框，存 IndexedDB）；隐藏系统提示词
- **思维链**：答案开始后自动折叠；展开滚动时标题吸附在顶部，方便随时折叠
- **滚动**：用户上滑即停止自动跟随；右下角回到底部按钮，生成时外圈蓝色呼吸光圈
- **移动端**：附件面板（拍照/相簿/文件）、底部弹层编辑消息（可删附件）；桌面内联编辑同样支持删附件

## 在线地址

| 托管 | 地址 |
|---|---|
| 自定义域名 | <https://chat.xszx.cc.cd/> |
| workers.dev | <https://chat-demo.w32394045.workers.dev/> |
| GitHub Pages | <https://w32394045-dotcom.github.io/chat-demo/> |

## 安装

- Android Chrome/Edge：地址栏「安装」
- iOS Safari：分享 → 添加到主屏幕
- 桌面 Chrome/Edge：地址栏「安装」

首次使用：设置 → 数据管理 → 填入**你自己的** DeepSeek API Key。

## 使用的官方接口字段

`model`、`system`、`messages`、`max_tokens`、`stream`、`thinking.type`、
`output_config.effort`（`low` / `high` / `max`）、`safety_check.level`（`low` / `high`）、
`tools`、`title`。参考：<https://api-docs.deepseek.com/api/create-message>

## 上游 v0.1.2 的安全修复（已包含）

1. 附件预览不再对任意类型创建 `blob:` 地址（此前可在本站源内执行脚本并读走 API Key）；
   类型按后缀判定，SVG 用 `data:`。
2. 回复中的链接只允许 `http(s)`。

## 校验

`tsc` 零错误 · `oxlint` 零告警 · 45 个单元测试通过 · 三处地址内容一致
（HTML 仅多出 Cloudflare 注入的 analytics）。

## 许可

上游 MIT，Copyright (c) 2026 DeepSeek。商标与插画归各自所有者。
非官方部署，详见 [`NOTICE.md`](NOTICE.md)。
