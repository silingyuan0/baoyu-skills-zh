---
name: font-dimension
description: 封面图字体风格维度
---

# 字体维度

控制排版风格和视觉个性。

## 值

| 字体 | 视觉风格 | 线条特征 | 个性 |
|------|----------|----------|------|
| `clean` | 几何无衬线体 | 锐利、统一 | 现代、精确、中性 |
| `handwritten` | 手写体、笔刷 | 有机、多变 | 温暖、个性化、亲切 |
| `serif` | 经典衬线体 | 精致、结构化 | 社论、权威感 |
| `display` | 粗体、装饰性 | 沉重、表现力强 | 引人注目、活泼 |

## 详细说明

### clean

现代、通用的排版，中性个性。

**特征**：
- 几何无衬线字母形态
- 锐利、统一的笔画粗细
- 干净的边缘，无装饰花饰
- 所有尺寸下均有高可读性
- 极简个性，最大清晰度

**适用场景**：
- 技术文档
- 专业/企业内容
- 极简设计方法
- 数据驱动文章
- 现代品牌美学

**提示词建议**：
- Use clean geometric sans-serif typography
- Modern, minimal letterforms
- Sharp edges, uniform stroke weight
- High contrast against background

### handwritten

温暖、有机的排版，个性化特征。

**特征**：
- 手写体或笔刷风格
- 有机、多变的笔画粗细
- 自然的不完美感
- 亲切、人性化感觉
- 随意但有意为之

**适用场景**：
- 个人故事
- 生活方式内容
- 健康和自我提升
- 创意教程
- 友好的品牌声音

**提示词建议**：
- Use warm hand-lettered typography with organic brush strokes
- Friendly, personal feel
- Natural variation in stroke weight
- Approachable, human character

### serif

经典、优雅的排版，社论权威感。

**特征**：
- 传统衬线字母形态
- 精致、结构化的笔画
- 优雅的比例
- 永恒的精致感
- 正式、可信赖的感觉

**适用场景**：
- 社论内容
- 学术文章
- 奢侈品牌内容
- 历史话题
- 文学作品

**提示词建议**：
- Use elegant serif typography with refined letterforms
- Classic, editorial character
- Structured, proportional spacing
- Authoritative, sophisticated feel

### display

粗体、装饰性排版，追求最大冲击力。

**特征**：
- 沉重、表现力强的字母形态
- 装饰元素
- 强烈的视觉存在感
- 活泼或戏剧性的个性
- 为标题而设计

**适用场景**：
- 公告
- 娱乐内容
- 促销材料
- 活动营销
- 游戏话题

**提示词建议**：
- Use bold decorative display typography
- Heavy, expressive headlines
- Strong visual impact
- Attention-grabbing character

## 默认值

`clean` — 通用性强，与大多数渲染风格搭配良好。

## 渲染风格兼容性

| 字体 × 渲染风格 | flat-vector | hand-drawn | painterly | digital | pixel | chalk | screen-print |
|------------------|:-----------:|:----------:|:---------:|:-------:|:-----:|:-----:|:------------:|
| clean | ✓✓ | ✗ | ✗ | ✓✓ | ✓ | ✗ | ✓ |
| handwritten | ✓ | ✓✓ | ✓✓ | ✓ | ✗ | ✓✓ | ✗ |
| serif | ✓ | ✗ | ✓ | ✓✓ | ✗ | ✗ | ✓ |
| display | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓✓ |

✓✓ = 强烈推荐 | ✓ = 兼容 | ✗ = 不推荐

## 类型兼容性

| 字体 × 类型 | hero | conceptual | typography | metaphor | scene | minimal |
|-------------|:----:|:----------:|:----------:|:--------:|:-----:|:-------:|
| clean | ✓ | ✓✓ | ✓✓ | ✓ | ✗ | ✓✓ |
| handwritten | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ |
| serif | ✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ |
| display | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✗ |

## 调色板交互

字体风格会根据调色板特征进行调整：

| 调色板类别 | clean | handwritten | serif | display |
|------------------|-------|-------------|-------|---------|
| 暖色系（warm, earth, pastel） | 更柔和的笔画粗细 | 自然契合 | 暖色调 | 活力感 |
| 冷色系（cool, mono, elegant） | 完美匹配 | 对比效果 | 经典搭配 | 大胆声明 |
| 暗色系（dark, vivid） | 高对比度 | 发光效果 | 戏剧性 | 最大冲击力 |
| 复古系（retro） | 现代对比 | 怀旧契合 | 时代感 | 复古标题 |
| 双色调系（duotone） | 锐利对比 | 不推荐 | 戏剧性搭配 | 电影冲击力 |

## 自动选择

当省略 `--font` 参数时，根据信号选择：

| 信号 | 字体 |
|---------|------|
| 个人、生活方式、人文、温暖、友好、故事 | `handwritten` |
| 技术、专业、简洁、现代、极简、数据 | `clean` |
| 社论、学术、奢华、经典、文学 | `serif` |
| 公告、娱乐、促销、大胆、活动、游戏 | `display` |

默认值：`clean`
