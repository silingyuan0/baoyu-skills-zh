# 风格参考

## 核心风格

简化的风格分级表，便于快速选择：

| 核心风格 | 对应风格 | 最适用于 |
|---------|---------|---------|
| `vector` | vector-illustration | 知识类文章、教程、技术内容 |
| `minimal-flat` | notion | 通用、知识分享、SaaS |
| `sci-fi` | blueprint | AI、前沿技术、系统设计 |
| `hand-drawn` | sketch/warm | 轻松、反思、休闲内容 |
| `editorial` | editorial | 流程、数据、新闻 |
| `scene` | warm/watercolor | 叙事、情感、生活方式 |
| `poster` | screen-print | 评论、社论、文化、电影感 |

大多数情况使用核心风格。如需更细粒度控制，请参阅下方的完整风格库。

---

## 风格库

| 风格 | 描述 | 最适用于 |
|------|------|---------|
| `vector-illustration` | 简洁的扁平矢量艺术，大胆的几何形状 | 知识类文章、教程、技术内容 |
| `notion` | 极简手绘线条艺术 | 知识分享、SaaS、效率工具 |
| `elegant` | 精致、考究 | 商务、思想领导力 |
| `warm` | 友好、亲切 | 个人成长、生活方式、教育 |
| `minimal` | 极致简洁、禅意 | 哲学、极简主义、核心概念 |
| `blueprint` | 技术蓝图 | 架构、系统设计、工程 |
| `watercolor` | 柔和的艺术感，自然的温暖 | 生活方式、旅行、创意 |
| `editorial` | 杂志风格信息图 | 技术解说、新闻 |
| `scientific` | 学术级精确图表 | 生物、化学、技术研究 |
| `chalkboard` | 教室粉笔绘画风格 | 教育、教学、讲解 |
| `fantasy-animation` | 吉卜力/迪士尼风格手绘 | 绘本、奇幻、情感 |
| `flat` | 现代大胆的几何形状 | 现代数字、当代 |
| `flat-doodle` | 可爱的扁平风格，粗描边 | 可爱、友好、亲切 |
| `intuition-machine` | 复古纸张技术简报 | 技术简报、学术 |
| `nature` | 有机自然风格插图 | 环保、健康 |
| `pixel-art` | 复古 8-bit 游戏美学 | 游戏、复古科技 |
| `playful` | 异想天开的粉彩涂鸦 | 有趣、休闲、教育 |
| `retro` | 80/90 年代霓虹几何 | 80/90 年代怀旧、大胆 |
| `sketch` | 粗犷铅笔笔记本风格 | 头脑风暴、创意探索 |
| `screen-print` | 大胆海报艺术，半色调纹理，限定色彩 | 评论、社论、文化、电影感 |
| `sketch-notes` | 柔和手绘温暖笔记 | 教育、温暖笔记 |
| `vintage` | 做旧羊皮纸历史感 | 历史、传承 |

完整规范：`references/styles/<style>.md`

## 类型 x 风格兼容性矩阵

| | vector-illustration | notion | warm | minimal | blueprint | watercolor | elegant | editorial | scientific | screen-print |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| infographic | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓✓ | ✓ |
| scene | ✓ | ✓ | ✓✓ | ✓ | ✗ | ✓✓ | ✓ | ✓ | ✗ | ✓✓ |
| flowchart | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✗ | ✓ | ✓✓ | ✓ | ✗ |
| comparison | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ |
| framework | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✗ | ✓✓ | ✓ | ✓✓ | ✓ |
| timeline | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓ |

✓✓ = 强烈推荐 | ✓ = 兼容 | ✗ = 不推荐

## 按类型自动选择

| 类型 | 首选风格 | 备选风格 |
|------|---------|---------|
| infographic | vector-illustration | notion、blueprint、editorial |
| scene | warm | watercolor、elegant |
| flowchart | vector-illustration | notion、blueprint |
| comparison | vector-illustration | notion、elegant |
| framework | blueprint | vector-illustration、notion |
| timeline | elegant | warm、editorial |

## 按内容信号自动选择

| 内容信号 | 推荐类型 | 推荐风格 |
|---------|---------|---------|
| API、指标、数据、对比、数字 | infographic | blueprint、vector-illustration |
| 知识、概念、教程、学习、指南 | infographic | vector-illustration、notion |
| 技术、AI、编程、开发、代码 | infographic | vector-illustration、blueprint |
| 操作指南、步骤、工作流、流程、教程 | flowchart | vector-illustration、notion |
| 框架、模型、架构、原则 | framework | blueprint、vector-illustration |
| 对比、优缺点、前后对比、替代方案 | comparison | vector-illustration、notion |
| 故事、情感、旅程、体验、个人 | scene | warm、watercolor |
| 历史、时间线、进展、演进 | timeline | elegant、warm |
| 效率、SaaS、工具、应用、软件 | infographic | notion、vector-illustration |
| 商务、职业、战略、企业 | framework | elegant |
| 评论、社论、文化、哲学、电影感、戏剧、海报 | scene | screen-print |
| 生物、化学、医学、科学 | infographic | scientific |
| 解说、新闻、杂志、调查 | infographic | editorial |

## 按类型的风格特征

### infographic + vector-illustration
- 简洁扁平的矢量形状，大胆的几何造型
- 鲜明但和谐的配色方案
- 清晰的视觉层次，配有图标和标签
- 现代、专业、高可读性
- 非常适合知识类文章和教程

### flowchart + vector-illustration
- 大胆的箭头和连接线
- 独特的步骤容器，配有图标
- 清晰的递进流程
- 高对比度，便于阅读

### comparison + vector-illustration
- 分割布局，视觉分隔清晰
- 每侧使用大胆的图标
- 颜色编码区分
- 一目了然的对比效果

### framework + vector-illustration
- 几何节点表示
- 清晰的层级结构
- 大胆的连接线
- 现代系统图美学

### infographic + blueprint
- 技术精确感，蓝图线条
- 网格布局，清晰分区
- 等宽字体标签，数据导向
- 蓝/白配色方案

### infographic + notion
- 手绘质感，平易近人
- 柔和图标，圆润元素
- 中性配色，干净背景
- 非常适合 SaaS/效率工具

### scene + warm
- 黄金时刻光线，温馨氛围
- 柔和渐变，自然纹理
- 引人入胜、亲切的感觉
- 非常适合叙事

### scene + watercolor
- 艺术感，绘画效果
- 柔和边缘，色彩晕染
- 梦幻、创意氛围
- 最适合生活方式/旅行

### flowchart + notion
- 清晰的步骤指示
- 简单的箭头连接
- 极简装饰
- 聚焦流程清晰度

### flowchart + blueprint
- 技术精确感
- 详细的连接节点
- 工程美学
- 适用于复杂系统

### comparison + elegant
- 精致的分隔线
- 均衡的字体排版
- 专业外观
- 商务对比

### framework + blueprint
- 精确的节点连接
- 层级清晰
- 系统架构感
- 技术框架

### timeline + elegant
- 精致的标记点
- 考究的字体排版
- 历史庄重感
- 专业展示

### timeline + warm
- 友好的递进感
- 有机流动
- 个人旅程感
- 成长叙事

### scene + screen-print
- 大胆的剪影，符号化构图
- 2-5 种扁平色，半色调纹理
- 图底反转（负空间讲述次要故事）
- 复古海报美学，概念化而非写实
- 非常适合评论文章和文化评论

### comparison + screen-print
- 分割双色调构图（每侧一种颜色）
- 大胆的几何分隔线
- 符号化图标优先于细节渲染
- 高对比度，即时视觉冲击

### framework + screen-print
- 几何节点表示，模版切割边缘
- 限定色彩编码（每个概念层级一种颜色）
- 简洁的剪影式图标
- 海报风格层级，粗体排版
