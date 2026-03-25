---
name: baoyu-markdown-to-html
description: 将 Markdown 转换为带样式的 HTML，支持微信公众号兼容主题。支持代码高亮、数学公式、PlantUML、脚注、提示框、信息图以及可选的外链底部引用。当用户要求"markdown to html"、"md 转 html"、"微信外链转底部引用"或需要将 Markdown 转换为带样式的 HTML 时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-markdown-to-html
    requires:
      anyBins:
        - bun
        - npx
---

# Markdown 转 HTML 转换器

将 Markdown 文件转换为带内联 CSS 样式的精美 HTML，针对微信公众号等平台优化。

## 脚本目录

**代理执行**：将本 SKILL.md 所在目录确定为 `{baseDir}`。解析 `${BUN_X}` 运行时：如果已安装 `bun` 则使用 `bun`；如果可用 `npx` 则使用 `npx -y bun`；否则提示安装 bun。将 `{baseDir}` 和 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 主入口 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按以下优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-markdown-to-html/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-markdown-to-html/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-markdown-to-html/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-markdown-to-html/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────────────────┬───────────────────┐
│                             Path                             │     Location      │
├──────────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-markdown-to-html/EXTEND.md               │ Project directory │
├──────────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md         │ User home         │
└──────────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Found     │ Read, parse, apply settings                                               │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Not found │ Use defaults                                                              │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认主题 | 自定义 CSS 变量 | 代码块样式

## 工作流

### 步骤 0：预检查（中文内容）

**条件**：仅在输入文件包含中文时执行。

**检测**：
1. 读取输入 Markdown 文件
2. 检查内容是否包含中日韩文字
3. 如果无中日韩内容 → 跳至步骤 1

**格式建议**：

如果检测到中日韩内容且 `baoyu-format-markdown` 技能可用：

使用 `AskUserQuestion` 询问是否先格式化。格式化可以修复：
- 标点符号在粗体标记内导致的 `**` 解析失败
- 中日韩/英文间距问题

**如果用户同意**：调用 `baoyu-format-markdown` 技能格式化文件，然后使用格式化后的文件作为输入。

**如果用户拒绝**：继续使用原始文件。

### 步骤 1：确定主题

**主题解析顺序**（首个匹配项生效）：
1. 用户明确指定的主题（CLI `--theme` 或对话中指定）
2. EXTEND.md 中的 `default_theme`（本技能自身的 EXTEND.md，在步骤 0 中检查）
3. `baoyu-post-to-wechat` EXTEND.md 中的 `default_theme`（跨技能回退）
4. 如果均未找到 → 使用 `AskUserQuestion` 确认

**跨技能 EXTEND.md 检查**（仅在本技能的 EXTEND.md 无 `default_theme` 时执行）：

```bash
# 检查 baoyu-post-to-wechat EXTEND.md 中的 default_theme
test -f "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && grep -o 'default_theme:.*' "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md"
```

```powershell
# PowerShell (Windows)
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { Select-String -Pattern 'default_theme:.*' -Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" | ForEach-Object { $_.Matches.Value } }
```

**如果主题从 EXTEND.md 解析到**：直接使用，不询问用户。

**如果未找到默认主题**：使用 `AskUserQuestion` 确认：

| 主题 | 说明 |
|-------|-------------|
| `default`（推荐） | 经典 - 传统布局，居中标题带底边框，H2 使用白字彩色背景 |
| `grace` | 优雅 - 文字阴影，圆角卡片，精致引用 |
| `simple` | 极简 - 现代简约，非对称圆角，干净留白 |
| `modern` | 现代 - 大圆角，药丸形标题，宽松行高（搭配 `--color red` 呈现传统红金风格） |

### 步骤 1.5：确定引用模式

**默认**：关闭。默认不询问。

**仅在用户明确要求** "微信外链转底部引用"、"底部引用"、"文末引用"，或传入 `--cite` 时启用。

**启用时的行为**：
- 普通外链以编号上标渲染，并收集在末尾的 `引用链接` 段落下。
- `https://mp.weixin.qq.com/...` 链接保持为直接链接，不移至底部。
- 链接文本等于 URL 的裸链接保持内联。

### 步骤 2：转换

```bash
${BUN_X} {baseDir}/scripts/main.ts <markdown_file> --theme <theme> [--cite]
```

### 步骤 3：报告结果

从 JSON 结果中显示输出路径。如果创建了备份则提及。

## 用法

```bash
${BUN_X} {baseDir}/scripts/main.ts <markdown_file> [options]
```

**选项：**

| 选项 | 说明 | 默认值 |
|--------|-------------|---------|
| `--theme <name>` | 主题名称（default、grace、simple、modern） | default |
| `--color <name\|hex>` | 主色：预设名称或十六进制值 | 主题默认 |
| `--font-family <name>` | 字体：sans、serif、serif-cjk、mono 或 CSS 值 | 主题默认 |
| `--font-size <N>` | 字号：14px、15px、16px、17px、18px | 16px |
| `--title <title>` | 覆盖前置元数据中的标题 | |
| `--cite` | 将外链转为底部引用，追加 `引用链接` 段落 | false（关闭） |
| `--keep-title` | 保留内容中的第一个标题 | false（移除） |
| `--help` | 显示帮助 | |

**颜色预设：**

| 名称 | 十六进制 | 标签 |
|------|-----|-------|
| blue | #0F4C81 | 经典蓝 |
| green | #009874 | 翡翠绿 |
| vermilion | #FA5151 | 朱红 |
| yellow | #FECE00 | 柠檬黄 |
| purple | #92617E | 薰衣草紫 |
| sky | #55C9EA | 天空蓝 |
| rose | #B76E79 | 玫瑰金 |
| olive | #556B2F | 橄榄绿 |
| black | #333333 | 石墨黑 |
| gray | #A9A9A9 | 烟灰色 |
| pink | #FFB7C5 | 樱花粉 |
| red | #A93226 | 中国红 |
| orange | #D97757 | 暖橙（modern 默认） |

**示例：**

```bash
# 基本转换（使用默认主题，移除第一个标题）
${BUN_X} {baseDir}/scripts/main.ts article.md

# 指定主题
${BUN_X} {baseDir}/scripts/main.ts article.md --theme grace

# 主题搭配自定义颜色
${BUN_X} {baseDir}/scripts/main.ts article.md --theme modern --color red

# 启用普通外链底部引用
${BUN_X} {baseDir}/scripts/main.ts article.md --cite

# 保留内容中的第一个标题
${BUN_X} {baseDir}/scripts/main.ts article.md --keep-title

# 覆盖标题
${BUN_X} {baseDir}/scripts/main.ts article.md --title "My Article"
```

## 输出

**文件位置**：与输入 Markdown 文件相同目录。
- 输入：`/path/to/article.md`
- 输出：`/path/to/article.html`

**冲突处理**：如果 HTML 文件已存在，将先进行备份：
- 备份：`/path/to/article.html.bak-YYYYMMDDHHMMSS`

**JSON 输出到 stdout：**

```json
{
  "title": "Article Title",
  "author": "Author Name",
  "summary": "Article summary...",
  "htmlPath": "/path/to/article.html",
  "backupPath": "/path/to/article.html.bak-20260128180000",
  "contentImages": [
    {
      "placeholder": "MDTOHTMLIMGPH_1",
      "localPath": "/path/to/img.png",
      "originalPath": "imgs/image.png"
    }
  ]
}
```

## 主题

| 主题 | 说明 |
|-------|-------------|
| `default` | 经典 - 传统布局，居中标题带底边框，H2 使用白字彩色背景 |
| `grace` | 优雅 - 文字阴影，圆角卡片，精致引用（by @brzhang） |
| `simple` | 极简 - 现代简约，非对称圆角，干净留白（by @okooo5km） |
| `modern` | 现代 - 大圆角，药丸形标题，宽松行高（搭配 `--color red` 呈现传统红金风格） |

## 支持的 Markdown 特性

| 特性 | 语法 |
|---------|--------|
| 标题 | `# H1` 到 `###### H6` |
| 粗体/斜体 | `**粗体**`、`*斜体*` |
| 代码块 | ` ```lang ` 带语法高亮 |
| 行内代码 | `` `code` `` |
| 表格 | GitHub 风格 Markdown 表格 |
| 图片 | `![alt](src)` |
| 链接 | `[text](url)`；添加 `--cite` 将普通外链移至底部引用 |
| 引用 | `> 引用` |
| 列表 | `-` 无序列表，`1.` 有序列表 |
| 提示框 | `> [!NOTE]`、`> [!WARNING]` 等 |
| 脚注 | `[^1]` 引用 |
| 注音 | `{base|annotation}` |
| Mermaid | ` ```mermaid ` 图表 |
| PlantUML | ` ```plantuml ` 图表 |

## 前置元数据

支持 YAML 前置元数据：

```yaml
---
title: Article Title
author: Author Name
description: Article summary
---
```

如果未找到标题，从第一个 H1/H2 标题提取，或使用文件名。

## 扩展支持

通过 EXTEND.md 自定义配置。路径和支持的选项参见**偏好设置**部分。
