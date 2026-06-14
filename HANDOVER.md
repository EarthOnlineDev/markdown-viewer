# HANDOVER

> 当前状态与待办。只保留最新状态。

## 当前状态（2026-06-14）

**已开源 + PWA 化，作为 EarthOnline 开源产品推广。**

- **双主题阅读器**：光谱（默认，取自 AISelf design-exploration 规范）+ 经典蓝（原样式）。原则：墨为底、色作点缀；用边框、不用大面积色块。
- **手机端 / PWA**：
  - **关键：file input 不能设 `accept`** —— iOS「文件」会把 .md 置灰、选不中（openradar #36726477）。这是"手机打不开"的真正原因，已移除 accept。
  - **iOS 真实路径**：微信/飞书 →「用其他应用打开」→ 存到「文件」→ 回本页「打开 Markdown 文件」。（微信不渲染 md；飞书新版能渲染但样式很普通——我们的卖点 = 更美观。**不要再写"在微信里全选复制"，那是错的，看不到文字**。）
  - **Android**：装 PWA 后可走 Web Share Target（分享→本应用）/ file_handlers（打开方式）。
  - **粘贴**：一个「粘贴 Markdown 文本」按钮，先试读剪贴板、失败则展开文本框。
  - 落地页只有一行提示，不写说明书（用户明确要求：好用的工具不需要长说明）。
  - 可安装（manifest + service worker + 图标）；离线缓存 app shell + marked。
- **底部署名**：落地页与阅读页均有 "Built by EarthOnline"（链 earthonline.site）。
- **开源**：MIT License；GitHub 仓库 `EarthOnlineDev/markdown-viewer`（公开）。
- **部署**：**https://md.earthonline.site**（自定义域名，已绑定+SSL）；备用 https://md-viewer-theta.vercel.app 。Vercel 团队 earthonlinedevs-projects / 项目 md-viewer。
- **本地**：`./mdview`；server.py 现也服务 manifest/sw/favicon/icons，与线上一致。

## 已验证

- 本地 200：`/`、`/manifest.webmanifest`、`/sw.js`、`/favicon.svg`、`/icons/*`；`/api/read` 正常。
- 落地页新入口（剪贴板/粘贴/URL/打开文件）与引导；粘贴→渲染通路；阅读页底部署名。
- Service Worker 注册成功；桌面/移动（390px）无横向溢出。
- 双主题切换、持久化、点 logo 返回首页仍正常。

## 待办 / 待决策

1. **URL 登录保护（"私有"）** — 当前 Vercel 计划不支持 production 的 Vercel Authentication（API `428`）。若要 URL 必须登录才可访问，需升级团队到 Pro。当前：私有团队项目 + 公开 URL。
2. ~~自定义域名~~ ✅ 已绑定 `md.earthonline.site`（Aliyun DNS: CNAME md → cname.vercel-dns.com；Vercel 已签发 SSL）。
3. **Git Integration** — 已（尝试）将 Vercel 项目连到 GitHub 仓库；之后 push 到 main 可自动部署。若未连成，仍可 `vercel deploy --prod --yes`。
4. **旧地址 `md-viewer-green.vercel.app`** — 属另一个无法访问的 Vercel 账号，仍是旧版；如需更新需切换 CLI 登录。

## iOS 限制（产品须知）

iOS 不支持 Web 分享目标与网页文件处理器，所以 iOS 用户走「复制文本→粘贴」或「存到文件→打开文件」；Android 才有「分享到 App / 打开方式」直达。落地页引导已据此分平台说明。

## 部署备忘

```bash
vercel deploy --prod --yes          # CLI 直接发布
# 或 push 到 main（若已连 Git Integration，自动部署）
```

- `.vercel/`、`.env.local`、`.DS_Store`、`url-to-markdown/`、`.claude/` 均已 gitignore，不进公开仓库。
