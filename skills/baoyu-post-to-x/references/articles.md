# X 文章 - 详细指南

发布带富文本格式和图片的 Markdown 文章到 X 文章编辑器。

## 前置要求

- X Premium 订阅（文章功能必需）
- 已安装 Google Chrome
- 已安装 `bun`

## 使用方法

```bash
# 发布 markdown 文章（预览模式）
${BUN_X} {baseDir}/scripts/x-article.ts article.md

# 带自定义封面图片
${BUN_X} {baseDir}/scripts/x-article.ts article.md --cover ./cover.jpg

# 实际发布
${BUN_X} {baseDir}/scripts/x-article.ts article.md --submit
```

## Markdown 格式

```markdown
---
title: My Article Title
cover_image: /path/to/cover.jpg
---

# Title (becomes article title)

Regular paragraph text with **bold** and *italic*.

## Section Header

More content here.

![Image alt text](./image.png)

- List item 1
- List item 2

1. Numbered item
2. Another item

> Blockquote text

[Link text](https://example.com)

```
Code blocks become blockquotes (X doesn't support code)
```
```

## 前置元数据字段

| 字段 | 描述 |
|-------|-------------|
| `title` | 文章标题（或使用首个 H1）|
| `cover_image` | 封面图片路径或 URL |
| `cover` | cover_image 的别名 |
| `image` | cover_image 的别名 |

## 图片处理

1. **封面图片**：首张图片或前置元数据中的 `cover_image`
2. **远程图片**：自动下载到临时目录
3. **占位符**：内容中的图片使用 `XIMGPH_N` 格式
4. **插入**：找到占位符、选中、用实际图片替换

## Markdown 到 HTML 脚本

转换 markdown 并检查结构：

```bash
# 获取带所有元数据的 JSON
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md

# 仅输出 HTML
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md --html-only

# 保存 HTML 到文件
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md --save-html /tmp/article.html
```

JSON 输出：
```json
{
  "title": "Article Title",
  "coverImage": "/path/to/cover.jpg",
  "contentImages": [
    {
      "placeholder": "XIMGPH_1",
      "localPath": "/tmp/x-article-images/img.png",
      "blockIndex": 5
    }
  ],
  "html": "<p>Content...</p>",
  "totalBlocks": 20
}
```

## 支持的格式

| Markdown | HTML 输出 |
|----------|-------------|
| `# H1` | 仅标题（不在正文中）|
| `## H2` - `###### H6` | `<h2>` |
| `**bold**` | `<strong>` |
| `*italic*` | `<em>` |
| `[text](url)` | `<a href>` |
| `> quote` | `<blockquote>` |
| `` `code` `` | `<code>` |
| ```` ``` ```` | `<blockquote>`（X 限制）|
| `- item` | `<ul><li>` |
| `1. item` | `<ol><li>` |
| `![](img)` | 图片占位符 |

## 工作流

1. **解析 Markdown**：提取标题、封面、正文图片，生成 HTML
2. **启动 Chrome**：通过 CDP 启动真实浏览器，持久化登录
3. **导航**：打开 `x.com/compose/articles`
4. **创建文章**：如果在列表页面，点击创建按钮
5. **上传封面**：使用文件输入上传封面图片
6. **填写标题**：将标题输入标题字段
7. **粘贴内容**：将 HTML 复制到剪贴板，粘贴到编辑器
8. **插入图片**：对于每个占位符（逆序）：
   - 在编辑器中找到占位符文本
   - 选中占位符
   - 将图片复制到剪贴板
   - 粘贴替换选区
9. **发布后检查**（自动）：
   - 扫描编辑器中剩余的 `XIMGPH_` 占位符
   - 比较预期 vs 实际图片数量
   - 如发现问题则警告
10. **审核**：浏览器保持打开 60 秒预览
11. **发布**：仅带 `--submit` 标志

## 示例会话

```
User: /post-to-x article ./blog/my-post.md --cover ./thumbnail.png

Claude:
1. Parses markdown: title="My Post", 3 content images
2. Launches Chrome with CDP
3. Navigates to x.com/compose/articles
4. Clicks create button
5. Uploads thumbnail.png as cover
6. Fills title "My Post"
7. Pastes HTML content
8. Inserts 3 images at placeholder positions
9. Reports: "Article composed. Review and use --submit to publish."
```

## 故障排除

- **无创建按钮**：确保 X Premium 订阅已激活
- **封面上传失败**：检查文件路径和格式（PNG、JPEG）
- **图片未插入**：验证粘贴内容中是否存在占位符
- **内容未粘贴**：检查 HTML 剪贴板：`${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts html --file /tmp/test.html`

## 工作原理

1. `md-to-html.ts` 将 Markdown 转换为 HTML：
   - 提取前置元数据（title、cover）
   - 将 markdown 转换为 HTML
   - 将图片替换为唯一占位符
   - 将远程图片下载到本地
   - 返回结构化 JSON

2. `x-article.ts` 通过 CDP 发布：
   - 启动真实 Chrome（绕过检测）
   - 使用持久化配置文件（保存登录）
   - 通过 DOM 操作导航和填写编辑器
   - 从系统剪贴板粘贴 HTML
   - 找到/选中/替换每个图片占位符
