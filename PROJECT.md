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

### v7.2 (2026-03-05) — 手机端第二轮修复
**触发**: Linda 真机二次测试发现 5 个问题
**修改内容**:
- Page 1 (Title): 恢复 `justify-center`（标题页内容少，不需要 justify-start）
- Page 5 (Solution): 每个 grid item 外包 `<div className="h-full">`，确保 01/02、03/04 底部对齐
- Page 7 (Six Pools): grid `grid-cols-5` → `grid-cols-2 md:grid-cols-5`；+ 号手机端可见；platform 框 `w-full md:w-36 mx-auto` 居中
- Page 10 (Fission): SVG viewBox `"0 0 800 120"` → `"50 0 280 120"` 裁剪空白；高度 `h-40 md:h-32`；父容器改 `flex justify-center`
- 全局: `pb-16` → `pb-28`（所有 slide 底部 padding 增大，避免与导航按钮重叠）

### v7.1 (2026-03-05) — 手机端 bug 修复
**触发**: Linda 真机测试发现 8 个问题
**修改内容**:
- 全部 15 张 slide: `justify-center` → `justify-start md:justify-center` + `overflow-y-auto` + `pt-20 pb-16`（手机端从顶部开始排列、支持滚动、避开 logo/导航区域）
- Page 1: 副标题加 `text-center` 修复居中
- Page 5 (Solution): grid 加 `w-full` 修复对齐
- Page 7 (Six Pools): 主容器改 `flex-col md:flex-row`，手机端垂直堆叠
- Page 10 (Fission): SVG 高度 `h-24 md:h-32` + 父容器 `overflow-x-auto`
- Page 15 (Closer): CTA 按钮从 `<div>` 改为 `<a href>` + `stopPropagation` + `z-50`

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

## 5. 手机适配设计经验（踩坑记录）

以下是 v7/v7.1 手机适配过程中遇到的典型问题和解法，**下次修改前必读**。

### 5.1 内容溢出丢失（最严重）

**问题**: Slide 使用 `absolute inset-0` + `flex justify-center` 实现垂直居中。桌面端内容少、屏幕大没问题。手机端屏幕小、内容多列变单列后高度暴增，超出屏幕的部分直接消失，用户无法滚动。

**解法**:
- `justify-center` → `justify-start md:justify-center`（手机从顶部排，桌面居中）
- 加 `overflow-y-auto`（允许垂直滚动）
- 加 `pt-20 pb-16`（手机端顶部留空避开 logo/语言切换，底部留空避开导航栏）

**教训**: 任何 `absolute` + `flex-center` 的全屏布局，在手机端都必须考虑内容溢出。不能假设内容总是比屏幕矮。

### 5.2 固定元素遮挡内容

**问题**: Logo（`absolute top-4 left-4`）、语言切换（`fixed top-4 right-4`）、底部导航栏（`fixed bottom-0`）都是浮在内容上方的。手机端内容从顶部开始时，第一行直接被 logo 挡住。

**解法**: 内容区域加 `pt-20`（给顶部固定元素留出空间）和 `pb-16`（给底部导航栏留出空间）。

**教训**: 有 fixed/absolute 浮层的页面，正文区域必须有对应的 padding 来"让路"。

### 5.3 多列网格变单列后高度爆炸

**问题**: 桌面端 3 列网格变手机 1 列后，高度变成原来 3 倍。如果原本就占满屏幕，变 1 列后必然溢出。

**解法**:
- `grid-cols-3` → `grid-cols-1 md:grid-cols-3`（配合 5.1 的滚动方案）
- 对于内容特别多的 slide（如 Six Pools），需要额外做手机专属布局简化，不能简单地把 3 列变 1 列

**教训**: 响应式不只是"减少列数"，还要考虑减列后总高度是否可接受。超过 2 屏的内容需要重新设计。

### 5.4 可点击按钮被全屏 overlay 拦截

**问题**: Pitch deck 有一个全屏透明 overlay（z-0）用于"点击任意位置翻页"。页面内的真正按钮（如 CTA 链接）的点击事件被 overlay 拦截，变成翻页而不是打开链接。

**解法**:
- 按钮加 `z-50`（高于 overlay 的 z-index）
- 按钮加 `onClick={(e) => e.stopPropagation()}`（阻止事件冒泡）
- 用 `<a>` 标签而非 `<div>`（语义正确，浏览器原生处理链接）

**教训**: 全屏点击区域和页面内交互元素必然冲突。每个可点击元素都需要 stopPropagation + 更高 z-index。

### 5.5 文字居中在响应式下丢失

**问题**: 桌面端内容自然居中（容器宽度有限 + flex justify-center），但手机端容器变窄、内容左对齐后，某些文字段落失去居中效果。

**解法**: 对需要居中的文本显式加 `text-center`，不依赖父容器的 flex 布局隐式居中。

**教训**: 不要依赖"看起来居中了"，显式声明 `text-center`。

### 5.6 SVG/图表在小屏上缩太小

**问题**: 固定尺寸的 SVG 图表（如 Fission 的 parent-child 关系图）在手机上按比例缩小后，细节完全看不清。

**解法**:
- 父容器加 `overflow-x-auto`（允许水平滚动查看完整图表）
- SVG 高度做响应式 `h-24 md:h-32`
- 加 `min-w-full` 保持最小宽度

**教训**: 信息密集的 SVG 图表不适合简单缩放，要么重新设计手机版、要么允许水平滚动查看原始大小。

### 5.7 修改后必须真机测试

**问题**: v7 做了全面的响应式修改，但没有在真实手机上测试就部署了。结果 Linda 发现 8 个严重问题。

**教训**:
- 浏览器 resize 窗口 ≠ 真实手机测试（viewport meta、触摸行为、Safari 特性都不同）
- 每次修改后至少逐页检查一遍，特别关注：内容是否被截断、元素是否重叠、按钮是否可点击
- **发布前 checklist**: 逐页截图 / 滚动测试 / 点击交互测试 / 横屏测试

## 6. 已知问题 / TODO

### 待验证
- [ ] v7.1 修复后需要在真机上重新验证所有 15 页
- [ ] Page 7 (Six Pools) 手机端垂直堆叠后是否可读
- [ ] 横屏模式下的显示效果

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
