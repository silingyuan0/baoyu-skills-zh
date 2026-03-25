# 提示词构建

## 提示词文件格式

每个提示词文件使用 YAML 前置元数据 + 正文内容：

```yaml
---
illustration_id: 01
type: infographic
style: blueprint
references:                    # ⚠️ 仅当 references/ 目录中实际存在文件时
  - ref_id: 01
    filename: 01-ref-diagram.png
    usage: direct              # direct | style | palette
---

[类型专用模板内容如下...]
```

**⚠️ 关键 — 何时包含 `references` 字段**：

| 情况 | 操作 |
|------|------|
| 引用文件已保存至 `references/` | 在前置元数据中包含 ✓ |
| 风格以文字描述提取（无文件） | 不包含在前置元数据中，改为追加到提示词正文 |
| 前置元数据中有文件路径但文件不存在 | 错误 — 移除 references 字段 |

**引用使用类型**（仅当文件存在时）：

| 使用类型 | 描述 | 生成操作 |
|---------|------|---------|
| `direct` | 主要视觉参考 | 传递至 `--ref` 参数 |
| `style` | 仅提取风格特征 | 在提示词文本中描述风格 |
| `palette` | 提取配色方案 | 在提示词中包含颜色信息 |

**若无引用文件但以文字方式提取了风格/配色**，直接追加到提示词正文：
```
COLORS (from reference):
- Primary: #E8756D coral
- Secondary: #7ECFC0 mint
...

STYLE (from reference):
- Clean lines, minimal shadows
- Gradient backgrounds
...
```

---

## 默认构图要求

**默认应用于所有提示词**：

| 要求 | 描述 |
|------|------|
| **简洁构图** | 简洁布局，无视觉杂乱 |
| **留白充足** | 宽裕的边距，元素周围留有呼吸空间 |
| **无复杂背景** | 仅使用纯色或柔和渐变，避免繁杂纹理 |
| **居中或按内容适配** | 主要视觉元素居中或按内容需求定位 |
| **匹配的图形元素** | 使用与内容主题一致的图形元素 |
| **突出核心信息** | 留白引导视线聚焦关键信息 |

**添加到所有提示词**：
> Clean composition with generous white space. Simple or no background. Main elements centered or positioned by content needs.

---

## 人物渲染

当绘制人物时：

| 指南 | 描述 |
|------|------|
| **风格** | 简化卡通剪影或符号化表达 |
| **避免** | 写实人物描绘、精细面部 |
| **多样性** | 展示多人时体型多样化 |
| **情感** | 通过姿态和简单手势表达 |

**添加到所有含有人物的提示词**：
> Human figures: simplified stylized silhouettes or symbolic representations, not photorealistic.

---

## 插图中的文字

| 元素 | 指南 |
|------|------|
| **大小** | 大号、醒目、一目了然 |
| **风格** | 优先使用手写字体增加亲和力 |
| **内容** | 仅包含简洁的关键词和核心概念 |
| **语言** | 与文章语言一致 |

**添加到含文字的提示词**：
> Text should be large and prominent with handwritten-style fonts. Keep minimal, focus on keywords.

---

## 原则

好的提示词必须包含：

1. **布局结构优先**：描述构图、分区、流向
2. **具体数据/标签**：使用文章中的实际数字和术语
3. **视觉关系**：元素之间如何连接
4. **语义化配色**：基于含义选择颜色（红色=警告、绿色=高效）
5. **风格特征**：线条处理、纹理、氛围
6. **宽高比**：以比例和复杂度级别结尾

## 类型专用模板

### 信息图

```
[标题] - Data Visualization

Layout: [grid/radial/hierarchical]

ZONES:
- Zone 1: [data point with specific values]
- Zone 2: [comparison with metrics]
- Zone 3: [summary/conclusion]

LABELS: [specific numbers, percentages, terms from article]
COLORS: [semantic color mapping]
STYLE: [style characteristics]
ASPECT: 16:9
```

**信息图 + vector-illustration**：
```
Flat vector illustration infographic. Clean black outlines on all elements.
COLORS: Cream background (#F5F0E6), Coral Red (#E07A5F), Mint Green (#81B29A), Mustard Yellow (#F2CC8F)
ELEMENTS: Geometric simplified icons, no gradients, playful decorative elements (dots, stars)
```

### 场景

```
[标题] - Atmospheric Scene

FOCAL POINT: [main subject]
ATMOSPHERE: [lighting, mood, environment]
MOOD: [emotion to convey]
COLOR TEMPERATURE: [warm/cool/neutral]
STYLE: [style characteristics]
ASPECT: 16:9
```

### 流程图

```
[标题] - Process Flow

Layout: [left-right/top-down/circular]

STEPS:
1. [Step name] - [brief description]
2. [Step name] - [brief description]
...

CONNECTIONS: [arrow types, decision points]
STYLE: [style characteristics]
ASPECT: 16:9
```

**流程图 + vector-illustration**：
```
Flat vector flowchart with bold arrows and geometric step containers.
COLORS: Cream background (#F5F0E6), steps in Coral/Mint/Mustard, black outlines
ELEMENTS: Rounded rectangles, thick arrows, simple icons per step
```

### 对比

```
[标题] - Comparison View

LEFT SIDE - [Option A]:
- [Point 1]
- [Point 2]

RIGHT SIDE - [Option B]:
- [Point 1]
- [Point 2]

DIVIDER: [visual separator]
STYLE: [style characteristics]
ASPECT: 16:9
```

**对比 + vector-illustration**：
```
Flat vector comparison with split layout. Clear visual separation.
COLORS: Left side Coral (#E07A5F), Right side Mint (#81B29A), cream background
ELEMENTS: Bold icons, black outlines, centered divider line
```

### 框架

```
[标题] - Conceptual Framework

STRUCTURE: [hierarchical/network/matrix]

NODES:
- [Concept 1] - [role]
- [Concept 2] - [role]

RELATIONSHIPS: [how nodes connect]
STYLE: [style characteristics]
ASPECT: 16:9
```

**框架 + vector-illustration**：
```
Flat vector framework diagram with geometric nodes and bold connectors.
COLORS: Cream background (#F5F0E6), nodes in Coral/Mint/Mustard/Blue, black outlines
ELEMENTS: Rounded rectangles or circles for nodes, thick connecting lines
```

### 时间线

```
[标题] - Chronological View

DIRECTION: [horizontal/vertical]

EVENTS:
- [Date/Period 1]: [milestone]
- [Date/Period 2]: [milestone]

MARKERS: [visual indicators]
STYLE: [style characteristics]
ASPECT: 16:9
```

### 丝网印刷风格覆盖

当 `style: screen-print` 时，将标准风格指令替换为：

```
Screen print / silkscreen poster art. Flat color blocks, NO gradients.
COLORS: 2-5 colors maximum. [Choose from style palette or duotone pair]
TEXTURE: Halftone dot patterns, slight color layer misregistration, paper grain
COMPOSITION: Bold silhouettes, geometric framing, negative space as storytelling element
FIGURES: Silhouettes only, no detailed faces, stencil-cut edges
TYPOGRAPHY: Bold condensed sans-serif integrated into composition (not overlaid)
```

**场景 + screen-print**：
```
Conceptual poster scene. Single symbolic focal point, NOT literal illustration.
COLORS: Duotone pair (e.g., Burnt Orange #E8751A + Deep Teal #0A6E6E) on Off-Black #121212
COMPOSITION: Centered silhouette or geometric frame, 60%+ negative space
TEXTURE: Halftone dots, paper grain, slight print misregistration
```

**对比 + screen-print**：
```
Split poster composition. Each side dominated by one color from duotone pair.
LEFT: [Color A] side with silhouette/icon for [Option A]
RIGHT: [Color B] side with silhouette/icon for [Option B]
DIVIDER: Geometric shape or negative space boundary
TEXTURE: Halftone transitions between sides
```

---

## 应避免的做法

- 模糊描述（"a nice image"）
- 字面比喻插图
- 缺少具体标签/注释
- 通用装饰元素

## 水印集成

如在偏好设置中启用水印，追加以下内容：

```
Include a subtle watermark "[content]" positioned at [position] with approximately [opacity*100]% visibility.
```
