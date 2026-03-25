---
name: baoyu-slide-deck
description: 根据内容生成专业幻灯片图片。根据大纲和样式说明生成幻灯片图像。当用户要求"创建幻灯片"、"制作演示文稿"、"生成幻灯片"或"PPT"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-slide-deck
    requires:
      anyBins:
        - bun
        - npx
---

# 幻灯片生成器

将内容转化为专业幻灯片图片。

## 用法

```bash
/baoyu-slide-deck path/to/content.md
/baoyu-slide-deck path/to/content.md --style sketch-notes
/baoyu-slide-deck path/to/content.md --audience executives
/baoyu-slide-deck path/to/content.md --lang zh
/baoyu-slide-deck path/to/content.md --slides 10
/baoyu-slide-deck path/to/content.md --outline-only
/baoyu-slide-deck  # 然后粘贴内容
```

## 脚本目录

**智能体执行说明**:
1. 将本SKILL.md文件所在目录路径确定为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 解析 `${BUN_X}` 运行时：如果已安装 `bun` → `bun`；如果可用 `npx` → `npx -y bun`；否则建议安装 bun

| 脚本 | 用途 |
|--------|---------|
| `scripts/merge-to-pptx.ts` | 合并为 PowerPoint |
| `scripts/merge-to-pdf.ts` | 合并为 PDF |

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--style <name>` | 视觉样式：预设名称、`custom` 或自定义样式名称 |
| `--audience <type>` | 目标受众：beginners, intermediate, experts, executives, general |
| `--lang <code>` | 输出语言 (en, zh, ja 等) |
| `--slides <number>` | 目标幻灯片数量（建议 8-25，最多 30） |
| `--outline-only` | 仅生成大纲，跳过图片生成 |
| `--prompts-only` | 生成大纲和提示词，跳过图片 |
| `--images-only` | 根据现有提示词目录生成图片 |
| `--regenerate <N>` | 重新生成指定幻灯片：`--regenerate 3` 或 `--regenerate 2,5,8` |

**按内容长度的幻灯片数量**：
| 内容 | 幻灯片数 |
|---------|--------|
| < 1000 字 | 5-10 |
| 1000-3000 字 | 10-18 |
| 3000-5000 字 | 15-25 |
| > 5000 字 | 20-30（考虑拆分） |

## 样式系统

### 预设

| 预设 | 维度 | 适用于 |
|--------|------------|----------|
| `blueprint`（默认） | grid + cool + technical + balanced | 架构、系统设计 |
| `chalkboard` | organic + warm + handwritten + balanced | 教育、教程 |
| `corporate` | clean + professional + geometric + balanced | 投资演示、提案 |
| `minimal` | clean + neutral + geometric + minimal | 高管简报 |
| `sketch-notes` | organic + warm + handwritten + balanced | 教育、教程 |
| `watercolor` | organic + warm + humanist + minimal | 生活、健康 |
| `dark-atmospheric` | clean + dark + editorial + balanced | 娱乐、游戏 |
| `notion` | clean + neutral + geometric + dense | 产品演示、SaaS |
| `bold-editorial` | clean + vibrant + editorial + balanced | 产品发布、主题演讲 |
| `editorial-infographic` | clean + cool + editorial + dense | 技术解说、研究 |
| `fantasy-animation` | organic + vibrant + handwritten + minimal | 教育故事讲述 |
| `intuition-machine` | clean + cool + technical + dense | 技术文档、学术 |
| `pixel-art` | pixel + vibrant + technical + balanced | 游戏、开发者演讲 |
| `scientific` | clean + cool + technical + dense | 生物、化学、医学 |
| `vector-illustration` | clean + vibrant + humanist + balanced | 创意、儿童内容 |
| `vintage` | paper + warm + editorial + balanced | 历史、遗产 |

### 样式维度

| 维度 | 选项 | 描述 |
|-----------|---------|-------------|
| **纹理 (Texture)** | clean, grid, organic, pixel, paper | 视觉纹理和背景处理 |
| **氛围 (Mood)** | professional, warm, cool, vibrant, dark, neutral | 色彩温度和调色板风格 |
| **排版 (Typography)** | geometric, humanist, handwritten, editorial, technical | 标题和正文字体样式 |
| **密度 (Density)** | minimal, balanced, dense | 每张幻灯片的信息密度 |

详细规格：`references/dimensions/*.md`

### 自动样式选择

| 内容信号 | 预设 |
|-----------------|--------|
| tutorial, learn, education, guide, beginner | `sketch-notes` |
| classroom, teaching, school, chalkboard | `chalkboard` |
| architecture, system, data, analysis, technical | `blueprint` |
| creative, children, kids, cute | `vector-illustration` |
| briefing, academic, research, bilingual | `intuition-machine` |
| executive, minimal, clean, simple | `minimal` |
| saas, product, dashboard, metrics | `notion` |
| investor, quarterly, business, corporate | `corporate` |
| launch, marketing, keynote, magazine | `bold-editorial` |
| entertainment, music, gaming, atmospheric | `dark-atmospheric` |
| explainer, journalism, science communication | `editorial-infographic` |
| story, fantasy, animation, magical | `fantasy-animation` |
| gaming, retro, pixel, developer | `pixel-art` |
| biology, chemistry, medical, scientific | `scientific` |
| history, heritage, vintage, expedition | `vintage` |
| lifestyle, wellness, travel, artistic | `watercolor` |
| 默认 | `blueprint` |

## 设计理念

幻灯片为**阅读和分享**而设计，而非现场演示：
- 每张幻灯片无需口头解说即可自解释
- 滚动时逻辑流畅
- 所有必要上下文都在每张幻灯片中
- 针对社交媒体分享优化

参见 `references/design-guidelines.md` 了解：
- 受众特定原则
- 视觉层次
- 内容密度指南
- 色彩和排版选择
- 字体推荐

参见 `references/layouts.md` 了解布局选项。

## 文件管理

### 输出目录

```
slide-deck/{topic-slug}/
├── source-{slug}.{ext}
├── outline.md
├── prompts/
│   └── 01-slide-cover.md, 02-slide-{slug}.md, ...
├── 01-slide-cover.png, 02-slide-{slug}.png, ...
├── {topic-slug}.pptx
└── {topic-slug}.pdf
```

**Slug**：提取主题（2-4个词，kebab-case）。例如："Introduction to Machine Learning" → `intro-machine-learning`

**冲突处理**：有关现有内容检测和用户选项，请参见步骤1.3。

## 语言处理

**检测优先级**：
1. `--lang` 标志（显式）
2. EXTEND.md 的 `language` 设置
3. 用户的对话语言（输入语言）
4. 源内容语言

**规则**：所有响应使用用户首选语言：
- 问题和确认
- 进度报告
- 错误消息
- 完成摘要

技术术语（样式名称、文件路径、代码）保持英文。

## 工作流程

复制此清单并在完成时勾选：

```
幻灯片进度：
- [ ] 步骤1：设置与分析
  - [ ] 1.1 加载偏好设置
  - [ ] 1.2 分析内容
  - [ ] 1.3 检查现有内容 ⚠️ 必须执行
- [ ] 步骤2：确认 ⚠️ 必须执行（第1轮，第2轮可选）
- [ ] 步骤3：生成大纲
- [ ] 步骤4：审核大纲（条件）
- [ ] 步骤5：生成提示词
- [ ] 步骤6：审核提示词（条件）
- [ ] 步骤7：生成图片
- [ ] 步骤8：合并为 PPTX/PDF
- [ ] 步骤9：输出摘要
```

### 流程

```
输入 → 偏好设置 → 分析 → [检查现有内容？] → 确认（1-2轮）→ 大纲 → [审核大纲？] → 提示词 → [审核提示词？] → 图片 → 合并 → 完成
```

### 步骤1：设置与分析

**1.1 加载偏好设置（EXTEND.md）**

检查 EXTEND.md 是否存在（优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-slide-deck/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-slide-deck/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-slide-deck/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-slide-deck/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md") { "user" }
```

| 路径 | 位置 |
| .baoyu-skills/baoyu-slide-deck/EXTEND.md | 项目目录 |
| $HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md | 用户主目录 |

**找到 EXTEND.md** → 读取、解析、**向用户输出摘要**：

```
📋 已从 [完整路径] 加载偏好设置
├─ 样式：[预设/自定义名称]
├─ 受众：[受众或"自动检测"]
├─ 语言：[语言或"自动检测"]
└─ 审核：[启用/禁用]
```

**未找到 EXTEND.md** → 首次设置，使用 AskUserQuestion 或使用默认设置继续。

**EXTEND.md 支持**：首选样式 | 自定义维度 | 默认受众 | 语言偏好 | 审核偏好

架构：`references/config/preferences-schema.md`

**1.2 分析内容**

1. 保存源内容（如果是粘贴的，保存为 `source.md`）
   - **备份规则**：如果 `source.md` 存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 按照 `references/analysis-framework.md` 进行内容分析
3. 分析内容信号以获取样式建议
4. 检测源语言
5. 确定推荐的幻灯片数量
6. 从内容生成 topic slug

**1.3 检查现有内容** ⚠️ 必须执行

**在继续步骤2之前必须执行。**

使用 Bash 检查输出目录是否存在：

```bash
test -d "slide-deck/{topic-slug}" && echo "exists"
```

**如果目录存在**，使用 AskUserQuestion：

```
header: "Existing"
question: "Existing content found. How to proceed?"
options:
  - label: "Regenerate outline"
    description: "Keep images, regenerate outline only"
  - label: "Regenerate images"
    description: "Keep outline, regenerate images only"
  - label: "Backup and regenerate"
    description: "Backup to {slug}-backup-{timestamp}, then regenerate all"
  - label: "Exit"
    description: "Cancel, keep existing content unchanged"
```

**保存到 `analysis.md`**，包含：
- 主题、受众、内容信号
- 推荐的样式（基于自动样式选择）
- 推荐的幻灯片数量
- 语言检测

### 步骤2：确认 ⚠️ 必须执行

**两轮确认**：第1轮始终执行，第2轮仅在选择"自定义维度"时执行。

**语言**：使用用户输入语言或保存的语言偏好。

**显示摘要**：
- 已识别的内容类型 + 主题
- 语言：[来自 EXTEND.md 或检测到]
- **推荐的样式**：[预设]（基于内容信号）
- **推荐的幻灯片**：[N]（基于内容长度）

#### 第1轮（始终）

**对所有5个问题使用 AskUserQuestion**：

**问题1：样式**
```
header: "Style"
question: "Which visual style for this deck?"
options:
  - label: "{recommended_preset} (Recommended)"
    description: "Best match based on content analysis"
  - label: "{alternative_preset}"
    description: "[alternative style description]"
  - label: "Custom dimensions"
    description: "Choose texture, mood, typography, density separately"
```

**问题2：受众**
```
header: "Audience"
question: "Who is the primary reader?"
options:
  - label: "General readers (Recommended)"
    description: "Broad appeal, accessible content"
  - label: "Beginners/learners"
    description: "Educational focus, clear explanations"
  - label: "Experts/professionals"
    description: "Technical depth, domain knowledge"
  - label: "Executives"
    description: "High-level insights, minimal detail"
```

**问题3：幻灯片数量**
```
header: "Slides"
question: "How many slides?"
options:
  - label: "{N} slides (Recommended)"
    description: "Based on content length"
  - label: "Fewer ({N-3} slides)"
    description: "More condensed, less detail"
  - label: "More ({N+3} slides)"
    description: "More detailed breakdown"
```

**问题4：审核大纲**
```
header: "Outline"
question: "Review outline before generating prompts?"
options:
  - label: "Yes, review outline (Recommended)"
    description: "Review slide titles and structure"
  - label: "No, skip outline review"
    description: "Proceed directly to prompt generation"
```

**问题5：审核提示词**
```
header: "Prompts"
question: "Review prompts before generating images?"
options:
  - label: "Yes, review prompts (Recommended)"
    description: "Review image generation prompts"
  - label: "No, skip prompt review"
    description: "Proceed directly to image generation"
```

#### 第2轮（仅在选择"自定义维度"时）

**对所有4个维度使用 AskUserQuestion**：

**问题1：纹理**
```
header: "Texture"
question: "Which visual texture?"
options:
  - label: "clean"
    description: "Pure solid color, no texture"
  - label: "grid"
    description: "Subtle grid overlay, technical"
  - label: "organic"
    description: "Soft textures, hand-drawn feel"
  - label: "pixel"
    description: "Chunky pixels, 8-bit aesthetic"
```
（注："paper"可通过 Other 获取）

**问题2：氛围**
```
header: "Mood"
question: "Which color mood?"
options:
  - label: "professional"
    description: "Cool-neutral, navy/gold"
  - label: "warm"
    description: "Earth tones, friendly"
  - label: "cool"
    description: "Blues, grays, analytical"
  - label: "vibrant"
    description: "High saturation, bold"
```
（注："dark"、"neutral"可通过 Other 获取）

**问题3：排版**
```
header: "Typography"
question: "Which typography style?"
options:
  - label: "geometric"
    description: "Modern sans-serif, clean"
  - label: "humanist"
    description: "Friendly, readable"
  - label: "handwritten"
    description: "Marker/brush, organic"
  - label: "editorial"
    description: "Magazine style, dramatic"
```
（注："technical"可通过 Other 获取）

**问题4：密度**
```
header: "Density"
question: "Information density?"
options:
  - label: "balanced (Recommended)"
    description: "2-3 key points per slide"
  - label: "minimal"
    description: "One focus point, maximum whitespace"
  - label: "dense"
    description: "Multiple data points, compact"
```

**第2轮之后**：将自定义维度存储为样式配置。

**确认之后**：
1. 使用确认的偏好设置更新 `analysis.md`
2. 存储来自问题4的 `skip_outline_review` 标志
3. 存储来自问题5的 `skip_prompt_review` 标志
4. → 步骤3

### 步骤3：生成大纲

使用步骤2确认的样式创建大纲。

**样式解析**：
- 如果选择了预设 → 读取 `references/styles/{preset}.md`
- 如果是自定义维度 → 从 `references/dimensions/` 读取维度文件并组合

**生成**：
1. 按照 `references/outline-template.md` 的结构
2. 从样式或维度构建 STYLE_INSTRUCTIONS
3. 应用确认的受众、语言、幻灯片数量
4. 保存为 `outline.md`

**生成之后**：
- 如果是 `--outline-only`，在此停止
- 如果 `skip_outline_review` 为 true → 跳过步骤4，转到步骤5
- 如果 `skip_outline_review` 为 false → 继续步骤4

### 步骤4：审核大纲（条件）

如果用户在步骤2中选择了"No, skip outline review"，则**跳过此步骤**。

**目的**：在生成提示词之前审核大纲结构。

**语言**：使用用户输入语言或保存的语言偏好。

**显示**：
- 总幻灯片数：N
- 样式：[预设名称或"custom: texture+mood+typography+density"]
- 逐幻灯片摘要表：

```
| # | 标题 | 类型 | 布局 |
|---|-------|------|--------|
| 1 | [title] | 封面 | title-hero |
| 2 | [title] | 内容 | [layout] |
| 3 | [title] | 内容 | [layout] |
| ... | ... | ... | ... |
```

**使用 AskUserQuestion**：
```
header: "Confirm"
question: "Ready to generate prompts?"
options:
  - label: "Yes, proceed (Recommended)"
    description: "Generate image prompts"
  - label: "Edit outline first"
    description: "I'll modify outline.md before continuing"
  - label: "Regenerate outline"
    description: "Create new outline with different approach"
```

**响应之后**：
1. 如果"Edit outline first" → 通知用户编辑 `outline.md`，准备好后再次询问
2. 如果"Regenerate outline" → 返回步骤3
3. 如果"Yes, proceed" → 继续步骤5

### 步骤5：生成提示词

1. 读取 `references/base-prompt.md`
2. 对于大纲中的每张幻灯片：
   - 从大纲中提取 STYLE_INSTRUCTIONS（不再从样式文件读取）
   - 添加幻灯片特定内容
   - 如果指定了 `Layout:`，包含来自 `references/layouts.md` 的布局指导
3. 保存到 `prompts/` 目录
   - **备份规则**：如果提示词文件存在，重命名为 `prompts/NN-slide-{slug}-backup-YYYYMMDD-HHMMSS.md`

**生成之后**：
- 如果是 `--prompts-only`，在此停止并输出提示词摘要
- 如果 `skip_prompt_review` 为 true → 跳过步骤6，转到步骤7
- 如果 `skip_prompt_review` 为 false → 继续步骤6

### 步骤6：审核提示词（条件）

如果用户在步骤2中选择了"No, skip prompt review"，则**跳过此步骤**。

**目的**：在图片生成之前审核提示词。

**语言**：使用用户输入语言或保存的语言偏好。

**显示**：
- 总提示词数：N
- 样式：[预设名称或自定义维度]
- 提示词列表：

```
| # | 文件名 | 幻灯片标题 |
|---|----------|-------------|
| 1 | 01-slide-cover.md | [title] |
| 2 | 02-slide-xxx.md | [title] |
| ... | ... | ... |
```

- 提示词目录路径：`prompts/`

**使用 AskUserQuestion**：
```
header: "Confirm"
question: "Ready to generate slide images?"
options:
  - label: "Yes, proceed (Recommended)"
    description: "Generate all slide images"
  - label: "Edit prompts first"
    description: "I'll modify prompts before continuing"
  - label: "Regenerate prompts"
    description: "Create new prompts with different approach"
```

**响应之后**：
1. 如果"Edit prompts first" → 通知用户编辑提示词，准备好后再次询问
2. 如果"Regenerate prompts" → 返回步骤5
3. 如果"Yes, proceed" → 继续步骤7

### 步骤7：生成图片

**对于 `--images-only`**：从这里开始，使用现有提示词。

**对于 `--regenerate N`**：仅重新生成指定的幻灯片。

**标准流程**：
1. 选择可用的图片生成智能体
2. 生成会话 ID：`slides-{topic-slug}-{timestamp}`
3. 对于每张幻灯片：
   - **备份规则**：如果图片文件存在，重命名为 `NN-slide-{slug}-backup-YYYYMMDD-HHMMSS.png`
   - 使用相同的会话 ID 顺序生成图片
4. 报告进度："已生成 X/N"（使用用户语言）
5. 失败时自动重试一次，然后再报告错误

### 步骤8：合并为 PPTX 和 PDF

```bash
${BUN_X} {baseDir}/scripts/merge-to-pptx.ts <slide-deck-dir>
${BUN_X} {baseDir}/scripts/merge-to-pdf.ts <slide-deck-dir>
```

### 步骤9：输出摘要

**语言**：使用用户输入语言或保存的语言偏好。

```
幻灯片完成！

主题：[topic]
样式：[预设名称或自定义维度]
位置：[directory path]
幻灯片：共 N 张

- 01-slide-cover.png - 封面
- 02-slide-intro.png - 内容
- ...
- {NN}-slide-back-cover.png - 封底

大纲：outline.md
PPTX：{topic-slug}.pptx
PDF：{topic-slug}.pdf
```

## 部分工作流程

| 选项 | 工作流程 |
|--------|----------|
| `--outline-only` | 仅步骤1-3（大纲后停止） |
| `--prompts-only` | 步骤1-5（生成提示词，跳过图片） |
| `--images-only` | 跳到步骤7（需要现有 prompts/） |
| `--regenerate N` | 仅重新生成指定幻灯片 |

### 使用 `--prompts-only`

生成大纲和提示词，不含图片：

```bash
/baoyu-slide-deck content.md --prompts-only
```

输出：准备好审核/编辑的 `outline.md` + `prompts/*.md`。

### 使用 `--images-only`

从现有提示词生成图片（从步骤7开始）：

```bash
/baoyu-slide-deck slide-deck/topic-slug/ --images-only
```

前提条件：
- `prompts/` 目录包含幻灯片提示词文件
- `outline.md` 包含样式信息

### 使用 `--regenerate`

重新生成指定幻灯片：

```bash
# 单张幻灯片
/baoyu-slide-deck slide-deck/topic-slug/ --regenerate 3

# 多张幻灯片
/baoyu-slide-deck slide-deck/topic-slug/ --regenerate 2,5,8
```

流程：
1. 读取指定幻灯片的现有提示词
2. 仅重新生成那些幻灯片的图片
3. 重新生成 PPTX/PDF

## 幻灯片修改

### 快速参考

| 操作 | 命令 | 手动步骤 |
|--------|---------|--------------|
| **编辑** | `--regenerate N` | **首先更新提示词文件** → 重新生成图片 → 重新生成 PDF |
| **添加** | 手动 | 创建提示词 → 生成图片 → 重新编号后续 → 更新大纲 → 重新生成 PDF |
| **删除** | 手动 | 删除文件 → 重新编号后续 → 更新大纲 → 重新生成 PDF |

### 编辑单张幻灯片

1. **首先更新** `prompts/NN-slide-{slug}.md` 中的提示词
2. 运行：`/baoyu-slide-deck <dir> --regenerate N`
3. 或手动重新生成图片 + PDF

**重要**：更新幻灯片时，**始终首先**更新提示词文件（`prompts/NN-slide-{slug}.md`），然后再重新生成。这确保更改被记录且可重现。

### 添加新幻灯片

1. 在位置创建提示词：`prompts/NN-slide-{new-slug}.md`
2. 使用相同的会话 ID 生成图片
3. **重新编号**：后续文件 NN+1（slug 不变）
4. 更新 `outline.md`
5. 重新生成 PPTX/PDF

### 删除幻灯片

1. 删除 `NN-slide-{slug}.png` 和 `prompts/NN-slide-{slug}.md`
2. **重新编号**：后续文件 NN-1（slug 不变）
3. 更新 `outline.md`
4. 重新生成 PPTX/PDF

### 文件命名

格式：`NN-slide-[slug].png`
- `NN`：两位数字序列（01, 02, ...）
- `slug`：来自内容的 kebab-case（2-5个词，唯一）

**重新编号规则**：只有 NN 变化，slug 保持不变。

参见 `references/modification-guide.md` 了解完整详情。

## 参考资料

| 文件 | 内容 |
|------|---------|
| `references/analysis-framework.md` | 演示文稿内容分析 |
| `references/outline-template.md` | 大纲结构和格式 |
| `references/modification-guide.md` | 编辑、添加、删除幻灯片工作流程 |
| `references/content-rules.md` | 内容和样式指南 |
| `references/design-guidelines.md` | 受众、排版、色彩、视觉元素 |
| `references/layouts.md` | 布局选项和选择技巧 |
| `references/base-prompt.md` | 图片生成基础提示词 |
| `references/dimensions/*.md` | 维度规格（纹理、氛围、排版、密度） |
| `references/dimensions/presets.md` | 预设 → 维度映射 |
| `references/styles/<style>.md` | 完整样式规格（旧版） |
| `references/config/preferences-schema.md` | EXTEND.md 结构 |

## 注意事项

- 图片生成：每张幻灯片 10-30 秒
- 生成失败时自动重试一次
- 对敏感公众人物使用风格化替代方案
- 通过会话 ID 保持样式一致性
- **必须执行步骤2确认** - 不要跳过（样式、受众、幻灯片、大纲审核、提示词审核）
- **步骤4条件** - 仅在用户在步骤2中请求大纲审核时执行
- **步骤6条件** - 仅在用户在步骤2中请求提示词审核时执行

## 扩展支持

通过 EXTEND.md 进行自定义配置。参见**步骤1.1**了解路径和支持的选项。
