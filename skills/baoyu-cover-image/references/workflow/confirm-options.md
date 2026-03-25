# 步骤 2：确认选项

## 目的

校验所有 6 个维度 + 宽高比。

## 跳过条件

| 条件 | 跳过的提问 | 仍需提问 |
|-----------|-------------------|-------------|
| `--quick` 标志 | 类型、配色、渲染方式、文本、氛围、字体 | **宽高比**（除非指定了 `--aspect`） |
| 所有 6 个维度 + `--aspect` 已指定 | 全部 | 无 |
| EXTEND.md 中 `quick_mode: true` | 类型、配色、渲染方式、文本、氛围、字体 | **宽高比**（除非指定了 `--aspect`） |
| 其他情况 | 无 | 全部 7 个问题 |

**重要**：除非通过 `--aspect` CLI 标志明确指定，否则宽高比始终会被询问。EXTEND.md 中的用户预设显示为推荐选项，而非自动选择。

## 快速模式输出

跳过 6 个维度时：

```
快速模式：自动选择的维度
• 类型：[类型]（[原因]）
• 配色：[配色]（[原因]）
• 渲染方式：[渲染]（[原因]）
• 文本：[文本]（[原因]）
• 氛围：[氛围]（[原因]）
• 字体：[字体]（[原因]）

[然后提问问题 7：宽高比]
```

## 确认流程

**语言**：自动确定（用户输入语言 > 保存的偏好 > 源语言）。无需询问。

在**单次 AskUserQuestion 调用**中呈现所有选项（最多 4 个问题）。

对于已通过 CLI 标志或 `--style` 预设指定维度的任何问题予以跳过。

### Q1：类型（如果指定了 `--type` 则跳过）

```yaml
header: "Type"
question: "Which cover type?"
multiSelect: false
options:
  - label: "[auto-recommended type] (推荐)"
    description: "[reason based on content signals]"
  - label: "hero"
    description: "Large visual impact, title overlay - product launch, announcements"
  - label: "conceptual"
    description: "Concept visualization - technical, architecture"
  - label: "typography"
    description: "Text-focused layout - opinions, quotes"
```

### Q2：配色（如果指定了 `--palette` 或 `--style` 则跳过）

```yaml
header: "Palette"
question: "Which color palette?"
multiSelect: false
options:
  - label: "[auto-recommended palette] (推荐)"
    description: "[reason based on content signals]"
  - label: "warm"
    description: "Friendly - orange, golden yellow, terracotta"
  - label: "elegant"
    description: "Sophisticated - soft coral, muted teal, dusty rose"
  - label: "cool"
    description: "Technical - engineering blue, navy, cyan"
```

### Q3：渲染方式（如果指定了 `--rendering` 或 `--style` 则跳过）

显示兼容的渲染方式（兼容性矩阵中 ✓✓ 优先）：

```yaml
header: "Rendering"
question: "Which rendering style?"
multiSelect: false
options:
  - label: "[best compatible rendering] (推荐)"
    description: "[reason based on palette + type + content]"
  - label: "flat-vector"
    description: "Clean outlines, flat fills, geometric icons"
  - label: "hand-drawn"
    description: "Sketchy, organic, imperfect strokes"
  - label: "digital"
    description: "Polished, precise, subtle gradients"
```

### Q4：字体（如果指定了 `--font` 则跳过）

```yaml
header: "Font"
question: "Which font style?"
multiSelect: false
options:
  - label: "[auto-recommended font] (推荐)"
    description: "[reason based on content signals]"
  - label: "clean"
    description: "Modern geometric sans-serif - tech, professional"
  - label: "handwritten"
    description: "Warm hand-lettered - personal, friendly"
  - label: "serif"
    description: "Classic elegant - editorial, luxury"
  - label: "display"
    description: "Bold decorative - announcements, entertainment"
```

### Q5：其他设置（如果所有剩余维度已指定则跳过）

将剩余设置合并为一个问题。包括：输出目录（如果没有偏好 + 文件路径输入）、文本、氛围、宽高比。显示自动选择的值作为推荐选项。用户可以通过"其他"接受全部或输入调整。

**当需要询问输出目录时**（无 `default_output_dir` 偏好 + 文件路径输入）：

```yaml
header: "Settings"
question: "Output / Text / Mood / Aspect?"
multiSelect: false
options:
  - label: "imgs/ / [auto-text] / [auto-mood] / [preset-aspect] (推荐)"
    description: "{article-dir}/imgs/, [text reason], [mood reason], [aspect source]"
  - label: "same-dir / [auto-text] / [auto-mood] / [preset-aspect]"
    description: "{article-dir}/, same directory as article"
  - label: "independent / [auto-text] / [auto-mood] / [preset-aspect]"
    description: "cover-image/{topic-slug}/, separate from article"
```

**当输出目录已设置时**（存在偏好或粘贴内容）：

```yaml
header: "Settings"
question: "Text / Mood / Aspect?"
multiSelect: false
options:
  - label: "[auto-text] / [auto-mood] / [preset-aspect] (推荐)"
    description: "Auto-selected: [text reason], [mood reason], [aspect source]"
  - label: "[auto-text] / bold / [preset-aspect]"
    description: "High contrast, vivid — matches [content signal]"
  - label: "[auto-text] / subtle / [preset-aspect]"
    description: "Low contrast, muted — calm, professional"
```

*注*："其他"（自动添加）允许输入自定义组合。解析用 `/` 分隔的值以匹配问题格式。

## 响应后

使用确认的维度继续步骤 3。
