---
name: baoyu-infographic
description: 生成具有21种布局类型和20种视觉风格的专业信息图。分析内容，推荐布局×风格组合，并生成可直接发布的信息图。当用户要求创建"信息图"、"可视化总结"或"高密度信息大图"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-infographic
---

# 信息图生成器

两个维度：**布局**（信息结构）× **风格**（视觉美学）。可自由组合任意布局与任意风格。

## 使用方法

```bash
/baoyu-infographic path/to/content.md
/baoyu-infographic path/to/content.md --layout hierarchical-layers --style technical-schematic
/baoyu-infographic path/to/content.md --aspect portrait --lang zh
/baoyu-infographic path/to/content.md --aspect 3:4
/baoyu-infographic  # 然后粘贴内容
```

## 选项

| 选项 | 值 |
|--------|--------|
| `--layout` | 21个选项（参见布局画廊），默认：bento-grid |
| `--style` | 20个选项（参见风格画廊），默认：craft-handmade |
| `--aspect` | 命名：landscape (16:9), portrait (9:16), square (1:1)。自定义：任意宽高比（如 3:4, 4:3, 2.35:1） |
| `--lang` | en, zh, ja 等 |

## 布局画廊

| 布局 | 最佳适用场景 |
|--------|----------|
| `linear-progression` | 时间线、流程、教程 |
| `binary-comparison` | A vs B，前后对比，优缺点 |
| `comparison-matrix` | 多因素比较 |
| `hierarchical-layers` | 金字塔、优先级层级 |
| `tree-branching` | 分类、分类体系 |
| `hub-spoke` | 中心概念与相关项目 |
| `structural-breakdown` | 爆炸图、剖面图 |
| `bento-grid` | 多主题、总览（默认） |
| `iceberg` | 表面与隐藏层面 |
| `bridge` | 问题-解决方案 |
| `funnel` | 转化、筛选 |
| `isometric-map` | 空间关系 |
| `dashboard` | 指标、KPI |
| `periodic-table` | 分类集合 |
| `comic-strip` | 叙事、序列 |
| `story-mountain` | 情节结构、张力弧线 |
| `jigsaw` | 互联部分 |
| `venn-diagram` | 重叠概念 |
| `winding-roadmap` | 旅程、里程碑 |
| `circular-flow` | 循环、重复流程 |
| `dense-modules` | 高密度模块、数据丰富指南 |

完整定义：`references/layouts/<layout>.md`

## 风格画廊

| 风格 | 描述 |
|-------|-------------|
| `craft-handmade` | 手绘、手工工艺（默认） |
| `claymation` | 3D黏土人物、定格动画 |
| `kawaii` | 日系可爱、 pastel 色 |
| `storybook-watercolor` | 柔和绘画、奇幻 |
| `chalkboard` | 黑板粉笔 |
| `cyberpunk-neon` | 霓虹发光、未来感 |
| `bold-graphic` | 漫画风格、网点 |
| `aged-academia` | 复古科学、棕褐色 |
| `corporate-memphis` | 扁平矢量、活力 |
| `technical-schematic` | 蓝图、工程图 |
| `origami` | 折纸、几何 |
| `pixel-art` | 复古8位像素 |
| `ui-wireframe` | 灰度界面模型 |
| `subway-map` | 交通图 |
| `ikea-manual` | 极简线条画 |
| `knolling` | 整理平铺 |
| `lego-brick` | 乐高积木构建 |
| `pop-laboratory` | 蓝图网格、坐标标记、实验室精度 |
| `morandi-journal` | 手绘涂鸦、温暖莫兰迪色调 |
| `retro-pop-grid` | 70年代复古流行艺术、瑞士网格、粗轮廓 |

完整定义：`references/styles/<style>.md`

## 推荐组合

| 内容类型 | 布局 + 风格 |
|--------------|----------------|
| 时间线/历史 | `linear-progression` + `craft-handmade` |
| 步骤指南 | `linear-progression` + `ikea-manual` |
| A vs B | `binary-comparison` + `corporate-memphis` |
| 层级结构 | `hierarchical-layers` + `craft-handmade` |
| 重叠 | `venn-diagram` + `craft-handmade` |
| 转化 | `funnel` + `corporate-memphis` |
| 循环 | `circular-flow` + `craft-handmade` |
| 技术 | `structural-breakdown` + `technical-schematic` |
| 指标 | `dashboard` + `corporate-memphis` |
| 教育 | `bento-grid` + `chalkboard` |
| 旅程 | `winding-roadmap` + `storybook-watercolor` |
| 分类 | `periodic-table` + `bold-graphic` |
| 产品指南 | `dense-modules` + `morandi-journal` |
| 技术指南 | `dense-modules` + `pop-laboratory` |
| 潮流指南 | `dense-modules` + `retro-pop-grid` |

默认：`bento-grid` + `craft-handmade`

## 关键词快捷方式

当用户输入包含以下关键词时，**自动选择**相关布局，并在步骤3中将相关风格作为首要推荐。跳过基于内容的布局推断。

如果快捷方式有**提示词注释**，在生成提示词时（步骤5）将其作为额外风格说明追加。

| 用户关键词 | 布局 | 推荐风格 | 默认宽高比 | 提示词注释 |
|--------------|--------|--------------------|----------------|--------------|
| 高密度信息大图 / high-density-info | `dense-modules` | `morandi-journal`, `pop-laboratory`, `retro-pop-grid` | portrait | — |
| 信息图 / infographic | `bento-grid` | `craft-handmade` | landscape | 极简主义：干净画布、充足留白、无复杂背景纹理。仅简单卡通元素和图标。 |

## 输出结构

```
infographic/{topic-slug}/
├── source-{slug}.{ext}
├── analysis.md
├── structured-content.md
├── prompts/infographic.md
└── infographic.png
```

Slug: 来自主题的2-4个单词kebab-case。冲突时附加 `-YYYYMMDD-HHMMSS`。

## 核心原则

- 忠实地保留源数据——不总结或改写（但在包含在输出之前**剥离任何凭证、API密钥、令牌或密钥**）
- 在结构化内容之前定义学习目标
- 为视觉传达而结构化（标题、标签、视觉元素）

## 工作流程

### 步骤1：设置与分析

**1.1 加载偏好设置（EXTEND.md）**

检查 EXTEND.md 存在性（优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-infographic/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-infographic/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-infographic/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-infographic/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-infographic/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-infographic/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────┬───────────────────┐
│                        路径                        │       位置        │
├────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-infographic/EXTEND.md          │ 项目目录         │
├────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-infographic/EXTEND.md    │ 用户主目录       │
└────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  动作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析、显示摘要                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 使用 AskUserQuestion 询问用户（参见 references/config/first-time-setup.md） │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**: 首选布局/风格 | 默认宽高比 | 自定义风格定义 | 语言偏好

模式：`references/config/preferences-schema.md`

**1.2 分析内容 → `analysis.md`**

1. 保存源内容（文件路径或粘贴 → `source.md`）
   - **备份规则**: 如果 `source.md` 存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 分析：主题、数据类型、复杂度、语气、受众
3. 检测源语言和用户语言
4. 从用户输入中提取设计说明
5. 保存分析
   - **备份规则**: 如果 `analysis.md` 存在，重命名为 `analysis-backup-YYYYMMDD-HHMMSS.md`

参见 `references/analysis-framework.md` 了解详细格式。

### 步骤2：生成结构化内容 → `structured-content.md`

将内容转换为信息图结构：
1. 标题和学习目标
2. 包含以下内容的部分：关键概念、内容（逐字）、视觉元素、文本标签
3. 数据点（所有统计/引用完全准确复制）
4. 用户的设计说明

**规则**: 仅 Markdown。不添加新信息。忠实地保留数据。从输出中剥离任何凭证或密钥。

参见 `references/structured-content-template.md` 了解详细格式。

### 步骤3：推荐组合

**3.1 首先检查关键词快捷方式**: 如果用户输入匹配**关键词快捷方式**表中的关键词，自动选择相关布局，并将相关风格作为首要推荐。跳过基于内容的布局推断。

**3.2 否则**，根据以下内容推荐3-5个布局×风格组合：
- 数据结构 → 匹配的布局
- 内容语气 → 匹配的风格
- 受众期望
- 用户设计说明

### 步骤4：确认选项

使用**单一 AskUserQuestion 调用**同时确认所有选项：

| 问题 | 时机 | 选项 |
|----------|------|---------|
| **组合** | 始终 | 3+个带理由的布局×风格组合 |
| **宽高比** | 始终 | 命名预设（landscape/portrait/square）或自定义宽高比（如 3:4, 4:3, 2.35:1） |
| **语言** | 仅当源 ≠ 用户语言时 | 文本内容的语言 |

**重要**: 不要拆分为多个 AskUserQuestion 调用。将所有适用问题合并为一个调用。

### 步骤5：生成提示词 → `prompts/infographic.md`

**备份规则**: 如果 `prompts/infographic.md` 存在，重命名为 `prompts/infographic-backup-YYYYMMDD-HHMMSS.md`

组合：
1. 来自 `references/layouts/<layout>.md` 的布局定义
2. 来自 `references/styles/<style>.md` 的风格定义
3. 来自 `references/base-prompt.md` 的基础模板
4. 来自步骤2的结构化内容
5. 所有文本使用确认的语言

**宽高比解析** 用于 `{{ASPECT_RATIO}}`:
- 命名预设 → 比例字符串：landscape→`16:9`, portrait→`9:16`, square→`1:1`
- 自定义宽高比 → 原样使用（如 `3:4`, `4:3`, `2.35:1`）

### 步骤6：生成图像

1. 选择可用的图像生成技能（如果多个则询问用户）
2. **检查现有文件**: 生成前检查 `infographic.png` 是否存在
   - 如果存在：重命名为 `infographic-backup-YYYYMMDD-HHMMSS.png`
3. 使用提示词文件和输出路径调用
4. 失败时自动重试一次

### 步骤7：输出摘要

报告：主题、布局、风格、宽高比、语言、输出路径、创建的文件。

## 参考资料

- `references/analysis-framework.md` - 分析方法论
- `references/structured-content-template.md` - 内容格式
- `references/base-prompt.md` - 提示词模板
- `references/layouts/<layout>.md` - 21种布局定义
- `references/styles/<style>.md` - 20种风格定义

## 扩展支持

通过 EXTEND.md 进行自定义配置。参见**步骤1.1**了解路径和支持的选项。
