# oc-desktop-website

> **OC Desktop** 的产品官网（落地页）—— [oc-desktop-hos](../oc-desktop-hos)（鸿蒙移植版）对外的主页，
> 同时把上游 **opencode** 的桌面发行版作为官方渠道引用出去。
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
| `images/hero.png` | 首屏产品截图（真实界面，见 §2.3） |

## 2. 定位与关键信息

### 2.1 一句话定位

**OC Desktop = opencode 的桌面端。** 四大桌面平台：

- **Windows / Linux / macOS**：使用**上游 opencode 官方发行版**（BETA）——本站只做引用与下载导流；
- **HarmonyOS（2in1 / 平板）**：由本工程（`oc-desktop-hos`）以**第三方移植**形态交付，品牌名 **OC Desktop**。

**Hero 主标**：ZH「让 opencode 在桌面安家」· EN *"opencode, at home on the desktop"*。

**四大桌面覆盖是首页头条差异点**：出现在 hero lede、`ul.os-strip` 四枚系统 chip（Windows / Linux / macOS /
HarmonyOS）、两张平台卡与平台免责声明里。

### 2.2 卖点（特性卡）

1. **上游 opencode，零改动** —— 官方 Web UI 与 agent 核心原样运行，只做幂等补丁。
2. **进程内托管，同源加载** —— 服务端在应用内部启动，界面经本机端口同源加载：无 CORS、无云端中转、无账号。
3. **为鸿蒙补齐的能力** —— 终端 PTY 与外部进程在鸿蒙不可得，改用纯 JS 工具覆盖 `shell` / `grep` / `glob`。
4. **克制的权限与品牌化** —— 只申请必要权限；界面、标题与图标统一为 OC Desktop。

**实现原理（"How it works"）**：应用内启动服务端 → ArkWeb / Chromium 同源加载官方 Web UI →
壳层接管凭据 / 数据目录 / 工具覆盖，收束于「上游零改动 · 无 CORS · 无自定义协议 · 数据留在本机」。

### 2.3 素材口径（hero 截图）

- `images/hero.png` 由**上游 opencode 仓库内的官方真实截图**生成：
  `../opencode/packages/web/src/assets/web/web-homepage-active-session.png`（2766×1970，Lanczos 缩放到 1600px 宽）。
- 该图是 **opencode 官方 Web UI 的真实界面**，也就是 OC Desktop 实际渲染的那一层界面 —— 因此**不构成误导**；
  页内 caption 明确标注「上游官方截图」。
- 素材（截图与品牌）均来自 MIT 许可的上游仓库 / 父工程 `assets/`，**不使用任何第三方来源图片**。
- 父工程 `../assets/screenshots/pc/` 目前为空，是**留给真机截图的位置**。补图后按 §8 的升级路径替换。

### 2.4 文案准确性护栏（长期有效）

- **本站的产品只有一个：OC Desktop。** 上游 `OpenCode`（CLI / agent）与 `OpenCode Desktop`（官方桌面发行版）
  **只作为「上游引用」出现**，不写成本站自研产品；相关链接一律指向上游官方渠道：
  - 源码：<https://github.com/anomalyco/opencode/tree/v2>
  - 下载：<https://opencode.ai/zh/download>
- **禁止编造能力**：不写「自动更新」「应用商店上架发行」「官方产品 / 与上游有隶属关系」；
  必须写明鸿蒙版是**第三方移植**、Windows / Linux / macOS 版由**上游官方分发**。
- **鸿蒙版的已知边界要如实写**（唯一一处诚实性硬要求）：终端 PTY、依赖外部进程的工具
  （LSP / MCP / git）不可用；插件在线安装默认关闭。
- **真机验证口径取保守值**：`oc-desktop-hos/README.md §当前状态` 记 v2.0.26「真机验证通过」，
  而 `docs/鸿蒙环境能力清单-v2.0.26.md §0` 记该轮 HAP **未上机**（设备掉线）、`docs/真机验证记录.md`
  仍是未填模板。三处**互相矛盾**，因此站上只写「**已构建并签名**」，**不写「真机验证通过」**；
  待 `docs/真机验证记录.md` 补齐后再放开。
- 产品事实变化时，**两套语言页 + 本文 §2 同时改**。

## 3. 页面结构

两页共用一条纵向结构（6 个地标区块）：

1. **Nav**（`header.site-header`）：logo 字标 + `OC Desktop` 后缀 · 锚点链接（特性 / 平台 / 实现原理）·
   胶囊语言切换（`EN / 中文`）· 主题切换。≤ 760px 隐藏锚点链接，品牌 + 语言 + 主题保留。
2. **Hero**（`section#top`）：左列——等宽 eyebrow chip、`<h1>`、lede、`ul.os-strip`（4 枚系统 chip）、
   CTA 行（主：`OpenCode 官方下载` → 上游下载页；次：鸿蒙版源码 → 本工程仓库；ghost：了解实现）、
   可信行（版本 / 上游基线 / 分发口径）；右列——带边框的截图「应用窗口」+ 等宽 caption。
3. **Features**（`#features`）：标题 + lede + 4 张图标卡（桌面 2×2，≤ 820px 单列）。
4. **Platforms**（`#platforms`）：两张版本卡（标签 + 能力要点 + 链接），下方平台覆盖免责声明与
   鸿蒙版能力边界说明。
5. **How it works**（`#how`）：3 个编号步骤 + 箭头连接，收尾一枚结果徽标。
6. **Footer**：字标 + 一句话 + ©/许可行 + 相关仓库链接（本工程鸿蒙版、上游 opencode v2 分支、上游下载页、
   harmonypc-electron、Electron）。

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

### 4.3 hero 截图为单图、不做主题切换

参考站有「浅色 / 深色两张真机截图 + 纯 CSS 切换」。本站**目前只有一张深色真实界面截图**，
因此 hero 只有一张 `<img>`（不引入 `data-shot` 机制，避免出现半套行为）。
升级路径见 §8 的「截图替换」。

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
- [ ] **截图替换**：把真机截图（建议 2560×1600 级别，浅 / 深各一张）放进 `images/`，
      缩到**1600px 宽**并压缩后覆盖 `images/hero.png`；
      若拿到浅 / 深两张，再在 `style.css` 恢复 `img[data-shot]` 的纯 CSS 切换规则（参考
      `dsh-desktop-website/style.css` 的 `data-shot` 段），两页 hero 同时加 `data-shot` 属性。
- [ ] 离线自检：`file://` 打开两页，控制台无报错、无网络请求。
- [ ] 视觉自检：1280 / 768 / 390 三个宽度 × 明暗两套主题。
- [ ] 事实自检：本文 §2.4 的护栏仍成立（尤其「不写官方隶属、不写未取证的真机结论」）。
