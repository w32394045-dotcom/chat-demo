# NOTICE — 非官方静态构建

本仓库是 [deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo) 的 **fork**，
在其构建产物上做了 PWA 与界面功能扩展，通过 Cloudflare Workers 与 GitHub Pages 提供访问。
仅供学习 / 演示，**非 DeepSeek 官方发布**。

- 上游：<https://github.com/deepseek-ai/chat-demo>（MIT，Copyright (c) 2026 DeepSeek）
- 本站（v0.3.0）：<https://chat.xszx.cc.cd/>

## 相对上游 v0.1.2 的改动

1. PWA 层：manifest、sw.js（缓存 v3）、icons（192/512/maskable/apple-touch/favicon）、
   index.html 注入图标与 iOS meta
2. 双界面模式：小白/极简（原界面）与开发者/极客（参数、思考强度、安全审查、统计、快捷键）
3. 生成参数：`model` / `system` / `temperature` / `top_p` / `max_tokens` / `stream` /
   `output_config.effort` / `safety_check.level`
4. 每对话设置与背景图；上下文自动压缩（摘要注入 latest_reminder）
5. 用量统计（按天/月/年、按对话）与每日/每月 token 限额
6. 消息资讯（含 Token 吐速）、头像上传与裁剪（IndexedDB）、经典/气泡排版
7. 思维链自动折叠与吸附标题、滚动跟随修复与回到底部按钮
8. 移动端附件面板、底部弹层编辑消息；桌面编辑可删除附件

## 权利说明

- 页面与脚本按 MIT License 分发
- DeepSeek 商标与标识归其所有者，本仓库不主张任何商标权利
- 图标插画素材权利归原作者，仅演示使用
- 非官方部署，与 DeepSeek 官方无隶属或背书关系
