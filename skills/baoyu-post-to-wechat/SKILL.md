---
name: baoyu-post-to-wechat
description: 通过 API 或 Chrome CDP 向微信公众号发布内容。支持文章发表（文章）使用 HTML、markdown 或纯文本输入，以及贴图发表（贴图，原图文）多图发布。Markdown 文章工作流默认将普通外部链接转换为底部引用以适配微信输出。当用户提及"发布公众号"、"post to wechat"、"微信公众号"或"贴图/图文/文章"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-wechat
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到微信公众号

## 语言

**匹配用户语言**：使用与用户相同的语言回复。如果用户用中文写作，用中文回复。如果用户用英文写作，用英文回复。

## 脚本目录

**智能体执行**：将此 SKILL.md 目录确定为 `{baseDir}`，然后使用 `{baseDir}/scripts/<name>.ts`。解析 `${BUN_X}` 运行时：如果已安装 `bun` → `bun`；如果 `npx` 可用 → `npx -y bun`；否则建议安装 bun。

| 脚本 | 用途 |
|--------|---------|
| `scripts/wechat-browser.ts` | 贴图发布（图文） |
| `scripts/wechat-article.ts` | 通过浏览器发布文章（文章） |
| `scripts/wechat-api.ts` | 通过 API 发布文章（文章） |
| `scripts/md-to-wechat.ts` | Markdown → 微信适配 HTML（带图片占位符） |
| `scripts/check-permissions.ts` | 验证环境与权限 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-wechat/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-wechat/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────┬───────────────────┐
│                          路径                            │       位置        │
├────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-wechat/EXTEND.md           │ 项目目录         │
├────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md     │ 用户主目录        │
└────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                    │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到     │ 读取、解析、应用设置                                                       │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到   │ 运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 保存 → 继续 │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认主题 | 默认颜色 | 默认发布方式（api/browser）| 默认作者 | 默认开启评论开关 | 默认粉丝评论开关 | Chrome 配置文件路径

首次设置：[references/config/first-time-setup.md](references/config/first-time-setup.md)

**最低支持键名**（不区分大小写，接受 `1/0` 或 `true/false`）：

| 键名 | 默认值 | 映射 |
|-----|---------|---------|
| `default_author` | 空 | 当 CLI/前置元数据未提供时作为 `author` 的备选 |
| `need_open_comment` | `1` | `draft/add` 请求中 `articles[].need_open_comment` |
| `only_fans_can_comment` | `0` | `draft/add` 请求中 `articles[].only_fans_can_comment` |

**推荐的 EXTEND.md 示例**：

```md
default_theme: default
default_color: blue
default_publish_method: api
default_author: 宝玉
need_open_comment: 1
only_fans_can_comment: 0
chrome_profile_path: /path/to/chrome/profile
```

**主题选项**：default、grace、simple、modern

**颜色预设**：blue、green、vermilion、yellow、purple、sky、rose、olive、black、gray、pink、red、orange（或十六进制值）

**值优先级**：
1. CLI 参数
2. 前置元数据
3. EXTEND.md（账号级 → 全局级）
4. 技能默认值

## 多账号支持

EXTEND.md 支持管理多个微信公众号。当存在 `accounts:` 块时，每个账号可拥有自己的凭据、Chrome 配置文件和默认设置。

**兼容性规则**：

| 条件 | 模式 | 行为 |
|-----------|------|----------|
| 无 `accounts` 块 | 单账号 | 当前行为，不变 |
| `accounts` 有 1 条目 | 单账号 | 自动选择，不提示 |
| `accounts` 有 2+ 条目 | 多账号 | 发布前提示选择 |
| `accounts` 有 `default: true` | 多账号 | 预选择，可切换 |

**多账号 EXTEND.md 示例**：

```md
default_theme: default
default_color: blue

accounts:
  - name: 宝玉的技术分享
    alias: baoyu
    default: true
    default_publish_method: api
    default_author: 宝玉
    need_open_comment: 1
    only_fans_can_comment: 0
    app_id: your_wechat_app_id
    app_secret: your_wechat_app_secret
  - name: AI工具集
    alias: ai-tools
    default_publish_method: browser
    default_author: AI工具集
    need_open_comment: 1
    only_fans_can_comment: 0
```

**每账号键名**（可每账号设置或作为全局备选）：
`default_publish_method`、`default_author`、`need_open_comment`、`only_fans_can_comment`、`app_id`、`app_secret`、`chrome_profile_path`

**仅全局键名**（始终跨账号共享）：
`default_theme`、`default_color`

### 账号选择（Step 0.5）

在文章发表工作流的 Step 0 和 Step 1 之间插入：

```
if no accounts block:
    → single-account mode (current behavior)
elif accounts.length == 1:
    → auto-select the only account
elif --account <alias> CLI arg:
    → select matching account
elif one account has default: true:
    → pre-select, show: "Using account: <name> (--account to switch)"
else:
    → prompt user:
      "Multiple WeChat accounts configured:
       1) <name1> (<alias1>)
       2) <name2> (<alias2>)
       Select account [1-N]:"
```

### 凭据解析（API 方式）

对于别名 `{alias}` 的选中账号：

1. EXTEND.md 账号块内联 `app_id` / `app_secret`
2. 环境变量 `WECHAT_{ALIAS}_APP_ID` / `WECHAT_{ALIAS}_APP_SECRET`（别名大写，连字符 → 下划线）
3. `.baoyu-skills/.env` 带前缀键 `WECHAT_{ALIAS}_APP_ID`
4. `~/.baoyu-skills/.env` 带前缀键
5. 备选无前缀 `WECHAT_APP_ID` / `WECHAT_APP_SECRET`

**.env 多账号示例**：

```bash
# Account: baoyu
WECHAT_BAOYU_APP_ID=your_wechat_app_id
WECHAT_BAOYU_APP_SECRET=your_wechat_app_secret

# Account: ai-tools
WECHAT_AI_TOOLS_APP_ID=your_ai_tools_wechat_app_id
WECHAT_AI_TOOLS_APP_SECRET=your_ai_tools_wechat_app_secret
```

### Chrome 配置文件（浏览器方式）

每个账号使用独立 Chrome 配置文件以保持独立登录会话：

| 来源 | 路径 |
|--------|------|
| EXTEND.md 中账号 `chrome_profile_path` | 原样使用 |
| 从别名自动生成 | `{shared_profile_parent}/wechat-{alias}/` |
| 单账号备选 | 共享默认配置文件（当前行为） |

### CLI `--account` 参数

所有发布脚本接受 `--account <alias>`：

```bash
${BUN_X} {baseDir}/scripts/wechat-api.ts <file> --theme default --account ai-tools
${BUN_X} {baseDir}/scripts/wechat-article.ts --markdown <file> --theme default --account baoyu
${BUN_X} {baseDir}/scripts/wechat-browser.ts --markdown <file> --images ./photos/ --account baoyu
```

## 飞行前检查（可选）

首次使用前，建议运行环境检查。如用户偏好，可跳过。

```bash
${BUN_X} {baseDir}/scripts/check-permissions.ts
```

检查项：Chrome、配置文件隔离、Bun、辅助功能、剪贴板、粘贴按键、API 凭据、Chrome 冲突。

**如有任何检查失败**，按项目提供修复指导：

| 检查项 | 修复 |
|-------|-----|
| Chrome | 安装 Chrome 或设置 `WECHAT_BROWSER_CHROME_PATH` 环境变量 |
| 配置文件目录 | 共享配置文件位于 `baoyu-skills/chrome-profile`（参见 CLAUDE.md Chrome 配置文件章节）|
| Bun 运行时 | `brew install oven-sh/bun/bun`（macOS）或 `npm install -g bun` |
| 辅助功能（macOS）| 系统设置 → 隐私与安全 → 辅助功能 → 启用终端应用 |
| 剪贴板复制 | 确保 Swift/AppKit 可用（macOS Xcode CLI 工具：`xcode-select --install`）|
| 粘贴按键（macOS）| 同辅助功能修复 |
| 粘贴按键（Linux）| 安装 `xdotool`（X11）或 `ydotool`（Wayland）|
| API 凭据 | 在 Step 2 跟随引导设置，或手动设置到 `.baoyu-skills/.env` |

## 贴图发表（图文）

适用于带多图（最多 9 张）的短帖子：

```bash
${BUN_X} {baseDir}/scripts/wechat-browser.ts --markdown article.md --images ./images/
${BUN_X} {baseDir}/scripts/wechat-browser.ts --title "标题" --content "内容" --image img.png --submit
```

详见 [references/image-text-posting.md](references/image-text-posting.md)。

## 文章发表工作流（文章）

复制此清单并在完成时勾选：

```
Publishing Progress:
- [ ] Step 0: Load preferences (EXTEND.md)
- [ ] Step 0.5: Resolve account (multi-account only)
- [ ] Step 1: Determine input type
- [ ] Step 2: Select method and configure credentials
- [ ] Step 3: Resolve theme/color and validate metadata
- [ ] Step 4: Publish to WeChat
- [ ] Step 5: Report completion
```

### Step 0: 加载偏好设置

检查并加载 EXTEND.md 设置（见上文偏好设置）。

**关键**：如未找到，在任何其他步骤或问题之前完成首次设置。

解析并存储以下默认值供后续步骤使用：
- `default_theme`（默认 `default`）
- `default_color`（如未设置则省略——应用主题默认）
- `default_author`
- `need_open_comment`（默认 `1`）
- `only_fans_can_comment`（默认 `0`）

### Step 1: 确定输入类型

| 输入类型 | 检测 | 操作 |
|------------|-----------|--------|
| HTML 文件 | 路径以 `.html` 结尾，文件存在 | 跳到 Step 3 |
| Markdown 文件 | 路径以 `.md` 结尾，文件存在 | 继续到 Step 2 |
| 纯文本 | 非文件路径，或文件不存在 | 保存为 markdown，继续到 Step 2 |

**纯文本处理**：

1. 从内容生成 slug（前 2-4 个有意义的词，kebab-case）
2. 创建目录并保存文件：

```bash
mkdir -p "$(pwd)/post-to-wechat/$(date +%Y-%m-%d)"
# 保存到: post-to-wechat/yyyy-MM-dd/[slug].md
```

3. 继续作为 markdown 文件处理

**Slug 示例**：
- "Understanding AI Models" → `understanding-ai-models`
- "人工智能的未来" → `ai-future`（翻译为英文用于 slug）

### Step 2: 选择发布方式并配置

**询问发布方式**（除非在 EXTEND.md 或 CLI 中指定）：

| 方式 | 速度 | 要求 |
|--------|-------|--------------|
| `api`（推荐） | 快 | API 凭据 |
| `browser` | 慢 | Chrome、登录会话 |

**如选择 API — 检查凭据**：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/.env && grep -q "WECHAT_APP_ID" .baoyu-skills/.env && echo "project"
test -f "$HOME/.baoyu-skills/.env" && grep -q "WECHAT_APP_ID" "$HOME/.baoyu-skills/.env" && echo "user"
```

```powershell
# PowerShell (Windows)
if ((Test-Path .baoyu-skills/.env) -and (Select-String -Quiet -Pattern "WECHAT_APP_ID" .baoyu-skills/.env)) { "project" }
if ((Test-Path "$HOME/.baoyu-skills/.env") -and (Select-String -Quiet -Pattern "WECHAT_APP_ID" "$HOME/.baoyu-skills/.env")) { "user" }
```

**如凭据缺失 — 引导设置**：

```
WeChat API credentials not found.

To obtain credentials:
1. Visit https://mp.weixin.qq.com
2. Go to: 开发 → 基本配置
3. Copy AppID and AppSecret

Where to save?
A) Project-level: .baoyu-skills/.env (this project only)
B) User-level: ~/.baoyu-skills/.env (all projects)
```

选择位置后，提示输入值并写入 `.env`：

```
WECHAT_APP_ID=<user_input>
WECHAT_APP_SECRET=<user_input>
```

### Step 3: 解析主题/颜色并验证元数据

1. **解析主题**（首个匹配，不resolved 时不询问用户）：
   - CLI `--theme` 参数
   - EXTEND.md `default_theme`（Step 0 中加载）
   - 备选：`default`

2. **解析颜色**（首个匹配）：
   - CLI `--color` 参数
   - EXTEND.md `default_color`（Step 0 中加载）
   - 如未设置则省略（应用主题默认）

3. **从前置元数据（markdown）或 HTML meta 标签（HTML 输入）验证元数据**：

| 字段 | 如缺失 |
|-------|------------|
| 标题 | 提示："Enter title, or press Enter to auto-generate from content" |
| 摘要 | 提示："Enter summary, or press Enter to auto-generate (recommended for SEO)" |
| 作者 | 使用备选链：CLI `--author` → 前置元数据 `author` → EXTEND.md `default_author` |

**自动生成逻辑**：
- **标题**：首个 H1/H2 标题，或首个句子
- **摘要**：首个段落，截断至 120 字符

4. **封面图片检查**（API `article_type=news` 必需）：
   1. 使用 CLI `--cover`（如提供）
   2. 否则使用前置元数据（`coverImage`、`featureImage`、`cover`、`image`）
   3. 否则检查文章目录默认路径：`imgs/cover.png`
   4. 否则回退到首个内联内容图片
   5. 如仍缺失，停止并请求封面图片再发布

### Step 4: 发布到微信

**关键**：发布脚本内部处理 markdown 转换。不要预转换 markdown 为 HTML——直接传递原始 markdown 文件。这确保 API 方式将图片渲染为 `<img>` 标签（用于 API 上传）而浏览器方式使用占位符（用于粘贴替换工作流）。

**Markdown 引用默认**：
- 对于 markdown 输入，普通外部链接默认转换为底部引用
- 仅在用户明确希望保留普通外部链接内联时使用 `--no-cite`
- 现有 HTML 输入保持原样；不应用额外引用转换

**API 方式**（接受 `.md` 或 `.html`）：

```bash
${BUN_X} {baseDir}/scripts/wechat-api.ts <file> --theme <theme> [--color <color>] [--title <title>] [--summary <summary>] [--author <author>] [--cover <cover_path>] [--no-cite]
```

**关键**：始终包含 `--theme` 参数。即使使用 `default` 也不要省略。仅在用户或 EXTEND.md 明确设置时包含 `--color`。

**`draft/add` payload 规则**：
- 使用端点：`POST https://api.weixin.qq.com/cgi-bin/draft/add?access_token=ACCESS_TOKEN`
- `article_type`：`news`（默认）或 `newspic`
- 对于 `news`，包含 `thumb_media_id`（封面必需）
- 始终解析并发送：
  - `need_open_comment`（默认 `1`）
  - `only_fans_can_comment`（默认 `0`）
- `author` 解析：CLI `--author` → 前置元数据 `author` → EXTEND.md `default_author`

如脚本参数未暴露两个评论字段，仍需确保最终 API 请求体包含解析后的值。

**浏览器方式**（接受 `--markdown` 或 `--html`）：

```bash
${BUN_X} {baseDir}/scripts/wechat-article.ts --markdown <markdown_file> --theme <theme> [--color <color>] [--no-cite]
${BUN_X} {baseDir}/scripts/wechat-article.ts --html <html_file>
```

### Step 5: 完成报告

**对于 API 方式**，包含草稿管理链接：

```
WeChat Publishing Complete!

Input: [type] - [path]
Method: API
Theme: [theme name] [color if set]

Article:
• Title: [title]
• Summary: [summary]
• Images: [N] inline images
• Comments: [open/closed], [fans-only/all users]

Result:
✓ Draft saved to WeChat Official Account
• media_id: [media_id]

Next Steps:
→ Manage drafts: https://mp.weixin.qq.com (登录后进入「内容管理」→「草稿箱」)

Files created:
[• post-to-wechat/yyyy-MM-dd/slug.md (if plain text)]
[• slug.html (converted)]
```

**对于浏览器方式**：

```
WeChat Publishing Complete!

Input: [type] - [path]
Method: Browser
Theme: [theme name] [color if set]

Article:
• Title: [title]
• Summary: [summary]
• Images: [N] inline images

Result:
✓ Draft saved to WeChat Official Account

Files created:
[• post-to-wechat/yyyy-MM-dd/slug.md (if plain text)]
[• slug.html (converted)]
```

## 详细参考

| 主题 | 参考 |
|-------|---------|
| 贴图参数、自动压缩 | [references/image-text-posting.md](references/image-text-posting.md) |
| 文章主题、图片处理 | [references/article-posting.md](references/article-posting.md) |

## 功能对比

| 功能 | 贴图 | 文章（API） | 文章（浏览器） |
|---------|------------|---------------|-------------------|
| 纯文本输入 | ✗ | ✓ | ✓ |
| HTML 输入 | ✗ | ✓ | ✓ |
| Markdown 输入 | 标题/内容 | ✓ | ✓ |
| 多图 | ✓（最多 9 张） | ✓（内联） | ✓（内联） |
| 主题 | ✗ | ✓ | ✓ |
| 自动生成元数据 | ✗ | ✓ | ✓ |
| 默认封面备选（`imgs/cover.png`）| ✗ | ✓ | ✗ |
| 评论控制（`need_open_comment`、`only_fans_can_comment`）| ✗ | ✓ | ✗ |
| 需要 Chrome | ✓ | ✗ | ✓ |
| 需要 API 凭据 | ✗ | ✓ | ✗ |
| 速度 | 中等 | 快 | 慢 |

## 前置要求

**对于 API 方式**：
- 微信公众号 API 凭据
- 在 Step 2 跟随引导设置，或手动设置到 `.baoyu-skills/.env`

**对于浏览器方式**：
- Google Chrome
- 首次运行：登录微信公众号（会话保存）

**配置文件位置**（优先级顺序）：
1. 环境变量
2. `<cwd>/.baoyu-skills/.env`
3. `~/.baoyu-skills/.env`

## 故障排除

| 问题 | 解决方案 |
|-------|---------|
| 缺少 API 凭据 | 在 Step 2 跟随引导设置 |
| Access token 错误 | 检查 API 凭据是否有效且未过期 |
| 未登录（浏览器）| 首次运行打开浏览器 — 扫描二维码登录 |
| Chrome 未找到 | 设置 `WECHAT_BROWSER_CHROME_PATH` 环境变量 |
| 标题/摘要缺失 | 使用自动生成或手动提供 |
| 无封面图片 | 添加前置元数据封面或在文章目录放置 `imgs/cover.png` |
| 错误的评论默认值 | 检查 `EXTEND.md` 键 `need_open_comment` 和 `only_fans_can_comment` |
| 粘贴失败 | 检查系统剪贴板权限 |

## 扩展支持

通过 EXTEND.md 自定义配置。见**偏好设置**章节了解路径和支持的选项。
