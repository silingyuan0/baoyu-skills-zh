# 大纲模板

带样式说明的幻灯片大纲标准结构。

## 大纲格式

```markdown
# 幻灯片大纲

**主题**：[topic description]
**样式**：[preset name OR "custom"]
**维度**：[texture] + [mood] + [typography] + [density]
**受众**：[target audience]
**语言**：[output language]
**幻灯片数量**：N slides
**生成时间**：YYYY-MM-DD HH:mm

---

<STYLE_INSTRUCTIONS>
设计美学：[结合维度特征的2-3句描述]

背景：
  纹理：[from texture dimension]
  基色：[from mood dimension palette]

排版：
  标题：[from typography dimension - 描述视觉外观]
  正文：[from typography dimension - 描述视觉外观]

色彩调色板：
  主文字：[Name] ([Hex]) - [usage]
  背景：[Name] ([Hex]) - [usage]
  强调1：[Name] ([Hex]) - [usage]
  强调2：[Name] ([Hex]) - [usage]

视觉元素：
  - [来自纹理 + 氛围组合的元素1]
  - [带渲染指导的元素2]
  - ...

密度指南：
  - 每张幻灯片内容：[from density dimension]
  - 留白：[from density dimension]

样式规则：
  做：[来自维度组合的指南]
  不做：[来自维度组合的反模式]
</STYLE_INSTRUCTIONS>

---

[幻灯片条目 follow...]
```

## 从维度构建 STYLE_INSTRUCTIONS

使用自定义维度或预设时，通过组合以下内容构建 STYLE_INSTRUCTIONS：

### 1. 设计美学

将所有四个维度的特征组合成2-3句话：

| 纹理 | 贡献 |
|---------|--------------|
| clean | "干净、数字精度、锐利边缘" |
| grid | "技术网格叠加，工程精度" |
| organic | "手绘感、柔和纹理" |
| pixel | "块状像素美学、8位魅力" |
| paper | "年旧纸张纹理、复古特色" |

| 氛围 | 贡献 |
|------|--------------|
| professional | "专业海军蓝和金色调色板" |
| warm | "温暖大地色调，创造平易近人氛围" |
| cool | "冷调分析蓝和灰色" |
| vibrant | "粗体、高饱和度色彩，充满能量" |
| dark | "深色电影背景，带发光强调" |
| neutral | "最小灰度精致感" |

### 2. 背景

来自 `references/dimensions/texture.md`：
- 纹理描述
- 来自氛围调色板的基色

### 3. 排版

来自 `references/dimensions/typography.md`：
- 标题视觉描述（不是字体名称）
- 正文字视觉描述（不是字体名称）

**重要**：为图片生成描述外观："粗体几何无衬线，完美的圆形 O 形状"而不是"Inter 字体"。

### 4. 色彩调色板

来自 `references/dimensions/mood.md`：
- 为所选氛围复制调色板规格
- 包含十六进制代码和用法说明

### 5. 视觉元素

组合纹理和氛围特征：

| 组合 | 视觉元素 |
|-------------|-----------------|
| clean + professional | 干净图表、带轮廓的图标、结构化网格 |
| grid + cool | 技术原理图、尺寸线、蓝图 |
| organic + warm | 手绘图标、brush 笔触、涂鸦 |
| pixel + vibrant | 像素艺术图标、复古游戏元素 |
| paper + warm | 复古印章、老化元素、棕褐色叠加 |

### 6. 密度指南

来自 `references/dimensions/density.md`：
- 每张幻灯片内容限制
- 留白要求
- 元素数量指南

### 7. 样式规则

组合维度特定规则：

**按纹理的做规则**：
- clean：保持锐利边缘，使用网格对齐
- grid：显示精确测量，使用技术图表
- organic：允许不完美，微妙叠加分层
- pixel：保持锯齿状边缘，使用块状元素
- paper：添加微妙老化效果，使用暖色调

**按纹理的不做规则**：
- clean：不要使用手绘元素
- grid：不要使用有机曲线
- organic：不要使用完美几何
- pixel：不要平滑边缘
- paper：不要使用明亮的数字颜色

## 封面幻灯片模板

```markdown
## 幻灯片 1 of N

**类型**：封面
**文件名**：01-slide-cover.png

// 叙事目标
[这个幻灯片在故事弧线中实现什么]

// 关键内容
标题：[main title]
副标题：[supporting tagline]

// 视觉
[详细视觉描述 - 特定元素、构图、氛围]

// 布局
布局：[optional: layout name from gallery, e.g., title-hero]
[构图、层次、空间排列]
```

## 内容幻灯片模板

```markdown
## 幻灯片 X of N

**类型**：内容
**文件名**：{NN}-slide-{slug}.png

// 叙事目标
[这个幻灯片在故事弧线中实现什么]

// 关键内容
标题：[main message - narrative, not label]
副标题：[supporting context]
正文：
- [带具体细节的要点1]
- [带具体细节的要点2]
- [带具体细节的要点3]

// 视觉
[详细视觉描述]

// 布局
布局：[optional: layout name from gallery]
[构图、层次、空间排列]
```

## 封底幻灯片模板

```markdown
## 幻灯片 N of N

**类型**：封底
**文件名**：{NN}-slide-back-cover.png

// 叙事目标
[有意义的结尾 - 不仅仅是"谢谢"]

// 关键内容
标题：[令人难忘的结尾声明或行动号召]
正文：[可选的摘要要点或后续步骤]

// 视觉
[强化核心信息的视觉]

// 布局
布局：[optional: layout name from gallery]
[干净、有影响力的构图]
```

## STYLE_INSTRUCTIONS 块

`<STYLE_INSTRUCTIONS>` 块是本大纲中样式信息的**唯一真实来源**。

| 部分 | 内容 | 来源 |
|---------|---------|--------|
| 设计美学 | 整体视觉方向 | 从所有维度组合 |
| 背景 | 基色和纹理细节 | 纹理 + 氛围维度 |
| 排版 | 字体描述（视觉，而非名称）| 排版维度 |
| 色彩调色板 | 带十六进制代码和用法的命名颜色 | 氛围维度 |
| 视觉元素 | 带渲染指导的图形元素 | 纹理 + 氛围维度 |
| 密度指南 | 内容限制和留白 | 密度维度 |
| 样式规则 | 做/不做指南 | 从维度组合 |

**重要**：
- 排版描述必须描述视觉外观（例如"圆润无衬线"、"粗体几何"），因为图片生成器无法使用字体名称
- 提示词应从本大纲提取 STYLE_INSTRUCTIONS，**不要**重新读取样式文件

## 预设 → 维度参考

使用预设时，在 `references/dimensions/presets.md` 中查找维度：

| 预设 | 维度 |
|--------|------------|
| blueprint | grid + cool + technical + balanced |
| sketch-notes | organic + warm + handwritten + balanced |
| corporate | clean + professional + geometric + balanced |
| minimal | clean + neutral + geometric + minimal |
| ... | 参见 presets.md 完整映射 |

## 章节分隔符

在以下内容之间使用 `---`（水平线）：
- 标题元数据和 STYLE_INSTRUCTIONS
- STYLE_INSTRUCTIONS 和第一张幻灯片
- 每个幻灯片条目

## 幻灯片编号

- 封面始终是幻灯片1
- 内容幻灯片使用顺序编号
- 封底始终是最后一张幻灯片（N）
- 文件名前缀匹配幻灯片位置：`01-`、`02-` 等

## 文件名 Slug

从幻灯片内容生成有意义的 slug：

| 幻灯片类型 | Slug 模式 | 示例 |
|------------|--------------|---------|
| 封面 | `cover` | `01-slide-cover.png` |
| 内容 | `{topic-slug}` | `02-slide-problem-statement.png` |
| 封底 | `back-cover` | `10-slide-back-cover.png` |

Slug 规则：
- Kebab-case（小写、连字符）
- 来自标题或主要主题
- 最多30个字符
- 在幻灯片中唯一
