# 小红书大纲模板

用于生成带有布局规范的信息图系列大纲的模板。

## 文件命名

大纲文件在名称中使用策略标识符：
- `outline-strategy-a.md` - 故事驱动变体
- `outline-strategy-b.md` - 信息密集变体
- `outline-strategy-c.md` - 视觉优先变体
- `outline.md` - 最终选定（从选定变体复制）

## 图像文件命名

图像使用有意义的 slug 以提高可读性：
```
NN-{type}-[slug].png
NN-{type}-[slug].md（在 prompts/ 中）
```

| 类型 | 用途 |
|------|-------|
| `cover` | 首图（封面） |
| `content` | 中间内容图像 |
| `ending` | 末图 |

**示例**：
- `01-cover-ai-tools.png`
- `02-content-why-ai.png`
- `03-content-chatgpt.png`
- `04-content-midjourney.png`
- `05-content-notion-ai.png`
- `06-ending-summary.png`

**Slug 规则**：
- 源自图像内容（kebab-case）
- 在系列内必须唯一
- 保持简短但有描述性（2-4 个词）

## 布局选择指南

### 基于密度的布局

| 布局 | 何时使用 | 信息点数 | 留白 |
|--------|-------------|-------------|------------|
| sparse | 封面、引言、有影响力陈述 | 1-2 | 60-70% |
| balanced | 标准内容、教程 | 3-4 | 40-50% |
| dense | 知识卡片、速查表 | 5-8 | 20-30% |

### 基于结构的布局

| 布局 | 何时使用 | 结构 |
|--------|-------------|-----------|
| list | 排名、清单、步骤 | 编号/项目符号垂直 |
| comparison | 前后、优缺点 | 左右分割 |
| flow | 流程、时间线 | 带箭头的连接节点 |

### 基于位置的推荐

| 位置 | 推荐 | 原因 |
|----------|-------------|-----------|
| 封面 | sparse | 最大冲击、清晰标题 |
| 铺垫 | balanced | 建立语境但不造成负担 |
| 核心 | balanced/dense/list | 匹配内容密度 |
| 收获 | balanced/list | 清晰要点 |
| 结尾 | sparse | 清晰 CTA、令人印象深刻 |

## 大纲格式

```markdown
# 小红书信息图系列大纲

---
strategy: a  # a, b, or c
name: Story-Driven
style: notion
default_layout: dense
image_count: 6
generated: YYYY-MM-DD HH:mm
---

## Image 1 of 6

**Position**: Cover
**Layout**: sparse
**Hook**: 打工人必看！
**Slug**: ai-tools
**Filename**: 01-cover-ai-tools.png

**Text Content**:
- Title: 「5个AI神器让你效率翻倍」
- Subtitle: 亲测好用，建议收藏

**Visual Concept**:
科技感背景，多个AI工具图标环绕，中心大标题，
霓虹蓝+深色背景，未来感十足

**Swipe Hook**: 第一个就很强大👇

---

## Image 2 of 6

**Position**: Content
**Layout**: balanced
**Core Message**: 为什么你需要AI工具
**Slug**: why-ai
**Filename**: 02-content-why-ai.png

**Text Content**:
- Title: 「为什么要用AI？」
- Points:
  - 重复工作自动化
  - 创意辅助不卡壳
  - 效率提升10倍

**Visual Concept**:
对比图：左边疲惫打工人，右边轻松使用AI的人
科技线条装饰，简洁有力

**Swipe Hook**: 接下来是具体工具推荐👇

---

## Image 3 of 6

**Position**: Content
**Layout**: dense
**Core Message**: ChatGPT使用技巧
**Slug**: chatgpt
**Filename**: 03-content-chatgpt.png

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

**Swipe Hook**: 下一个更适合创意工作者👇

---

## Image 4 of 6

**Position**: Content
**Layout**: dense
**Core Message**: Midjourney绘图
**Slug**: midjourney
**Filename**: 04-content-midjourney.png

**Text Content**:
- Title: 「Midjourney」
- Subtitle: AI绘画神器
- Points:
  - 输入描述，秒出图片
  - 风格多样：写实/插画/3D
  - 做封面、做头像、做素材
  - 不会画画也能当设计师

**Visual Concept**:
展示几张MJ生成的不同风格图片
画框/画布元素装饰

**Swipe Hook**: 还有一个效率神器👇

---

## Image 5 of 6

**Position**: Content
**Layout**: balanced
**Core Message**: Notion AI笔记
**Slug**: notion-ai
**Filename**: 05-content-notion-ai.png

**Text Content**:
- Title: 「Notion AI」
- Subtitle: 智能笔记助手
- Points:
  - 自动总结长文
  - 头脑风暴出点子
  - 整理会议记录

**Visual Concept**:
Notion界面风格，简洁黑白配色
展示笔记整理前后对比

**Swipe Hook**: 最后总结一下👇

---

## Image 6 of 6

**Position**: Ending
**Layout**: sparse
**Core Message**: 总结与互动
**Slug**: summary
**Filename**: 06-ending-summary.png

**Text Content**:
- Title: 「工具只是工具」
- Subtitle: 关键是用起来！
- CTA: 收藏备用 | 转发给需要的朋友
- Interaction: 你最常用哪个？评论区见👇

**Visual Concept**:
简洁背景，大字标题
底部互动引导文字
收藏/分享图标

---
```

## 滑动钩子策略

每张图应以下一个的钩子结束：

| 策略 | 示例 |
|----------|---------|
| 悬念 | "第一个就很强大👇" |
| 编号 | "接下来是第2个👇" |
| 最高级 | "下一个更厉害👇" |
| 提问 | "猜猜下一个是什么？👇" |
| 承诺 | "最后一个最实用👇" |
| 紧迫 | "最重要的来了👇" |

## 策略差异化

三种策略应有实质性差异：

| 策略 | 重点 | 结构 | 页数 |
|----------|-------|-----------|------------|
| A: 故事驱动 | 情感、个人 | 钩子→问题→发现→体验→结论 | 4-6 |
| B: 信息密集 | 事实、结构 | 核心→信息卡片→对比→推荐 | 3-5 |
| C: 视觉优先 | 氛围、最少文字 | 英雄→细节→生活方式→CTA | 3-4 |

**"AI工具推荐"示例**：
- `outline-strategy-a.md`：温暖 + Balanced - AI的个人旅程
- `outline-strategy-b.md`：Notion + Dense - 知识卡片风格
- `outline-strategy-c.md`：Minimal + Sparse - 时尚科技美学
