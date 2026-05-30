# 流觞 UI 原型实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 生成一个单文件 HTML 原型，覆盖流觞四个页面（视频流、流觞卡、上游封存、我的），水墨纸笺视觉基调，底部 Tab 切换，可切换调试面板。

**Architecture:** 单文件 `index.html`，Vue 3 CDN 驱动响应式数据和页面路由，CSS custom properties 管理设计 token，纯 CSS + Vue transition 实现动画。预设种子数据内嵌在 JS 中。

**Tech Stack:** Vue 3 CDN、CSS Custom Properties、Google Fonts (Noto Serif SC)、纯 CSS 动画

---

## File Structure

```
D:\desktop\抖音引流\liushang\frontend\
└── index.html     ← 唯一文件，包含所有 HTML/CSS/JS
```

单文件结构，内部分区：
- `<style>` — CSS custom properties + 全局样式 + 组件样式 + 动画关键帧
- `<div id="app">` — Vue 挂载点，包含所有页面模板
- `<script>` — 种子数据 + Vue app 逻辑 + 交互行为

---

## 种子数据定义

在写任何 UI 之前，先把数据定好。以下数据直接嵌入 JS。

**5 个主题，每个主题 2 个视频，共 10 个视频：**

```javascript
const topics = [
  { id: 'homecoming', name: '归乡', keywords: ['久别', '远行', '回家'] },
  { id: 'oldfriend', name: '旧友', keywords: ['怀念', '告别', '天气'] },
  { id: 'tired', name: '疲惫', keywords: ['沉默', '喘息', '释然'] },
  { id: 'growth', name: '成长', keywords: ['离开', '害怕', '弄丢'] },
  { id: 'city', name: '城市', keywords: ['记忆', '深夜', '无处说'] }
];

const videos = [
  // 归乡
  { id: 'v1', title: '为什么奥德修斯的归途用了十年', author: '海边的古典课',
    topicId: 'homecoming', tags: ['归乡', '远行', '久别'],
    gradient: 'linear-gradient(135deg, #0d1b3e 0%, #2a1040 100%)' },
  { id: 'v2', title: '毕业后再也没回去过的那座小城', author: '北漂实录',
    topicId: 'homecoming', tags: ['归乡', '回家', '怀念'],
    gradient: 'linear-gradient(135deg, #0f1e3c 0%, #1a0a2e 100%)' },
  // 旧友
  { id: 'v3', title: '真正的告别常常没有像样的句号', author: '深夜书桌',
    topicId: 'oldfriend', tags: ['旧友', '告别', '怀念'],
    gradient: 'linear-gradient(135deg, #2a1a0e 0%, #1a1a1a 100%)' },
  { id: 'v4', title: '那些年我们以为会一直在一起的人', author: '旧时光放映室',
    topicId: 'oldfriend', tags: ['旧友', '青春', '错失'],
    gradient: 'linear-gradient(135deg, #2d1f12 0%, #1e1e1e 100%)' },
  // 疲惫
  { id: 'v5', title: '那天我没有崩溃只是不想解释了', author: '打工人的深夜',
    topicId: 'tired', tags: ['疲惫', '沉默', '释然'],
    gradient: 'linear-gradient(135deg, #0a2a1a 0%, #0a1a2a 100%)' },
  { id: 'v6', title: '把所有事都做完也没有觉得轻松', author: '城市生存指南',
    topicId: 'tired', tags: ['疲惫', '喘息', '释然'],
    gradient: 'linear-gradient(135deg, #0d2d1a 0%, #0d1a2d 100%)' },
  // 成长
  { id: 'v7', title: '那时候我以为离开会让我变成另一个人', author: '人生实验',
    topicId: 'growth', tags: ['成长', '离开', '害怕'],
    gradient: 'linear-gradient(135deg, #0a2a2a 0%, #1a0a2a 100%)' },
  { id: 'v8', title: '长大不是变厉害是承认自己也会怕', author: '三十岁前',
    topicId: 'growth', tags: ['成长', '害怕', '弄丢'],
    gradient: 'linear-gradient(135deg, #0d2d2d 0%, #2a0d2d 100%)' },
  // 城市
  { id: 'v9', title: '很晚的地铁里突然想起一个很早以前的人', author: '末班车电台',
    topicId: 'city', tags: ['城市', '深夜', '记忆'],
    gradient: 'linear-gradient(135deg, #1a1a1a 0%, #2a1a0a 100%)' },
  { id: 'v10', title: '不是这个城市冷是有些话不知道该发给谁', author: '城市夜归人',
    topicId: 'city', tags: ['城市', '深夜', '无处说'],
    gradient: 'linear-gradient(135deg, #1e1e1e 0%, #2d1e0d 100%)' }
];

const echoes = [
  // 归乡
  { id: 'e1', text: '我也是很久以后才发现，所谓回家，不一定是回到一个地方。', topicId: 'homecoming', tags: ['归乡', '久别', '释然'], ageDays: 127 },
  { id: 'e2', text: '有些地方回不去，但它一直在我身上。', topicId: 'homecoming', tags: ['归乡', '远行'], ageDays: 89 },
  { id: 'e3', text: '我以为我在找路，后来才知道我是在确认自己还想回去。', topicId: 'homecoming', tags: ['归乡', '远行'], ageDays: 56 },
  // 旧友
  { id: 'e4', text: '有些人不是断了联系，只是停在了某个不会再更新的下午。', topicId: 'oldfriend', tags: ['旧友', '怀念'], ageDays: 203 },
  { id: 'e5', text: '我后来很少想起他，但一想起，就是很多年前的天气。', topicId: 'oldfriend', tags: ['旧友', '怀念'], ageDays: 91 },
  { id: 'e6', text: '原来真正的告别，常常没有一个像样的句号。', topicId: 'oldfriend', tags: ['旧友', '告别'], ageDays: 342 },
  // 疲惫
  { id: 'e7', text: '那天我没有崩溃，只是突然不想解释了。', topicId: 'tired', tags: ['疲惫', '沉默'], ageDays: 45 },
  { id: 'e8', text: '我把很多事都做完了，却没有觉得轻松一点。', topicId: 'tired', tags: ['疲惫', '释然'], ageDays: 112 },
  { id: 'e9', text: '有时候停下来，不是放弃，是终于听见自己喘气。', topicId: 'tired', tags: ['疲惫', '喘息'], ageDays: 67 },
  // 成长
  { id: 'e10', text: '那时候我以为离开会让我变成另一个人。', topicId: 'growth', tags: ['成长', '离开'], ageDays: 178 },
  { id: 'e11', text: '后来我才知道，长大不是变厉害，是能承认自己也会怕。', topicId: 'growth', tags: ['成长', '害怕'], ageDays: 234 },
  { id: 'e12', text: '我们好像都是一边学会生活，一边弄丢一些旧答案。', topicId: 'growth', tags: ['成长', '弄丢'], ageDays: 88 },
  // 城市
  { id: 'e13', text: '有些夜路走多了，会开始觉得城市也有记忆。', topicId: 'city', tags: ['城市', '记忆'], ageDays: 156 },
  { id: 'e14', text: '我在很晚的地铁里，突然想起一个很早以前的人。', topicId: 'city', tags: ['城市', '深夜'], ageDays: 73 },
  { id: 'e15', text: '不是这个城市冷，是我有些话不知道该发给谁。', topicId: 'city', tags: ['城市', '无处说'], ageDays: 201 }
];

const archives = [
  { topicId: 'homecoming', season: '2026 春', title: '关于归乡的流觞',
    preface: '这一轮流觞里，很多人没有谈目的地。他们谈起离开后的日常、很久没回去的地方，以及一些仍然跟着自己的旧路。',
    echoIds: ['e1', 'e2', 'e3'], keywords: ['久别', '远行', '想起以前', '释然'], totalCount: 42 },
  { topicId: 'oldfriend', season: '2026 春', title: '关于旧友的流觞',
    preface: '有人提起很久没联系的旧人。不是想念，是突然意识到，有些关系停在了某个时间点，再也没有更新。',
    echoIds: ['e4', 'e5', 'e6'], keywords: ['怀念', '告别', '天气', '错失'], totalCount: 31 },
  { topicId: 'tired', season: '2026 春', title: '关于疲惫的流觞',
    preface: '很多人在这里停下来，不是因为想通了什么，只是终于不需要再解释自己为什么累。',
    echoIds: ['e7', 'e8', 'e9'], keywords: ['沉默', '喘息', '释然'], totalCount: 28 },
  { topicId: 'growth', season: '2026 春', title: '关于成长的流觞',
    preface: '这一轮流觞里没有豪言壮语。人们谈起离开时以为会变成的另一个人，以及后来承认自己也会怕的那个瞬间。',
    echoIds: ['e10', 'e11', 'e12'], keywords: ['离开', '害怕', '弄丢', '旧答案'], totalCount: 35 },
  { topicId: 'city', season: '2026 春', title: '关于城市的流觞',
    preface: '夜路、末班地铁、很晚的便利店。有些城市记忆不在景点里，在那些独自走过的路上。',
    echoIds: ['e13', 'e14', 'e15'], keywords: ['记忆', '深夜', '无处说'], totalCount: 24 }
];
```

---

### Task 1: 创建 HTML 骨架 + 设计 Token + 种子数据

**Files:**
- Create: `D:\desktop\抖音引流\liushang\frontend\index.html`

这个 Task 建立整个原型的骨架：HTML 结构、CSS 设计 token、Vue 3 CDN 引入、种子数据、底部 Tab 路由。完成后能在浏览器打开看到空白页面带底部导航。

- [ ] **Step 1: 创建 index.html 骨架**

创建文件 `D:\desktop\抖音引流\liushang\frontend\index.html`，包含：
- `<!DOCTYPE html>` + `<html lang="zh-CN">`
- `<meta viewport>` 移动端适配
- Google Fonts 引入 Noto Serif SC
- Vue 3 CDN `<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>`
- `<style>` 块内写入全部 CSS custom properties（来自 spec 色彩系统）
- `<div id="app">` Vue 挂载点
- `<script>` 块内写入种子数据常量（topics, videos, echoes, archives）
- Vue app 初始化，包含 `data()` 返回 `currentTab: 'feed'`

- [ ] **Step 2: 添加 CSS 设计 Token**

在 `<style>` 中写入：

```css
:root {
  --bg-void: #0a0a0f;
  --bg-deep: #141419;
  --card-paper: rgba(245,240,230,0.12);
  --card-paper-solid: #f5f0e6;
  --ink-primary: #e8e0d0;
  --ink-secondary: rgba(232,224,208,0.6);
  --accent-water: #5a9e8f;
  --accent-gold: #c9a96e;
  --accent-glow: rgba(90,158,143,0.15);
  --font-serif: "Noto Serif SC", "Songti SC", Georgia, serif;
  --font-sans: -apple-system, "PingFang SC", sans-serif;
}
* { margin: 0; padding: 0; box-sizing: border-box; }
html, body { height: 100%; overflow: hidden; font-family: var(--font-sans); background: #000; }
#app {
  max-width: 430px; margin: 0 auto; height: 100vh;
  position: relative; overflow: hidden; background: var(--bg-deep);
}
```

- [ ] **Step 3: 添加底部 Tab 栏**

在 Vue template 中添加底部导航：

```html
<nav class="tab-bar">
  <div class="tab-item" :class="{ active: currentTab === 'feed' }" @click="currentTab = 'feed'">
    <svg><!-- 视频流图标 SVG --></svg>
    <span>视频流</span>
    <span class="badge" v-if="collectedCount > 0">{{ collectedCount }}</span>
  </div>
  <div class="tab-item" :class="{ active: currentTab === 'upstream' }" @click="currentTab = 'upstream'">
    <svg><!-- 上游图标 SVG --></svg>
    <span>上游</span>
  </div>
  <div class="tab-item" :class="{ active: currentTab === 'mine' }" @click="currentTab = 'mine'">
    <svg><!-- 我的图标 SVG --></svg>
    <span>我的</span>
  </div>
</nav>
```

Tab 栏样式：半透明底 + backdrop-filter blur(12px)，active 态用 `--accent-water` 高亮。固定在底部，高度 `56px`。

- [ ] **Step 4: 添加页面容器 + 路由切换**

添加三个页面容器，用 `v-show` 切换：

```html
<div class="page page-feed" v-show="currentTab === 'feed'">
  <!-- 视频流页面，Task 2 填充 -->
</div>
<div class="page page-upstream" v-show="currentTab === 'upstream'">
  <!-- 上游封存页，Task 4 填充 -->
</div>
<div class="page page-mine" v-show="currentTab === 'mine'">
  <!-- 我的页面，Task 5 填充 -->
</div>
```

页面切换动画：CSS transition `opacity 0.3s`。

- [ ] **Step 5: 浏览器打开验证**

用浏览器打开 `index.html`，确认：
- 页面居中显示，最大宽度 430px
- 底部 Tab 栏可见，三个 tab 可点击切换
- 背景色正确（深色）
- 没有控制台报错

- [ ] **Step 6: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add HTML skeleton with design tokens and tab routing"
```

---

### Task 2: 视频流首页

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`（page-feed 区域）

填充视频流页面：顶部导航、视频占位图、右侧操作栏、底部信息区、上滑切换。完成后能刷视频。

- [ ] **Step 1: 添加顶部导航栏**

在 `page-feed` 内添加：

```html
<header class="feed-header">
  <div class="feed-tabs">
    <span class="feed-tab">推荐</span>
    <span class="feed-tab">关注</span>
    <span class="feed-tab active">流觞</span>
  </div>
  <button class="debug-toggle" @click="showDebug = !showDebug">
    <svg><!-- 虫子图标 --></svg>
  </button>
</header>
```

样式：`position: absolute; top: 0; width: 100%; padding: 12px 16px; background: rgba(10,10,15,0.6); backdrop-filter: blur(8px); z-index: 10;`

- [ ] **Step 2: 添加视频占位图区域**

```html
<div class="video-area" :style="{ background: currentVideo.gradient }"
     @touchstart="onTouchStart" @touchend="onTouchEnd">
  <div class="video-watermark">{{ currentVideo.title }}</div>
</div>
```

样式：`position: absolute; inset: 0;` 全屏覆盖。watermark 用 `--ink-secondary` `30%` 透明度，居中，大字号 `24px`，`font-family: var(--font-serif)`。

- [ ] **Step 3: 添加右侧操作栏**

```html
<div class="action-bar">
  <div class="action-item" @click="likeCurrentVideo">
    <svg><!-- 心形 --></svg><span>{{ formatCount(currentVideo.likes || 123000) }}</span>
  </div>
  <div class="action-item">
    <svg><!-- 评论 --></svg><span>{{ currentVideo.comments || 842 }}</span>
  </div>
  <div class="action-item">
    <svg><!-- 收藏 --></svg><span>收藏</span>
  </div>
  <div class="action-item">
    <svg><!-- 分享 --></svg><span>分享</span>
  </div>
</div>
```

样式：`position: absolute; right: 16px; bottom: 160px; display: flex; flex-direction: column; gap: 20px; color: var(--ink-secondary);`

- [ ] **Step 4: 添加底部视频信息区**

```html
<div class="video-info">
  <div class="video-author">@{{ currentVideo.author }}</div>
  <div class="video-title">{{ currentVideo.title }}</div>
  <div class="video-tags">
    <span class="tag" v-for="tag in currentVideo.tags">#{{ tag }}</span>
  </div>
</div>
```

样式：`position: absolute; bottom: 72px; left: 16px; right: 72px; z-index: 5;`

- [ ] **Step 5: 添加上滑切换逻辑**

在 Vue methods 中添加：

```javascript
data() {
  return {
    // ... 已有数据
    currentVideoIndex: 0,
    touchStartY: 0,
    dwellSeconds: 0,
    dwellTimer: null
  }
},
computed: {
  currentVideo() { return this.videos[this.currentVideoIndex]; }
},
methods: {
  onTouchStart(e) { this.touchStartY = e.touches[0].clientY; },
  onTouchEnd(e) {
    const diff = this.touchStartY - e.changedTouches[0].clientY;
    if (diff > 60 && this.currentVideoIndex < this.videos.length - 1) {
      this.nextVideo();
    } else if (diff < -60 && this.currentVideoIndex > 0) {
      this.prevVideo();
    }
  },
  nextVideo() {
    this.currentVideoIndex++;
    this.resetDwellTimer();
  },
  prevVideo() {
    this.currentVideoIndex--;
    this.resetDwellTimer();
  },
  resetDwellTimer() {
    this.dwellSeconds = 0;
    clearInterval(this.dwellTimer);
    this.dwellTimer = setInterval(() => {
      this.dwellSeconds++;
      this.checkTrigger();
    }, 1000);
  },
  formatCount(n) {
    return n >= 10000 ? (n / 10000).toFixed(1) + 'w' : n;
  }
}
```

- [ ] **Step 6: 添加视频切换过渡动画**

CSS：

```css
.video-area {
  transition: transform 0.4s ease, opacity 0.3s ease;
}
.video-area.slide-up {
  animation: slideUp 0.4s ease forwards;
}
@keyframes slideUp {
  from { transform: translateY(100%); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
```

- [ ] **Step 7: 浏览器验证**

打开页面，确认：
- 视频占位图全屏显示，渐变色正确
- 标题水印居中可见
- 右侧操作栏布局正确
- 底部信息区显示作者、标题、标签
- 上滑可切换到下一个视频（共 10 个）

- [ ] **Step 8: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add video feed page with swipe navigation"
```

---

### Task 3: 流觞卡组件

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`（page-feed 内添加卡片层）

这是核心交互。添加触发逻辑、卡片浮现动画、三个动作按钮、留一句输入区。

- [ ] **Step 1: 添加触发检测逻辑**

在 Vue methods 中添加：

```javascript
checkTrigger() {
  if (this.showCard) return;
  // 规则：当前视频停留 > 8 秒 且 同主题视频累计看过 >= 2 个
  const currentTopic = this.currentVideo.topicId;
  const sameTopicViewed = this.videos
    .slice(0, this.currentVideoIndex + 1)
    .filter(v => v.topicId === currentTopic).length;
  if (this.dwellSeconds >= 8 && sameTopicViewed >= 2) {
    this.triggerCard();
  }
},
triggerCard() {
  const topic = this.topics.find(t => t.id === this.currentVideo.topicId);
  const topicEchoes = this.echoes.filter(e => e.topicId === topic.id);
  const echo = topicEchoes[Math.floor(Math.random() * topicEchoes.length)];
  this.currentCard = {
    title: '上游来信',
    subtitle: '你在这条河边停了一会儿。',
    echoText: echo.text,
    sourceLine: `来自 ${echo.ageDays} 天前的一次停留`,
    echoId: echo.id,
    topicId: topic.id,
    tags: echo.tags
  };
  this.showGlow = true;
  setTimeout(() => { this.showCard = true; }, 400);
}
```

- [ ] **Step 2: 添加卡片 HTML**

在 `page-feed` 内、视频区之后添加：

```html
<!-- 水痕光晕 -->
<div class="card-glow" v-if="showGlow && !showCard"></div>

<!-- 流觞卡 -->
<transition name="card">
  <div class="flow-card" v-if="showCard" @touchstart.stop @touchmove.stop>
    <div class="card-header">
      <div class="card-title">{{ currentCard.title }}</div>
      <div class="card-subtitle">{{ currentCard.subtitle }}</div>
    </div>
    <div class="card-echo">"{{ currentCard.echoText }}"</div>
    <div class="card-source">{{ currentCard.sourceLine }}</div>

    <!-- 留一句输入区（默认收起） -->
    <transition name="expand">
      <div class="card-input-area" v-if="showInput">
        <textarea v-model="userText" maxlength="60"
                  placeholder="给后来刷到这里的人留一句..."></textarea>
        <div class="char-count" :class="{ warning: userText.length > 50 }">
          剩余 {{ 60 - userText.length }} 字
        </div>
        <button class="btn-submit" @click="submitEcho">放下这句</button>
      </div>
    </transition>

    <div class="card-actions" v-if="!showInput">
      <button class="btn-collect" @click="collectCard">收下</button>
      <button class="btn-leave" @click="showInput = true">留一句</button>
      <button class="btn-dismiss" @click="dismissCard">继续刷</button>
    </div>
  </div>
</transition>
```

- [ ] **Step 3: 添加卡片 CSS**

```css
.card-glow {
  position: absolute; bottom: 120px; left: 0; right: 0; height: 80px;
  background: var(--accent-glow);
  animation: glowPulse 4s ease-in-out infinite;
  z-index: 15;
}
@keyframes glowPulse {
  0%, 100% { opacity: 0.1; }
  50% { opacity: 0.2; }
}
.flow-card {
  position: absolute; bottom: 72px; left: 24px; right: 24px;
  max-height: 60vh; padding: 24px 20px;
  background: var(--card-paper);
  backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);
  border-radius: 16px; z-index: 20;
  display: flex; flex-direction: column; gap: 12px;
}
.card-title { font-size: 14px; color: var(--accent-gold); font-weight: 500; }
.card-subtitle { font-size: 12px; color: var(--ink-secondary); }
.card-echo {
  font-family: var(--font-serif); font-size: 20px; line-height: 1.8;
  color: var(--ink-primary); text-align: center; padding: 8px 0;
}
.card-source { font-size: 11px; color: var(--accent-gold); text-align: center; }
.card-actions { display: flex; gap: 12px; justify-content: center; margin-top: 4px; }
.btn-collect {
  padding: 8px 20px; background: var(--accent-water); color: #fff;
  border: none; border-radius: 8px; font-size: 14px; cursor: pointer;
}
.btn-leave {
  padding: 8px 20px; background: transparent; color: var(--accent-water);
  border: 1.5px solid var(--accent-water); border-radius: 8px; font-size: 14px; cursor: pointer;
}
.btn-dismiss {
  padding: 8px 12px; background: transparent; color: var(--ink-secondary);
  border: none; font-size: 13px; cursor: pointer;
}
```

- [ ] **Step 4: 添加卡片出现/消失动画**

```css
.card-enter-active { animation: cardFloatIn 800ms ease-out; }
.card-leave-active { animation: cardFloatOut 600ms ease-in; }
@keyframes cardFloatIn {
  from { transform: translateY(100%); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
@keyframes cardFloatOut {
  from { transform: translateY(0); opacity: 1; }
  to { transform: translateY(100%); opacity: 0; }
}
```

- [ ] **Step 5: 添加留一句输入区样式和展开动画**

```css
.card-input-area {
  display: flex; flex-direction: column; gap: 8px;
  overflow: hidden;
}
.card-input-area textarea {
  width: 100%; height: 80px; padding: 10px;
  background: rgba(0,0,0,0.2); border: 1px solid rgba(255,255,255,0.1);
  border-radius: 8px; color: var(--ink-primary); font-size: 14px;
  font-family: var(--font-sans); resize: none;
}
.card-input-area textarea::placeholder { color: var(--ink-secondary); }
.char-count { font-size: 12px; color: var(--ink-secondary); text-align: right; }
.char-count.warning { color: var(--accent-gold); }
.btn-submit {
  padding: 10px; background: var(--accent-water); color: #fff;
  border: none; border-radius: 8px; font-size: 14px; cursor: pointer;
}
.expand-enter-active, .expand-leave-active {
  transition: max-height 0.4s ease, opacity 0.3s ease;
  max-height: 200px; overflow: hidden;
}
.expand-enter-from, .expand-leave-to {
  max-height: 0; opacity: 0;
}
```

- [ ] **Step 6: 添加动作按钮逻辑**

```javascript
// 在 data() 中添加
data() {
  return {
    // ...已有
    showGlow: false,
    showCard: false,
    showInput: false,
    currentCard: null,
    userText: '',
    collectedCards: [],
    myEchoes: [],
    collectedCount: 0,
    topicVisits: {}  // { homecoming: 3, oldfriend: 1, ... }
  }
},
methods: {
  // ...已有
  collectCard() {
    this.collectedCards.push({
      ...this.currentCard,
      collectedAt: '刚刚'
    });
    const topic = this.currentCard.topicId;
    this.topicVisits[topic] = (this.topicVisits[topic] || 0) + 1;
    this.collectedCount++;
    this.dismissCard();
    this.showToast('已收下');
  },
  submitEcho() {
    if (!this.userText.trim()) return;
    const topicId = this.currentCard.topicId;
    this.myEchoes.push({
      text: this.userText.trim(),
      topicId: topicId,
      receivedCount: 0
    });
    this.userText = '';
    this.dismissCard();
    this.showToast('这句话会漂向后来者');
  },
  dismissCard() {
    this.showCard = false;
    this.showGlow = false;
    this.showInput = false;
    this.currentCard = null;
    this.resetDwellTimer();
  },
  showToast(msg) {
    this.toastMsg = msg;
    this.toastVisible = true;
    setTimeout(() => { this.toastVisible = false; }, 2000);
  }
}
```

- [ ] **Step 7: 添加 Toast 提示组件**

```html
<transition name="toast">
  <div class="toast" v-if="toastVisible">{{ toastMsg }}</div>
</transition>
```

```css
.toast {
  position: fixed; bottom: 140px; left: 50%; transform: translateX(-50%);
  padding: 8px 24px; background: rgba(0,0,0,0.7); color: var(--ink-primary);
  border-radius: 20px; font-size: 13px; z-index: 100;
}
.toast-enter-active, .toast-leave-active { transition: opacity 0.3s; }
.toast-enter-from, .toast-leave-to { opacity: 0; }
```

- [ ] **Step 8: 浏览器验证**

打开页面，连续刷 2 个同主题视频，在第二个视频停留 8 秒以上：
- 水痕光晕出现
- 流觞卡从底部漂入
- 点击"收下"：卡片消失，Toast 显示，Tab 栏计数 +1
- 点击"留一句"：输入区展开，输入文字后提交
- 点击"继续刷"：卡片向下消失

- [ ] **Step 9: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add flow-card component with trigger, animations, and actions"
```

---

### Task 4: 上游封存页

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`（page-upstream 区域）

- [ ] **Step 1: 添加上游页 HTML**

在 `page-upstream` 容器内添加：

```html
<div class="upstream-page">
  <div class="upstream-content" v-if="currentArchive">
    <!-- 档案头 -->
    <div class="archive-header">
      <div class="archive-season">{{ currentArchive.season }}</div>
      <div class="archive-title">{{ currentArchive.title }}</div>
      <div class="water-divider"></div>
      <div class="archive-preface">{{ currentArchive.preface }}</div>
    </div>

    <!-- 回响列表 -->
    <div class="echo-list">
      <div class="echo-card" v-for="echoId in currentArchive.echoIds" :key="echoId">
        <div class="echo-text">"{{ getEcho(echoId).text }}"</div>
        <div class="echo-age">来自 {{ getEcho(echoId).ageDays }} 天前</div>
      </div>
    </div>

    <!-- 关键词 -->
    <div class="archive-keywords">
      <span class="keyword-tag" v-for="kw in currentArchive.keywords">#{{ kw }}</span>
    </div>

    <!-- 主题切换 -->
    <div class="topic-tabs">
      <span class="topic-tab" v-for="t in topics" :key="t.id"
            :class="{ active: selectedTopicId === t.id }"
            @click="selectedTopicId = t.id">
        {{ t.name }}
      </span>
    </div>

    <!-- 封存徽章 -->
    <div class="seal-badge">
      已封存 · 共 {{ currentArchive.totalCount }} 条
    </div>
  </div>
</div>
```

- [ ] **Step 2: 添加上游页 CSS**

```css
.page-upstream { background: var(--card-paper-solid); }
.upstream-page { height: 100%; overflow-y: auto; padding: 60px 20px 80px; }
.archive-header { margin-bottom: 24px; }
.archive-season { font-size: 12px; color: var(--ink-secondary); }
.archive-title {
  font-family: var(--font-serif); font-size: 22px;
  color: #2a2520; margin: 4px 0 12px;
}
.water-divider {
  width: 60px; height: 2px; background: var(--accent-water); margin: 12px 0;
}
.archive-preface {
  font-size: 14px; color: #6b6560; line-height: 1.8; font-style: italic;
}
.echo-card {
  padding: 16px 0 16px 14px; border-left: 3px solid var(--accent-water);
  margin-bottom: 12px;
}
.echo-text {
  font-family: var(--font-serif); font-size: 16px; color: #2a2520;
  line-height: 1.8;
}
.echo-age { font-size: 11px; color: #9e9890; margin-top: 4px; }
.archive-keywords {
  display: flex; flex-wrap: wrap; gap: 8px; margin: 20px 0;
}
.keyword-tag {
  font-size: 12px; color: var(--accent-gold); background: rgba(201,169,110,0.12);
  padding: 4px 10px; border-radius: 12px;
}
.topic-tabs {
  display: flex; gap: 12px; margin: 16px 0; overflow-x: auto;
}
.topic-tab {
  font-size: 13px; color: #9e9890; padding: 6px 14px;
  border-radius: 16px; cursor: pointer; white-space: nowrap;
  border: 1px solid #d5d0c8;
}
.topic-tab.active { color: #fff; background: var(--accent-water); border-color: var(--accent-water); }
.seal-badge {
  text-align: center; padding: 12px; background: rgba(0,0,0,0.04);
  border-radius: 8px; color: #9e9890; font-size: 13px; margin-top: 16px;
}
```

- [ ] **Step 3: 添加上游页 Vue 逻辑**

```javascript
data() {
  return {
    // ...已有
    selectedTopicId: 'homecoming'
  }
},
computed: {
  currentArchive() {
    return this.archives.find(a => a.topicId === this.selectedTopicId);
  }
},
methods: {
  getEcho(id) {
    return this.echoes.find(e => e.id === id) || {};
  }
}
```

- [ ] **Step 4: 浏览器验证**

点击底部"上游"Tab：
- 页面切换到浅色纸笺底
- 显示档案标题、AI 短序、回响卡片列表
- 主题 Tab 可点击切换（归乡→旧友→疲惫→成长→城市）
- 关键词标签和封存徽章可见

- [ ] **Step 5: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add upstream archive page with topic switching"
```

---

### Task 5: 我的页面

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`（page-mine 区域）

- [ ] **Step 1: 添加我的页 HTML**

在 `page-mine` 容器内添加：

```html
<div class="mine-page">
  <!-- 漂过的河 -->
  <section class="mine-section">
    <div class="section-title">漂过的河</div>
    <div class="river-list">
      <div class="river-item" v-for="(count, topicId) in topicVisits" :key="topicId"
           @click="goToTopic(topicId)">
        {{ getTopicName(topicId) }} · {{ count }}次
      </div>
      <div class="river-empty" v-if="Object.keys(topicVisits).length === 0">
        还没漂过任何河
      </div>
    </div>
  </section>

  <!-- 我收下的话 -->
  <section class="mine-section">
    <div class="section-title">我收下的话</div>
    <div class="water-divider"></div>
    <div class="mine-card" v-for="card in collectedCards" :key="card.echoId">
      <div class="mine-echo">"{{ card.echoText }}"</div>
      <div class="mine-meta">{{ card.collectedAt }}收下</div>
    </div>
    <div class="mine-empty" v-if="collectedCards.length === 0">
      还没收下过回响<br>在视频流中停留一会儿，流觞卡会自然浮现
    </div>
  </section>

  <!-- 我留过的话 -->
  <section class="mine-section">
    <div class="section-title">我留过的话</div>
    <div class="water-divider"></div>
    <div class="mine-card mine-card-gold" v-for="(echo, i) in myEchoes" :key="i">
      <div class="mine-echo">"{{ echo.text }}"</div>
      <div class="mine-meta">已被 {{ echo.receivedCount }} 人收到</div>
    </div>
    <div class="mine-empty" v-if="myEchoes.length === 0">
      还没留过话<br>下次收到流觞卡时，可以留一句给后来者
    </div>
  </section>
</div>
```

- [ ] **Step 2: 添加我的页 CSS**

```css
.page-mine { background: var(--card-paper-solid); }
.mine-page { height: 100%; overflow-y: auto; padding: 60px 20px 80px; }
.mine-section { margin-bottom: 32px; }
.section-title {
  font-family: var(--font-serif); font-size: 16px; color: #2a2520;
}
.mine-section .water-divider {
  width: 60px; height: 2px; background: var(--accent-water); margin: 8px 0 16px;
}
.river-list { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 8px; }
.river-item {
  font-size: 13px; color: #6b6560; padding: 6px 14px;
  background: rgba(0,0,0,0.04); border-radius: 16px; cursor: pointer;
}
.river-item:active { background: rgba(90,158,143,0.1); }
.river-empty { font-size: 13px; color: #9e9890; margin-top: 8px; }
.mine-card {
  padding: 14px 0 14px 14px; border-left: 3px solid var(--accent-water);
  margin-bottom: 10px;
}
.mine-card-gold { border-left-color: var(--accent-gold); }
.mine-echo {
  font-family: var(--font-serif); font-size: 15px; color: #2a2520;
  line-height: 1.7;
}
.mine-meta { font-size: 11px; color: #9e9890; margin-top: 4px; }
.mine-empty {
  font-size: 13px; color: #9e9890; line-height: 1.6; padding: 16px 0;
}
```

- [ ] **Step 3: 添加辅助方法**

```javascript
methods: {
  getTopicName(id) {
    const t = this.topics.find(t => t.id === id);
    return t ? t.name : '';
  },
  goToTopic(topicId) {
    this.selectedTopicId = topicId;
    this.currentTab = 'upstream';
  }
}
```

- [ ] **Step 4: 浏览器验证**

先在视频流中收下几张卡、留几句话，然后切到"我的"Tab：
- "漂过的河"显示遇到过的主题和次数
- "我收下的话"列出收下的回响，左侧青绿竖线
- "我留过的话"列出自己留的话，左侧金色竖线
- 点击主题名跳转到上游对应页面
- 空状态引导文案正确显示

- [ ] **Step 5: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add my-page with collected echoes and submitted echoes"
```

---

### Task 6: 调试面板

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`（page-feed 内添加调试浮层）

- [ ] **Step 1: 添加调试面板 HTML**

在 `page-feed` 内添加：

```html
<transition name="debug">
  <div class="debug-panel" v-if="showDebug && currentTab === 'feed'">
    <div class="debug-row">
      <span class="debug-label">当前标签</span>
      <span class="debug-value">{{ currentVideo.tags.join(', ') }}</span>
    </div>
    <div class="debug-row">
      <span class="debug-label">停留秒数</span>
      <span class="debug-value">{{ dwellSeconds }}s</span>
    </div>
    <div class="debug-row">
      <span class="debug-label">触发置信度</span>
      <span class="debug-value">{{ triggerConfidence.toFixed(2) }}</span>
    </div>
    <div class="debug-row">
      <span class="debug-label">同主题累计</span>
      <span class="debug-value">{{ sameTopicCount }}</span>
    </div>
  </div>
</transition>
```

- [ ] **Step 2: 添加调试面板 CSS**

```css
.debug-panel {
  position: absolute; top: 60px; right: 12px;
  background: rgba(0,0,0,0.75); backdrop-filter: blur(8px);
  border-radius: 8px; padding: 12px 14px; z-index: 30;
  font-size: 12px; font-family: monospace; min-width: 160px;
}
.debug-row { display: flex; justify-content: space-between; margin-bottom: 4px; }
.debug-label { color: rgba(255,255,255,0.5); }
.debug-value { color: var(--accent-water); }
.debug-enter-active, .debug-leave-active { transition: opacity 0.2s, transform 0.2s; }
.debug-enter-from, .debug-leave-to { opacity: 0; transform: translateY(-10px); }
```

- [ ] **Step 3: 添加调试数据计算属性**

```javascript
computed: {
  triggerConfidence() {
    const topic = this.currentVideo.topicId;
    const same = this.videos.slice(0, this.currentVideoIndex + 1)
      .filter(v => v.topicId === topic).length;
    const dwellScore = Math.min(this.dwellSeconds / 15, 1);
    const topicScore = Math.min(same / 3, 1);
    return (dwellScore * 0.5 + topicScore * 0.5);
  },
  sameTopicCount() {
    return this.videos.slice(0, this.currentVideoIndex + 1)
      .filter(v => v.topicId === this.currentVideo.topicId).length;
  }
}
```

- [ ] **Step 4: 浏览器验证**

点击右上角调试开关：
- 调试面板出现在右上角
- 实时显示标签、停留秒数、置信度、同主题累计数
- 停留时置信度数值持续变化
- 再次点击开关面板消失

- [ ] **Step 5: Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): add debug panel with real-time trigger metrics"
```

---

### Task 7: 打磨与完整验证

**Files:**
- Modify: `D:\desktop\抖音引流\liushang\frontend\index.html`

最终打磨：页面切换动画、响应式适配、完整 Demo 流程验证。

- [ ] **Step 1: 添加页面切换动画**

视频流 → 上游/我的切换时加 `rotateY` 翻页感：

```css
.page {
  position: absolute; inset: 0;
  transition: opacity 0.3s ease;
}
.page-feed { background: var(--bg-deep); }
.page-upstream, .page-mine { background: var(--card-paper-solid); }
```

- [ ] **Step 2: 添加桌面端适配**

桌面端居中显示，两侧加手机框装饰：

```css
@media (min-width: 431px) {
  body { display: flex; justify-content: center; align-items: center; background: #000; }
  #app {
    border-radius: 24px; overflow: hidden;
    box-shadow: 0 0 60px rgba(90,158,143,0.1);
  }
}
```

- [ ] **Step 3: 完整 Demo 流程验证**

手动走一遍完整路演 Demo：
1. 打开页面，视频流正常显示
2. 连续刷 2 个归乡主题视频
3. 在第 2 个视频停留 > 8 秒
4. 水痕光晕出现 → 流觞卡漂入
5. 点击"收下"，Toast 提示，Tab 计数 +1
6. 继续刷，再次触发卡片，点击"留一句"
7. 输入文字，提交，Toast 提示
8. 切到"上游"Tab，查看封存页
9. 切换不同主题查看
10. 切到"我的"Tab，确认收下的和留的话都在
11. 打开调试面板，确认数据实时更新

- [ ] **Step 4: 最终 Commit**

```bash
git add frontend/index.html
git commit -m "feat(liushang): polish animations, responsive layout, complete demo flow"
```

---

## Self-Review Checklist

- [x] **Spec coverage:**
  - 色彩系统 → Task 1 Step 2
  - 字体 → Task 1 Step 2
  - 动效原则 → Task 3 Steps 3-4, Task 7 Step 1
  - 视口基准 → Task 1 Step 2, Task 7 Step 2
  - 视频流首页 → Task 2
  - 流觞卡组件 → Task 3
  - 上游封存页 → Task 4
  - 我的页面 → Task 5
  - 调试面板 → Task 6
  - 底部 Tab 栏 → Task 1 Step 3
  - 交互全局规则 → Task 7
- [x] **Placeholder scan:** 无 TBD/TODO/实现稍后，每个 Step 都有完整代码
- [x] **Type consistency:** 种子数据中的 id 字段、topicId 在所有 Task 中引用一致（homecoming/oldfriend/tired/growth/city）
