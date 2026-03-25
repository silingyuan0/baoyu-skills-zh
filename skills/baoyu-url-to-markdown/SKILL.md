---
name: baoyu-url-to-markdown
description: 通过 Chrome CDP 获取任意 URL 并转换为 Markdown。保存渲染后的 HTML 快照，使用升级版 Defuddle 管道（改进的 Web Component 处理和 YouTube 字幕提取），需要时自动回退到预 Defuddle 的 HTML 转 Markdown 管道。如果本地浏览器捕获完全失败，可回退到托管的 defuddle.md API。支持两种模式 - 页面加载后自动捕获，或等待用户信号（适用于需要登录的页面）。当用户要求将网页保存为 Markdown 时使用。
version: 1.58.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-url-to-markdown
    requires:
      anyBins:
        - bun
        - npx
---

# URL 转 Markdown

通过 Chrome CDP 获取任意 URL，保存渲染后的 HTML 快照，并转换为干净的 Markdown。

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
| `scripts/main.ts` | URL 获取命令行入口 |
| `scripts/html-to-markdown.ts` | Markdown 转换入口和转换器选择 |
| `scripts/defuddle-converter.ts` | 基于 Defuddle 的转换 |
| `scripts/legacy-converter.ts` | 预 Defuddle 的旧版提取和 Markdown 转换 |
| `scripts/markdown-conversion-shared.ts` | 共享的元数据解析和 Markdown 文档辅助工具 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按以下优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-url-to-markdown/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-url-to-markdown/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-url-to-markdown/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-url-to-markdown/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────┬───────────────────┐
│                          Path                          │     Location      │
├────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-url-to-markdown/EXTEND.md          │ Project directory │
├────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md    │ User home         │
└────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Found     │ Read, parse, apply settings                                               │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ Not found │ **必须**运行首次设置（见下文） — 不要静默创建默认值 │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认下载媒体 | 默认输出目录 | 默认捕获模式 | 超时设置

### 首次设置（阻塞）

**关键**：当未找到 EXTEND.md 时，**必须使用 `AskUserQuestion`** 在创建 EXTEND.md 前询问用户的偏好。**绝不**在未询问的情况下以默认值创建 EXTEND.md。这是一个**阻塞**操作 — 在设置完成之前不要进行任何转换。

使用 `AskUserQuestion` 将所有问题在一次调用中提出：

**问题 1** — 标题："Media"，问题："如何处理页面中的图片和视频？"
- "每次询问（推荐）" — 保存 Markdown 后，询问是否下载媒体
- "始终下载" — 始终将媒体下载到本地 imgs/ 和 videos/ 目录
- "从不下载" — 在 Markdown 中保留原始远程 URL

**问题 2** — 标题："Output"，问题："默认输出目录？"
- "url-to-markdown（推荐）" — 保存到 ./url-to-markdown/{domain}/{slug}.md
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
| `default_output_dir` | 空 | 路径或空 | 默认输出目录（空 = `./url-to-markdown/`） |

**EXTEND.md → CLI 映射**：
| EXTEND.md 键 | CLI 参数 | 备注 |
|---------------|-------------|-------|
| `download_media: 1` | `--download-media` | |
| `default_output_dir: ./posts/` | `--output-dir ./posts/` | 目录路径。不要传给 `-o`（`-o` 期望文件路径） |

**值优先级**：
1. CLI 参数（`--download-media`、`-o`、`--output-dir`）
2. EXTEND.md
3. 技能默认值

## 功能特性

- 通过 Chrome CDP 实现完整 JavaScript 渲染
- 两种捕获模式：自动或等待用户
- 将渲染后的 HTML 保存为同目录下的 `-captured.html` 文件
- 带元数据的干净 Markdown 输出
- 升级版 Defuddle 优先的 Markdown 转换，自动回退到从 git 历史恢复的预 Defuddle 提取器
- 在转换前将影子 DOM 内容实例化，使 Web Component 页面能更好地序列化
- YouTube 页面可在 YouTube 提供字幕轨道时将字幕文本包含在 Markdown 中
- 如果本地浏览器捕获完全失败，可回退到 `defuddle.md/<url>` 仍能保存 Markdown
- 通过等待模式处理需要登录的页面
- 下载图片和视频到本地目录

## 用法

```bash
# 自动模式（默认）- 页面加载后捕获
${BUN_X} {baseDir}/scripts/main.ts <url>

# 等待模式 - 等待用户信号后再捕获
${BUN_X} {baseDir}/scripts/main.ts <url> --wait

# 保存到指定文件
${BUN_X} {baseDir}/scripts/main.ts <url> -o output.md

# 保存到自定义输出目录（自动生成文件名）
${BUN_X} {baseDir}/scripts/main.ts <url> --output-dir ./posts/

# 下载图片和视频到本地目录
${BUN_X} {baseDir}/scripts/main.ts <url> --download-media
```

## 选项

| 选项 | 说明 |
|--------|-------------|
| `<url>` | 要获取的 URL |
| `-o <path>` | 输出文件路径 — 必须是**文件**路径，不是目录（默认：自动生成） |
| `--output-dir <dir>` | 基础输出目录 — 自动生成 `{dir}/{domain}/{slug}.md`（默认：`./url-to-markdown/`） |
| `--wait` | 等待用户信号后再捕获 |
| `--timeout <ms>` | 页面加载超时时间（默认：30000） |
| `--download-media` | 将图片/视频资源下载到本地 `imgs/` 和 `videos/`，并将 Markdown 链接重写为本地相对路径 |

## 捕获模式

| 模式 | 行为 | 适用场景 |
|------|----------|----------|
| 自动（默认） | 网络空闲时捕获 | 公开页面、静态内容 |
| 等待（`--wait`） | 用户发出就绪信号后捕获 | 需要登录、懒加载、付费墙 |

**等待模式工作流**：
1. 使用 `--wait` 运行 → 脚本输出"Press Enter when ready"
2. 要求用户确认页面已就绪
3. 向 stdin 发送换行符以触发捕获

## 输出格式

每次运行保存两个文件：

- Markdown：YAML 前置元数据包含 `url`、`title`、`description`、`author`、`published`、可选 `coverImage` 和 `captured_at`，后跟转换后的 Markdown 内容
- HTML 快照：`*-captured.html`，包含从 Chrome 捕获的渲染页面 HTML

当 Defuddle 或页面元数据提供语言提示时，Markdown 前置元数据也会包含 `language`。

HTML 快照在任何 Markdown 媒体本地化之前保存，因此它保持为用于转换的页面 DOM 的忠实快照。
如果使用了托管的 `defuddle.md` API 回退，Markdown 仍会保存，但该次运行不会有本地 `-captured.html` 快照。

## 输出目录

默认：`url-to-markdown/<domain>/<slug>.md`
使用 `--output-dir ./posts/` 时：`./posts/<domain>/<slug>.md`

HTML 快照路径使用相同的基本名：

- `url-to-markdown/<domain>/<slug>-captured.html`
- `./posts/<domain>/<slug>-captured.html`

- `<slug>`：来自页面标题或 URL 路径（kebab-case，2-6 个单词）
- 冲突解决：追加时间戳 `<slug>-YYYYMMDD-HHMMSS.md`

启用 `--download-media` 时：
- 图片保存到 Markdown 文件旁的 `imgs/` 目录
- 视频保存到 Markdown 文件旁的 `videos/` 目录
- Markdown 媒体链接重写为本地相对路径

## 转换回退

转换顺序：

1. 首先尝试 Defuddle
2. 对于 YouTube 等富内容页面，优先使用 Defuddle 的特定提取器输出（包括可用的字幕），而不是用旧版管道替换
3. 如果 Defuddle 抛出异常、无法加载、返回明显不完整的 Markdown，或捕获的内容质量低于旧版管道，自动回退到预 Defuddle 提取器
4. 如果整个本地浏览器捕获流程在生成 Markdown 之前失败，尝试托管的 `https://defuddle.md/<url>` API 并直接保存其 Markdown 输出
5. 旧版回退路径使用从 git 历史恢复的基于 Readability/selector/Next.js-data 的 HTML 转 Markdown 实现

CLI 输出将显示：

- Defuddle 成功时显示 `Converter: defuddle`
- 需要回退时显示 `Converter: legacy:...` 加 `Fallback used: ...`
- 本地浏览器捕获失败并使用托管 API 时显示 `Converter: defuddle-api`

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

## 环境变量

| 变量 | 说明 |
|----------|-------------|
| `URL_CHROME_PATH` | 自定义 Chrome 可执行文件路径 |
| `URL_DATA_DIR` | 自定义数据目录 |
| `URL_CHROME_PROFILE_DIR` | 自定义 Chrome 配置文件目录 |

**故障排除**：Chrome 未找到 → 设置 `URL_CHROME_PATH`。超时 → 增加 `--timeout`。复杂页面 → 尝试 `--wait` 模式。如果 Markdown 质量不佳，检查已保存的 `-captured.html` 并查看运行日志是否记录了旧版回退。

### YouTube 说明

- 升级版 Defuddle 路径使用异步提取器，因此 YouTube 页面可直接在 Markdown 正文中包含字幕文本。
- 字幕可用性取决于 YouTube 提供字幕轨道。字幕已禁用、播放受限或区域访问被阻止的视频可能仍仅生成描述内容。
- 如果页面需要时间完成加载描述、章节或播放器元数据，建议使用 `--wait` 并在观看页面完全加载后捕获。

### 托管 API 回退

- 托管回退端点为 `https://defuddle.md/<url>`。Shell 形式：`curl https://defuddle.md/stephango.com`
- 仅在本地 Chrome/CDP 捕获路径完全失败时使用。本地路径保真度更高，因为它可以保存捕获的 HTML 并处理已认证的页面。
- 托管 API 已返回带 YAML 前置元数据的 Markdown，因此直接保存该响应，如果要求则在之后执行正常的媒体本地化步骤。

## 扩展支持

通过 EXTEND.md 自定义配置。路径和支持的选项参见**偏好设置**部分。
