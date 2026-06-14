# HANDOVER

> 当前状态与待办，供新开发随时接手。只保留最新状态。

**一句话**：单文件 Markdown 美化阅读器（PWA），EarthOnline 开源产品。线上 **https://md.earthonline.site** ｜ 仓库 `EarthOnlineDev/markdown-viewer`（MIT）。

## 当前状态（2026-06）

- **单一「光谱」阅读样式**（早期的"经典蓝"主题与样式切换器已移除）。规范取自 AISelf `design-exploration`。原则：**墨为底、色作点缀；用边框、不用大面积色块**。具体：标题前彩色圆点、描边式语义徽章、描边卡片式引用块、浅底描边代码块、彩虹渐变分隔线。
- **Logo**：
  - 产品 logo = 彩虹「**MD**」字标（工具栏 / 落地页 / favicon / PWA 图标）。
  - 彩虹无限符号 **∞** = EarthOnline 的 logo，仅出现在页脚 "Built by ∞ EarthOnline"。
  - 两者统一使用**硬边五段光谱**（紫→蓝→绿→橙→红），保持品牌一致。
- **手机端 / PWA**：
  - **关键坑：file input 不能设 `accept`** —— iOS「文件」会把 `.md` 置灰、选不中（openradar #36726477）。这是"手机打不开"的真因，已移除 accept，勿加回。
  - **iOS 打开路径**：微信/飞书 →「用其他应用打开」→ 存到「文件」→ 回本页「打开 Markdown 文件」。（微信不渲染 md，看不到文字，**不能"全选复制"**；飞书新版能渲染但样式普通——我们的卖点是"打开后更美观"。）
  - **Android**：装 PWA 后可「分享 → Markdown Viewer」(share_target) 或系统「打开方式」(file_handlers) 直接打开。
  - **粘贴**：一个「粘贴 Markdown 文本」按钮——先试读剪贴板，失败再展开文本框。
  - 落地页只有一行提示，**不写长说明书**（产品要求）。
  - 可安装；service worker 离线缓存 app shell + marked。
- **部署**：https://md.earthonline.site（自定义域名 + SSL）。Vercel 团队 `earthonlinedevs-projects` / 项目 `md-viewer`。**Git Integration 已连——push 到 `main` 自动部署（已验证）**。

## 代码结构

```
md-viewer.html        单文件主体：HTML + CSS + JS 全部内联
                      · CSS 走变量；<html data-theme="aiself"> 静态保留，
                        结构规则仍带 html[data-theme="aiself"] 前缀（单主题下无害，
                        将来若加主题照此模式扩展）
manifest.webmanifest  PWA 清单（share_target + file_handlers）
sw.js                 Service Worker（离线缓存 + 接收分享 POST → 渲染）
favicon.svg           站点图标（彩虹 MD）
icons/                PWA 图标 PNG + 源 SVG（md-icon-src.svg / md-maskable-src.svg，均彩虹 MD）
server.py / mdview    本地阅读服务（/api/read + 静态资源 manifest/sw/favicon/icons）
vercel.json           / → /md-viewer.html 重写；sw.js no-cache 头
```

## 开发流程

```bash
./mdview path/to/file.md     # 本地起服务并打开（http://127.0.0.1:9274）
# 改 md-viewer.html → 本地验证 → git commit → git push origin main（自动部署）
vercel deploy --prod --yes   # 兜底：CLI 手动发布
```

- **改样式前**先读 `/Users/rik/projects/AISelf/design-exploration`（`design-system.html` §07 令牌 / `LOGO_SPEC.md`），别自创色值。
- **改图标**：编辑 `icons/md-*-src.svg` → `rsvg-convert -w N -h N <src>.svg -o <out>.png` 重新生成（192/512/maskable-512/apple-touch-icon-180）。SVG 文字用 `textLength` 锁宽，字体回退不串位。
- **gitignore**：`.vercel/`、`.env*.local`、`.DS_Store`、`url-to-markdown/`、`.claude/` 不进仓库。

## 待办 / 待决策

1. **URL 登录保护**：已决定**不做**——与"开源推广"目标冲突（加登录墙就没人能用）。"私有"已由"私有团队项目 + 公开源码(MIT)"满足。若将来要"仅内部可访问"的私有版，应另起项目并把团队升级到 Vercel Pro。
2. **旧地址 `md-viewer-green.vercel.app`**：属于另一个本机无法访问的 Vercel 账号，仍是旧版样式。正式地址是 `md.earthonline.site`，旧地址可忽略；若必须更新需先把 Vercel CLI 切到那个账号。

## 已验证

- 本地 + 线上：`/`、`manifest`、`sw`、`favicon`、`icons/*` 全 200；`/api/read` 正常。
- 单主题渲染、MD logo（内联 + 图标 + favicon）、页脚 ∞、桌面/移动（390px）无横向溢出。
- 打开文件 / 粘贴 / URL / 复制 / 目录滚动高亮 / 点 logo 回首页 均正常。
- `git push origin main` → Vercel 自动部署 已验证生效。
- 多轮对抗式 code review 通过（无 breakage），主题移除后的死代码已清理。
