# Click-to-Reveal 渐进揭示模式

用于教学场景中按讲师节奏逐步展示内容，避免信息一次性涌入。

## 适用场景

- 步骤说明（如 AI 工作原理的 4 步）
- 答案揭示（先遮盖标题关键词，点击后显示）
- 选项卡片（点击翻转显示答案）

---

## 模式 A: 逐步显示卡片

### HTML

```html
<div class="steps-col">
  <div class="step-card hidden-step" id="step-0">
    <div class="step-icon">📚</div>
    <div class="step-content">
      <div class="step-title">① 第一步标题</div>
      <div class="step-desc">详细说明文字</div>
    </div>
  </div>
  <div class="step-card hidden-step" id="step-1">...</div>
  <div class="step-card hidden-step" id="step-2">...</div>
  <div class="step-card hidden-step" id="step-3">...</div>
  <div class="click-hint" id="clickHint" onclick="revealNext()">
    👆 点击显示下一步
  </div>
</div>
```

### CSS

```css
.step-card.hidden-step {
  opacity: 0;
  transform: translateY(16px);
  pointer-events: none;
  max-height: 0;
  padding: 0; margin: 0;
  overflow: hidden;
}
.step-card.show-step {
  opacity: 1;
  transform: translateY(0);
  max-height: 200px;
  transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
.click-hint {
  text-align: center;
  font-size: var(--fs-small);
  color: var(--neutral-400);
  cursor: pointer;
  animation: pulse-hint 2s ease-in-out infinite;
}
@keyframes pulse-hint {
  0%, 100% { opacity: 0.5; }
  50% { opacity: 1; }
}
```

### JS

```javascript
let currentStep = 0;
const totalSteps = 4;

function revealNext() {
  if (currentStep < totalSteps) {
    const card = document.getElementById('step-' + currentStep);
    card.classList.remove('hidden-step');
    void card.offsetWidth; // force reflow
    card.classList.add('show-step');
    currentStep++;

    if (currentStep >= totalSteps) {
      document.getElementById('clickHint').textContent = '✅ 全部显示完毕';
      document.getElementById('clickHint').style.animation = 'none';
      document.getElementById('clickHint').style.opacity = '0.4';
    } else {
      document.getElementById('clickHint').textContent =
        '👆 点击显示第 ' + (currentStep + 1) + ' 步';
    }
  }
}

// 支持点击整个区域触发
document.querySelector('.steps-col').addEventListener('click', function(e) {
  if (!e.target.closest('.click-hint')) revealNext();
});
```

---

## 模式 B: 标题答案揭示

在标题中预留答案占位，初始透明，首次点击时淡入：

```html
<h2>AI 在做的事只有一件——
  <span class="accent" id="titleAnswer" style="opacity:0; transition:opacity 0.6s;">
    猜下一个字
  </span>
</h2>
```

在 `revealNext()` 的第一步揭示中加入：

```javascript
if (currentStep === 0) {
  document.getElementById('titleAnswer').style.opacity = '1';
}
```

---

## 模式 C: 翻转卡片

使用共享 CSS 中的 `.flip-card` 组件，`slide-nav.js` 自动绑定点击翻转：

```html
<div class="flip-card" style="width:200px; height:160px;">
  <div class="flip-card-inner">
    <div class="flip-front card card-orange">
      <div style="font-size:2rem;">🤔</div>
      <div>点击翻转查看答案</div>
      <div class="flip-hint">👆 点击翻转</div>
    </div>
    <div class="flip-back">
      <div style="font-size:1.5rem; font-weight:700;">答案内容</div>
    </div>
  </div>
</div>
```

无需额外 JS，`slide-nav.js` 已自动为所有 `.flip-card` 绑定点击事件。
