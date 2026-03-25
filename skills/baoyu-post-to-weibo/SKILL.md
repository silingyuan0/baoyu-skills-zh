---
name: baoyu-post-to-weibo
description: 将内容发布到微博。支持带文本、图片和视频的普通帖子，以及通过 Chrome CDP 以 Markdown 输入发布头条文章。当用户要求"发微博"、"发布微博"、"post to Weibo"、"share on Weibo"、"写微博"或"微博头条文章"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-weibo
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到微博

通过真实 Chrome 浏览器（绕过反机器人检测）将文本、图片、视频和长文发布到微博。

## 脚本目录

**重要**：所有脚本位于本技能的 `scripts/` 子目录中。

**代理执行说明**：
1. 将本 SKILL.md 文件的目录路径确定为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 将本文档中所有 `{baseDir}` 替换为实际路径
4. 解析 `${BUN_X}` 运行时：如果已安装 `bun` 则使用 `bun`；如果可用 `npx` 则使用 `npx -y bun`；否则提示安装 bun

**脚本参考**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/weibo-post.ts` | 普通帖子（文本 + 图片） |
| `scripts/weibo-article.ts` | 头条文章发布（Markdown） |
| `scripts/copy-to-clipboard.ts` | 复制内容到剪贴板 |
| `scripts/paste-from-clipboard.ts` | 发送真实粘贴按键 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按以下优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-weibo/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-weibo/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-weibo/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-weibo/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────┬───────────────────┐
│                       Path                       │     Location      │
├──────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-weibo/EXTEND.md      │ Project directory │
├──────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md│ User home         │
└──────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Found     │ Read, parse, apply settings                                               │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Not found │ Use defaults                                                              │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认 Chrome 配置文件

## 前提条件

- Google Chrome 或 Chromium
- `bun` 运行时
- 首次运行：手动登录微博（会话将保存）

---

## 普通帖子

文本 + 图片/视频（最多 18 个文件）。在微博首页发布。

```bash
${BUN_X} {baseDir}/scripts/weibo-post.ts "Hello Weibo!" --image ./photo.png
${BUN_X} {baseDir}/scripts/weibo-post.ts "Watch this" --video ./clip.mp4
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<text>` | 帖子内容（位置参数） |
| `--image <path>` | 图片文件（可重复） |
| `--video <path>` | 视频文件（可重复） |
| `--profile <dir>` | 自定义 Chrome 配置文件 |

**注意**：脚本打开浏览器并填入内容。用户需手动审核后发布。

---

## 头条文章

以 Markdown 格式的长文发布到 `https://card.weibo.com/article/v3/editor`。

```bash
${BUN_X} {baseDir}/scripts/weibo-article.ts article.md
${BUN_X} {baseDir}/scripts/weibo-article.ts article.md --cover ./cover.jpg
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<markdown>` | Markdown 文件（位置参数） |
| `--cover <path>` | 封面图 |
| `--title <text>` | 覆盖标题（最多 32 字符，超出则截断） |
| `--summary <text>` | 覆盖摘要（最多 44 字符，超出则自动重新生成） |
| `--profile <dir>` | 自定义 Chrome 配置文件 |

**前置元数据**：支持在 YAML 前置元数据中使用 `title`、`summary`、`cover_image`。

**字数限制**：
- 标题：最多 32 字符（超出则截断并警告）
- 摘要/导语：最多 44 字符（超出则从内容自动重新生成）

**文章发布流程**：
1. 打开 `https://card.weibo.com/article/v3/editor`
2. 点击"写文章"按钮，等待编辑器变为可编辑状态
3. 填入标题（验证 32 字符限制）
4. 填入摘要/导语（验证 44 字符限制）
5. 通过粘贴将 HTML 内容插入 ProseMirror 编辑器
6. 逐个替换图片占位符（复制图片 → 选中占位符 → 粘贴）

**发布后检查**：脚本在所有图片插入后自动验证：
- 编辑器内容中剩余的 `WBIMGPH_` 占位符
- 预期与实际图片数量

如果检查失败（输出中有警告），在用户发布前提醒具体问题。

---

## 故障排除

### Chrome 调试端口未就绪

如果脚本因 `Chrome debug port not ready` 或 `Unable to connect` 而失败，先终止现有 Chrome CDP 实例，然后重试：

```bash
pkill -f "Chrome.*remote-debugging-port" 2>/dev/null; pkill -f "Chromium.*remote-debugging-port" 2>/dev/null; sleep 2
```

**重要**：此操作应自动执行 — 遇到此错误时，终止 Chrome CDP 实例并重试命令，无需询问用户。

## 备注

- 首次运行：需要手动登录（会话将持久保存）
- 所有脚本仅在浏览器中填入内容，用户必须手动审核后发布
- 跨平台：macOS、Linux、Windows

## 扩展支持

通过 EXTEND.md 自定义配置。路径和支持的选项参见**偏好设置**部分。
