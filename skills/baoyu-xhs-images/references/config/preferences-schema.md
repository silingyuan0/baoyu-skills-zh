# 偏好设置 Schema

## 完整 Schema

```yaml
---
version: 1

watermark:
  enabled: false
  content: ""
  position: bottom-right  # bottom-right|bottom-left|bottom-center|top-right

preferred_style:
  name: null              # 内置或自定义风格名称
  description: ""         # 覆盖/备注

preferred_layout: null    # sparse|balanced|dense|list|comparison|flow

language: null            # zh|en|ja|ko|auto

custom_styles:
  - name: my-style
    description: "Style description"
    color_palette:
      primary: ["#FED7E2", "#FEEBC8"]
      background: "#FFFAF0"
      accents: ["#FF69B4", "#FF6B6B"]
    visual_elements: "Hearts, stars, sparkles"
    typography: "Rounded, bubbly hand lettering"
    best_for: "Lifestyle, beauty"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 描述 |
|-------|------|---------|-------------|
| `version` | int | 1 | Schema 版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文字（@username 或自定义） |
| `watermark.position` | enum | bottom-right | 图像上的位置 |
| `preferred_style.name` | string | null | 风格名称或 null |
| `preferred_style.description` | string | "" | 自定义备注/覆盖 |
| `preferred_layout` | string | null | 布局偏好或 null |
| `language` | string | null | 输出语言（null = 自动检测） |
| `custom_styles` | array | [] | 用户定义的风格 |

## 位置选项

| 值 | 描述 |
|-------|-------------|
| `bottom-right` | 右下角（默认，最常用） |
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中 |
| `top-right` | 右上角 |

## 自定义样式字段

| 字段 | 必填 | 描述 |
|-------|----------|-------------|
| `name` | 是 | 唯一风格标识符（kebab-case） |
| `description` | 是 | 风格的描述 |
| `color_palette.primary` | 否 | 主色（数组） |
| `color_palette.background` | 否 | 背景色 |
| `color_palette.accents` | 否 | 强调色（数组） |
| `visual_elements` | 否 | 装饰元素 |
| `typography` | 否 | 字体/文字风格 |
| `best_for` | 否 | 建议的内容类型 |

## 示例：最小化偏好

```yaml
---
version: 1
watermark:
  enabled: true
  content: "@myusername"
preferred_style:
  name: notion
---
```

## 示例：完整偏好

```yaml
---
version: 1
watermark:
  enabled: true
  content: "@myxhsaccount"
  position: bottom-right

preferred_style:
  name: notion
  description: "Clean knowledge cards for tech content"

preferred_layout: dense

language: zh

custom_styles:
  - name: corporate
    description: "Professional B2B style"
    color_palette:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    visual_elements: "Clean lines, subtle gradients, geometric shapes"
    typography: "Modern sans-serif, professional"
    best_for: "Business, SaaS, enterprise"
---
```
