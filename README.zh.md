# 课程 HTML Slides 构建器

> **一个 AI Skill，将课程大纲批量转化为多页独立 HTML 课件**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Skill Type](https://img.shields.io/badge/Type-AI%20Skill-blue.svg)]()
[![Language](https://img.shields.io/badge/lang-English%20%7C%20中文-brightgreen)](README.md)

**🌐 Language / 语言：** [English](README.md) · [中文](README.zh.md)

---

## 🌟 这是什么？

这是一个为 **Claude / Antigravity** 设计的 AI Skill，帮助你把课程大纲快速转化为专业级的 HTML 课件 Slides。

**三阶段完整流程：**

```
课程大纲  →  [Phase 1] 风格设计  →  [Phase 2] Markdown 设计稿  →  [Phase 3] HTML 生成
```

每一页都是独立的 HTML 文件，通过底部导航栏串联，适合课堂投影演示。

---

## ✨ 主要特性

- 🎨 **5 种预设风格** — 暖色教育风、科技冷色风、清新自然风、暗黑极客风、极简雅致风
- 📝 **8 种页面类型** — 封面、幕间标题、内容页、互动演示、任务指南、实战练习、总结、二维码
- 🤖 **交互动画内置** — 聊天气泡动画、click-to-reveal、翻转卡片、倒计时器
- ⌨️ **键盘/触屏翻页** — 左右键、空格键、滑动手势
- 📱 **响应式设计** — 适配不同分辨率的投影仪和屏幕

---

## 📁 Skill 结构

```
course-html-slides/
├── SKILL.md                     # 主指令文件（AI 读取）
├── chat-animation-pattern.md    # 聊天动画实现参考
├── click-to-reveal-pattern.md   # 渐进揭示实现参考
└── references/
    ├── style-presets.md         # 5 个风格预设方案
    └── design-spec-format.md    # Markdown 设计稿格式规范
```

---

## 🚀 如何使用

### 前提条件

需要使用 [Antigravity](https://antigravity.ai) 或支持 Skill 系统的 Claude 客户端。

### 安装方法

**全局安装（推荐）：**
```bash
# 克隆到全局 skills 目录
cd ~/.agents/skills
git clone https://github.com/HelenSong/course-html-slides-skill.git course-html-slides
```

**项目级安装：**
```bash
# 克隆到项目的 .agents/skills 目录
cd your-project/.agents/skills
git clone https://github.com/HelenSong/course-html-slides-skill.git course-html-slides
```

### 调用方式

安装后，在对话中说：

> "帮我把这个课程大纲做成 HTML 课件"

> "我要给'Python 入门课'做 slides，风格要科技感"

> "把这个 Markdown 设计稿生成 HTML 文件"

---

## 📖 工作流程

### Phase 1 — 风格设计

Skill 会询问你的风格偏好，或者快速选择预设方案：

| 方案 | 适合场景 | 主色 |
|---|---|---|
| 🍊 暖色教育风 | K12、亲子教育 | 橙 + 蓝 |
| 🧊 科技冷色风 | STEM、编程、企培 | 靛蓝 + 青 |
| 🌿 清新自然风 | 生物、手工、户外 | 森林绿 + 琥珀 |
| 🌙 暗黑极客风 | 技术分享、黑客松 | 深色 + 霓虹 |
| 🎨 极简雅致风 | 人文、通识、高端企培 | 黑白 + 单色 |

输出：`style-guide.md`

### Phase 2 — Markdown 设计稿

Skill 将课程大纲转化为逐页的结构化设计文档，每页包含：类型、内容、布局、交互描述。

输出：`design-spec.md`

### Phase 3 — HTML 生成

按设计稿批量生成 HTML 文件，同时生成共享的 CSS/JS 设计系统：

```
slides/
├── styles/
│   ├── theme.css       # 主题（含品牌色、字号、圆角）
│   ├── animations.css  # 动画系统
│   └── components.css  # 可复用组件
├── js/
│   └── slide-nav.js    # 导航控制器
├── p01-cover.html
├── p02-*.html
└── ...
```

---

## 🎮 示例：宠物日记课件

> **《宠物视角的人类观察日记》** — PBL 项目课，12 页，清新自然风

![课件预览拼图](docs/collage.png)

| 页码 | 类型 | 特色 |
|---|---|---|
| 01 | 封面 | 暗色底 + 浮动爪印 + 渐变大标题 |
| 02 | 钩子 | 三栏宠物日记卡片 |
| 03 | 驱动问题 | 全屏绿渐变 + 关键词标签 |
| 04 | 项目介绍 | PBL 五步 + 成品预览双栏 |
| 05 | 角色选择 | 可点击宠物性格卡片 |
| 06 | AI 润色指南 | Click-to-Reveal 步骤 + 一键复制提示词 |
| 07 | 实战练习 | 15 分钟计时器 + 阶段追踪 + 勾选清单 |
| 08 | 总结反思 | 反思问题 + 技能收获墙 |

---

## 🛠 技术规格

- **纯 HTML/CSS/JS** — 无框架依赖，可离线运行
- **CSS 变量驱动** — 主题可通过修改 `theme.css` 中的变量一键切换
- **所有尺寸用 `clamp()`** — 自适应不同分辨率
- **满屏不滚动** — 严格限制每页内容量在 `100vh` 内

---

## 📄 License

MIT License — 自由使用、修改、分享

---

## 🙏 致谢

- 由 [HelenSong](https://github.com/HelenSong) 创建
- Skill 系统基于 [Antigravity](https://antigravity.ai)
- 使用 [skill-creator](https://github.com/anthropics/skills) 方法论构建
