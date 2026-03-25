# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**⛔ 阻塞操作**：此设置必须完成才能执行任何其他工作流步骤。不得：
- 询问关于参考图
- 询问关于内容/文章
- 询问关于尺寸（类型、配色、渲染方式）
- 继续进行内容分析

仅询问此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
No EXTEND.md found
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ (all questions)     │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ Create EXTEND.md    │
└─────────────────────┘
        │
        ▼
    Continue to Step 1
```

## 问题

**语言**：使用用户的输入语言或保存的语言偏好。

使用 AskUserQuestion 在一次调用中提出所有问题：

### 问题 1：水印

```yaml
header: "Watermark"
question: "Watermark text for generated cover images?"
options:
  - label: "No watermark (Recommended)"
    description: "Clean covers, can enable later in EXTEND.md"
```

### 问题 2：首选类型

```yaml
header: "Type"
question: "Default cover type preference?"
options:
  - label: "Auto-select (Recommended)"
    description: "Choose based on content analysis each time"
  - label: "hero"
    description: "Large visual impact - product launch, announcements"
  - label: "conceptual"
    description: "Concept visualization - technical, architecture"
```

### 问题 3：首选配色

```yaml
header: "Palette"
question: "Default color palette preference?"
options:
  - label: "Auto-select (Recommended)"
    description: "Choose based on content analysis each time"
  - label: "elegant"
    description: "Sophisticated - soft coral, muted teal, dusty rose"
  - label: "warm"
    description: "Friendly - orange, golden yellow, terracotta"
  - label: "cool"
    description: "Technical - engineering blue, navy, cyan"
```

### 问题 4：首选渲染方式

```yaml
header: "Rendering"
question: "Default rendering style preference?"
options:
  - label: "Auto-select (Recommended)"
    description: "Choose based on content analysis each time"
  - label: "hand-drawn"
    description: "Sketchy organic illustration with personal touch"
  - label: "flat-vector"
    description: "Clean modern vector with geometric shapes"
  - label: "digital"
    description: "Polished precise digital illustration"
```

### 问题 5：默认宽高比

```yaml
header: "Aspect"
question: "Default aspect ratio for cover images?"
options:
  - label: "16:9 (Recommended)"
    description: "Standard widescreen - YouTube, presentations, versatile"
  - label: "2.35:1"
    description: "Cinematic widescreen - article headers, blog posts"
  - label: "1:1"
    description: "Square - Instagram, WeChat, social cards"
  - label: "3:4"
    description: "Portrait - Xiaohongshu, Pinterest, mobile content"
```

注：更多比例（4:3、3:2）在生成时可用。这设置默认推荐。

### 问题 6：默认输出目录

```yaml
header: "Output"
question: "Default output directory for cover images?"
options:
  - label: "Independent (Recommended)"
    description: "cover-image/{topic-slug}/ - separate from article"
  - label: "Same directory"
    description: "{article-dir}/ - alongside the article file"
  - label: "imgs subdirectory"
    description: "{article-dir}/imgs/ - images folder near article"
```

### 问题 7：快速模式

```yaml
header: "Quick"
question: "Enable quick mode by default?"
options:
  - label: "No (Recommended)"
    description: "Confirm dimension choices each time"
  - label: "Yes"
    description: "Skip confirmation, use auto-selection"
```

### 问题 8：保存位置

```yaml
header: "Save"
question: "Where to save preferences?"
options:
  - label: "Project (Recommended)"
    description: ".baoyu-skills/ (this project only)"
  - label: "User"
    description: "~/.baoyu-skills/ (all projects)"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-cover-image/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-cover-image/EXTEND.md` | 所有项目 |

## 设置后

1. 如需要则创建目录
2. 使用前置元数据写入 EXTEND.md
3. 确认："偏好设置已保存到 [path]"
4. 继续步骤 1

## EXTEND.md 模板

```yaml
---
version: 3
watermark:
  enabled: [true/false]
  content: "[user input or empty]"
  position: bottom-right
  opacity: 0.7
preferred_type: [selected type or null]
preferred_palette: [selected palette or null]
preferred_rendering: [selected rendering or null]
preferred_text: title-only
preferred_mood: balanced
default_aspect: [16:9/2.35:1/1:1/3:4]
default_output_dir: [independent/same-dir/imgs-subdir]
quick_mode: [true/false]
language: null
custom_palettes: []
---
```

## 之后修改偏好设置

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 以触发设置
- 编辑 YAML 前置元数据进行快速更改
- 完整 schema：`preferences-schema.md`

**EXTEND.md 支持**：水印 | 首选类型 | 首选配色 | 首选渲染方式 | 首选文本 | 首选氛围 | 默认宽高比 | 默认输出目录 | 快速模式 | 自定义配色定义 | 语言偏好
