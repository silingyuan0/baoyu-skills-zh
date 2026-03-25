# 布局图库

为单张幻灯片提供的可选布局提示。在大纲的 `// LAYOUT` 部分指定。

## 幻灯片特定布局

| 布局 | 描述 | 适用于 |
|--------|-------------|----------|
| `title-hero` | 大标题居中 + 副标题 | 封面幻灯片、章节分隔 |
| `quote-callout` | 带归属的精选引用 | 推荐信、关键洞察 |
| `key-stat` | 以大数字为焦点 | 影响统计数据、指标 |
| `split-screen` | 半图半文 | 功能亮点、比较 |
| `icon-grid` | 带标签的图标网格 | 功能、能力、收益 |
| `two-columns` | 平衡栏中的内容 | 配对信息、双观点 |
| `three-columns` | 三栏内容 | 三重比较、类别 |
| `image-caption` | 全出血图像 + 文字叠加 | 视觉故事讲述、情感 |
| `agenda` | 带高亮的编号列表 | 会议概述、路线图 |
| `bullet-list` | 结构化子弹点 | 简单内容、列表 |

## 信息图衍生布局

| 布局 | 描述 | 适用于 |
|--------|-------------|----------|
| `linear-progression` | 从左到右的顺序流 | 时间线、循序渐进 |
| `binary-comparison` | 并排 A vs B | 之前/之后、利弊 |
| `comparison-matrix` | 多因素网格 | 功能比较 |
| `hierarchical-layers` | 金字塔或堆叠层 | 优先级、重要性 |
| `hub-spoke` | 带辐射项的中心节点 | 概念图、生态系统 |
| `bento-grid` | 不同大小的瓦片 | 概述、摘要 |
| `funnel` | 收窄阶段 | 转化、过滤 |
| `dashboard` | 带图表/数字的指标 | KPI、数据展示 |
| `venn-diagram` | 重叠圆圈 | 关系、交集 |
| `circular-flow` | 连续循环 | 循环过程 |
| `winding-roadmap` | 带里程碑的弯曲路径 | 旅程、时间线 |
| `tree-branching` | 父子层次结构 | 组织图、分类法 |
| `iceberg` | 可见与隐藏层 | 表面与深度 |
| `bridge` | 带连接的间隙 | 问题-解决方案 |

**用法**：在幻灯片的 `// LAYOUT` 部分添加 `Layout: <name>`。

## 布局选择技巧

**将布局与内容匹配**：
| 内容类型 | 推荐布局 |
|--------------|-------------------|
| 单个叙事 | `bullet-list`、`image-caption` |
| 两个概念 | `split-screen`、`binary-comparison` |
| 三个项目 | `three-columns`、`icon-grid` |
| 流程/步骤 | `linear-progression`、`winding-roadmap` |
| 数据/指标 | `dashboard`、`key-stat` |
| 关系 | `hub-spoke`、`venn-diagram` |
| 层次结构 | `hierarchical-layers`、`tree-branching` |

**布局流模式**：
| 位置 | 推荐布局 |
|----------|-------------------|
| 开场 | `title-hero`、`agenda` |
| 中间 | 内容特定布局 |
| 结尾 | `quote-callout`、`key-stat` |

**要避免的常见错误**：
- 对2个项目使用3栏布局（留下空列）
- 在文本下方堆叠图表/表格（改用并排）
- 没有实际图片的图片布局
- 用于强调的引用布局（仅用于带归属的真实引用）
