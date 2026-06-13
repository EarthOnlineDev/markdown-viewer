# HANDOVER

> 当前状态与待办。只保留最新状态。

## 当前状态（2026-06-14）

- **新增「光谱」默认样式 + 保留「经典蓝」可选样式**，已上线。
  - 视觉规范取自 `/Users/rik/projects/AISelf/design-exploration`（`design-system.html` / `LOGO_SPEC.md`）。
  - 原则：墨为底、色作点缀；用边框、不用大面积色块；无深色代码块。
  - 工具栏分段切换，选择持久化到 `localStorage['md_theme']`，前置脚本避免 FOUC。
- **已部署**：https://md-viewer-theta.vercel.app
  - Vercel 团队 `earthonlinedevs-projects` / 项目 `md-viewer`（本会话新建并 link）。
  - 提交：`cb26c80`（main，本地；无 GitHub 远端）。
- **已通过验证**：双主题 × 桌面/移动（390px）；代码块、表格、徽章、引用块、TOC 滚动高亮、主题持久化、点 logo 返回首页；移动端工具栏图标化不溢出；打印样式。
- **已做对抗式多维 review**（设计还原 / 主题隔离 / JS / 响应式·打印·a11y）并修复确认项（徽章对比度、链接墨色、引用块边框化、彩虹分隔线、FOUC 归一化、打印表头转浅底）。

## 待办 / 待决策

1. **URL 访问保护（"私有"）** — 想让线上 URL 必须登录才能访问，需把 EarthOnlineDev 团队升级到 **Pro**（当前计划不支持 production 的 Vercel Authentication，API 返回 `428 invalid_sso_protection`）。当前：项目归属私有团队、源码不公开，但 URL 公开可访问。待用户决定是否升级。
2. **自定义域名** — 可挂 `earthonline.site` 子域（该域名已在本团队，如 `flow.earthonline.site`）。建议 `md.earthonline.site` 或 `mdview.earthonline.site`，待用户确认后用 `vercel domains` / `vercel alias` 绑定。
3. **旧地址 `md-viewer-green.vercel.app`** — 属于另一个无法访问的 Vercel 账号（`.vercel` 原 orgId `team_OZsXkxsfDaL3x9LGsFgnx6bA`），仍是旧样式。若要更新它，需先把 Vercel CLI 切到该账号再部署。

## 部署备忘

```bash
vercel deploy --prod --yes     # 在本目录直接发布到生产
```

- `.vercel/`、`.env.local` 均已 gitignore；`.env.local` 为 Vercel 拉取的本地开发环境变量（含短期 OIDC token），不要提交。
- 改样式 → 本地 `./mdview` 验证 → `vercel deploy --prod --yes`。
