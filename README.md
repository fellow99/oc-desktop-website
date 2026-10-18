# oc-desktop-website

> **OC Desktop** 的产品官网（落地页）—— 统一呈现**两个**第三方鸿蒙版别：
> [`oc-desktop-ark`](../oc-desktop-ark)（**HarmonyOS Edition**）与 [`oc-desktop-hos`](../oc-desktop-hos)（**Electron on HarmonyOS Edition**），并引用上游 **opencode** 官方桌面发行版。
>
> 预期线上地址：<https://fellow99.github.io/oc-desktop-website/>

本文是本站的**设计文档**：定位与口径、文案、页面结构、设计系统、交互、本地化与部署规则。
（命名与章节沿用同型工程 `dsh-desktop-website` 的 `README.md`，便于两站同步演进。）

---

## 1. 概览

**纯静态、零依赖**站点：HTML / CSS / 原生 JS，无框架、无构建步骤、无外部请求（字体 / CDN / 统计）。
必须能在 `file://` 下**完全离线**渲染，并由 [workflow](./.github/workflows/pages.yml) 部署到 GitHub Pages。

| 文件 | 用途 |
|---|---|
| `index.html` | 英文页（`lang="en"`） |
| `index_zh.html` | 简体中文页（`lang="zh-CN"`），与 `index.html` **结构 1:1 镜像** |
| `style.css` | 共享样式表 —— 全部主题化走 CSS 自定义属性 |
| `script.js` | 共享主题切换逻辑（语言中立，`data-label-*` 驱动） |
| `images/logo.svg` | 品牌 logo（取自父工程 `../assets/logo.svg`），同时用作 favicon 与导航/页脚字标 |
| `images/light.png` `images/dark.png` | 首屏产品截图对（浅色 / 深色，取自 **Electron on HarmonyOS Edition** 的 `../assets/screenshots/hos/`，见 §2.3） |

## 2. 定位与关键信息

### 2.1 一句话定位

**OC Desktop = opencode 的桌面端。** 四大桌面平台，鸿蒙端**两个版别**：

- **Windows / Linux / macOS**：使用**上游 opencode 官方发行版**（BETA）——本站只做引用与下载导流；
- **HarmonyOS（2in1 / 平板）**：同一产品的**两个独立第三方版别**，品牌名统一为 **OC Desktop**：**HarmonyOS Edition**（[`oc-desktop-ark`](../oc-desktop-ark)，进程内 Node v24.2.0 + **ArkWeb**，无 Electron、无 Chromium）与 **Electron on HarmonyOS Edition**（[`oc-desktop-hos`](../oc-desktop-hos)，Electron 37 = Node 22.17.0 / Chromium 138）。

两版同一上游基线 **v2.0.26**、同一份官方 Web UI、可同机共存；差异只在「谁提供 Node、谁渲染」（口径以父工程 `README.md §一` 为准）。

**Hero 主标**：ZH「让 opencode 在桌面安家」· EN *"opencode, at home on the desktop"*。
**四大桌面覆盖 + 双版别是首页头条差异点**：hero lede、`ul.os-strip` 四枚 chip（Windows / Linux / macOS / HarmonyOS）、CTA 的两个版别按钮、三张平台卡与免责声明都要出现。

### 2.2 卖点（特性卡）

1. **上游 opencode，零改动** —— 官方 Web UI 与 agent 核心原样运行，只做幂等补丁。
2. **进程内托管，同源加载** —— 服务端在应用内部启动，界面经本机端口同源加载：无 CORS、无云端中转、无账号。
3. **为鸿蒙补齐的能力** —— 终端 PTY 与外部进程在鸿蒙不可得，改用纯 JS 工具覆盖 `shell` / `grep` / `glob`。
4. **克制的权限与品牌化** —— 只申请必要权限；界面、标题与图标统一为 OC Desktop。

**实现原理（"How it works"）**：应用内启动服务端 → ArkWeb / Chromium 同源加载官方 Web UI →
壳层接管凭据 / 数据目录 / 工具覆盖，收束于「上游零改动 · 无 CORS · 无自定义协议 · 数据留在本机」。

### 2.3 素材口径（hero 截图）

- `images/light.png` / `images/dark.png` 由父工程的真机截图转录而来，**源文件是唯一事实来源**：
  `../assets/screenshots/hos/1.png` → `images/light.png`（1280×720，浅色主题）、
  `../assets/screenshots/hos/2.png` → `images/dark.png`（1280×720，深色主题）。
- 转录只做**无损**处理：原图是 RGBA 但 alpha 全为 255（不透明），因此转成 RGB 重新编码 PNG
  （约 27–29 KB），**不裁切、不缩放、不改内容**。
- 画面内容是 **OC Desktop · Electron on HarmonyOS Edition 的真实界面**（已用 Windows OCR 逐字核对）：新建会话页的
  「会话 / 随便问点什么、`/` 可查看命令、`@` 可添加上下文」输入区、模型 `DeepSeek V4 Flash`、
  agent `Default`、工作区 `workspace`、git 状态「无 Git」。因此 caption 不含任何编造内容；
  英文页额外标注「界面本身是简体中文」，避免英文读者误判。
- **不写「实机 / 设备实拍」之类无法自证的措辞**：截图取自 `assets/screenshots/hos/`（Electron 版），只能确证
  「这是 Electron on HarmonyOS Edition 的界面」，**不得**把它当作 HarmonyOS Edition 的界面证据（Ark 版暂无截图）。

### 2.4 文案准确性护栏（长期有效）

- **本站的产品只有一个：OC Desktop。** 上游 `OpenCode`（CLI / agent）与 `OpenCode Desktop`（官方桌面发行版）
  **只作为「上游引用」出现**，不写成本站自研产品；相关链接一律指向上游官方渠道：
  - 源码：<https://github.com/anomalyco/opencode/tree/v2>
  - 下载：<https://opencode.ai/zh/download>
- **禁止编造能力**：不写「自动更新」「应用商店上架发行」「官方产品 / 与上游有隶属关系」；
  必须写明两个鸿蒙版别都是**第三方移植**、Windows / Linux / macOS 版由**上游官方分发**。
- **鸿蒙端的已知边界（两版相同）要如实写**（唯一一处诚实性硬要求）：终端 PTY、依赖外部进程的工具
  （LSP / MCP / git）不可用；插件在线安装默认关闭；HarmonyOS Edition 另外不带 Chromium。
- **两个版别的验证口径分开写、互不挪用**：
  - **HarmonyOS Edition** 只写「**已构建 · host 侧门禁全绿 · 未签名 · 待真机**」（证据 `oc-desktop-ark/logs/20261018-1/DEV_CHECKLIST.md`；`TC-D-*` 全部未执行），**不得**写「真机验证通过」「已签名」「可安装」；
  - **Electron on HarmonyOS Edition** 可写「**已构建 · 签名 · 真机安装启动 + 基本流程验证**」（`docs/鸿蒙环境能力清单-v2.0.26.md` §10 / §11.2），
    但须同时承认 §5 的 ⬜-2 / ⬜-12 未取证、⬜-9 部分取证、`docs/真机验证记录.md` 仍是模板 —— **不得**写「全项真机验证通过」。
- 产品事实变化时，**两套语言页 + 本文 §2 同时改**。

## 3. 页面结构

两页共用一条纵向结构（6 个地标区块）：

1. **Nav**（`header.site-header`）：logo 字标 + `OC Desktop` 后缀 · 锚点链接（特性 / 平台 / 实现原理）·
   胶囊语言切换（`EN / 中文`）· 主题切换。≤ 760px 隐藏锚点链接，品牌 + 语言 + 主题保留。
2. **Hero**（`section#top`）：左列——等宽 eyebrow chip、`<h1>`、lede（点明两个版别）、`ul.os-strip`（4 枚系统 chip）、
   CTA 行（主：`OpenCode 官方下载` → 上游下载页；两个版别按钮：HarmonyOS Edition → `oc-desktop-ark`、Electron on HarmonyOS Edition → `oc-desktop-hos`；ghost：了解实现）、
   可信行（**两个版别的版本与验证状态各一行** + Windows / Linux / macOS 上游分发口径）；右列——带边框的截图「应用窗口」（浅 / 深两张，纯 CSS 按主题切换，见 §4.3）+ 等宽 caption（标注 Electron on HarmonyOS Edition）。
3. **Features**（`#features`）：标题 + lede + 4 张图标卡（桌面 2×2，≤ 820px 单列）。
4. **Platforms**（`#platforms`）：**三张**卡 —— HarmonyOS Edition、Electron on HarmonyOS Edition、上游 Windows / Linux / macOS（标签 + 能力要点 + 链接），下方
   免责声明（写明两个鸿蒙版别均为第三方移植）与鸿蒙端共同能力边界说明。
5. **How it works**（`#how`）：3 个编号步骤 + 箭头连接，收尾一枚结果徽标。
6. **Footer**：字标 + 一句话 + ©/许可行 + 相关链接（HarmonyOS Edition、Electron on HarmonyOS Edition、`@fellow99/node-bridge`、
   上游 opencode v2 分支、上游下载页、harmonypc-electron、Electron）。

中英两页必须**结构完全一致** —— 区块顺序、元素 class、交互行为不得分叉，只有文案被翻译。

## 4. 设计系统

### 4.1 主题 tokens（`:root` / `html[data-theme="dark"]` 上的 CSS 自定义属性）

品牌主色取父工程 **logo 的琥珀金**（`assets/logo.svg` = `#db9612`），与参考站的靛蓝体系刻意区分。

| Token | Light | Dark |
|---|---|---|
| `--bg` | `#f7f6f2` | `#0c0b0a` |
| `--surface` | `#ffffff` | `#15130f` |
| `--text` | `#1b1916` | `#f0ece3` |
| `--muted` | `#5f5a51` | `#a49c8d` |
| `--hairline` | `#e6e2d9` | `#292520` |
| `--accent` | `#a8620a` | `#e0a63c` |
| `--accent-strong` | `#8a4f05` | `#f0b955` |
| `--accent-contrast` | `#ffffff` | `#17140d` |
| `--accent-soft` | `#fbf1e0` | `#241c10` |
| `--accent-soft-border` | `#f0dfc0` | `#3a2d17` |

- **浅色主色是深琥珀 `#a8620a` 而不是 logo 的亮金 `#db9612`**：亮金配白字对比度只有 ≈2.5:1，
  达不到 AA；深琥珀配白字 ≈4.8:1 达标。**亮金 `#db9612` 只用于暗色主题**（配深色文字 ≈7:1）。
- 截图阴影 token `--shot-shadow` / `--shot-shadow-hover` 同样按主题切换。
- **主题决策顺序**：`<head>` 内联脚本在首帧前依 `localStorage['oc-theme']` → 否则
  `prefers-color-scheme` 设置 `data-theme`（不闪屏）→ CSS 渲染 tokens；
  `@media (prefers-color-scheme: dark) :root:not([data-theme])` 覆盖**无 JS** 场景。
- **`<html>` 上不得写死 `data-theme`**（参考站写了 `data-theme="light"`，那会让上面那条无 JS 兜底永远
  不命中，禁用 JS 时系统深色偏好被忽略）。本站两页的 `<html>` 只带 `lang`，主题一律由内联脚本写入。
  实测（Edge headless，禁用 JS）：系统深色 → `#0c0b0a`，系统浅色 → `#f7f6f2`，均正确。

### 4.2 字体与视觉语言

- 只用系统字体栈 —— 无衬线含中文（`PingFang SC` / `Microsoft YaHei`），等宽用于 eyebrow、标签、
  可信行与 caption（CLI / 工具气质）。
- 区块 eyebrow 用等宽 `$` 前缀，如 `$ 为什么是 OC Desktop` / `$ why OC Desktop`。
- 单一强调色（琥珀金），克制使用（实心主按钮、细线、chip 圆点）—— 不做渐变卡片。
  圆角 `--radius-lg 14px`，缓动 `--ease = cubic-bezier(0.22, 0.61, 0.36, 1)`。
- Hero 背景装饰：极淡的径向琥珀光晕 + 64px 网格，全部 token 驱动。
- 截图按「应用窗口」呈现：1px 细线边框、`--radius-lg`、分层静止阴影。**不做假窗口装饰**
  （截图本身就是真实界面）。

### 4.3 hero 截图：浅 / 深两张 + 纯 CSS 切换

两张截图**同时**存在于 DOM，由纯 CSS 按主题显示恰好一张（参考实现：`pi-desktop-website/style.css`
的 `data-shot` 段）：

| 选择器 | 作用 |
|---|---|
| `.hero-shot img[data-shot]` | 基线：全部隐藏 |
| `.hero-shot img[data-shot="light"]` | 浅色下显示 |
| `html[data-theme="dark"] .hero-shot img[data-shot="light"]` / `[data-shot="dark"]` | 深色下切换 |
| `@media (prefers-color-scheme: dark) html:not([data-theme]) …` | **无 JS** 且系统深色时切换 |

三条硬约束：

1. **禁止用 JS 切图**：CSS 必须在 JS 运行前（以及 JS 被禁用时）就选对那张；
2. 切换规则必须**限定在 `.hero-shot` 作用域内**，不能命中页面其他 `<img>`；
3. 无 JS 分支用 `html:not([data-theme])` —— 依赖 §4.1「`<html>` 上不写死 `data-theme`」这条约束，
   两者是配套的，改一个就要复查另一个。

## 5. 交互

1. **主题切换** —— 图标按钮；浅色模式显示**月亮**（点击 → 深色），深色模式显示**太阳**。
   `aria-label` 描述的是**动作**语义，通过 `data-label-dark/light` 实时互换。手动选择持久化到
   `localStorage['oc-theme']`。
2. **截图悬停微动效** —— 柔和的**减速**位移：`transform: translateY(-4px)` 用 `0.5s`，
   `box-shadow` 光晕用 `0.65s`（`--shot-shadow-hover`）。刻意慢而软；`prefers-reduced-motion` 下关闭。
3. **首屏入场** —— 一次性错峰淡入上移（`riseIn`，`fill-mode: backwards`，保证后续悬停 transform 不被覆盖）；
   页面其余部分不动画。

## 6. 响应式

- ≥ 921px 双列 hero（文案 : 截图 = `1fr : 1.44fr`，hero 容器 `max-width: 1380px`，让宽屏下截图足够大）；
  ≤ 920px 单列。
- 特性 / 平台栅格 ≤ 820px 变单列；导航锚点链接 ≤ 760px 隐藏；`.cta-row` 按钮 ≤ 640px 通栏堆叠、
  `.steps` ≤ 640px 转纵向。

## 7. 开发与部署

```bash
# 本地预览（任何静态服务器均可；或直接打开 index.html）
npx serve .        # 然后访问 http://localhost:3000/index_zh.html
```

部署由 GitHub Actions 负责（[`.github/workflows/pages.yml`](./.github/workflows/pages.yml)）：
每次推送到 `main`（以及手动 `workflow_dispatch`）都会上传仓库根目录并发布到 GitHub Pages。
需在仓库里做一次性设置：**Settings → Pages → Source → "GitHub Actions"**。

首次部署后的预期地址：<https://fellow99.github.io/oc-desktop-website/>

## 8. 贡献清单

- [ ] 改了文案？`index.html` **和** `index_zh.html` 一起改（必要时同步本文 §2 的事实）。
- [ ] 新增区块？两页 1:1 镜像 + 在 `style.css` 补 token / 样式。
- [ ] 主题或交互改动？保持 token 驱动 + 无 JS 兜底 + reduced-motion 路径。
- [ ] **截图替换**：把浅 / 深两张 **Electron on HarmonyOS Edition** 截图覆盖 `images/light.png` 与 `images/dark.png`
      （原图多大就多大，**不要**为体积缩到失真；两张尺寸必须一致，否则切换时会跳动），
      同步改两页 hero 的 `alt`、`width` / `height` 与 caption，并在本文 §2.3 更新归属（**不得**把 Electron 版截图标成 HarmonyOS Edition）。
- [ ] 换图后复查 §4.3 的三条硬约束：纯 CSS 切图、作用域限定 `.hero-shot`、无 JS 分支仍命中。
- [ ] 离线自检：`file://` 打开两页，控制台无报错、无网络请求。
- [ ] 视觉自检：1280 / 768 / 390 三个宽度 × 明暗两套主题。
- [ ] 事实自检：本文 §2.4 的护栏仍成立（尤其「不写官方隶属、不写未取证的真机结论、两版验证口径不互借」）。
- [ ] 版别自检：两页 + 本文 §2 + 父工程 `README.md §一` 用同一组名字 —— **HarmonyOS Edition**（`oc-desktop-ark`）/ **Electron on HarmonyOS Edition**（`oc-desktop-hos`），不混称「鸿蒙版」。
