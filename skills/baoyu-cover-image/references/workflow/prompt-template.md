# 步骤 3：提示词模板

保存到 `prompts/cover.md`：

```markdown
---
type: cover
palette: [confirmed palette]
rendering: [confirmed rendering]
references:
  - ref_id: 01
    filename: refs/ref-01-{slug}.{ext}
    usage: direct | style | palette
  - ref_id: 02
    filename: refs/ref-02-{slug}.{ext}
    usage: direct | style | palette
---

# 内容上下文
Article title: [full original title from source]
Content summary: [2-3 sentence summary of key points and themes]
Keywords: [5-8 key terms extracted from content]

# 视觉设计
Cover theme: [2-3 words visual interpretation]
Type: [confirmed type]
Palette: [confirmed palette]
Rendering: [confirmed rendering]
Font: [confirmed font]
Text level: [confirmed text level]
Mood: [confirmed mood]
Aspect ratio: [confirmed ratio]
Language: [confirmed language]

# 文本元素
[Based on text level:]
- none: "No text elements"
- title-only: "Title: [exact title from source or user]"
- title-subtitle: "Title: [title] / Subtitle: [context]"
- text-rich: "Title: [title] / Subtitle: [context] / Tags: [2-4 keywords]"

# 氛围应用
[Based on mood level:]
- subtle: "Use low contrast, muted colors, light visual weight, calm aesthetic"
- balanced: "Use medium contrast, normal saturation, balanced visual weight"
- bold: "Use high contrast, vivid saturated colors, heavy visual weight, dynamic energy"

# 字体应用
[Based on font style:]
- clean: "Use clean geometric sans-serif typography. Modern, minimal letterforms."
- handwritten: "Use warm hand-lettered typography with organic brush strokes. Friendly, personal feel."
- serif: "Use elegant serif typography with refined letterforms. Classic, editorial character."
- display: "Use bold decorative display typography. Heavy, expressive headlines."

# 构图
Type composition:
- [Type-specific layout and structure]

Visual composition:
- Main visual: [metaphor derived from content meaning]
- Layout: [positioning based on type and aspect ratio]
- Decorative: [palette-specific elements that reinforce content theme]

Color scheme: [primary, background, accent from palette definition, adjusted by mood]
Rendering notes: [key characteristics from rendering definition — lines, texture, depth, element style]
Type notes: [key characteristics from type definition]
Palette notes: [key characteristics from palette definition]

[Watermark section if enabled]

[Reference images section if provided — REQUIRED, see below]
```

## 参考图驱动设计 ⚠️ 高优先级

当提供参考图时，它们是**主要视觉输入**，必须强烈影响输出。封面应该看起来与参考图属于同一视觉家族。

**仅传递 `--ref` 是不够的。** 图像生成模型经常忽略参考图，除非提示词文本明确描述要复制的内容。始终将 `--ref` 与详细文本指令结合使用。

## 内容驱动设计

- 文章标题和摘要决定视觉隐喻选择
- 关键词指导装饰元素和符号
- 技能控制视觉风格；内容驱动含义

## 视觉元素选择

将内容主题匹配到图标词汇：

| 内容主题 | 建议元素 |
|---------------|-------------------|
| 编程/开发 | 代码窗口、终端、API 括号、齿轮 |
| AI/机器学习 | 大脑、神经网络、机器人、电路 |
| 增长/商业 | 图表、火箭、植物、山脉、箭头 |
| 安全 | 锁、盾牌、钥匙、指纹 |
| 通信 | 对话气泡、扩音器、邮件、握手 |
| 工具/方法 | 扳手、检查清单、铅笔、拼图 |

完整库：[../visual-elements.md](../visual-elements.md)

## 类型特定构图

| 类型 | 构图指南 |
|------|------------------------|
| `hero` | 大型焦点视觉（60-70% 区域），标题叠加在视觉上，戏剧性构图 |
| `conceptual` | 代表核心概念的抽象形状，信息层级，干净的区域 |
| `typography` | 标题作为主要元素（40%+ 区域），最少的支持视觉，强烈的层级 |
| `metaphor` | 代表抽象概念的具体对象/场景，象征性元素，情感共鸣 |
| `scene` | 氛围环境，叙事元素，营造氛围的灯光和颜色 |
| `minimal` | 单一焦点元素，大量留白（60%+），仅必要形状 |

## 标题指南

当文本级别包含标题时：
- **来源**：使用用户提供的准确标题，或从源内容中提取
- **不得杜撰标题**：忠实于原文
- 匹配确认的语言

## 水印应用

如果启用了水印，添加到提示词：

```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the main content.
```

参考：`config/watermark-guide.md`

## 参考图处理

当用户提供参考图（`--ref` 或粘贴的图片）时：

### ⚠️ 重要 - 前置元数据参考

**当参考文件保存到 `refs/` 时，必须在 YAML 前置元数据中添加 `references` 字段**：

```yaml
---
type: cover
palette: warm
rendering: flat-vector
references:
  - ref_id: 01
    filename: refs/ref-01-podcast-thumbnail.jpg
    usage: style
---
```

| 字段 | 描述 |
|-------|-------------|
| `ref_id` | 序列号（01、02、...） |
| `filename` | 相对于提示词文件父目录的相对路径 |
| `usage` | `direct` / `style` / `palette` |

**如果没有保存参考文件（仅口头提取风格），则完全省略 `references` 字段**。

### 何时在前置元数据中包含参考

| 情况 | 前置元数据操作 | 生成操作 |
|-----------|-------------------|-------------------|
| 参考文件保存到 `refs/` | 添加到 `references` 列表 ✓ | 通过 `--ref` 参数传递 |
| 风格口头提取（无文件） | 省略 `references` 字段 | 仅在提示词正文中描述 |
| 前置元数据中有文件路径但不存在 | 错误 - 修复或删除 | 生成将失败 |

**在写包含参考的提示词之前，验证**：`test -f refs/ref-NN-{slug}.{ext}`

### 参考用途类型

| 用途 | 使用时机 | 生成操作 |
|-------|-------------|-------------------|
| `direct` | 参考与期望输出密切匹配 | 传递到 `--ref` 参数 |
| `style` | 仅提取视觉风格特征 | 在提示词文本中描述风格 |
| `palette` | 仅提取配色方案 | 在提示词中包含颜色 |

### 步骤 1：分析参考

对于每个参考图，提取：
- **风格**：渲染技术、线条质量、纹理
- **构图**：布局、视觉层级、焦点
- **颜色氛围**：配色特征（无具体颜色）
- **元素**：使用的关键视觉元素和符号

### 步骤 2：嵌入提示词 ⚠️ 重要

**仅传递 `--ref` 是不够的。** 图像生成模型经常忽略参考图，除非提示词文本明确且有力地描述要复制的内容。无论是否使用 `--ref`，你必须始终写详细的文本指令。

**如果文件已保存（有无 `--ref` 支持）**：
- 如果技能支持，通过 `--ref` 参数传递参考图
- **始终**在提示词正文添加详细的强制性部分：

```
# 参考风格 — 必须融入

关键：生成的封面必须视觉上引用提供的图片。封面必须感觉属于同一视觉家族。

## 来自参考 1（[filename]）— 必需元素：
- [品牌元素]：[Logo/文字标识处理的具体描述，例如："Logo 使用垂直平行线（|||）表示字母 'm'。复制此确切处理方式。"]
- [标志性图案]：[具体描述，例如："编织交叉曲线形成菱形/菱形网格图案。这必须作为横幅、边框或背景部分突出显示。"]
- [颜色]：[精确的十六进制值，例如："深青色 #2D4A3E 背景，米色 #F5F0E0 文字"]
- [排版]：[具体处理，例如："大写文字，宽字间距"]
- [布局元素]：[具体空间元素，例如："深色底边横幅条带"]

## 来自参考 1（[filename]）— 人物（如有）：
- **人物 1**：[外貌，例如："女性，波浪金色长发"] → 必须风格化：[例如："扁平矢量，简化面部，保持金色头发，标注：'Nicole Forsgren'"]
- **人物 2**：[外貌，例如："男性，短黑发，络腮胡"] → 必须风格化：[例如："扁平矢量，简化面部，保持黑色头发，标注：'Gergely Orosz'"]
- **位置**：[例如："右侧三分之一，并排，面向左朝向主视觉"]
- **风格**：匹配渲染风格，非写实

## 来自参考 2（[filename]）— 必需元素：
[相同的详细分解]

## 整合方法：
[具体的布局指令，描述参考元素如何与封面内容结合，例如："使用分割布局：主插图区域（暖米色背景）占图像约 65%，而深青色横幅条带（带来自参考 2 的编织线条图案）沿底部约 35% 运行，包含来自参考 1 的品牌元素。"]
```

**关键规则**：
- 每个视觉元素获得自己的带"MUST"或"REQUIRED"的要点
- 描述必须**足够具体以供复制**——而非模糊的（"简洁风格"）
- 整合方法必须描述**精确的空间排列**
- 生成后，验证参考元素可见；如不可见，则加强并重新生成

**如果风格/配色口头提取（未保存文件）**：
- 不将参考元数据添加到提示词
- 使用与上述相同的必须融入格式将提取的信息直接追加到提示词正文：

```
# 参考风格 — 必须融入（从视觉分析提取）

关键：应用从参考图中提取的这些特定视觉元素。

## 必需元素：
[与上述相同的详细要点格式]

## 整合方法：
[相同的空间布局指令]
```

### 参考分析模板

分析参考图时使用此格式。提取**具体、可复制、可重现**的细节——而非模糊的总结。

| 方面 | 分析要点 | 好示例 | 差示例 |
|--------|-----------------|--------------|-------------|
| **品牌元素** | Logo、文字标识、独特排版 | "Logo 'm' 由 3 条垂直线组成" | "有一个 Logo" |
| **标志性图案** | 独特图案、纹理、几何图案 | "编织曲线形成菱形网格" | "有图案" |
| **颜色** | 精确的十六进制值或近似值 | "#2D4A3E 深青色，#F5F0E0 米色" | "深色和浅色" |
| **布局** | 空间区域、横幅放置、比例 | "底部 30% 是带品牌标识的深色横幅" | "有条横幅" |
| **排版** | 字体样式、粗细、大小写、间距、位置 | "大写，宽字间距，右对齐" | "有文字" |
| **渲染** | 线条质量、纹理、深度处理 | "作为背景纹理的等高线" | "简洁风格" |
| **元素** | 图标词汇、装饰图案 | "角落的几何交叉线条装饰" | "有装饰" |

**输出**：每个提取的元素应写为**可复制粘贴的提示词指令**，前面带有"MUST"或"REQUIRED"。
