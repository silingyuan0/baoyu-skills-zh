---
name: preferences-schema
description: baoyu-comic 用户首选项的 EXTEND.md YAML 架构
---

# 首选项架构

## 完整架构

```yaml
---
version: 2

watermark:
  enabled: false
  content: ""
  position: bottom-right  # bottom-right|bottom-left|bottom-center|top-right

preferred_art: null       # ligne-claire|manga|realistic|ink-brush|chalk
preferred_tone: null      # neutral|warm|dramatic|romantic|energetic|vintage|action
preferred_layout: null    # standard|cinematic|dense|splash|mixed|webtoon
preferred_aspect: null    # 3:4|4:3|16:9

language: null            # zh|en|ja|ko|auto

character_presets:
  - name: my-characters
    roles:
      learner: "Name"
      mentor: "Name"
      challenge: "Name"
      support: "Name"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 描述 |
|-------|------|---------|-------------|
| `version` | int | 2 | 架构版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文本（@username 或自定义）|
| `watermark.position` | enum | bottom-right | 图片上的位置 |
| `preferred_art` | string | null | 艺术风格（ligne-claire, manga, realistic, ink-brush, chalk）|
| `preferred_tone` | string | null | 基调（neutral, warm, dramatic, romantic, energetic, vintage, action）|
| `preferred_layout` | string | null | 布局偏好或 null |
| `preferred_aspect` | string | null | 纵横比（3:4, 4:3, 16:9）|
| `language` | string | null | 输出语言（null = 自动检测）|
| `character_presets` | array | [] | 角色预设（如 ohmsha 风格）|

## 艺术风格选项

| 值 | 中文 | 描述 |
|-------|------|-------------|
| `ligne-claire` | 清线 | 均匀线条，平坦色彩，欧洲漫画传统 |
| `manga` | 日漫 | 大眼睛，漫画 conventions，表情丰富 |
| `realistic` | 写实 | 数字绘画，写实比例 |
| `ink-brush` | 水墨 | 中国画笔触，水墨效果 |
| `chalk` | 粉笔 | 黑板美学，手绘温暖感 |

## 基调选项

| 值 | 中文 | 描述 |
|-------|------|-------------|
| `neutral` | 中性 | 平衡、理性、教育 |
| `warm` | 温馨 | 怀旧、个人、舒适 |
| `dramatic` | 戏剧 | 高对比、强烈、有力 |
| `romantic` | 浪漫 | 柔和、美丽、装饰元素 |
| `energetic` | 活力 | 明亮、动态、兴奋 |
| `vintage` | 复古 | 历史、年龄、时代真实性 |
| `action` | 动作 | 速度线、冲击效果、战斗 |

## 位置选项

| 值 | 描述 |
|-------|-------------|
| `bottom-right` | 右下角（默认，适合大多数面板布局）|
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中（适合网页漫画垂直滚动）|
| `top-right` | 右上角（避免 - 与页码冲突）|

## 角色预设字段

| 字段 | 必填 | 描述 |
|-------|----------|-------------|
| `name` | 是 | 唯一预设标识符 |
| `roles.learner` | 否 | 代表学习者/主角的角色 |
| `roles.mentor` | 否 | 代表老师/向导的角色 |
| `roles.challenge` | 否 | 代表障碍/反派的角色 |
| `roles.support` | 否 | 提供支持/喜剧调节的角色 |

## 示例：最小首选项

```yaml
---
version: 2
watermark:
  enabled: true
  content: "@myusername"
preferred_art: ligne-claire
preferred_tone: neutral
---
```

## 示例：完整首选项

```yaml
---
version: 2
watermark:
  enabled: true
  content: "@comicstudio"
  position: bottom-right

preferred_art: manga
preferred_tone: neutral

preferred_layout: webtoon

preferred_aspect: "3:4"

language: zh

character_presets:
  - name: tech-tutorial
    roles:
      learner: "小明"
      mentor: "教授"
      challenge: "难题怪"
      support: "小助手"
  - name: doraemon
    roles:
      learner: "大雄"
      mentor: "哆啦A梦"
      challenge: "胖虎"
      support: "静香"
---
```

## 从 v1 迁移

如果您有 v1 首选项文件与 `preferred_style`，请按如下迁移：

| 旧 `preferred_style.name` | 新 `preferred_art` | 新 `preferred_tone` |
|---------------------------|---------------------|---------------------|
| classic | ligne-claire | neutral |
| dramatic | ligne-claire | dramatic |
| warm | ligne-claire | warm |
| sepia | realistic | vintage |
| vibrant | manga | energetic |
| ohmsha | manga | neutral |
| realistic | realistic | neutral |
| wuxia | ink-brush | action |
| shoujo | manga | romantic |
| chalkboard | chalk | neutral |
