---
name: baoyu-danger-x-to-markdown
description: 将 X (Twitter) 的推文和文章转换为带 YAML 前置元数据的 Markdown。使用需要用户授权的逆向工程 API。当用户提到"X to markdown"、"tweet to markdown"、"save tweet"，或提供 x.com/twitter.com URL 要求转换时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-danger-x-to-markdown
    requires:
      anyBins:
        - bun
        - npx
---

# X 转 Markdown

将 X 内容转换为 Markdown：
- 推文/推文串 → 带 YAML 前置元数据的 Markdown
- X Articles → 完整内容提取

## 脚本目录

脚本位于 `scripts/` 子目录。

**路径解析**：
1. `{baseDir}` = 本 SKILL.md 所在目录
2. 脚本路径 = `{baseDir}/scripts/main.ts`
3. 解析 `${BUN_X}` 运行时：如果已安装 `bun` 则使用 `bun`；如果可用 `npx` 则使用 `npx -y bun`；否则提示安装 bun

## 授权要求

**在任何转换之前**，检查并获取用户授权。

### 授权流程

**步骤 1**：检查授权文件

```bash
# macOS
cat ~/Library/Application\ Support/baoyu-skills/x-to-markdown/consent.json

# Linux
cat ~/.local/share/baoyu-skills/x-to-markdown/consent.json
```

**步骤 2**：如果 `accepted: true` 且 `disclaimerVersion: "1.0"` → 打印警告并继续：
```
Warning: Using reverse-engineered X API. Accepted on: <acceptedAt>
```

**步骤 3**：如果缺失或版本不匹配 → 显示免责声明：
```
DISCLAIMER

This tool uses a reverse-engineered X API, NOT official.

Risks:
- May break if X changes API
- No guarantees or support
- Possible account restrictions
- Use at your own risk

Accept terms and continue?
```

使用 `AskUserQuestion`，选项："是的，我接受" | "不，我拒绝"

**步骤 4**：接受后 → 创建授权文件：
```json
{
  "version": 1,
  "accepted": true,
  "acceptedAt": "<ISO timestamp>",
  "disclaimerVersion": "1.0"
}
```

**步骤 5**：拒绝后 → 输出"用户已拒绝。退出。"并停止。

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按以下优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────────┬───────────────────┐
│                            Path                            │     Location      │
├────────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md         │ Project directory │
├────────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md   │ User home         │
└────────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Found     │ Read, parse, apply settings                                               │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Not found │ **必须**运行首次设置（见下文） — 不要静默创建默认值 │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认下载媒体 | 默认输出目录

### 首次设置（阻塞）

**关键**：当未找到 EXTEND.md 时，**必须使用 `AskUserQuestion`** 在创建 EXTEND.md 前询问用户的偏好。**绝不**在未询问的情况下以默认值创建 EXTEND.md。这是一个**阻塞**操作 — 在设置完成之前不要进行任何转换。

使用 `AskUserQuestion` 将所有问题在一次调用中提出：

**问题 1** — 标题："Media"，问题："如何处理推文中的图片和视频？"
- "每次询问（推荐）" — 保存 Markdown 后，询问是否下载媒体
- "始终下载" — 始终将媒体下载到本地 imgs/ 和 videos/ 目录
- "从不下载" — 在 Markdown 中保留原始远程 URL

**问题 2** — 标题："Output"，问题："默认输出目录？"
- "x-to-markdown（推荐）" — 保存到 ./x-to-markdown/{username}/{tweet-id}.md
- （用户可选择"其他"输入自定义路径）

**问题 3** — 标题："Save"，问题："偏好设置保存位置？"
- "User（推荐）" — ~/.baoyu-skills/（所有项目）
- "Project" — .baoyu-skills/（仅当前项目）

用户回答后，在所选位置创建 EXTEND.md，确认"偏好设置已保存到 [路径]"，然后继续。

完整参考：[references/config/first-time-setup.md](references/config/first-time-setup.md)

### 支持的键

| 键 | 默认值 | 值 | 说明 |
|-----|---------|--------|-------------|
| `download_media` | `ask` | `ask` / `1` / `0` | `ask` = 每次询问，`1` = 始终下载，`0` = 从不 |
| `default_output_dir` | 空 | 路径或空 | 默认输出目录（空 = `./x-to-markdown/`） |

**值优先级**：
1. CLI 参数（`--download-media`、`-o`）
2. EXTEND.md
3. 技能默认值

## 用法

```bash
${BUN_X} {baseDir}/scripts/main.ts <url>
${BUN_X} {baseDir}/scripts/main.ts <url> -o output.md
${BUN_X} {baseDir}/scripts/main.ts <url> --download-media
${BUN_X} {baseDir}/scripts/main.ts <url> --json
```

## 选项

| 选项 | 说明 |
|--------|-------------|
| `<url>` | 推文或文章 URL |
| `-o <path>` | 输出路径 |
| `--json` | JSON 输出 |
| `--download-media` | 将图片/视频资源下载到本地 `imgs/` 和 `videos/`，并将 Markdown 链接重写为本地相对路径 |
| `--login` | 仅刷新 Cookie |

## 支持的 URL

- `https://x.com/<user>/status/<id>`
- `https://twitter.com/<user>/status/<id>`
- `https://x.com/i/article/<id>`

## 输出

```markdown
---
url: "https://x.com/user/status/123"
author: "Name (@user)"
tweetCount: 3
coverImage: "https://pbs.twimg.com/media/example.jpg"
---

Content...
```

**文件结构**：`x-to-markdown/{username}/{tweet-id}/{content-slug}.md`

启用 `--download-media` 时：
- 图片保存到 Markdown 文件旁的 `imgs/` 目录
- 视频保存到 Markdown 文件旁的 `videos/` 目录
- Markdown 媒体链接重写为本地相对路径

## 媒体下载工作流

基于 EXTEND.md 中的 `download_media` 设置：

| 设置 | 行为 |
|---------|----------|
| `1`（始终） | 使用 `--download-media` 参数运行脚本 |
| `0`（从不） | 不使用 `--download-media` 参数运行脚本 |
| `ask`（默认） | 按照以下每次询问流程执行 |

### 每次询问流程

1. **不使用** `--download-media` 运行脚本 → Markdown 已保存
2. 检查已保存的 Markdown 中是否有远程媒体 URL（图片/视频链接中的 `https://`）
3. **如果未找到远程媒体** → 完成，无需提示
4. **如果发现远程媒体** → 使用 `AskUserQuestion`：
   - 标题："Media"，问题："下载 N 个图片/视频到本地文件？"
   - "是" — 下载到本地目录
   - "否" — 保留远程 URL
5. 如果用户确认 → **再次**使用 `--download-media` 运行脚本（以本地化链接覆盖 Markdown）

## 身份验证

1. **环境变量**（推荐）：`X_AUTH_TOKEN`、`X_CT0`
2. **Chrome 登录**（回退）：自动打开 Chrome，将 Cookie 缓存到本地

## 扩展支持

通过 EXTEND.md 自定义配置。路径和支持的选项参见**偏好设置**部分。
