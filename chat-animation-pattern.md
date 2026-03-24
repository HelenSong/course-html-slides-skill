# 聊天气泡打字机动画模式

这是课件中最复杂的交互模式，用于「任务指南」(task-guide) 类型页面。通过模拟 AI 对话过程，逐步展示提示词的构建逻辑。

## 适用场景

- 向家长/学生演示「如何写提示词」
- 分步骤展示人机对话的过程
- 最终展示完整提示词并提供一键复制

## 架构

右侧区域包含**两个叠放的层**：

1. **聊天动画层 (`chat-stage`)** — 默认可见，播放对话动画
2. **提示词面板 (`prompt-panel`)** — 默认隐藏，动画结束后淡入

```
┌─────────────────────────────────────┐
│       chat-stage (z-index:2)        │  ← 初始可见
│  ┌─────────────────────────────────┐│
│  │ chat-header (活动指示点)         ││
│  │ chat-body (气泡区域)            ││
│  │   🤖 AI 气泡 (bubble-robot)    ││
│  │   👨‍👩‍👧 家长气泡 (bubble-parent)  ││
│  │   [继续按钮]                    ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│     prompt-panel (z-index:1→3)      │  ← 动画完成后切换到前面
│  ┌─────────────────────────────────┐│
│  │ pp-title + 复制/重播按钮        ││
│  │ prompt-block (完整提示词)        ││
│  │ prompt-hint (使用说明)           ││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

## 动画数据结构

```javascript
const ACTS = [
  {
    robot: 'AI 的台词（自动打字显示）',
    parent: '家长/用户的台词（打字显示）= 提示词的一部分',
    label: '🎭 角色'  // 本幕主题标签
  },
  // ... 通常 4 幕：角色→任务→要求→格式
];
```

## 核心 JS 函数

```javascript
// 创建气泡
function createBubble(type, nameText) { ... }  // type='robot'|'parent'

// 显示气泡（带动画）
function showBubble(bubbleEl) { ... }

// 逐字打字
async function typeInBubble(bodyEl, text) { ... }

// 打字指示器（三个跳动的点）
function showTypingDots() { ... }

// 等待用户点击「继续」
function waitForClick(labelText) { ... }

// 播放单幕
async function runAct(actIndex) { ... }

// 播放全部 + 切换到提示词面板
async function runAnimation() { ... }

// 重播
function replayAll() { ... }
```

## CSS 要点

```css
/* 气泡基础 */
.bubble {
  max-width: 88%;
  padding: 12px 16px;
  border-radius: var(--r-lg);
  opacity: 0; transform: translateY(12px);
  transition: opacity 0.4s, transform 0.4s;
}
.bubble.show { opacity: 1; transform: translateY(0); }

/* AI 气泡 vs 用户气泡 */
.bubble-robot  { align-self: flex-start; background: var(--blue-50); }
.bubble-parent { align-self: flex-end; background: var(--orange-50); }

/* 打字光标 */
.typing-cursor {
  display: inline-block; width: 2px; height: 1em;
  background: var(--primary);
  animation: blink-c 0.6s step-end infinite;
}

/* 打字指示器 */
.typing-indicator span {
  width: 8px; height: 8px; border-radius: 50%;
  animation: dot-bounce 1.2s ease-in-out infinite;
}

/* 继续按钮 */
.continue-btn {
  padding: 10px 28px; border-radius: var(--r-full);
  background: var(--primary); color: #fff;
  animation: fade-up 0.4s 0.2s forwards;
}

/* 层切换 */
.chat-stage.hidden { opacity: 0; pointer-events: none; transform: scale(0.96); }
.prompt-panel.visible { opacity: 1; transform: scale(1); z-index: 3; }
```

## 活动指示点

聊天头部显示 N 个圆点，表示当前进度：
- `.act-dot` — 默认灰色
- `.act-dot.active` — 当前播放中，橙色带脉冲
- `.act-dot.done` — 已完成，绿色
