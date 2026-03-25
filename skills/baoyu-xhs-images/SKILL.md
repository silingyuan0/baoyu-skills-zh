---
name: baoyu-xhs-images
description: 生成小红书信息图系列，支持 11 种视觉风格和 8 种布局。将内容拆解为 1-10 张卡通风格图片，针对小红书平台互动进行优化。当用户提到"小红书图片"、"XHS images"、"RedNote infographics"、"小红书种草"，或想为中文社交平台生成信息图时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-xhs-images
---

# 小红书信息图系列生成器

将复杂内容拆解为吸睛的小红书信息图系列，提供多种风格选项。

## 使用方法

```bash
# 基于内容自动选择风格和布局
/baoyu-xhs-images posts/ai-future/article.md

# 指定风格
/baoyu-xhs-images posts/ai-future/article.md --style notion

# 指定布局
/baoyu-xhs-images posts/ai-future/article.md --layout dense

# 组合风格和布局
/baoyu-xhs-images posts/ai-future/article.md --style notion --layout list

# 使用预设（风格 + 布局简写）
/baoyu-xhs-images posts/ai-future/article.md --preset knowledge-card

# 预设 + 覆盖
/baoyu-xhs-images posts/ai-future/article.md --preset poster --layout quadrant

# 直接输入内容
/baoyu-xhs-images
[粘贴内容]

# 带选项的直接输入
/baoyu-xhs-images --style bold --layout comparison
[粘贴内容]
```

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--style <name>` | 视觉风格（见风格画廊） |
| `--layout <name>` | 信息布局（见布局画廊） |
| `--preset <name>` | 风格 + 布局简写（见[风格预设](references/style-presets.md)） |

## 两个维度

| 维度 | 控制内容 | 选项 |
|-----------|----------|---------|
| **风格** | 视觉美学：颜色、线条、装饰 | cute、fresh、warm、bold、minimal、retro、pop、notion、chalkboard、study-notes、screen-print |
| **布局** | 信息结构：密度、排列 | sparse、balanced、dense、list、comparison、flow、mindmap、quadrant |

风格与布局可自由组合。例如：`--style notion --layout dense` 会创建一张具有高信息密度的知识感信息图。

也可使用预设：`--preset knowledge-card` 即可在一条命令中指定风格与布局。见[风格预设](references/style-presets.md)。

## 风格画廊

| 风格 | 描述 |
|-------|-------------|
| `cute`（默认） | 甜美、可爱、少女风 —— 经典小红书美学 |
| `fresh` | 清爽、清新、自然 |
| `warm` | 温馨、友好、亲切 |
| `bold` | 高冲击力、引人注目 |
| `minimal` | 极致简洁、精致高级 |
| `retro` | 复古、怀旧、时髦 |
| `pop` | 鲜艳、活力、吸睛 |
| `notion` | 极简手绘线条风，知识感 |
| `chalkboard` | 彩色粉笔黑板风，教育感 |
| `study-notes` | 逼真手写笔记照片风格，蓝色圆珠笔 + 红色批注 + 黄色荧光笔 |
| `screen-print` | 粗犷海报艺术，半调纹理，限色配色，符号化叙事 |

详细风格定义：`references/presets/<style>.md`

## 预设画廊

按内容场景划分的快速启动预设。使用 `--preset <name>` 或在步骤 2 中推荐。

**知识学习类**：

| 预设 | 风格 | 布局 | 最适合 |
|--------|-------|--------|----------|
| `knowledge-card` | notion | dense | 干货知识卡、概念科普 |
| `checklist` | notion | list | 清单、排行榜、必备清单 |
| `concept-map` | notion | mindmap | 概念图、知识脉络 |
| `swot` | notion | quadrant | SWOT分析、四象限分类 |
| `tutorial` | chalkboard | flow | 教程步骤、操作流程 |
| `classroom` | chalkboard | balanced | 课堂笔记、知识讲解 |
| `study-guide` | study-notes | dense | 学习笔记、考试重点 |

**生活分享类**：

| 预设 | 风格 | 布局 | 最适合 |
|--------|-------|--------|----------|
| `cute-share` | cute | balanced | 少女风分享、日常种草 |
| `girly` | cute | sparse | 甜美封面、氛围感 |
| `cozy-story` | warm | balanced | 生活故事、情感分享 |
| `product-review` | fresh | comparison | 产品对比、测评 |
| `nature-flow` | fresh | flow | 健康流程、自然主题 |

**观点冲击类**：

| 预设 | 风格 | 布局 | 最适合 |
|--------|-------|--------|----------|
| `warning` | bold | list | 避坑指南、重要提醒 |
| `versus` | bold | comparison | 正反对比、强烈对照 |
| `clean-quote` | minimal | sparse | 金句、极简封面 |
| `pro-summary` | minimal | balanced | 专业总结、商务内容 |

**潮流趣味类**：

| 预设 | 风格 | 布局 | 最适合 |
|--------|-------|--------|----------|
| `retro-ranking` | retro | list | 复古排行、经典盘点 |
| `throwback` | retro | balanced | 怀旧分享、老物件 |
| `pop-facts` | pop | list | 趣味冷知识、好玩的事 |
| `hype` | pop | sparse | 炸裂封面、惊叹分享 |

**海报社论类**：

| 预设 | 风格 | 布局 | 最适合 |
|--------|-------|--------|----------|
| `poster` | screen-print | sparse | 海报风封面、影评书评 |
| `editorial` | screen-print | balanced | 观点文章、文化评论 |
| `cinematic` | screen-print | comparison | 电影对比、戏剧张力 |

完整预设定义：[references/style-presets.md](references/style-presets.md)

## 布局画廊

| 布局 | 描述 |
|--------|-------------|
| `sparse`（默认） | 信息极简，冲击力最大化（1-2 个要点） |
| `balanced` | 标准内容布局（3-4 个要点） |
| `dense` | 高信息密度，知识卡风格（5-8 个要点） |
| `list` | 列举和排行格式（4-7 项） |
| `comparison` | 左右对比布局 |
| `flow` | 流程和时间线布局（3-6 步） |
| `mindmap` | 中心辐射式思维导图布局（4-8 个分支） |
| `quadrant` | 四象限 / 环形分区布局 |

详细布局定义：`references/elements/canvas.md`

## 自动选择

| 内容信号 | 风格 | 布局 | 推荐预设 |
|-----------------|-------|--------|--------------------|
| 美妆、时尚、可爱、少女、粉色 | `cute` | sparse/balanced | `cute-share`、`girly` |
| 健康、自然、干净、清新、有机 | `fresh` | balanced/flow | `product-review`、`nature-flow` |
| 生活、故事、情感、感受、温暖 | `warm` | balanced | `cozy-story` |
| 警告、重要、必须、关键 | `bold` | list/comparison | `warning`、`versus` |
| 专业、商务、优雅、简洁 | `minimal` | sparse/balanced | `clean-quote`、`pro-summary` |
| 经典、复古、怀旧、传统 | `retro` | balanced | `throwback`、`retro-ranking` |
| 有趣、激动、惊艳 | `pop` | sparse/list | `hype`、`pop-facts` |
| 知识、概念、效率、SaaS | `notion` | dense/list | `knowledge-card`、`checklist` |
| 教育、教程、学习、教学、课堂 | `chalkboard` | balanced/dense | `tutorial`、`classroom` |
| 笔记、手写、学习指南、知识、逼真、照片 | `study-notes` | dense/list/mindmap | `study-guide` |
| 电影、专辑、演唱会、海报、观点、社论、戏剧、电影感 | `screen-print` | sparse/comparison | `poster`、`editorial`、`cinematic` |

## 大纲策略

针对不同内容目标的差异化大纲策略：

### 策略 A：故事驱动型

| 方面 | 描述 |
|--------|-------------|
| **概念** | 以个人体验为主轴，情感共鸣优先 |
| **特点** | 从痛点切入，展示前后变化，真实感强 |
| **最适合** | 测评、个人分享、转变故事 |
| **结构** | 钩子 → 问题 → 发现 → 体验 → 总结 |

### 策略 B：信息密集型

| 方面 | 描述 |
|--------|-------------|
| **概念** | 价值优先，高效传递信息 |
| **特点** | 结构清晰，要点明确，专业可信 |
| **最适合** | 教程、对比、产品测评、清单 |
| **结构** | 核心结论 → 信息卡 → 优缺点 → 推荐 |

### 策略 C：视觉优先型

| 方面 | 描述 |
|--------|-------------|
| **概念** | 以视觉冲击为核心，文字极简 |
| **特点** | 大图、氛围感、即时吸引力 |
| **最适合** | 高颜值产品、生活方式、氛围感内容 |
| **结构** | 主图 → 细节图 → 生活场景 → 行动号召 |

## 文件结构

每次会话创建一个以内容简短标识符命名的独立目录：

```
xhs-images/{topic-slug}/
├── source-{slug}.{ext}             # 源文件（文本、图片等）
├── analysis.md                     # 深度分析 + 提问记录
├── outline-strategy-a.md           # 策略 A：故事驱动型
├── outline-strategy-b.md           # 策略 B：信息密集型
├── outline-strategy-c.md           # 策略 C：视觉优先型
├── outline.md                      # 最终选定/合并的大纲
├── prompts/
│   ├── 01-cover-[slug].md
│   ├── 02-content-[slug].md
│   └── ...
├── 01-cover-[slug].png
├── 02-content-[slug].png
└── NN-ending-[slug].png
```

**简短标识符生成**：
1. 从内容中提取主题（2-4 个词，kebab-case）
2. 示例："AI工具推荐" → `ai-tools-recommend`

**冲突处理**：
如果 `xhs-images/{topic-slug}/` 已存在：
- 追加时间戳：`{topic-slug}-YYYYMMDD-HHMMSS`
- 示例：`ai-tools` 已存在 → `ai-tools-20260118-143052`

**源文件**：
将所有源文件以 `source-{slug}.{ext}` 命名复制：
- `source-article.md`、`source-photo.jpg` 等
- 支持多个源文件：文本、图片、对话中的文件

## 工作流程

### 进度清单

复制并跟踪进度：

```
小红书信息图进度：
- [ ] 步骤 0：检查偏好（EXTEND.md）⛔ 阻塞操作
  - [ ] 已找到 → 加载偏好 → 继续
  - [ ] 未找到 → 运行首次设置 → 必须在步骤 1 之前完成
- [ ] 步骤 1：分析内容 → analysis.md
- [ ] 步骤 2：智能确认 ⚠️ 必需
  - [ ] 路径 A：快速确认 → 生成推荐大纲
  - [ ] 路径 B：自定义 → 调整后生成大纲
  - [ ] 路径 C：详细模式 → 3 份大纲 → 二次确认 → 生成大纲
- [ ] 步骤 3：生成图片（顺序执行）
- [ ] 步骤 4：完成报告
```

### 流程图

```
输入 → [步骤 0：偏好] ─┬─ 已找到 → 继续
                       │
                       └─ 未找到 → 首次设置 ⛔ 阻塞操作
                                      │
                                      └─ 完成设置 → 保存 EXTEND.md → 继续
                                                                                      │
        ┌───────────────────────────────────────────────────────────────────────────┘
        ↓
分析 → [智能确认] ─┬─ 快速：确认推荐 → outline.md → 生成 → 完成
                   │
                   ├─ 自定义：调整选项 → outline.md → 生成 → 完成
                   │
                   └─ 详细：3 份大纲 → [确认 2] → outline.md → 生成 → 完成
```

### 步骤 0：加载偏好（EXTEND.md）⛔ 阻塞操作

**目的**：加载用户偏好或运行首次设置。

**关键要求**：如果未找到 EXTEND.md，必须在任何其他问题或步骤之前完成首次设置。不要进入内容分析，不要询问风格，不要询问布局——只先完成偏好设置。

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS、Linux、WSL、Git Bash
test -f .baoyu-skills/baoyu-xhs-images/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-xhs-images/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md" && echo "user"
```

```powershell
# PowerShell（Windows）
if (Test-Path .baoyu-skills/baoyu-xhs-images/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-xhs-images/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────┬───────────────────┐
│                        路径                        │       位置        │
├────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-xhs-images/EXTEND.md           │ 项目目录          │
├────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md     │ 用户主目录        │
└────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│   结果    │                                              操作                                              │
├───────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 已找到    │ 读取、解析、显示摘要 → 继续到步骤 1                                                    │
├───────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ ⛔ 阻塞操作：仅运行首次设置（见下方）→ 完成并保存 EXTEND.md → 然后进入步骤 1              │
└───────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────┘

**首次设置**（当 EXTEND.md 未找到时）：

**语言**：使用用户的输入语言或已保存的语言偏好。

使用 AskUserQuestion 一次性提出所有问题。问题详情见 `references/config/first-time-setup.md`。

**EXTEND.md 支持的功能**：水印 | 首选风格/布局 | 自定义风格定义 | 语言偏好

配置结构：`references/config/preferences-schema.md`

### 步骤 1：分析内容 → `analysis.md`

读取源内容，必要时保存，并执行深度分析。

**操作**：
1. **保存源内容**（如果还不是文件）：
   - 如果用户提供文件路径：直接使用
   - 如果用户粘贴内容：保存到目标目录的 `source.md`
   - **备份规则**：如果 `source.md` 已存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 读取源内容
3. **深度分析**，遵循 `references/workflows/analysis-framework.md`：
   - 内容类型分类（种草/干货/测评/教程/避坑...）
   - 钩子分析（爆款标题潜力）
   - 目标受众识别
   - 互动潜力（收藏/分享/评论）
   - 视觉机会映射
   - 滑动流程设计
4. 检测源语言
5. 确定推荐图片数量（2-10）
6. **自动推荐**基于内容信号的最佳策略 + 风格 + 布局
7. **保存到 `analysis.md`**

### 步骤 2：智能确认 ⚠️

**目的**：展示自动推荐方案，让用户确认或调整。**不要跳过。**

**自动推荐逻辑**：
1. 使用自动选择表匹配内容信号 → 最佳策略 + 风格 + 布局
2. 从内容密度推断最佳图片数量
3. 从预设加载风格的默认元素

**展示内容**（分析摘要 + 推荐方案）：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 内容分析
  主题：[topic] | 类型：[content_type]
  要点：[key points summary]
  受众：[target audience]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎨 推荐方案（自动匹配）
  策略：[A/B/C] [strategy name]（[reason]）
  风格：[style] · 布局：[layout] · 预设：[preset]
  图片：[N]张（封面+[N-2]内容+结尾）
  元素：[background] / [decorations] / [emphasis]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**使用 AskUserQuestion**，单个问题：

| 选项 | 描述 |
|--------|-------------|
| 1. 确认，直接生成（推荐） | 信任自动推荐，立即执行 |
| 2. 自定义调整 | 一步调整策略/风格/布局/数量 |
| 3. 详细模式 | 生成 3 份大纲，然后选择（两次确认） |

#### 路径 A：快速确认（选项 1）

使用推荐策略 + 风格生成单份大纲 → 保存到 `outline.md` → 步骤 3。

#### 路径 B：自定义（选项 2）

**使用 AskUserQuestion**，可调整选项（留空 = 保持推荐）：

1. **策略风格**：当前：[strategy + style]。选项：A 故事驱动型(warm) | B 信息密集型(notion) | C 视觉优先型(screen-print)。或直接指定风格：cute/fresh/warm/bold/minimal/retro/pop/notion/chalkboard/study-notes/screen-print。或使用预设：knowledge-card / checklist / tutorial / poster / cinematic 等。
2. **布局**：当前：[layout]。选项：sparse | balanced | dense | list | comparison | flow | mindmap | quadrant
3. **图片数量**：当前：[N]。范围：2-10
4. **补充说明**（可选）：卖点强调、受众调整、颜色偏好等。

**回复后**：使用用户选择生成单份大纲 → 保存到 `outline.md` → 步骤 3。

#### 路径 C：详细模式（选项 3）

提供完整的两次确认流程，实现最大控制度：

**步骤 2a：内容理解**

**使用 AskUserQuestion** 询问：
1. 核心卖点（multiSelect: true）
2. 目标受众
3. 风格偏好：真实分享 / 专业测评 / 氛围感 / 自动
4. 补充背景（可选）

**回复后**：更新 `analysis.md`。

**步骤 2b：生成 3 份大纲变体**

| 策略 | 文件名 | 大纲 | 推荐风格 |
|----------|----------|---------|-------------------|
| A | `outline-strategy-a.md` | 故事驱动型：情感化、前后对比 | warm、cute、fresh |
| B | `outline-strategy-b.md` | 信息密集型：结构化、事实性 | notion、minimal、chalkboard |
| C | `outline-strategy-c.md` | 视觉优先型：氛围感、文字极简 | bold、pop、retro、screen-print |

**大纲格式**（YAML 前置元数据 + 内容）：
```yaml
---
strategy: a  # a、b 或 c
name: Story-Driven
style: warm  # 此策略的推荐风格
style_reason: "暖色调增强情感叙事和亲和力"
elements:  # 来自风格预设，可自定义
  background: solid-pastel
  decorations: [clouds, stars-sparkles]
  emphasis: star-burst
  typography: highlight
layout: balanced  # 主布局
image_count: 5
---

## P1 封面
**Type**: cover
**Hook**: "入冬后脸不干了🥹终于找到对的面霜"
**Visual**: 产品主图配温馨冬日氛围
**Layout**: sparse

## P2 问题
**Type**: pain-point
**Message**: 之前的干皮困扰
**Visual**: 痛点状态，引发共鸣的场景
**Layout**: balanced

...
```

**差异化要求**：
- 每种策略必须有不同的大纲结构和不同的推荐风格
- 适配页数：A 通常 4-6 页，B 通常 3-5 页，C 通常 3-4 页
- 包含 `style_reason` 解释为什么此风格适合该策略

参考：`references/workflows/outline-template.md`

**步骤 2c：大纲与风格选择**

**使用 AskUserQuestion**，三个问题：

**Q1：大纲策略**：A / B / C / 组合（指定各策略的页数）

**Q2：视觉风格**：使用推荐 | 选择预设 | 选择风格 | 自定义描述

**Q3：视觉元素**：使用默认（推荐）| 调整背景 | 调整装饰 | 自定义

**回复后**：将选定/合并的大纲保存到 `outline.md`，附上确认的风格和元素 → 步骤 3。

### 步骤 3：生成图片

使用确认的大纲 + 风格 + 布局：

**视觉一致性 — 参考图片链**：
为确保系列中所有图片的角色/风格一致性：
1. **首先生成图片 1（封面）**——不使用 `--ref`
2. **将图片 1 作为所有后续图片的 `--ref`**（2、3、...、N）
   - 这会锚定角色设计、色彩渲染和插图风格
   - 命令模式：为每次后续生成添加 `--ref <path-to-image-01.png>`

这对使用重复角色、吉祥物或插图元素的风格至关重要。图片 1 成为整个系列的视觉锚点。

**对每张图片（封面 + 内容 + 结尾）**：
1. 保存提示词到 `prompts/NN-{type}-[slug].md`（使用用户首选语言）
   - **备份规则**：如果提示词文件已存在，重命名为 `prompts/NN-{type}-[slug]-backup-YYYYMMDD-HHMMSS.md`
2. 生成图片：
   - **图片 1**：不使用 `--ref` 生成（建立视觉锚点）
   - **图片 2+**：使用 `--ref <image-01-path>` 生成以确保一致性
   - **备份规则**：如果图片文件已存在，重命名为 `NN-{type}-[slug]-backup-YYYYMMDD-HHMMSS.png`
3. 每次生成后报告进度

**水印应用**（如果在偏好中启用）：
在每张图片生成提示词中添加：
```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the main content.
```
参考：`references/config/watermark-guide.md`

**图片生成技能选择**：
- 检查可用的图片生成技能
- 如果有多个可用，询问用户偏好

**会话管理**：
如果图片生成技能支持 `--sessionId`：
1. 生成唯一会话 ID：`xhs-{topic-slug}-{timestamp}`
2. 所有图片使用相同会话 ID
3. 结合参考图片链，确保最大程度的视觉一致性

### 步骤 4：完成报告

```
小红书信息图系列完成！

主题：[topic]
模式：[快速 / 自定义 / 详细]
策略：[A/B/C/组合]
风格：[style name]
布局：[layout name 或"混合"]
位置：[目录路径]
图片：共 N 张

✓ analysis.md
✓ outline.md
✓ outline-strategy-a/b/c.md（仅详细模式）

文件：
- 01-cover-[slug].png ✓ 封面（sparse）
- 02-content-[slug].png ✓ 内容（balanced）
- 03-content-[slug].png ✓ 内容（dense）
- 04-ending-[slug].png ✓ 结尾（sparse）
```

## 图片修改

| 操作 | 步骤 |
|--------|-------|
| **编辑** | **先更新提示词文件** → 使用相同会话 ID 重新生成 |
| **添加** | 指定位置 → 创建提示词 → 生成 → 后续文件重新编号（NN+1）→ 更新大纲 |
| **删除** | 删除文件 → 后续重新编号（NN-1）→ 更新大纲 |

**重要**：更新图片时，务必先更新提示词文件（`prompts/NN-{type}-[slug].md`）再重新生成。这确保更改有记录且可复现。

## 内容拆解原则

1. **封面（图片 1）**：钩子 + 视觉冲击 → `sparse` 布局
2. **内容（中间）**：每张图传递核心价值 → `balanced`/`dense`/`list`/`comparison`/`flow`
3. **结尾（最后一张）**：行动号召 / 总结 → `sparse` 或 `balanced`

**风格 × 布局矩阵**（✓✓ = 强烈推荐，✓ = 效果良好）：

| | sparse | balanced | dense | list | comparison | flow | mindmap | quadrant |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| cute | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ |
| fresh | ✓✓ | ✓✓ | ✓ | ✓ | ✓ | ✓✓ | ✓ | ✓ |
| warm | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ |
| bold | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ |
| minimal | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| retro | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ |
| pop | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓ |
| notion | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ |
| chalkboard | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ |
| study-notes | ✗ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓ |
| screen-print | ✓✓ | ✓✓ | ✗ | ✓ | ✓✓ | ✓ | ✗ | ✓✓ |

## 参考文档

`references/` 目录中的详细模板：

**元素**（视觉构建模块）：
- `elements/canvas.md` - 画幅比例、安全区域、网格布局
- `elements/image-effects.md` - 抠图、描边、滤镜
- `elements/typography.md` - 花字、标签、文字方向
- `elements/decorations.md` - 强调标记、背景、涂鸦、边框

**预设**（风格预设）：
- `presets/<name>.md` - 元素组合定义（cute、notion、warm...）
- `style-presets.md` - 预设快捷方式（风格与布局组合）

**工作流**（流程指南）：
- `workflows/analysis-framework.md` - 内容分析框架
- `workflows/outline-template.md` - 大纲模板与布局指南
- `workflows/prompt-assembly.md` - 提示词组装指南

**配置**（设置）：
- `config/preferences-schema.md` - EXTEND.md 配置结构
- `config/first-time-setup.md` - 首次设置流程
- `config/watermark-guide.md` - 水印配置

## 注意事项

- 失败后自动重试一次 | 敏感人物使用卡通替代
- 使用确认的语言偏好 | 保持风格一致性
- **智能确认必需**（步骤 2）——不可跳过；详细模式使用两次子确认

## 扩展支持

通过 EXTEND.md 进行自定义配置。见**步骤 0**了解路径和支持的选项。
