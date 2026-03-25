---
name: baoyu-post-to-x
description: 向 X（Twitter）发布内容和文章。支持带图片/视频的常规帖子和 X 文章（长篇 Markdown）。使用真实 Chrome 配合 CDP 绕过反自动化检测。当用户要求"发布到 X"、"发推"、"发布到 Twitter"或"分享到 X"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-x
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到 X（Twitter）

通过真实 Chrome 浏览器（绕过反自动化检测）发布文本、图片、视频和长篇文章到 X。

## 脚本目录

**重要**：所有脚本位于本 skill 的 `scripts/` 子目录中。

**智能体执行说明**：
1. 将此 SKILL.md 文件所在目录路径确定为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<脚本名>.ts`
3. 将本文档中所有 `{baseDir}` 替换为实际路径
4. 解析 `${BUN_X}` 运行时：如已安装 `bun` → `bun`；如 `npx` 可用 → `npx -y bun`；否则建议安装 bun

**脚本参考**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/x-browser.ts` | 常规帖子（文本 + 图片）|
| `scripts/x-video.ts` | 视频帖子（文本 + 视频）|
| `scripts/x-quote.ts` | 带评论的引用推文 |
| `scripts/x-article.ts` | 长篇文章发布（Markdown）|
| `scripts/md-to-html.ts` | Markdown → HTML 转换 |
| `scripts/copy-to-clipboard.ts` | 内容复制到剪贴板 |
| `scripts/paste-from-clipboard.ts` | 发送真实粘贴按键 |
| `scripts/check-paste-permissions.ts` | 验证环境与权限 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-x/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-x/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-x/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-x/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────┬───────────────────┐
│                       路径                        │       位置        │
├──────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-x/EXTEND.md          │ 项目目录         │
├──────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md    │ 用户主目录        │
└──────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                    │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到     │ 读取、解析、应用设置                                                       │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到   │ 使用默认值                                                                │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认 Chrome 配置文件

## 前置要求

- Google Chrome 或 Chromium
- `bun` 运行时
- 首次运行：手动登录 X（会话保存）

## 飞行前检查（可选）

首次使用前，建议运行环境检查。如用户偏好，可跳过。

```bash
${BUN_X} {baseDir}/scripts/check-paste-permissions.ts
```

检查项：Chrome、配置文件隔离、Bun、辅助功能、剪贴板、粘贴按键、Chrome 冲突。

**如有任何检查失败**，按项目提供修复指导：

| 检查项 | 修复 |
|-------|-----|
| Chrome | 安装 Chrome 或设置 `X_BROWSER_CHROME_PATH` 环境变量 |
| 配置文件目录 | 共享配置文件位于 `baoyu-skills/chrome-profile`（参见 CLAUDE.md Chrome 配置文件章节）|
| Bun 运行时 | `brew install oven-sh/bun/bun`（macOS）或 `npm install -g bun` |
| 辅助功能（macOS）| 系统设置 → 隐私与安全 → 辅助功能 → 启用终端应用 |
| 剪贴板复制 | 确保 Swift/AppKit 可用（macOS Xcode CLI 工具：`xcode-select --install`）|
| 粘贴按键（macOS）| 同辅助功能修复 |
| 粘贴按键（Linux）| 安装 `xdotool`（X11）或 `ydotool`（Wayland）|

## 参考

- **常规帖子**：参见 `references/regular-posts.md` 了解手动工作流、故障排除和技术细节
- **X 文章**：参见 `references/articles.md` 了解长篇文章发布指南

---

## 帖子类型选择

除非用户明确指定帖子类型：
- **纯文本** + 少于 10,000 字符 → **常规帖子**（Premium 会员支持最多 10,000 字符，非 Premium：280）
- **Markdown 文件**（.md）→ **X 文章**

## 常规帖子

```bash
${BUN_X} {baseDir}/scripts/x-browser.ts "Hello!" --image ./photo.png
```

**参数**：
| 参数 | 描述 |
|-----------|-------------|
| `<text>` | 帖子内容（位置参数）|
| `--image <path>` | 图片文件（可重复，最多 4 张）|
| `--profile <dir>` | 自定义 Chrome 配置文件 |

**注**：脚本打开浏览器并填入内容。用户审核并手动发布。

---

## 视频帖子

文本 + 视频文件。

```bash
${BUN_X} {baseDir}/scripts/x-video.ts "Check this out!" --video ./clip.mp4
```

**参数**：
| 参数 | 描述 |
|-----------|-------------|
| `<text>` | 帖子内容（位置参数）|
| `--video <path>` | 视频文件（MP4、MOV、WebM）|
| `--profile <dir>` | 自定义 Chrome 配置文件 |

**注**：脚本打开浏览器并填入内容。用户审核并手动发布。

**限制**：常规 140 秒最长，Premium 60 分钟。处理时间：30-60 秒。

---

## 引用推文

带评论引用现有推文。

```bash
${BUN_X} {baseDir}/scripts/x-quote.ts https://x.com/user/status/123 "Great insight!"
```

**参数**：
| 参数 | 描述 |
|-----------|-------------|
| `<tweet-url>` | 要引用的 URL（位置参数）|
| `<comment>` | 评论文本（位置参数，可选）|
| `--profile <dir>` | 自定义 Chrome 配置文件 |

**注**：脚本打开浏览器并填入内容。用户审核并手动发布。

---

## X 文章

长篇 Markdown 文章（需要 X Premium）。

```bash
${BUN_X} {baseDir}/scripts/x-article.ts article.md
${BUN_X} {baseDir}/scripts/x-article.ts article.md --cover ./cover.jpg
```

**参数**：
| 参数 | 描述 |
|-----------|-------------|
| `<markdown>` | Markdown 文件（位置参数）|
| `--cover <path>` | 封面图片 |
| `--title <text>` | 覆盖标题 |

**前置元数据**：支持 YAML 前置元数据中的 `title`、`cover_image`。

**注**：脚本打开浏览器并填入文章内容。用户审核并手动发布。

**发布后检查**：脚本在所有图片插入后自动验证：
- 编辑器内容中剩余的 `XIMGPH_` 占位符
- 预期 vs 实际图片数量

如检查失败（输出中有警告），在用户发布前提醒具体问题。

---

## 故障排除

### Chrome 调试端口未就绪

如脚本失败并显示 `Chrome debug port not ready` 或 `Unable to connect`，首先终止现有 Chrome CDP 实例，然后重试：

```bash
pkill -f "Chrome.*remote-debugging-port" 2>/dev/null; pkill -f "Chromium.*remote-debugging-port" 2>/dev/null; sleep 2
```

**重要**：这应自动完成——遇到此错误时，终止 Chrome CDP 实例并重试命令，无需询问用户。

## 注意事项

- 首次运行：需要手动登录（会话保持）
- 所有脚本仅将内容填入浏览器，用户必须审核并手动发布
- 跨平台：macOS、Linux、Windows

## 扩展支持

通过 EXTEND.md 自定义配置。见**偏好设置**章节了解路径和支持的选项。
