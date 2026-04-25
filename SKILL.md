---
name: 课程 HTML Slides 构建器
description: 批量生成多页独立 HTML 课件 Slides / Web PPT，包含完整工作流：配色风格设计 → Markdown 设计稿 → HTML 文件生成 → 浏览器截图验证。支持封面、关卡过渡页、内容页、互动演示、投票页、任务指南、实战练习、总结页等页面类型。每个 slide 是一个独立 HTML 文件，必须通过共享 CSS/JS、左右热区、键盘/触屏支持和底部导航栏链接成完整课件流。适用于课堂投影、训练营、工作坊和可交互网页 PPT。当用户提到"课件"、"web PPT"、"网页 PPT"、"slides"、"HTML 演示"、"课堂 PPT"、"教学课件"或需要将课程大纲转为可演示的网页时，使用此 skill。
---

# 课程 HTML Slides 构建器

将课程大纲转化为**多页独立 HTML 文件**形式的课件 Slides / Web PPT。默认产物要能直接投影、翻页、互动和交付给用户打开。完整流程：

1. **风格设计** — 确定配色、字体、圆角等视觉系统
2. **Markdown 设计稿** — 将大纲转为逐页结构化设计文档
3. **HTML 生成** — 按设计稿批量生成独立 HTML 文件
4. **截图验证** — 用浏览器检查导航、溢出、图片、交互和美观程度

每一页都是可独立打开的 HTML 网页，通过底部导航栏串联为完整的演示流。

---

## 核心原则

1. **每页一个独立 HTML 文件** — 命名规则：`pXX-slug.html`（如 `p01-cover.html`、`p05-part1-title.html`）
2. **共享设计系统** — 所有页面引用同一套 CSS（`styles/theme.css`、`styles/animations.css`、`styles/components.css`）和 JS（`js/slide-nav.js`）
3. **满屏不滚动** — 每个 slide 适配 `100vh`，内容超出时拆分为多页，绝不允许页内纵向滚动
4. **教学场景优先** — 为投影仪/大屏设计，字大、对比度高、操作区域大，支持键盘和触屏翻页
5. **底部导航栏是硬要求** — 每页都必须有统一的底部导航栏、页码、当前章节 badge、上一页/下一页按钮；封面和结尾也不能省略
6. **渐进揭示** — 利用 `.reveal` 动画类和 click-to-reveal 交互，支持按教学节奏逐步展示内容
7. **先做可用体验，再做装饰** — 每一页必须一眼看懂主题、操作和下一步；视觉装饰不能遮挡文本、导航或交互控件

---

## Web PPT 质量基线

生成的页面不能只是“能打开的 HTML”，而要像一套真正可上课的网页 PPT。

### 必须具备

1. **统一底部导航栏**：固定在页面底部，包含上一页、当前章节 badge、`PXX / 总数`、下一页。
2. **顶部进度条**：每页使用 `calc(当前页/总页数*100%)`，和导航页码一致。
3. **左右翻页热区**：每页都包含 `slide-hotzone-left/right`，配合 `slide-nav.js` 支持点击、键盘和触屏。
4. **大屏可读性**：1280×720 或 1280×800 下标题、按钮、卡片文字必须清楚；正文不要低对比灰字堆满页面。
5. **有视觉主锚点**：封面、关卡页、案例页、任务页必须有一个明显视觉中心，如大标题、大图、路径图、产品截图、视频、流程图或互动面板。
6. **交互有状态**：投票、点击揭示、计时器、连线游戏等交互要有选中态、完成态或反馈文案。
7. **截图验证后交付**：生成或修改后，至少检查核心页面的桌面截图；有复杂响应式时再检查手机尺寸。

### 明确避免

- 不要生成纯文字堆叠页、普通卡片网格堆满页、没有主视觉的“文档式 PPT”。
- 不要让页面主体内容被底部导航栏遮挡。
- 不要把标签、按钮、说明文字放在会被浮层盖住的位置。
- 不要使用过多深色页；除非课程风格要求，默认优先浅色高对比页面。
- 不要在共享 CSS/JS 里塞单页样式；页面专用样式放在当前 HTML 的 `<style>`。

---

## Phase 0: 确定工作模式

| 模式 | 说明 | 流程 |
|---|---|---|
| **Mode A: 从零开始** | 拿到课程大纲，从头构建完整课件 | Phase 1 → 2 → 3 → 4 → 5 → 6 → 7 |
| **Mode B: 从设计稿生成** | 已有 Markdown 设计稿和风格方案 | Phase 4 → 5 → 6 → 7 |
| **Mode C: 从现有 CSS 生成** | 已有 CSS 和设计稿，直接生成 HTML | Phase 5 → 6 → 7 |
| **Mode D: 修改/新增页面** | 修改已有的 HTML slide 文件 | Phase 8 |

---

## Phase 1: 风格设计

目标：确定整套课件的视觉风格，输出 `style-guide.md`。

### 1.1 快速选择或自定义

先问用户偏好：

> "你希望课件是什么风格？我有 5 个预设方案可以选，也可以完全自定义：
> - 🍊 暖色教育风（橙+蓝）
> - 🧊 科技冷色风（靛蓝+青色）
> - 🌿 清新自然风（绿+琥珀）
> - 🌙 暗黑极客风（深色+霓虹）
> - 🎨 极简雅致风（黑白+红）
>
> 或者告诉我你的品牌色 / 关键词，我来设计。"

→ 读取 `references/style-presets.md` 获取预设方案的完整参数。

### 1.2 确认设计参数

无论选预设还是自定义，最终确认以下参数：

| 参数 | 说明 |
|---|---|
| `--base-bg` | 页面底色 |
| `--primary` | 主色（按钮、强调） |
| `--secondary` | 辅色（第二层级强调） |
| `--accent-text` | 正文色 |
| `--surface` | 卡片底色 |
| 字体 | 标题字体 + 正文字体 |
| 圆角 | 风格（大/中/小/无） |
| 渐变 | 主色渐变 + 辅色渐变方向和色值 |

### 1.3 输出 style-guide.md

生成 `style-guide.md` 放在项目目录中，包含所有确认的设计参数，作为后续 `theme.css` 生成的唯一依据。

---

## Phase 2: Markdown 设计稿

目标：将课程大纲转为逐页结构化设计文档。

→ 读取 `references/design-spec-format.md` 获取标准格式规范。

### 2.1 理解课程大纲

1. 读取用户提供的课程大纲（可能是文本、Markdown、或口头描述）
2. 识别课程的逻辑结构：几个部分？每部分几个知识点？有哪些互动环节？
3. 向用户确认理解是否正确

### 2.2 规划页面序列

为每个内容块分配页面类型：

| 类型 | Emoji | 典型用途 |
|---|---|---|
| `cover` | 🎬 | 课程封面、结尾页 |
| `section-title` | 🔷 | 章节间隔页 |
| `level-transition` | 🎮 | 闯关/阶段过渡页 |
| `content` | 📖 | 知识讲解、概念介绍 |
| `interactive-demo` | 🤖 | 嵌入演示、工具体验 |
| `vote` | 🗳️ | 课堂投票、选择题、情绪投票 |
| `task-guide` | 📝 | 操作步骤指引 |
| `practice` | ⏱ | 动手练习（含计时器） |
| `summary` | 📊 | 要点回顾、总结 |
| `qrcode` | 🔗 | 扫码加群、资源链接 |

输出页面目录表：`页码 | 文件名 | 类型 | 标题 | 所属部分`

### 2.3 编写逐页设计

按 `references/design-spec-format.md` 的格式，为每一页编写：
- 类型、背景、布局
- 具体内容（文字、卡片、列表等）
- 交互形式（reveal、click-to-reveal、翻转卡片等）
- 导航信息（上一页、下一页、当前章节 badge、页码）
- 备注（教学节奏提示）

### 2.4 用户审核

将设计稿交给用户审核，确认每页内容和交互方案后才进入下一步。

---

## Phase 3: 搭建项目结构

确保目录结构如下：

```
slides/
├── styles/
│   ├── theme.css          # 全局主题（Phase 4 生成）
│   ├── animations.css     # 动画系统
│   └── components.css     # 可复用组件
├── js/
│   └── slide-nav.js       # 导航控制器
├── images/                # 图片资源
├── p01-cover.html
├── p02-xxx.html
├── ...
└── index.html             # 可选：目录页
```

**如果 `styles/` 和 `js/` 文件已存在，不要修改它们。** 直接使用已有的设计系统。

---

## Phase 4: 生成 theme.css

根据 `style-guide.md` 生成参数化的 `theme.css`，核心是 CSS 变量：

```css
:root {
  /* === 色盘（从 style-guide.md 提取） === */
  --base-bg: [值];
  --primary: [值];
  --secondary: [值];
  --accent-text: [值];
  --surface: [值];

  /* === 色阶（从主色/辅色自动推导 50~900） === */
  --primary-50: [...];  --primary-100: [...]; /* ... */
  --secondary-50: [...]; /* ... */
  --neutral-50: [...]; /* ... */

  /* === 字号层级（全部 clamp()） === */
  --fs-display: clamp(2.5rem, 5vw, 4rem);
  --fs-title: clamp(1.8rem, 3.5vw, 2.8rem);
  --fs-h2: clamp(1.4rem, 2.5vw, 2rem);
  --fs-h3: clamp(1.1rem, 2vw, 1.5rem);
  --fs-body: clamp(0.95rem, 1.5vw, 1.2rem);
  --fs-small: clamp(0.8rem, 1.2vw, 1rem);
  --fs-label: clamp(0.7rem, 1vw, 0.85rem);

  /* === 圆角（根据风格调整） === */
  --r-xs: [值]; --r-sm: [值]; --r-md: [值];
  --r-lg: [值]; --r-xl: [值]; --r-full: 9999px;

  /* === 间距 === */
  --slide-pad: clamp(24px, 4vw, 48px);
  --gap-xs: 4px; --gap-sm: 8px; --gap-md: 16px; --gap-lg: 24px;
}
```

同时生成配套的背景类：

```css
.bg-cream   { background: var(--base-bg); color: var(--accent-text); }
.bg-primary-grad { background: linear-gradient([角度], var(--primary), [浅色值]); color: #fff; }
.bg-secondary-grad { background: linear-gradient([角度], var(--secondary), [浅色值]); color: #fff; }
.bg-dark    { background: #1a1a2e; color: #f0f0f0; }
```

如果已有 `theme.css`、`animations.css`、`components.css`，**不要重新生成**，直接复用。

---

## Phase 4.5: 共享导航系统最低要求

如果项目尚未提供共享 CSS/JS，必须生成可用的导航系统。核心目标：每个 HTML 文件直接打开也能翻页。

### components.css 必须包含

- `.slide-page`：`min-height:100vh; max-height:100vh; display:flex; flex-direction:column; overflow:hidden;`
- `.slide-body`：`flex:1; padding:var(--slide-pad); overflow:hidden;`
- `.top-progress`：固定在顶部的进度条，`z-index` 高于主体。
- `.slide-nav`：底部导航栏，`display:flex; justify-content:space-between; flex-shrink:0; backdrop-filter:blur(...)`。
- `.slide-nav.dark`：暗色/视频/渐变页的半透明深色导航。
- `.nav-btn`、`.nav-btn-primary`、`.nav-btn-secondary`、`.nav-btn.disabled`。
- `.slide-hotzone-left/right` 和 `.hotzone-icon`。
- `.badge-*`、`.reveal`、`.delay-*` 等基础组件。

### slide-nav.js 必须支持

1. 读取 `#btn-prev` 和 `#btn-next` 的 `href`。
2. 键盘：`ArrowLeft` / `PageUp` 到上一页，`ArrowRight` / `Space` / `PageDown` 到下一页。
3. 左右热区点击翻页。
4. 触屏水平滑动翻页。
5. 自动为 `.reveal` 元素添加 `.is-visible`。
6. 提供 `initTimer(elementId,totalSeconds)`，供练习页倒计时调用。
7. 跳转前忽略 `.disabled` 按钮和 `href="#"`。

---

## Phase 5: 逐页生成 HTML

### 5.1 HTML 骨架模板

每个页面遵循以下骨架：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PXX · 页面标题 — 课程名称</title>
  <link rel="stylesheet" href="styles/theme.css">
  <link rel="stylesheet" href="styles/animations.css">
  <link rel="stylesheet" href="styles/components.css">
  <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
  <style>
    /* ===== 本页专用样式 ===== */
  </style>
</head>
<body>
  <div class="slide-page [bg-class]">

    <!-- 顶部进度条 -->
    <div class="top-progress" style="width: calc(当前页/总页数*100%)"></div>

    <!-- 左右热区（悬停显示翻页箭头） -->
    <div id="hotzone-left" class="slide-hotzone-left">
      <div class="hotzone-icon">←</div>
    </div>
    <div id="hotzone-right" class="slide-hotzone-right">
      <div class="hotzone-icon">→</div>
    </div>

    <!-- ===== 页面主体内容 ===== -->
    <div class="slide-body [center]">
      <!-- 内容在这里 -->
    </div>

    <!-- 底部导航栏：每页必备 -->
    <nav class="slide-nav">
      <a id="btn-prev" class="nav-btn nav-btn-secondary" href="上一页.html">
        ← 上一页
      </a>
      <div style="display:flex; align-items:center; gap:var(--gap-sm);">
        <span class="badge badge-primary"><i data-lucide="sparkles" style="width:12px;height:12px"></i> 当前部分</span>
        <span class="nav-page-info">PXX / 总数</span>
      </div>
      <a id="btn-next" class="nav-btn nav-btn-primary" href="下一页.html">
        下一页 →
      </a>
    </nav>

  </div>
  <script src="js/slide-nav.js"></script>
  <script>lucide.createIcons();</script>
  <!-- 本页专用脚本（如有） -->
</body>
</html>
```

### 5.2 关键规则

#### 背景选择

| 场景 | CSS 类 |
|---|---|
| 普通内容页 | `bg-cream` |
| 主色幕间标题 | `bg-primary-grad` |
| 辅色幕间标题 | `bg-secondary-grad` |
| 少量强氛围页 | `bg-dark` |

在渐变/暗色背景上，导航栏需覆盖半透明暗色样式：
```html
<nav class="slide-nav dark">
```

#### 底部导航栏规范

每页都要有完整导航，不允许省略。导航栏必须：

1. 放在 `.slide-page` 内、`.slide-body` 后。
2. 使用 `<nav class="slide-nav">`，暗色页使用 `<nav class="slide-nav dark">`。
3. 上一页链接为 `id="btn-prev"`，下一页链接为 `id="btn-next"`，这样 `slide-nav.js` 才能绑定键盘、热区和滑动。
4. 第一页上一页按钮添加 `disabled`；最后一页下一页按钮添加 `disabled` 或明确的结束文案。
5. 中间区域显示当前章节 badge 和 `PXX / 总数`。如果中途插入 `p05b`、`p10a` 等页面，页码显示也要同步。
6. 视觉验证时检查 `.slide-body` 底部不要压到 `.slide-nav` 顶部。

推荐中间区：

```html
<div style="display:flex;align-items:center;gap:var(--gap-sm);">
  <span class="badge badge-primary"><i data-lucide="map" style="width:12px;height:12px"></i> 本章</span>
  <span class="nav-page-info">P08 / 18</span>
</div>
```

#### 美观与清晰度规则

1. **每页一个主视觉**：封面用课程名 + 任务地图/产品截图；关卡页用大数字 + 关卡名；投票页用选择面板；练习页用计时器或工作台。
2. **内容分层**：标题、导语、主体、收束句四层清楚。不要把所有信息放成同等大小的卡片。
3. **浅色优先**：常规课件页优先 `bg-cream` 或浅色自定义背景，暗色只用于少量转场或强情绪页。
4. **卡片要有功能**：卡片用于重复项目、投票、步骤、角色、工具面板；不要把整页拆成无意义卡片墙。
5. **使用真实资产**：有产品、人物、作品或案例时，优先使用真实截图、图片、视频或已有 `images/` 资源；不要只用抽象图形。
6. **文字不要压线**：标签、按钮、说明条不要放在浮层底部或导航附近；复杂页面优先用 Playwright 截图检查。
7. **图标统一**：优先使用 lucide 图标，页面 `<head>` 引入 lucide，末尾调用 `lucide.createIcons()`。
8. **响应式兜底**：桌面保持满屏不滚动；移动端可用 `@media(max-width:640px){.slide-page{overflow-y:auto;max-height:100vh}}` 作为兜底。

#### 入场动画

1. 给需要入场动效的元素添加 `reveal` 类
2. 用 `delay-1` ~ `delay-6` 控制级联延迟
3. 支持方向变体：`from-left`、`from-right`、`scale-in`
4. `slide-nav.js` 自动触发 `.is-visible`，无需手动写 JS

```html
<div class="reveal delay-1">第一个出现</div>
<div class="reveal delay-2">第二个出现</div>
<div class="reveal delay-3 from-left">从左滑入</div>
```

#### 所有尺寸使用 `clamp()`

```css
/* ✅ 正确 */
font-size: clamp(1rem, 2vw, 1.5rem);
padding: clamp(10px, 2vw, 20px);

/* ❌ 禁止 */
font-size: 24px;
padding: 20px;
```

#### 首页 & 末页特殊处理

- 第一页的「上一页」按钮添加 `disabled` 类
- 最后一页的「下一页」按钮添加 `disabled` 类或替换为结束文案

---

## Phase 5.5: 页面类型详细模板

### 🎬 封面 (Cover)

结构：课程名必须成为第一视觉。推荐“左侧课程标题 + 右侧任务地图/产品截图/主视觉”的浅色封面；只有明确要求时才用暗色全屏背景。

```html
<div class="slide-page bg-cream">
  <div class="cover-scene"></div>
  <div class="slide-body">
    <div class="cover-layout">
      <section class="cover-copy">
        <div class="eyebrow reveal delay-1">课程眉标</div>
        <h1 class="title reveal delay-2">课程主标题<span>强调词</span></h1>
        <p class="subtitle reveal delay-3">一句话说明这节课会做出什么。</p>
      </section>
      <section class="visual-stage reveal delay-3">
        <!-- 任务地图 / 产品截图 / 课程产出预览 -->
      </section>
    </div>
  </div>
  <!-- 必须保留底部导航栏 -->
</div>
```

关键样式：
- `.cover-scene`：浅色底 + 轻网格/淡色面，不使用抢眼装饰背景。
- `.cover-layout`：`display:grid; grid-template-columns:.95fr 1.05fr; align-items:center;`
- `.title`：主标题足够大，品牌/课程名第一眼可见。
- `.visual-stage`：放课程产出预览、路径图、产品截图或任务地图，不能只是抽象渐变。

### 🔷 幕间标题 (Section Title)

结构：渐变色全屏 + 居中大字 + 部分编号 + 关键词标签

```html
<div class="slide-page bg-primary-grad">
  <div class="section-title-page">
    <div class="deco-blob-1"></div> <!-- 装饰 blob -->
    <div class="deco-blob-2"></div>
    <div class="section-number-bg">1</div> <!-- 超大装饰数字 -->
    <div class="reveal delay-1">
      <span class="badge badge-white">📘 第 1 部分</span>
    </div>
    <h1 class="section-big-title reveal delay-2">
      <span class="emoji-large">🌍</span>
      标题文字
    </h1>
    <div class="keyword-tags reveal delay-3">
      <span class="tag">关键词1</span>
      <span class="tag">关键词2</span>
    </div>
  </div>
</div>
```

### 🎮 关卡过渡页 (Level Transition)

用于训练营、闯关式课程、阶段切换。页面目标是让学生知道“我们进入下一关，要做什么”。

结构：左侧或居中大数字 + 关卡名称，右侧 3-4 个模块/任务卡。适合使用浅色或深色，但导航必须清楚。

```html
<div class="slide-page bg-cream">
  <div class="level-bg"></div>
  <div class="slide-body">
    <div class="transition-layout">
      <div class="level-panel reveal delay-1">
        <div class="level-ring"></div>
        <div class="level-core">
          <div class="level-chip">LEVEL 05</div>
          <div class="level-num">05</div>
          <div class="level-name">成卡</div>
        </div>
      </div>
      <div class="module-area">
        <div class="eyebrow reveal delay-2">final output</div>
        <div class="main-copy reveal delay-2">把想法压成一张产品设计卡。</div>
        <!-- 模块卡片 / 下一步提示 -->
      </div>
    </div>
  </div>
  <!-- 底部导航 -->
</div>
```

### 📖 内容页 (Content)

布局方式：
- 单列：`flex-direction: column`
- 双列：`grid-template-columns: 1fr 1fr`
- 左窄右宽：`grid-template-columns: 1fr 1.6fr`

常用组件：
- `.card` / `.card-primary` / `.card-secondary`：带左色条的强调卡片
- `.quote-box` / `.quote-box.alt`：引用框
- `.badge-primary` / `.badge-secondary`：标签徽章
- `.step-list` + `.step-item`：编号步骤
- `.tag-list` + `.tag`：关键词标签

### 🤖 互动演示 (Interactive Demo)

结构：左侧步骤说明 + 右侧 iframe 嵌入

关键交互：
- **Click-to-reveal 步骤卡**：初始 `hidden-step`，点击后添加 `show-step`
  → 读取 `click-to-reveal-pattern.md` 获取完整实现
- **iframe 嵌入**：带头部标签 + 新窗口打开按钮 + 跨域失败后备方案
- **演示提示词芯片**：点击复制到剪贴板

```html
<!-- iframe 嵌入模板 -->
<div class="iframe-wrap">
  <div class="iframe-header">
    <div class="iframe-title-row">
      <div class="iframe-dot"></div>
      <span>工具名称 — 演示窗口</span>
    </div>
    <button class="iframe-tip-btn" onclick="window.open('URL','_blank')">
      🔗 新窗口打开
    </button>
  </div>
  <div class="iframe-frame-area">
    <iframe src="URL" allow="microphone; camera" sandbox="allow-scripts allow-same-origin allow-forms allow-popups"></iframe>
  </div>
</div>
```

### 🗳️ 投票页 (Vote / Poll)

用于课堂选择、情绪投票、方案投票。必须有可点击选项和可见反馈。

推荐结构：
- 左侧：问题背景、投票规则、金句或教师提示。
- 右侧：投票卡片、选项按钮、结果条或揭示区。

交互规则：
1. 选项点击后添加 `.is-picked` / `.is-selected`。
2. 多选题允许 toggle；单选题点击时清空同组其他选项。
3. 结果用数字、进度条、揭示文案或高亮态展示。
4. 交互后页面布局不能跳动压到导航栏。

### 📝 任务指南 (Task Guide)

结构：左侧操作步骤 + 右侧聊天动画（逐步展示对话构建过程）

→ 读取 `chat-animation-pattern.md` 获取完整实现方案。

高级交互模式：
1. **聊天气泡打字机动画**：逐字显示对话
2. **分幕演示**：多幕对话，每幕点击「继续」推进
3. **提示词面板切换**：动画播完后，聊天面板淡出，完整内容面板淡入
4. **一键复制**：`navigator.clipboard.writeText()` + 视觉反馈

### ⏱ 实战练习 (Practice)

结构：嵌入 iframe + 倒计时器

```html
<!-- 倒计时器 -->
<div class="timer-widget">
  <div class="timer-display" id="timer1">05:00</div>
  <div class="timer-label">剩余时间</div>
  <div class="timer-controls">
    <button class="timer-btn start" data-timer="timer1" data-action="start">开始</button>
    <button class="timer-btn reset" data-timer="timer1" data-action="reset">重置</button>
  </div>
</div>
<script>
  // slide-nav.js 提供 initTimer
  initTimer('timer1', 300); // 5分钟
</script>
```

---

## Phase 6: 导航链接检查

生成所有页面后，逐页检查：

1. **`btn-prev` 的 `href`** 指向正确的上一页文件名
2. **`btn-next` 的 `href`** 指向正确的下一页文件名
3. **进度条宽度** `calc(当前页/总页数*100%)` 数值正确
4. **页码显示** `PXX / 总数` 一致
5. 第一页 `btn-prev` 带 `disabled` 类
6. 最后一页 `btn-next` 带 `disabled` 类或指向合理目标

---

## Phase 7: 验证 & 交付

### 验证清单

- [ ] 每页在 1280×720 或 1280×800 分辨率下无纵向滚动、无横向溢出
- [ ] 每页都有底部导航栏、顶部进度条和左右热区
- [ ] 主体内容没有和底部导航栏重叠
- [ ] 所有 `reveal` 元素正常入场
- [ ] 前后翻页导航链接正确
- [ ] 进度条数值准确
- [ ] 暗色/渐变页的导航栏文字可见
- [ ] 键盘左右键翻页正常
- [ ] 复制按钮功能正常（如有）
- [ ] 倒计时器运行正常（如有）
- [ ] 图片 / 视频资源正常加载，不出现空白占位
- [ ] lucide 图标正常渲染，没有只显示空白或原始属性
- [ ] 响应式：在不同屏幕尺寸下布局不崩坏
- [ ] 配色与 style-guide.md 一致

### 推荐自动检查

生成后优先使用浏览器/Playwright 对关键页面截图并检查：

```js
const overflow = document.documentElement.scrollWidth > innerWidth + 2 ||
  document.documentElement.scrollHeight > innerHeight + 2;
const lucideMissing = [...document.querySelectorAll('[data-lucide]')]
  .filter(el => !el.querySelector('svg') && el.tagName.toLowerCase() !== 'svg').length;
const nav = document.querySelector('.slide-nav')?.getBoundingClientRect();
const body = document.querySelector('.slide-body')?.getBoundingClientRect();
const navOverlap = nav && body ? body.bottom > nav.top + 2 : false;
```

如果 `overflow`、`lucideMissing`、`navOverlap` 任一为真，先修复再交付。

### 交付产物

向用户报告：
1. 生成/修改的文件清单
2. 总页数和课程结构
3. 特殊交互说明（如哪些页面有 click-to-reveal、聊天动画等）
4. 浏览方式：用浏览器直接打开 HTML 文件，← → 键翻页

---

## Phase 8: 修改现有页面

修改前必须：
1. **读取当前页面**：完整阅读目标 HTML 文件
2. **读取 CSS 文件**：确认可用的样式类和变量
3. **检查内容密度**：确保修改后内容仍在 100vh 内显示

修改原则：
- **不修改共享文件**（theme.css / animations.css / components.css / slide-nav.js）
- 仅在 `<style>` 块中添加本页专用样式
- 仅在 `<script>` 块中添加本页专用脚本
- 内容超出时主动拆页，不等用户要求
- 修改后必须重新检查该页和相邻页导航链接
- 如果用户说“更好看/美化/重新设计”，不仅换颜色，还要重做信息层级、主视觉和交互反馈

---

## 可用组件速查（定义在共享 CSS 中）

### 布局
- `.slide-page` — 全屏容器 (`min-height:100vh; flex-direction:column`)
- `.slide-body` — 主内容区 (`flex:1; padding:var(--slide-pad)`)
- `.slide-body.center` — 居中内容
- `.grid-2` / `.grid-3` / `.grid-4` — 网格布局
- `.flex` / `.flex-col` / `.flex-center` — Flexbox 快捷类

### 卡片 & 容器
- `.card` — 白底圆角卡片
- `.card-primary` / `.card-secondary` — 带左色条的强调卡片
- `.quote-box` / `.quote-box.alt` — 引用框
- `.prompt-box` — 暗色代码/提示词框

### 标签 & 徽章
- `.badge-primary` / `.badge-secondary` / `.badge-white` / `.badge-dark`
- `.time-badge` — 时间徽章（半透明毛玻璃风格）
- `.tag` / `.tag.dark` — 关键词标签

### 导航
- `.slide-nav` — 底部导航栏
- `.nav-btn-primary` — 主色主按钮（下一页）
- `.nav-btn-secondary` — 轮廓次按钮（上一页）
- `.nav-btn-index` — 辅色目录按钮
- `.top-progress` — 顶部进度条
- `.slide-hotzone-left/right` — 两侧悬浮翻页热区

### 动画
- `.reveal` + `.delay-1`~`.delay-6` — 入场显示
- `.from-left` / `.from-right` / `.scale-in` — 方向变体
- `.float` / `.float-xy` / `.float-slow` — 浮动装饰
- `.pulse-primary` / `.pulse-secondary` — 呼吸脉冲
- `.flip-card` + `.flip-card-inner` + `.flip-front` + `.flip-back` — 翻转卡片
- `.anim-slide-up` / `.anim-fade-in` / `.anim-scale-in` — 进入动画
- `.cursor` — 打字机光标

### 特殊组件
- `.roadmap` + `.roadmap-station` — 时间轴路线图
- `.timer-widget` — 倒计时器
- `.tip-bar` / `.tip-bar.primary` — 讲师提示条
- `.blob-primary` / `.blob-secondary` — 装饰背景 blob
- `.iframe-wrap` + `.iframe-label` — iframe 嵌入容器
- `.media-placeholder` — 媒体占位符
- `.step-list` + `.step-item` + `.step-num` — 编号步骤列表

### JS 功能（由 slide-nav.js 自动提供）
- 键盘 ← → / 空格 / PageUp/Down 翻页
- 触屏水平滑动翻页
- 热区点击翻页
- `initTimer(elementId, totalSeconds)` — 初始化倒计时器
- `.flip-card` 自动绑定点击翻转
- `.copy-btn` 自动绑定复制到剪贴板
- `.reveal` 元素自动触发入场动画

---

## 响应式断点

| 断点 | 调整 |
|---|---|
| `max-height: 800px` | 收紧间距和字号（笔记本适配） |
| `max-height: 650px` / `max-width: 1024px` | 进一步缩小、网格转单列 |
| `max-width: 768px` | 双列网格变单列 |
| `prefers-reduced-motion: reduce` | 禁用所有动画 |

---

## 常见陷阱

1. ❌ **忘记引用共享 CSS/JS** → 每页必须包含 3 个 CSS link + 1 个 JS script
2. ❌ **导航链接写错文件名** → Phase 6 检查
3. ❌ **渐变背景页忘记覆盖导航栏颜色** → 白色文字在暗背景上
4. ❌ **内容塞太多导致溢出** → 严格限制每页内容量，宁可拆页
5. ❌ **使用固定 px 尺寸** → 全部用 `clamp()` 或 CSS 变量
6. ❌ **在共享 CSS/JS 中添加页面专用代码** → 页面专用代码只写在页面内的 `<style>` 和 `<script>` 中
7. ❌ **跳过风格设计直接生成** → 没有确认配色就生成，后续返工成本极高
8. ❌ **设计稿和 HTML 不一致** → 生成时严格对照 Markdown 设计稿
9. ❌ **忘记底部导航栏** → Web PPT 每页都必须能前后翻页
10. ❌ **只做“漂亮背景”** → 真正的美观来自清晰层级、可读内容、稳定布局和明确交互

---

## 参考文件

- `references/style-presets.md` — 5 个预设风格方案的完整参数
- `references/design-spec-format.md` — Markdown 设计稿的标准格式规范
- `chat-animation-pattern.md` — 聊天气泡打字机动画的实现模式
- `click-to-reveal-pattern.md` — Click-to-Reveal 渐进揭示的实现模式
