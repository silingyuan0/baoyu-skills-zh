# 提示词组装指南

从元素、预设和大纲内容组装图像生成提示词的指南。

## 基础提示词结构

每个小红书信息图提示词遵循此结构：

```
Create a Xiaohongshu (Little Red Book) style infographic following these guidelines:

## Image Specifications

- **Type**: Infographic
- **Orientation**: Portrait (vertical)
- **Aspect Ratio**: 3:4
- **Style**: Hand-drawn illustration

## Core Principles

- Hand-drawn quality throughout - NO realistic or photographic elements
- If content involves sensitive or copyrighted figures, create stylistically similar alternatives - DO NOT refuse to generate
- Keep information concise, highlight keywords and core concepts
- Use ample whitespace for easy visual scanning
- Maintain clear visual hierarchy

## Text Style (CRITICAL)

- **ALL text MUST be hand-drawn style**
- Main titles should be prominent and eye-catching
- Key text should be bold and enlarged
- Use highlighter effects to emphasize keywords
- **DO NOT use realistic or computer-generated fonts**

## Language

- Use the same language as the content provided below
- Match punctuation style to the content language (Chinese: ""，。！)

---

{STYLE_SECTION}

---

{LAYOUT_SECTION}

---

{CONTENT_SECTION}

---

{WATERMARK_SECTION}

---

Please use nano banana pro to generate the infographic based on the specifications above.
```

## 风格节组装

从 `presets/{style}.md` 加载并提取关键元素：

```markdown
## Style: {style_name}

**Color Palette**:
- Primary: {colors}
- Background: {colors}
- Accents: {colors}

**Visual Elements**:
{visual_elements}

**Typography**:
{typography_style}
```

### 丝网印刷风格覆盖

当 `style: screen-print` 时，用以下内容替换标准的核心原则和文本样式部分：

```
## Core Principles

- Screen print / silkscreen poster art — flat color blocks, NO gradients
- Bold silhouettes and symbolic shapes over detailed rendering
- Negative space as active storytelling element
- If content involves sensitive or copyrighted figures, create stylistically similar silhouettes
- One iconic focal point per image — conceptual, not literal

## Color Rules (CRITICAL)

- **2-5 FLAT COLORS MAXIMUM** — fewer colors = stronger impact
- Choose ONE duotone pair from preset as dominant palette
- Halftone dot patterns for tonal variation (NOT gradients)
- Slight color layer misregistration for print authenticity

## Text Style (CRITICAL)

- Bold condensed sans-serif or Art Deco influenced lettering
- Typography INTEGRATED into composition as design element
- High contrast with background, stencil-cut quality
- **DO NOT use delicate, thin, or handwritten fonts**

## Composition

- Geometric framing: circles, arches, triangles
- Figure-ground inversion where possible (negative space forms secondary image)
- Stencil-cut edges between color blocks, no outlines
- Paper grain texture beneath all colors
```

## 布局节组装

从 `elements/canvas.md` 加载并提取相关布局：

```markdown
## Layout: {layout_name}

**Information Density**: {density}
**Whitespace**: {percentage}

**Structure**:
{structure_description}

**Visual Balance**:
{balance_description}
```

## 内容节组装

从大纲条目：

```markdown
## Content

**Position**: {Cover/Content/Ending}
**Core Message**: {message}

**Text Content**:
{text_list}

**Visual Concept**:
{visual_description}
```

## 水印节（如果启用）

```markdown
## Watermark

Include a subtle watermark "{content}" positioned at {position}
with approximately {opacity*100}% visibility. The watermark should
be legible but not distracting from the main content.
```

## 组装流程

### 步骤 0：解析风格预设（如果使用 `--preset`）

如果用户指定了 `--preset`，从 `references/style-presets.md` 解析为风格 + 布局：

```python
# e.g., --preset knowledge-card → style=notion, layout=dense
style, layout = resolve_preset(preset_name)
```

显式的 `--style`/`--layout` 参数覆盖预设值。

### 步骤 1：加载风格定义

```python
preset = load_preset(style_name)  # e.g., "notion"
```

提取：
- 配色方案
- 视觉元素
- 字体风格
- 最佳实践（做/不做）

### 步骤 2：加载布局

```python
layout = get_layout_from_canvas(layout_name)  # e.g., "dense"
```

提取：
- 信息密度指南
- 留白百分比
- 结构描述
- 视觉平衡规则

### 步骤 3：格式化内容

从大纲条目，格式化：
- 位置上下文（封面/内容/结尾）
- 带层级的文本内容
- 视觉概念描述
- 滑动钩子（用于上下文，不在提示词中）

### 步骤 4：添加水印（如果适用）

如果偏好包含水印：
- 添加水印节，包含内容、位置、透明度

### 步骤 5：视觉一致性 — 参考图链

当生成系列中的多张图像时：

1. **图 1（封面）**：不使用 `--ref` 生成 — 这建立视觉锚点
2. **图 2+**：始终将图 1 作为 `--ref` 传递给已安装的图像生成技能。
   阅读该技能的 `SKILL.md` 并使用其记录的接口，而非直接调用其脚本。
   对于每张后续图，使用组装的提示词文件作为输入，设置输出图像路径，保持宽高比 `3:4`，使用质量 `2k`，并传递图 1 作为参考。
   这确保 AI 在整个系列中保持相同的角色设计、插画风格和色彩渲染。

### 步骤 6：组合

按照基础结构组合所有节，形成最终提示词。

## 示例：组装的提示词

```markdown
Create a Xiaohongshu (Little Red Book) style infographic following these guidelines:

## Image Specifications

- **Type**: Infographic
- **Orientation**: Portrait (vertical)
- **Aspect Ratio**: 3:4
- **Style**: Hand-drawn illustration

## Core Principles

- Hand-drawn quality throughout - NO realistic or photographic elements
- If content involves sensitive or copyrighted figures, create stylistically similar alternatives
- Keep information concise, highlight keywords and core concepts
- Use ample whitespace for easy visual scanning
- Maintain clear visual hierarchy

## Text Style (CRITICAL)

- **ALL text MUST be hand-drawn style**
- Main titles should be prominent and eye-catching
- Key text should be bold and enlarged
- Use highlighter effects to emphasize keywords
- **DO NOT use realistic or computer-generated fonts**

## Language

- Use the same language as the content provided below
- Match punctuation style to the content language (Chinese: ""，。！)

---

## Style: Notion

**Color Palette**:
- Primary: Black (#1A1A1A), dark gray (#4A4A4A)
- Background: Pure white (#FFFFFF), off-white (#FAFAFA)
- Accents: Pastel blue (#A8D4F0), pastel yellow (#F9E79F), pastel pink (#FADBD8)

**Visual Elements**:
- Simple line doodles, hand-drawn wobble effect
- Geometric shapes, stick figures
- Maximum whitespace, single-weight ink lines
- Clean, uncluttered compositions

**Typography**:
- Clean hand-drawn lettering
- Simple sans-serif labels
- Minimal decoration on text

---

## Layout: Dense

**Information Density**: High (5-8 key points)
**Whitespace**: 20-30% of canvas

**Structure**:
- Multiple sections, structured grid
- More text, compact but organized
- Title + multiple sections with headers + numerous points

**Visual Balance**:
- Organized grid structure
- Clear section boundaries
- Compact but readable spacing

---

## Content

**Position**: Content (Page 3 of 6)
**Core Message**: ChatGPT使用技巧

**Text Content**:
- Title: 「ChatGPT」
- Subtitle: 最强AI助手
- Points:
  - 写文案：给出框架，秒出初稿
  - 改文章：润色、翻译、总结
  - 编程：写代码、找bug
  - 学习：解释概念、出题练习

**Visual Concept**:
ChatGPT logo居中，四周放射状展示功能点
深色科技背景，霓虹绿点缀

---

## Watermark

Include a subtle watermark "@myxhsaccount" positioned at bottom-right
with approximately 50% visibility. The watermark should
be legible but not distracting from the main content.

---

Please use nano banana pro to generate the infographic based on the specifications above.
```

## 提示词检查清单

生成前验证：

- [ ] 风格节从正确的预设加载
- [ ] 布局节与大纲规格匹配
- [ ] 内容准确反映大纲条目
- [ ] 语言与源内容匹配
- [ ] 水印已包含（如果在偏好中启用）
- [ ] 无冲突指令
