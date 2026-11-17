# DS Chat · 可安装的 PWA 站点

> **[打开应用 →](https://chat.xszx.cc.cd/)**
>
> 这是 [`deepseek-ai/chat-demo`](https://github.com/deepseek-ai/chat-demo) 的**静态构建 + PWA 部署**，
> 非官方，仅供学习 / 自用。上游代码 MIT License，Copyright (c) 2026 DeepSeek。
>
> English: [`README.md`](README.md)

---

## 这个仓库解决什么问题

手机上用 AI 聊天，如果每次都要「开浏览器 → 输网址 / 翻书签 → 再进页面」，体验就废了。

所以本仓库的目标只有一个：

> **让这个网页能像 App 一样装到手机 / 桌面，点一个图标直接全屏打开，看不到地址栏。**

靠 PWA 的三件事：

| 能力 | 靠什么 | 效果 |
|---|---|---|
| 装到桌面 / 主屏 | `manifest.json` + 图标 | 系统里出现「DS Chat」图标，不是书签 |
| 全屏无浏览器外壳 | `display: standalone` + iOS `apple-mobile-web-app-capable` | 打开就是纯应用界面 |
| 秒开 / 离线可开外壳 | `sw.js` Service Worker 预缓存 | 弱网先出界面 |
| 图标正确 | `maskable-512`（Android）/ `apple-touch-icon`（iOS）/ `favicon`（标签页） | 不糊、不被裁歪 |

> 注意：**聊天本身需要联网**（调用 DeepSeek API）。离线只保证应用外壳能打开，不是离线聊天。

---

## 在线地址

| 托管 | 地址 | 说明 |
|---|---|---|
| 自定义域名（主） | <https://chat.xszx.cc.cd/> | Cloudflare Worker + KV 静态托管 |
| Cloudflare workers.dev | <https://chat-demo.w32394045.workers.dev/> | 备用地址 |
| GitHub Pages | <https://w32394045-dotcom.github.io/chat-demo/> | 同一份构建的镜像 |

三处服务的内容逐字节一致。

---

## 装到手机上

### Android · Chrome / Edge

1. 打开 <https://chat.xszx.cc.cd/>
2. 地址栏右侧「安装」按钮（或菜单 →「添加到主屏幕」）
3. 桌面出现图标 → 点开即全屏，无地址栏

### iOS · Safari（必须 Safari；微信内置浏览器装不了）

1. Safari 打开同一网址
2. 底部「分享」→ **「添加到主屏幕」**
3. 桌面图标 → 打开即全屏（已配 `apple-mobile-web-app-capable`）

### 桌面 · Chrome / Edge

地址栏右侧「安装」图标 → 装成独立窗口应用。

---

## 首次使用

安装后在 **设置 → 通用设置** 填入**你自己的** DeepSeek API Key。
费用由你的 Key 承担，本部署不在服务器上保存任何 Key。

---

## 相对上游构建产物的改动

上游只有网页壳，图标是纯色占位块，且没有 favicon / iOS 图标。本仓库补齐「能装」的最后一公里：

| 文件 | 改动 |
|---|---|
| `icons/icon-192.png`、`icon-512.png` | 换成正式图标（脸部特写，96 色调色板压缩） |
| `icons/maskable-512.png` | Android 自适应图标（`purpose: maskable`） |
| `icons/apple-touch-icon.png` | iOS 180×180 图标 |
| `icons/favicon.ico`、`favicon-32.png` | 浏览器标签页图标（原来是 404） |
| `index.html` | 补 `favicon` / `apple-touch-icon` / iOS meta；`theme-color` 改为 `#445CA4` |
| `manifest.json` | 补 `id`、主题色、图标引用 |
| `sw.js` | 缓存版本 `ds-chat-v1 → ds-chat-v2`，预缓存图标 |
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

## 部署

### Cloudflare（主站）

用 **Cloudflare Worker** 读取 **Workers KV**（`chat-demo-assets`）里的静态文件，
按扩展名给正确 MIME，`/static/*` 一年强缓存，未知路径回退到 `index.html`（SPA）。

| 组成 | 值 |
|---|---|
| Worker 脚本 | `chat-demo` |
| KV 命名空间 | `chat-demo-assets` |
| 自定义域名 | `chat.xszx.cc.cd`（区域 `xszx.cc.cd`） |
| 缓存策略 | `/static/*` 一年 immutable，其余 5 分钟 |

更新：

```powershell
python cf_workers_deploy.py     # 全量：上传文件到 KV + 部署 Worker
python cf_worker_only.py        # 只更新 Worker 代码
```

> 说明：最初尝试 Cloudflare **Pages** Direct Upload，但该账号下所有 API 部署（连只有 1 个文件的测试项目）
> 都返回边缘 500，属平台侧问题，因此改用 Workers + KV；静态内容相同，自定义域名与 PWA 能力也一样。

### GitHub Pages（镜像）

由本 fork 的 `gh-pages` 分支托管：

```powershell
python deploy_ghpages_v2.py
```

---

## 校验

不是「应该能跑」，是实测过：

- 三处地址的壳、manifest、Service Worker、图标、bundle 全部 200
- 抓下来的文件逐个与本地构建比对 **SHA-256**（`ALL MATCH`）
- 安装条件全过：HTTPS、manifest（`name` / `short_name` / `start_url` / `display: standalone`）、
  192 与 512 图标、`maskable` 图标、`apple-touch-icon` 180×180、带 `fetch` 处理的 Service Worker
- Android 圆形裁切预览确认脸部图标不被裁坏

---

## 许可与归属

- 代码：[deepseek-ai/chat-demo](https://github.com/deepseek-ai/chat-demo)，MIT License，Copyright (c) 2026 DeepSeek
- 商标：DeepSeek 及相关标识归其所有者，本仓库不主张任何商标权利
- 图标插画素材：权利归原作者，此处仅作演示用途
- 本仓库为**非官方**部署，与 DeepSeek 官方无隶属或背书关系；详见 [`NOTICE.md`](NOTICE.md)
