---
name: baoyu-danger-gemini-web
description: 通过逆向工程的 Gemini Web API 生成文本和图片。支持文本生成、提示词图片生成、参考图片视觉输入和多轮对话。当其他技能需要图片生成后端，或用户要求"使用 Gemini 生成图片"、"Gemini 文本生成"，或需要具备视觉能力的 AI 生成时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-danger-gemini-web
    requires:
      anyBins:
        - bun
        - npx
---

# Gemini Web 客户端

通过 Gemini Web API 生成文本/图片。支持参考图片和多轮对话。

## 脚本目录

**重要**：所有脚本位于本技能的 `scripts/` 子目录中。

**代理执行说明**：
1. 将本 SKILL.md 文件的目录路径确定为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 解析 `${BUN_X}` 运行时：如果已安装 `bun` 则使用 `bun`；如果可用 `npx` 则使用 `npx -y bun`；否则提示安装 bun
4. 将本文档中所有 `{baseDir}` 和 `${BUN_X}` 替换为实际值

**脚本参考**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 文本/图片生成命令行入口 |
| `scripts/gemini-webapi/*` | `gemini_webapi` 的 TypeScript 移植（GeminiClient、类型、工具函数） |

## 同意检查（必需）

首次使用前，验证用户是否同意使用逆向工程 API。

**同意文件位置**：
- macOS: `~/Library/Application Support/baoyu-skills/gemini-web/consent.json`
- Linux: `~/.local/share/baoyu-skills/gemini-web/consent.json`
- Windows: `%APPDATA%\baoyu-skills\gemini-web\consent.json`

**流程**：
1. 检查同意文件是否存在且包含 `accepted: true` 和 `disclaimerVersion: "1.0"`
2. 如果存在有效同意 → 打印包含 `acceptedAt` 日期的警告，继续执行
3. 如果无同意记录 → 显示免责声明，通过 `AskUserQuestion` 询问用户：
   - "是的，我接受" → 创建包含 ISO 时间戳的同意文件，继续执行
   - "不，我拒绝" → 输出拒绝消息，停止执行
4. 同意文件格式：`{"version":1,"accepted":true,"acceptedAt":"<ISO>","disclaimerVersion":"1.0"}`

---

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按以下优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-danger-gemini-web/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-danger-gemini-web/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────────────┬───────────────────┐
│                           Path                           │     Location      │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md          │ Project directory │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md    │ User home         │
└──────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Found     │ Read, parse, apply settings                                               │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Not found │ Use defaults                                                              │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认模型 | 代理设置 | 自定义数据目录

## 用法

```bash
# 文本生成
${BUN_X} {baseDir}/scripts/main.ts "Your prompt"
${BUN_X} {baseDir}/scripts/main.ts --prompt "Your prompt" --model gemini-3-flash

# 图片生成
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cute cat" --image cat.png
${BUN_X} {baseDir}/scripts/main.ts --promptfiles system.md content.md --image out.png

# 视觉输入（参考图片）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Describe this" --reference image.png
${BUN_X} {baseDir}/scripts/main.ts --prompt "Create variation" --reference a.png --image out.png

# 多轮对话
${BUN_X} {baseDir}/scripts/main.ts "Remember: 42" --sessionId session-abc
${BUN_X} {baseDir}/scripts/main.ts "What number?" --sessionId session-abc

# JSON 输出
${BUN_X} {baseDir}/scripts/main.ts "Hello" --json
```

## 选项

| 选项 | 说明 |
|--------|-------------|
| `--prompt`, `-p` | 提示词文本 |
| `--promptfiles` | 从文件读取提示词（拼接） |
| `--model`, `-m` | 模型：gemini-3-pro（默认）、gemini-3-flash、gemini-3-flash-thinking、gemini-3.1-pro-preview |
| `--image [path]` | 生成图片（默认：generated.png） |
| `--reference`, `--ref` | 用于视觉输入的参考图片 |
| `--sessionId` | 多轮对话的会话 ID |
| `--list-sessions` | 列出已保存的会话 |
| `--json` | JSON 格式输出 |
| `--login` | 刷新 Cookie，然后退出 |
| `--cookie-path` | 自定义 Cookie 文件路径 |
| `--profile-dir` | Chrome 配置文件目录 |

## 模型

| 模型 | 说明 |
|-------|-------------|
| `gemini-3-pro` | 默认，最新的 3.0 Pro |
| `gemini-3-flash` | 快速、轻量的 3.0 Flash |
| `gemini-3-flash-thinking` | 带思考能力的 3.0 Flash |
| `gemini-3.1-pro-preview` | 3.1 Pro 预览版（空请求头，自动路由） |

## 身份验证

首次运行打开浏览器进行 Google 登录。Cookie 自动缓存。

支持的浏览器（自动检测）：Chrome、Chrome Canary/Beta、Chromium、Edge。

强制刷新：`--login` 参数。覆盖浏览器：`GEMINI_WEB_CHROME_PATH` 环境变量。

## 环境变量

| 变量 | 说明 |
|----------|-------------|
| `GEMINI_WEB_DATA_DIR` | 数据目录 |
| `GEMINI_WEB_COOKIE_PATH` | Cookie 文件路径 |
| `GEMINI_WEB_CHROME_PROFILE_DIR` | Chrome 配置文件目录 |
| `GEMINI_WEB_CHROME_PATH` | Chrome 可执行文件路径 |
| `HTTP_PROXY`, `HTTPS_PROXY` | 用于访问 Google 的代理（与命令内联设置） |

## 会话

会话文件存储在数据目录的 `sessions/<id>.json` 下。

包含：`id`、`metadata`（Gemini 聊天状态）、`messages` 数组、时间戳。

## 扩展支持

通过 EXTEND.md 自定义配置。路径和支持的选项参见**偏好设置**部分。
