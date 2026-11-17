# DS Chat · 可安装的 PWA 站点

> **[👉 在线打开](https://w32394045-dotcom.github.io/chat-demo/)**
>
> 这是 [`deepseek-ai/chat-demo`](https://github.com/deepseek-ai/chat-demo) 的**静态构建 + PWA 部署**，
> 非官方，仅供学习 / 自用。上游代码 MIT License，Copyright (c) 2026 DeepSeek。

---

## 这个仓库到底在解决什么问题

手机上用 AI 聊天，最烦的是：**开浏览器 → 输网址 / 翻书签 → 再进页面**，每次都这样，用得越顺手越像折磨。

所以这个仓库的目标只有一个：

> **让这个网页能像 App 一样装到手机上，点桌面图标直接全屏打开，看不到地址栏。**

靠 PWA 的三件事实现：

| 能力 | 靠什么 | 效果 |
|---|---|---|
| 装到桌面 / 主屏 | `manifest.json` + 图标 | 系统里出现一个「DS Chat」图标，不是书签 |
| 全屏无浏览器外壳 | `display: standalone` + iOS `apple-mobile-web-app-capable` | 打开就是纯应用界面 |
| 秒开 / 离线可开外壳 | `sw.js` Service Worker 预缓存 | 弱网先出界面，不用等资源 |
| 图标正确 | `maskable-512`（Android）/ `apple-touch-icon`（iOS）/ `favicon`（浏览器标签） | 各种位置不糊、不被裁歪 |

> 注意：**聊天本身需要联网**（调用 DeepSeek API）。离线只保证应用外壳能打开，不是离线聊天。

---

## 怎么装（移动端）

### Android · Chrome / Edge

1. 打开 <https://w32394045-dotcom.github.io/chat-demo/>
2. 地址栏右侧出现「安装」按钮（或菜单 →「添加到主屏幕」/「安装应用」）
3. 桌面出现图标 → 点开即全屏，无地址栏

### iOS · Safari（必须 Safari，微信内置浏览器不行）

1. Safari 打开同一网址
2. 底部「分享」按钮 → 向下找 **「添加到主屏幕」**
3. 桌面图标 → 打开即全屏（已配 `apple-mobile-web-app-capable` 与 180×180 图标）

### 桌面 · Chrome / Edge

地址栏右侧「安装」图标 → 装成独立窗口应用，任务栏/程序坞直接启动。

---

## 首次使用

安装后仍需在 **设置 → 通用设置** 里填入**你自己的** DeepSeek API Key。
费用由你自己的 Key 承担，本页面不代持、不上传 Key（存在本地）。

---

## 本仓库相对上游构建产物的改动

上游只提供网页壳，图标是程序画的纯色占位块，且没接 favicon / iOS 图标。本仓库补齐了「能装」的最后一公里：

| 文件 | 改动 |
|---|---|
| `icons/icon-192.png` `icon-512.png` | 换成正式图标（脸部特写，96 色调色板压缩） |
| `icons/maskable-512.png` | 新增 Android 自适应图标（`purpose: maskable`） |
| `icons/apple-touch-icon.png` | 新增 iOS 180×180 图标 |
| `icons/favicon.ico` `favicon-32.png` | 新增浏览器标签页图标（原来是 404） |
| `index.html` | 补 `favicon` / `apple-touch-icon` / iOS meta；`theme-color` 改为 `#445CA4` |
| `manifest.json` | 补 `id`、主题色、图标引用 |
| `sw.js` | 缓存版本 `ds-chat-v1 → ds-chat-v2`，预缓存图标（避免旧 SW 一直吐旧壳） |
| `static/**` | **未改动**，与上游同一次构建（`main.9820cd3a.js`） |
| `NOTICE.md` | 归属与非官方声明 |

---

## 目录结构

```
.
├── index.html            # 入口（含 favicon / iOS meta / SW 注册）
├── manifest.json         # PWA 清单：名称、图标、standalone、主题色
├── sw.js                 # Service Worker：预缓存 + 网络优先回退缓存
├── icons/                # 192 / 512 / maskable-512 / apple-touch-icon / favicon
├── static/               # 前端构建产物（js / css / KaTeX 字体），未改动
├── LICENSE               # 上游 MIT License
├── NOTICE.md             # 非官方说明与改动清单
└── .nojekyll             # 关闭 GitHub Pages 的 Jekyll 处理
```

---

## 部署 / 更新

由 GitHub Pages 从 `gh-pages` 分支直接托管（不是 fork 里上游的源码分支）。

上游更新后重新构建，然后执行：

```powershell
python C:\Users\ptfm\Downloads\chat-dist\deploy_ghpages.py
```

脚本会：拉 `gh-pages` → 覆盖站点文件 → 提交 → 推送，Pages 约 20 秒后自动重建。

---

## 许可与归属

- 代码：[deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo)，MIT License，Copyright (c) 2026 DeepSeek
- 商标：DeepSeek 及相关标识归其所有者，本仓库不主张任何商标权利
- 图标插画素材：权利归原作者，此处仅作演示用途
- 本仓库为非官方部署，与 DeepSeek 官方无隶属关系；详细改动见 [`NOTICE.md`](NOTICE.md)
