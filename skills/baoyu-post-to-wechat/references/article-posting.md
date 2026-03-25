# 文章发表

发布 markdown 文章到微信公众号，支持完整格式。

## 使用方法

```bash
# 发布 markdown 文章
${BUN_X} ./scripts/wechat-article.ts --markdown article.md

# 带主题
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --theme grace

# 禁用普通外部链接的底部引用
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --no-cite

# 带明确选项
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --author "作者名" --summary "摘要"
```

## 参数

| 参数 | 描述 |
|-----------|-------------|
| `--markdown <path>` | 要转换和发布的 Markdown 文件 |
| `--theme <name>` | 主题：default、grace、simple、modern |
| `--no-cite` | 将普通外部链接保留内联而非转换为底部引用 |
| `--title <text>` | 覆盖标题（从 markdown 自动提取）|
| `--author <name>` | 作者名 |
| `--summary <text>` | 文章摘要 |
| `--html <path>` | 预渲染 HTML 文件（markdown 的替代方案）|
| `--profile <dir>` | Chrome 配置文件目录 |

## Markdown 格式

```markdown
---
title: Article Title
author: Author Name
---

# Title (becomes article title)

Regular paragraph with **bold** and *italic*.

## Section Header

![Image description](./image.png)

- List item 1
- List item 2

> Blockquote text

[Link text](https://example.com)
```

Markdown 模式默认将普通外部链接转换为底部引用以适配微信输出。使用 `--no-cite` 禁用该行为。

## 图片处理

1. **解析**：markdown 中的图片替换为 `WECHATIMGPH_N`
2. **渲染**：生成带占位符的 HTML
3. **粘贴**：将 HTML 内容粘贴到微信编辑器
4. **替换**：对于每个占位符：
   - 找到并选中占位符文本
   - 滚动到视图
   - 按 Backspace 删除占位符
   - 从剪贴板粘贴图片

## 脚本

| 脚本 | 用途 |
|--------|---------|
| `wechat-article.ts` | 文章发布主脚本 |
| `md-to-wechat.ts` | 带占位符的 Markdown 到 HTML 转换 |
| `md/render.ts` | 带主题的 Markdown 渲染 |

## 示例会话

```
User: /post-to-wechat --markdown ./article.md

Claude:
1. Parses markdown, finds 5 images
2. Generates HTML with placeholders
3. Opens Chrome, navigates to WeChat editor
4. Pastes HTML content
5. For each image:
   - Selects WECHATIMGPH_1
   - Scrolls into view
   - Presses Backspace to delete
   - Pastes image
6. Reports: "Article composed with 5 images."
```
