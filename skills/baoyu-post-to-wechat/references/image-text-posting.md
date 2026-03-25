# 贴图发表（Image-Text Posting，原图文）

发布带多图的图文消息到微信公众号。

> **注**：微信已在公众号菜单中将"图文"更名为"贴图"（自 2026 年起）。

## 使用方法

```bash
# 带图片和 Markdown 文件发布（自动提取标题/内容）
${BUN_X} ./scripts/wechat-browser.ts --markdown source.md --images ./images/

# 带明确标题和内容发布
${BUN_X} ./scripts/wechat-browser.ts --title "标题" --content "内容" --image img1.png --image img2.png

# 保存为草稿
${BUN_X} ./scripts/wechat-browser.ts --markdown source.md --images ./images/ --submit
```

## 参数

| 参数 | 描述 |
|-----------|-------------|
| `--markdown <path>` | Markdown 文件，用于提取标题/内容 |
| `--images <dir>` | 包含图片的目录（按名称排序）|
| `--title <text>` | 文章标题（最多 20 字符，超长自动压缩）|
| `--content <text>` | 文章内容（最多 1000 字符，超长自动压缩）|
| `--image <path>` | 单张图片（可重复）|
| `--submit` | 保存为草稿（默认：仅预览）|
| `--profile <dir>` | Chrome 配置文件目录 |

## 从 Markdown 自动提取标题/内容

使用 `--markdown` 时，脚本：

1. **解析前置元数据**获取标题和作者：
   ```yaml
   ---
   title: 文章标题
   author: 作者名
   ---
   ```

2. **回退到 H1** 如无前置元数据标题：
   ```markdown
   # 这将成为标题
   ```

3. **压缩标题**至 20 字符（如超长）：
   - 原文："如何在一天内彻底重塑你的人生"
   - 压缩后："一天彻底重塑你的人生"

4. **提取首段**作为内容（最多 1000 字符）

## 图片目录模式

使用 `--images <dir>` 时：

- 上传目录中所有 PNG/JPG 文件
- 按名称字母顺序排序
- 命名约定：`01-cover.png`、`02-content.png` 等

## 限制

| 字段 | 最大长度 | 备注 |
|-------|------------|-------|
| 标题 | 20 字符 | 超长自动压缩 |
| 内容 | 1000 字符 | 超长自动压缩 |
| 图片 | 最多 9 张 | 微信限制 |

## 示例会话

```
User: /post-to-wechat --markdown ./article.md --images ./xhs-images/

Claude:
1. Parses markdown meta:
   - Title: "如何在一天内彻底重塑你的人生" → "一天内重塑你的人生"
   - Author: from frontmatter or default
2. Extracts content from first paragraphs
3. Finds 7 images in xhs-images/
4. Opens Chrome, navigates to WeChat "图文" editor
5. Uploads all images
6. Fills title and content
7. Reports: "Image-text posted with 7 images."
```

## 脚本

| 脚本 | 用途 |
|--------|---------|
| `wechat-browser.ts` | 贴图发布主脚本 |
| `cdp.ts` | Chrome DevTools Protocol 工具 |
| `copy-to-clipboard.ts` | 剪贴板操作 |
