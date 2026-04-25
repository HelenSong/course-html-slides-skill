# Course HTML Slides Builder

> **An AI Skill that turns course outlines into multi-page HTML slide decks**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Skill Type](https://img.shields.io/badge/Type-AI%20Skill-blue.svg)]()
[![Language](https://img.shields.io/badge/lang-English%20%7C%20中文-brightgreen)](README.zh.md)

**🌐 Language / 语言：** [English](README.md) · [中文](README.zh.md)

---

## 🌟 What is this?

An AI Skill for **Claude / Antigravity** that transforms course outlines into professional HTML slides / Web PPT decks — no design skills needed.

**Four-step workflow:**

```
Course Outline → Style Design → Markdown Spec → HTML Generation → Browser Screenshot Verification
```

Each slide is an independent HTML file linked by a shared bottom navigation bar, side hotzones, keyboard controls, and touch gestures. Built for classroom projection and interactive workshops.

---

## ✨ Features

- 🎨 **5 preset style themes** — Warm Education, Cool Tech, Fresh Nature, Dark Geek, Minimal Elegant
- 📝 **10+ page types** — Cover, Section Title, Level Transition, Content, Interactive Demo, Vote/Poll, Task Guide, Practice, Summary, QR Code
- 🤖 **Built-in interactions** — Chat bubble typewriter, click-to-reveal, flip cards, countdown timer
- 🧭 **Required bottom navigation** — Prev/next buttons, page number, section badge, and top progress bar on every slide
- ⌨️ **Keyboard / touch navigation** — Arrow keys, spacebar, swipe gestures, side hotzones
- 📱 **Responsive design** — Adapts to projectors and different screen sizes
- ✅ **Screenshot verification standard** — Checks overflow, nav overlap, icon rendering, and media loading

---

## 📁 Skill Structure

```
course-html-slides/
├── SKILL.md                     # Main instruction file (read by AI)
├── chat-animation-pattern.md    # Chat animation reference
├── click-to-reveal-pattern.md   # Progressive disclosure reference
└── references/
    ├── style-presets.md         # 5 complete style presets
    └── design-spec-format.md    # Markdown design spec format
```

---

## 🚀 How to Use

### Prerequisites

[Antigravity](https://antigravity.ai) or any Claude client that supports the Skill system.

### Installation

**Global install (recommended):**
```bash
cd ~/.agents/skills
git clone https://github.com/HelenSong/course-html-slides-skill.git course-html-slides
```

**Project-level install:**
```bash
cd your-project/.agents/skills
git clone https://github.com/HelenSong/course-html-slides-skill.git course-html-slides
```

### How to trigger

After installing, just say:

> "Help me turn this course outline into HTML slides"

> "I want slides for a Python intro course with a tech vibe"

> "Generate HTML files from this Markdown design spec"

---

## 📖 Workflow

### Phase 1 — Style Design

The Skill asks about your style preferences or lets you pick a preset:

| Preset | Best For | Primary Color |
|---|---|---|
| 🍊 Warm Education | K12, parent-child education | Orange + Blue |
| 🧊 Cool Tech | STEM, coding, corporate training | Indigo + Cyan |
| 🌿 Fresh Nature | Biology, crafts, outdoor education | Forest Green + Amber |
| 🌙 Dark Geek | Tech talks, hackathons | Dark + Neon |
| 🎨 Minimal Elegant | Humanities, general education | Black-White + Accent |

Output: `style-guide.md`

### Phase 2 — Markdown Design Spec

The Skill converts your outline into a structured per-page design document covering: page type, content, layout, and interaction.

Output: `design-spec.md`

### Phase 3 — HTML Generation

Batch-generates HTML files alongside a shared CSS/JS design system:

```
slides/
├── styles/
│   ├── theme.css       # Theme (colors, typography, border-radius)
│   ├── animations.css  # Animation system
│   └── components.css  # Reusable components
├── js/
│   └── slide-nav.js    # Navigation controller
├── p01-cover.html      # each slide includes bottom navigation
├── p02-*.html
└── ...
```

### Phase 4 — Browser Verification

After generation, check key slide screenshots to ensure:

- Bottom navigation and top progress bar are present
- Main content does not overlap the nav bar
- Images, videos, and lucide icons render correctly
- Polls, click-to-reveal, timers, and other interactions have visible feedback
- Content is clear at 1280×720 / 1280×800 projection sizes

---

## 🎮 Example: Pet Diary Course

> **"Human Observation Diary from a Pet's Perspective"** — PBL course, 12 slides, Fresh Nature style

![Slide Collage](docs/collage.png)

| Page | Type | Highlights |
|---|---|---|
| 01 | Cover | Dark bg + floating paw + gradient title |
| 02 | Hook | Three-column pet diary cards |
| 03 | Driving Question | Full-screen green gradient + keyword tags |
| 04 | Project Intro | PBL 5-step + outcome preview (two-column) |
| 05 | Role Selection | Clickable pet character cards |
| 06 | AI Writing Guide | Click-to-Reveal steps + one-click copy prompt |
| 07 | Practice | 15-min timer + phase tracker + checklist |
| 08 | Reflection | Discussion questions + skill wall |

---

## 🛠 Technical Specs

- **Pure HTML/CSS/JS** — No framework dependencies, works offline
- **CSS variable-driven** — Switch themes by editing variables in `theme.css`
- **All sizes via `clamp()`** — Responsive across different resolutions
- **Required navigation** — Bottom navigation + top progress bar + side hotzones on every slide
- **No scroll per slide** — Desktop projection content stays within `100vh`
- **Visual quality baseline** — Every slide needs a clear visual anchor, readable hierarchy, and interaction feedback

---

## 📄 License

MIT License — Free to use, modify, and share

---

## 🙏 Acknowledgements

- Created by [HelenSong](https://github.com/HelenSong)
- Built with [Antigravity](https://antigravity.ai) Skill system
- Methodology: [skill-creator](https://github.com/anthropics/skills)
