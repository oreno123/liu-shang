# 流觞

> 刷视频时，AI 把你此刻的情绪，和某个陌生人过去留下的一句话，对上了。

<p align="center">
  <img src="preview.png" alt="流觞卡预览" width="360">
</p>

## 这是什么

「流觞」是一个 AI 短视频卡片体验。当用户在某类内容上停留时，AI 会把视频内容、用户此刻的停留状态和陌生人过去留下的回响句匹配起来，生成一张自然浮现的流觞卡。

它不是评论区，也不是社交群。它更像一只从上游漂来的杯盏：有人曾经留下了一句话，而你刚好刷到了。

## 快速体验

```bash
cd frontend
python -m http.server 8080
# 浏览器打开 http://localhost:8080
```

1. 连续刷 2 个同主题视频（如归乡）
2. 在第 2 个视频停留 8 秒以上
3. 流觞卡从底部漂入
4. 点击「收下」或「留一句」

## 核心体验

- **视频流** — 类抖音竖屏信息流，10 个预设视频覆盖 5 个情绪主题
- **流觞卡** — 半透明纸笺从底部漂入，展示一句陌生人的回响
- **上游页** — 主题封存档案，查看同一主题下的所有回响
- **我的页** — 收下的话、留过的话、漂过的河
- **调试面板** — 实时展示 AI 标签、停留秒数、触发置信度

## 项目结构

```
liushang/
├── frontend/
│   └── index.html          # 单文件原型（Vue 3 CDN）
├── docs/
│   ├── 01-project-proposal.md      # 项目方案
│   ├── 02-ai-skill-prompts.md      # AI 提示词 Skill 设计
│   ├── 03-product-packaging.md     # 产品包装与路演叙事
│   └── superpowers/
│       ├── specs/                  # UI 设计 Spec
│       └── plans/                  # 实现计划
├── openspec/               # OpenSpec 变更管理
└── preview.png             # 效果预览
```

## 设计体系

水墨纸笺视觉基调：

- 深色视频背景 + 半透明毛玻璃纸笺卡片
- 青绿水痕（`#5a9e8f`）+ 暖金点缀（`#d4b478`）
- 回响句用衬线体（Noto Serif SC），UI 文字用无衬线
- 卡片从底部漂入，三阶段浮现（光晕→卡片→回响句揭幕）

## 技术栈

- Vue 3 CDN + CSS Custom Properties
- 单文件 HTML 原型，后续转 React + Vite 工程

## License

MIT
