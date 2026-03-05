# SkillCap Pitch Deck — 项目文档

> 最后更新: 2026-03-05 | 当前版本: v7 | 部署: GitHub Pages → skillcap.ai

## 1. 项目概览

单页 HTML 应用，15 张 slide 的 pitch deck，支持中英文切换，部署在 skillcap.ai。

**技术栈**: React 18 + Tailwind CSS（均通过 CDN 加载） + Babel Standalone（浏览器端 JSX 编译）

**部署方式**: GitHub Pages（仓库 lindaminusai/skillcap-pitch，main 分支，index.html）

## 2. 文件结构

```
skillcap.ai/
├── skillcap-pitch/          ← 部署 git 仓库
│   ├── .git/
│   ├── index.html           ← 生产文件（当前 = v6）
│   └── PROJECT.md           ← 本文件
├── skillcap-pitch.jsx       ← 最早的 JSX 源文件（已弃用）
├── skillcap-pitch.html      ← v5 的副本
├── skillcap-pitch v5.html   ← v5
├── skillcap-pitch v6.html   ← v6（添加中英文切换）
└── skillcap-pitch v7.html   ← v7（手机响应式适配）
```

## 3. 架构说明

### 3.1 组件树

```
SkillCapPitch (主组件，管理 state)
├── GridOverlay          — 全屏 SVG 网格背景（fixed, opacity 5%）
├── LanguageToggle       — 中英文切换按钮（fixed top-right, z-50）
├── SLIDES[15]           — 15 张 slide（absolute inset-0, 通过 opacity 切换可见性）
│   └── SlideContainer   — 每张 slide 的可见性动画容器
│       └── AnimatedElement — 元素进场动画（staggered）
├── SlideNav             — 底部导航圆点条（fixed bottom, z-30）
├── Next/Back Buttons    — 前进/后退按钮（absolute bottom, z-20）
└── Click Overlay        — 全屏点击区域（z-0, 点击切换下一张）
```

### 3.2 Slide 列表

| # | id | 标签 | 内容 |
|---|---|---|---|
| 0 | title | Title | SkillCap 主标题 + tagline |
| 1 | axioms | Axioms | 三条第一性原理 |
| 2 | worldview | Worldview | 世界观和设计原则 |
| 3 | problem | Problem | 当前 AI 经济的问题 |
| 4 | solution | Solution | SkillCap 的解决方案 |
| 5 | ecosystem | Ecosystem | 生态架构图 |
| 6 | six-pools | Six Pools | 六个价值池（所有权结构） |
| 7 | market | Market | 双阶段市场结构 |
| 8 | skill-pnl | P&L | Skill 的损益模型 |
| 9 | fission | Fission | Fork 机制（活资产裂变） |
| 10 | trust | Trust | 信任与安全机制 |
| 11 | agents | Agents | AI Agent 集成 |
| 12 | tech | Tech | 技术架构（三层） |
| 13 | flywheel | Flywheel | 飞轮效应 |
| 14 | closer | Closer | 结尾 CTA |

### 3.3 导航系统

- **键盘**: 右箭头/空格 = 下一张，左箭头 = 上一张
- **鼠标**: 点击主区域 = 下一张，底部导航圆点 = 跳转
- **触摸**: 左右滑动切换（v7 新增，阈值 50px）
- **按钮**: 右下角 Next / 左下角 Back

### 3.4 Z-Index 层级

| z-index | 元素 |
|---|---|
| 50 | LanguageToggle |
| 30 | SlideNav |
| 20 | Next/Back 按钮 |
| 10 | 当前 slide |
| 0 | 未来 slide / Click Overlay |
| -10 | 过去 slide |

### 3.5 关键 Tailwind 断点

v7 使用 `md:` (768px) 作为主要断点：
- `< 768px` = 手机布局（小字体、单列网格、紧凑间距）
- `≥ 768px` = 桌面布局（原始大字体、多列网格、宽松间距）

## 4. 版本历史

### v7 (2026-03-05) — 手机响应式适配
**修改内容**:
- 12 处 slide 容器: `px-12 py-16` → `px-4 py-8 md:px-12 md:py-16`
- 字体缩放: `text-8xl` → `text-4xl md:text-8xl`; `text-5xl` → `text-2xl md:text-5xl`
- GradientText 默认: `text-4xl` → `text-xl md:text-4xl`
- 网格: `grid-cols-3` → `grid-cols-1 md:grid-cols-3`; `grid-cols-4` → `grid-cols-2 md:grid-cols-4`
- 间距: `mb-16/12/10` 加手机缩小版; `gap-10/8/6` 加手机缩小版
- 固定元素: logo/语言切换/导航按钮位置手机适配
- 导航圆点: 手机端缩小 `w-2 h-2`
- body 加 `overflow-x: hidden`
- 新增触摸滑动支持（touchstart/touchend，阈值 50px）

### v6 (2026-03-04) — 中英文切换
**修改内容**:
- 添加 LanguageToggle 组件
- 所有 slide 内容支持 `lang === 'en'` / `lang === 'zh'` 条件渲染
- 默认语言: en

### v5 (2026-03-02) — 初始完整版
**修改内容**:
- 15 张 slide 完整内容
- 导航系统（键盘 + 鼠标 + 底部导航条）
- 动画系统（SlideContainer + AnimatedElement）
- 深色主题（slate-950 背景）

## 5. 已知问题 / TODO

### 待验证
- [ ] v7 手机适配还未在真实手机上测试
- [ ] 部分 slide 内容较多（如 Six Pools、Trust），手机端可能需要滚动或进一步简化

### 已知问题
- 使用 Babel Standalone 浏览器端编译 JSX，首次加载有编译延迟（~1-2s）
- 所有 slide 同时在 DOM 中（通过 opacity 切换），15 张 slide 的 DOM 较重
- Logo 使用 base64 内嵌（~100KB），增加文件体积

### 未来改进方向
- [ ] 考虑预编译 JSX 减少加载时间
- [ ] 考虑图片资源外置减少文件体积
- [ ] 考虑添加 `sm:` (640px) 断点做更精细的平板适配
- [ ] Slide 内容更新时需要同步中英文两个版本
- [ ] 测试不同手机浏览器兼容性（Safari、Chrome、微信内置浏览器）

## 6. 部署流程

```bash
# 1. 把最新版本复制到部署仓库
cp "skillcap-pitch v7.html" skillcap-pitch/index.html

# 2. 提交并推送
cd skillcap-pitch
git add index.html
git commit -m "v7: mobile responsive"
git push origin main

# 3. GitHub Pages 自动部署到 skillcap.ai
# （通常 1-2 分钟生效）
```
