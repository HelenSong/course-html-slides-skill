---
name: 课程 HTML Slides 构建器
description: 批量生成多页独立 HTML 课件 Slides，包含完整的三阶段工作流：配色风格设计 → Markdown 设计稿 → HTML 文件生成。支持封面、幕间标题、内容页、互动演示、任务指南、实战练习等页面类型。每个 slide 是一个独立 HTML 文件，通过共享 CSS/JS 和底部导航栏链接成完整课件流。适用于教学场景的课堂投影演示。当用户提到"课件"、"slides"、"HTML 演示"、"课堂 PPT"、"教学课件"或需要将课程大纲转为可演示的网页时，使用此 skill。
---

# 课程 HTML Slides 构建器

将课程大纲转化为**多页独立 HTML 文件**形式的课件 Slides。三阶段完整流程：

1. **风格设计** — 确定配色、字体、圆角等视觉系统
2. **Markdown 设计稿** — 将大纲转为逐页结构化设计文档
3. **HTML 生成** — 按设计稿批量生成独立 HTML 文件

每一页都是可独立打开的 HTML 网页，通过底部导航栏串联为完整的演示流。

---

## 核心原则

1. **每页一个独立 HTML 文件** — 命名规则：`pXX-slug.html`（如 `p01-cover.html`、`p05-part1-title.html`）
2. **共享设计系统** — 所有页面引用同一套 CSS（`styles/theme.css`、`styles/animations.css`、`styles/components.css`）和 JS（`js/slide-nav.js`）
3. **满屏不滚动** — 每个 slide 适配 `100vh`，内容超出时拆分为多页，绝不允许页内纵向滚动
4. **教学场景优先** — 为投影仪/大屏设计，字大、对比度高、操作区域大，支持键盘和触屏翻页
5. **渐进揭示** — 利用 `.reveal` 动画类和 click-to-reveal 交互，支持按教学节奏逐步展示内容

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
| `content` | 📖 | 知识讲解、概念介绍 |
| `interactive-demo` | 🤖 | 嵌入演示、工具体验 |
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

    <!-- 底部导航栏 -->
    <nav class="slide-nav">
      <a id="btn-prev" class="nav-btn nav-btn-secondary" href="上一页.html">
        ← 上一页
      </a>
      <div style="display:flex; align-items:center; gap:var(--gap-sm);">
        <span class="badge badge-primary">当前部分</span>
        <span class="nav-page-info">PXX / 总数</span>
        <span class="nav-kbd-hint">← → 键 或 滑动</span>
      </div>
      <a id="btn-next" class="nav-btn nav-btn-primary" href="下一页.html">
        下一页 →
      </a>
    </nav>

  </div>
  <script src="js/slide-nav.js"></script>
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
| 暗色封面/结尾 | `bg-dark` |

在渐变/暗色背景上，导航栏需覆盖半透明暗色样式：
```html
<nav class="slide-nav" style="background:rgba(30,26,24,0.5); border-top-color:rgba(255,255,255,0.15);">
```

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

结构：全屏背景图 + 居中内容（眉标 + 大标题 + 副标题 + 标签组）

```html
<div class="slide-page" style="background:#1a1a2e;">
  <div class="cover-bg"></div> <!-- 背景图层 -->
  <div class="cover-content">  <!-- 居中内容 -->
    <div class="cover-eyebrow reveal delay-1">✨ 眉标文字</div>
    <h1 class="cover-title reveal delay-2">
      主标题<br><span class="highlight">渐变色强调词</span>
    </h1>
    <p class="cover-subtitle reveal delay-3">副标题说明</p>
    <div class="cover-tags reveal delay-4">
      <span class="cover-tag tag-primary">标签1</span>
      <span class="cover-tag tag-secondary">标签2</span>
    </div>
  </div>
</div>
```

关键样式：
- `.cover-bg`：`position:absolute; inset:0; background:url(...) center/cover; filter:brightness(0.55);` + `::after` 渐变蒙版
- `.highlight`：`background:linear-gradient(...); -webkit-background-clip:text; -webkit-text-fill-color:transparent;`
- 添加装饰粒子：`.particle` + `float-xy` 动画

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

- [ ] 每页在 1280×800 分辨率下无纵向滚动
- [ ] 所有 `reveal` 元素正常入场
- [ ] 前后翻页导航链接正确
- [ ] 进度条数值准确
- [ ] 暗色/渐变页的导航栏文字可见
- [ ] 键盘左右键翻页正常
- [ ] 复制按钮功能正常（如有）
- [ ] 倒计时器运行正常（如有）
- [ ] 响应式：在不同屏幕尺寸下布局不崩坏
- [ ] 配色与 style-guide.md 一致

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

---

## 参考文件

- `references/style-presets.md` — 5 个预设风格方案的完整参数
- `references/design-spec-format.md` — Markdown 设计稿的标准格式规范
- `chat-animation-pattern.md` — 聊天气泡打字机动画的实现模式
- `click-to-reveal-pattern.md` — Click-to-Reveal 渐进揭示的实现模式
