# Markdown Viewer

一个**单文件**的 Markdown 美化阅读器（`md-viewer.html`），零构建、零依赖（仅 CDN 引入 `marked`）。支持本地文件、URL、粘贴加载，自动生成目录，并提供两套可切换的阅读样式。

线上地址：**https://md-viewer-theta.vercel.app**

---

## 功能

- **两套样式**（工具栏右上角分段切换，选择记忆在 `localStorage`，刷新不闪烁）
  - **光谱**（默认）— 采用 AISelf 设计规范：纯白纸底 + 宋体阅读 + 苹方界面，墨为底、彩虹色作边框/点缀。
  - **经典蓝** — 原始蓝色样式，完整保留为可选主题。
- **多种加载方式**：拖放文件、点击选择、粘贴路径/URL、直接 `Cmd/Ctrl+V` 粘贴 Markdown 文本。
- **目录（TOC）**：≥3 个标题时自动生成左侧栏，带滚动高亮；可折叠。
- **本地文件**：配合本地服务（`mdview` / `server.py`）按绝对路径读取；在线版可回连本地服务。
- **GitHub 链接**：`github.com/.../blob/...` 自动转 `raw` 加载。
- **图片**：拖入文件夹自动解析相对路径图片；或手动填目录绝对路径。
- **复制**：一键复制为富文本（HTML）+ 纯文本。
- **打印**：内置 A4 打印样式（代码块/表头自动转浅底省墨）。

## 两套样式说明

| | 光谱（默认 `aiself`） | 经典蓝（`classic`） |
|---|---|---|
| 纸底 | 纯白 `#FFFFFF` | `#faf9f7` |
| 阅读字体 | 宋体 / Noto Serif SC | Noto Serif SC |
| 界面字体 | 苹方 PingFang | Inter |
| 强调色 | 五段光谱（紫蓝绿橙红），仅作边框/点缀 | 蓝 `#2563eb` |
| 代码块 | 浅底 + 描边 | 浅底 + 描边 |
| 引用块 | 描边卡片 + 彩色左条（蓝/橙/绿） | 蓝色填充块 |
| 语义徽章 | 描边式（彩色边 + 深色字） | 填充式 |
| 分隔线 | 彩虹渐变 | 灰线 |

设计原则：**墨为底、色作点缀；用边框，不用大面积色块。** 光谱样式的视觉规范来源见下方「设计来源」。

## 本地使用

```bash
./mdview                 # 启动本地服务并打开浏览器
./mdview path/to/file.md # 启动并直接打开指定文件
# 或
python3 server.py [file.md]
```

本地服务监听 `http://127.0.0.1:9274/`，提供 `/api/read?path=<绝对路径>` 读取本地 Markdown（带 CORS，便于线上版回连本地）。

## 在线使用

打开 https://md-viewer-theta.vercel.app ，拖放文件、粘贴 URL/路径，或直接粘贴 Markdown 文本。

## 部署

托管在 Vercel（团队 `earthonlinedevs-projects`，项目 `md-viewer`），无 GitHub 远端，使用 Vercel CLI 直接部署：

```bash
vercel deploy --prod --yes
```

`vercel.json` 将 `/` 重写到 `/md-viewer.html`；`.vercelignore` 排除 `server.py`、`mdview`、`url-to-markdown/` 等本地文件。

## 设计来源

光谱样式 1:1 取自 **`/Users/rik/projects/AISelf/design-exploration`**：
- 设计令牌（颜色/字体/间距/圆角/阴影）：`design-system.html` §07
- 品牌（∞ 光谱标记、字标、配色叙事）：`LOGO_SPEC.md`

修改光谱样式前请先对齐该规范，不要自创色值。

## 文件结构

```
md-viewer.html   主体（HTML + CSS + JS 全部内联）
server.py        本地阅读服务（含 /api/read 本地文件接口）
mdview           启动脚本（调 server.py）
vercel.json      Vercel 重写规则
.vercelignore    部署排除项
url-to-markdown/ 独立小工具（网页转 Markdown 的素材，与阅读器无关）
```
