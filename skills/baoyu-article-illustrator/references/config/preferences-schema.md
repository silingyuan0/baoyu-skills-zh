---
name: preferences-schema
description: baoyu-article-illustrator 用户首选项的 EXTEND.md YAML 架构
---

# 首选项架构

## 完整架构

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

language: null            # zh|en|ja|ko|auto

default_output_dir: null  # same-dir|illustrations-subdir|independent

custom_styles:
  - name: my-style
    description: "风格描述"
    color_palette:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    visual_elements: "简洁线条、几何形状"
    typography: "现代无衬线"
    best_for: "商业、教育"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 描述 |
|-------|------|---------|-------------|
| `version` | int | 1 | 架构版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文本（@username 或自定义）|
| `watermark.position` | enum | bottom-right | 图片上的位置 |
| `preferred_style.name` | string | null | 风格名称或 null |
| `preferred_style.description` | string | "" | 自定义备注/覆盖 |
| `language` | string | null | 输出语言（null = 自动检测）|
| `default_output_dir` | enum | null | 输出目录偏好（null = 每次询问）|
| `custom_styles` | array | [] | 用户定义的风格 |

## 位置选项

| 值 | 描述 |
|-------|-------------|
| `bottom-right` | 右下角（默认，最常见）|
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中 |
| `top-right` | 右上角 |

## 输出目录选项

| 值 | 描述 |
|-------|-------------|
| `same-dir` | 与文章相同目录 |
| `illustrations-subdir` | `{article-dir}/illustrations/` 子目录 |
| `independent` | 工作目录中的 `illustrations/{topic-slug}/` |

## 自定义风格字段

| 字段 | 必填 | 描述 |
|-------|----------|-------------|
| `name` | 是 | 唯一风格标识符（kebab-case，即用连字符分隔的小写单词）|
| `description` | 是 | 风格的含义 |
| `color_palette.primary` | 否 | 主色（数组）|
| `color_palette.background` | 否 | 背景色 |
| `color_palette.accents` | 否 | 强调色（数组）|
| `visual_elements` | 否 | 装饰元素 |
| `typography` | 否 | 字体/文字风格 |
| `best_for` | 否 | 推荐的内容类型 |

## 示例：最小首选项

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

## 示例：完整首选项

```yaml
---
version: 1
watermark:
  enabled: true
  content: "@myaccount"
  position: bottom-right

preferred_style:
  name: notion
  description: "科技文章的简洁插图"

language: zh

custom_styles:
  - name: corporate
    description: "专业 B2B 风格"
    color_palette:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    visual_elements: "简洁线条、微妙渐变、几何形状"
    typography: "现代无衬线、专业"
    best_for: "商业、SaaS、企业"
---
```
