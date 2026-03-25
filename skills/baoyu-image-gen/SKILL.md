# 图像生成（AI SDK）

官方 API 图像生成。支持 OpenAI、Google、OpenRouter、DashScope（阿里通义万象）、Jimeng（即梦）、Seedream（豆包）和 Replicate 提供商。

## 脚本目录

**智能体执行**：
1. `{baseDir}` = 此 SKILL.md 文件所在目录
2. 脚本路径 = `{baseDir}/scripts/main.ts`
3. 解析 `${BUN_X}` 运行时：如果安装了 `bun` → `bun`；如果 `npx` 可用 → `npx -y bun`；否则建议安装 bun

## 步骤 0：加载偏好设置 ⛔ 阻塞

**重要**：此步骤必须在任何图像生成之前完成。不得跳过或延迟。

检查 EXTEND.md 是否存在（优先级：项目 → 用户）：

```bash
# macOS、Linux、WSL、Git Bash
test -f .baoyu-skills/baoyu-image-gen/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-image-gen/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-image-gen/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-image-gen/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|--------|--------|
| 找到 | 加载、解析、应用设置。如果 `default_model.[provider]` 为 null → 仅询问模型（流程 2） |
| 未找到 | ⛔ 运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 保存 EXTEND.md → 然后继续 |

**重要**：如果未找到，完成完整设置（提供商 + 模型 + 质量 + 保存位置）使用 AskUserQuestion在任何图像生成之前。生成被阻塞直到 EXTEND.md 被创建。

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-image-gen/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 用户主目录 |

**EXTEND.md 支持**：默认提供商 | 默认质量 | 默认宽高比 | 默认图像尺寸 | 默认模型 | 批处理工作器上限 | 提供商特定批处理限制

架构：`references/config/preferences-schema.md`

## 使用方法

```bash
# 基本用法
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image cat.png

# 带宽高比
${BUN_X} {baseDir}/scripts/main.ts --prompt "A landscape" --image out.png --ar 16:9

# 高质量
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --quality 2k

# 从提示词文件
${BUN_X} {baseDir}/scripts/main.ts --promptfiles system.md content.md --image out.png

# 带参考图（Google、OpenAI、OpenRouter 或 Replicate）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --ref source.png

# 带参考图（显式提供商/模型）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --provider google --model gemini-3-pro-image-preview --ref source.png

# OpenRouter（推荐的默认模型）
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider openrouter

# OpenRouter 带参考图
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --provider openrouter --model google/gemini-3.1-flash-image-preview --ref source.png

# 指定提供商
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider openai

# DashScope（阿里通义万象）
${BUN_X} {baseDir}/scripts/main.ts --prompt "一只可爱的猫" --image out.png --provider dashscope

# DashScope Qwen-Image 2.0 Pro（推荐用于自定义尺寸和文字渲染）
${BUN_X} {baseDir}/scripts/main.ts --prompt "为咖啡品牌设计一张 21:9 横幅海报，包含清晰中文标题" --image out.png --provider dashscope --model qwen-image-2.0-pro --size 2048x872

# DashScope legacy Qwen 固定尺寸模型
${BUN_X} {baseDir}/scripts/main.ts --prompt "一张电影感海报" --image out.png --provider dashscope --model qwen-image-max --size 1664x928

# Replicate (google/nano-banana-pro)
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate

# Replicate 带特定模型
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate --model google/nano-banana

# 批处理模式（使用保存的提示词文件）
${BUN_X} {baseDir}/scripts/main.ts --batchfile batch.json

# 批处理模式（带显式工作器数量）
${BUN_X} {baseDir}/scripts/main.ts --batchfile batch.json --jobs 4 --json
```

### 批处理文件格式

```json
{
  "jobs": 4,
  "tasks": [
    {
      "id": "hero",
      "promptFiles": ["prompts/hero.md"],
      "image": "out/hero.png",
      "provider": "replicate",
      "model": "google/nano-banana-pro",
      "ar": "16:9",
      "quality": "2k"
    },
    {
      "id": "diagram",
      "promptFiles": ["prompts/diagram.md"],
      "image": "out/diagram.png",
      "ref": ["references/original.png"]
    }
  ]
}
```

`promptFiles`、`image` 和 `ref` 中的路径相对于批处理文件的目录。`jobs` 是可选的（被 CLI `--jobs` 覆盖）。也接受顶级数组格式（不带 `jobs` 包装器）。

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--prompt <text>`、`-p` | 提示词文本 |
| `--promptfiles <files...>` | 从文件读取提示词（连接） |
| `--image <path>` | 输出图像路径（单图模式必需） |
| `--batchfile <path>` | 多图生成的 JSON 批处理文件 |
| `--jobs <count>` | 批处理模式工作器数量（默认：自动，来自配置，最大 10） |
| `--provider google\|openai\|openrouter\|dashscope\|jimeng\|seedream\|replicate` | 强制指定提供商（默认：自动检测） |
| `--model <id>`、`-m` | 模型 ID（Google：`gemini-3-pro-image-preview`；OpenAI：`gpt-image-1.5`；OpenRouter：`google/gemini-3.1-flash-image-preview`；DashScope：`qwen-image-2.0-pro`） |
| `--ar <ratio>` | 宽高比（例如 `16:9`、`1:1`、`4:3`） |
| `--size <WxH>` | 尺寸（例如 `1024x1024`） |
| `--quality normal\|2k` | 质量预设（默认：`2k`） |
| `--imageSize 1K\|2K\|4K` | Google/OpenRouter 的图像尺寸（默认：来自质量） |
| `--ref <files...>` | 参考图。由 Google 多模态、OpenAI GPT Image 编辑、OpenRouter 多模态模型和 Replicate 支持。不支持 Jimeng 或 Seedream |
| `--n <count>` | 图像数量 |
| `--json` | JSON 输出 |

## 环境变量

| 变量 | 描述 |
|----------|-------------|
| `OPENAI_API_KEY` | OpenAI API 密钥 |
| `OPENROUTER_API_KEY` | OpenRouter API 密钥 |
| `GOOGLE_API_KEY` | Google API 密钥 |
| `DASHSCOPE_API_KEY` | DashScope API 密钥（阿里云） |
| `REPLICATE_API_TOKEN` | Replicate API token |
| `JIMENG_ACCESS_KEY_ID` | Jimeng（即梦）火山引擎 access key |
| `JIMENG_SECRET_ACCESS_KEY` | Jimeng（即梦）火山引擎 secret key |
| `ARK_API_KEY` | Seedream（豆包）火山引擎 ARK API 密钥 |
| `OPENAI_IMAGE_MODEL` | OpenAI 模型覆盖 |
| `OPENROUTER_IMAGE_MODEL` | OpenRouter 模型覆盖（默认：`google/gemini-3.1-flash-image-preview`） |
| `GOOGLE_IMAGE_MODEL` | Google 模型覆盖 |
| `DASHSCOPE_IMAGE_MODEL` | DashScope 模型覆盖（默认：`qwen-image-2.0-pro`） |
| `REPLICATE_IMAGE_MODEL` | Replicate 模型覆盖（默认：google/nano-banana-pro） |
| `JIMENG_IMAGE_MODEL` | Jimeng 模型覆盖（默认：jimeng_t2i_v40） |
| `SEEDREAM_IMAGE_MODEL` | Seedream 模型覆盖（默认：doubao-seedream-5-0-260128） |
| `OPENAI_BASE_URL` | 自定义 OpenAI 端点 |
| `OPENROUTER_BASE_URL` | 自定义 OpenRouter 端点（默认：`https://openrouter.ai/api/v1`） |
| `OPENROUTER_HTTP_REFERER` | 可选的 app/site URL 用于 OpenRouter 属性 |
| `OPENROUTER_TITLE` | 可选的 app 名称用于 OpenRouter 属性 |
| `GOOGLE_BASE_URL` | 自定义 Google 端点 |
| `DASHSCOPE_BASE_URL` | 自定义 DashScope 端点 |
| `REPLICATE_BASE_URL` | 自定义 Replicate 端点 |
| `JIMENG_BASE_URL` | 自定义 Jimeng 端点（默认：`https://visual.volcengineapi.com`） |
| `JIMENG_REGION` | Jimeng 区域（默认：`cn-north-1`） |
| `SEEDREAM_BASE_URL` | 自定义 Seedream 端点（默认：`https://ark.cn-beijing.volces.com/api/v3`） |
| `BAOYU_IMAGE_GEN_MAX_WORKERS` | 覆盖批处理工作器上限 |
| `BAOYU_IMAGE_GEN_<PROVIDER>_CONCURRENCY` | 覆盖提供商并发数，例如 `BAOYU_IMAGE_GEN_REPLICATE_CONCURRENCY` |
| `BAOYU_IMAGE_GEN_<PROVIDER>_START_INTERVAL_MS` | 覆盖提供商启动间隔，例如 `BAOYU_IMAGE_GEN_REPLICATE_START_INTERVAL_MS` |

**加载优先级**：CLI 参数 > EXTEND.md > 环境变量 > `<cwd>/.baoyu-skills/.env` > `~/.baoyu-skills/.env`

## 模型解析

模型优先级（高 → 低），适用于所有提供商：

1. CLI 标志：`--model <id>`
2. EXTEND.md：`default_model.[provider]`
3. 环境变量：`<PROVIDER>_IMAGE_MODEL`（例如 `GOOGLE_IMAGE_MODEL`）
4. 内置默认值

**EXTEND.md 覆盖环境变量**。如果同时存在 EXTEND.md `default_model.google: "gemini-3-pro-image-preview"` 和环境变量 `GOOGLE_IMAGE_MODEL=gemini-3.1-flash-image-preview`，EXTEND.md 优先。

**智能体必须在每次生成前显示模型信息**：
- 显示：`Using [provider] / [model]`
- 显示切换提示：`Switch model: --model <id> | EXTEND.md default_model.[provider] | env <PROVIDER>_IMAGE_MODEL`

### DashScope 模型

当用户需要官方 Qwen-Image 行为时，使用 `--model qwen-image-2.0-pro` 或设置 `default_model.dashscope` / `DASHSCOPE_IMAGE_MODEL`。

官方 DashScope 模型系列：

- `qwen-image-2.0-pro`、`qwen-image-2.0-pro-2026-03-03`、`qwen-image-2.0`、`qwen-image-2.0-2026-03-03`
  - 自由格式 `size` 为 `宽*高` 格式
  - 总像素必须保持在 `512*512` 和 `2048*2048` 之间
  - 默认尺寸约为 `1024*1024`
  - 自定义比例（如 `21:9`）和中英文文字渲染的最佳选择
- `qwen-image-max`、`qwen-image-max-2025-12-30`、`qwen-image-plus`、`qwen-image-plus-2026-01-09`、`qwen-image`
  - 仅固定尺寸：`1664*928`、`1472*1104`、`1328*1328`、`1104*1472`、`928*1664`
  - 默认尺寸为 `1664*928`
  - `qwen-image` 目前与 `qwen-image-plus` 能力相同
- 遗留 DashScope 模型如 `z-image-turbo`、`z-image-ultra`、`wanx-v1`
  - 仅当用户明确要求遗留行为或兼容性时继续使用

将 CLI 参数转换为 DashScope 行为时：

- `--size` 优先于 `--ar`
- 对于 `qwen-image-2.0*`，优先使用显式 `--size`；否则从 `--ar` 推断并使用以下官方推荐尺寸
- 对于 `qwen-image-max/plus/image`，仅使用五种官方固定尺寸；如果请求的比例不支持，切换到 `qwen-image-2.0-pro`
- `--quality` 是 baoyu-image-gen 兼容性预设，不是原生 DashScope API 字段。将 `normal` / `2k` 映射到下表 `qwen-image-2.0*` 是实现推断，不是官方 API 保证

常见宽高比的推荐 `qwen-image-2.0*` 尺寸：

| 比例 | `normal` | `2k` |
|-------|----------|------|
| `1:1` | `1024*1024` | `1536*1536` |
| `2:3` | `768*1152` | `1024*1536` |
| `3:2` | `1152*768` | `1536*1024` |
| `3:4` | `960*1280` | `1080*1440` |
| `4:3` | `1280*960` | `1440*1080` |
| `9:16` | `720*1280` | `1080*1920` |
| `16:9` | `1280*720` | `1920*1080` |
| `21:9` | `1344*576` | `2048*872` |

DashScope 官方 API 还暴露 `negative_prompt`、`prompt_extend` 和 `watermark`，但 `baoyu-image-gen` 目前不将它们作为专用 CLI 标志暴露。

官方参考：

- [Qwen-Image API](https://help.aliyun.com/zh/model-studio/qwen-image-api)
- [Text-to-image guide](https://help.aliyun.com/zh/model-studio/text-to-image)
- [Qwen-Image Edit API](https://help.aliyun.com/zh/model-studio/qwen-image-edit-api)

### OpenRouter 模型

使用完整的 OpenRouter 模型 ID，例如：

- `google/gemini-3.1-flash-image-preview`（推荐，支持图像输出和参考图工作流）
- `google/gemini-2.5-flash-image-preview`
- `black-forest-labs/flux.2-pro`
- 其他支持图像的 OpenRouter 模型 ID

注意：

- OpenRouter 图像生成使用 `/chat/completions`，不是 OpenAI `/images` 端点
- 如果使用 `--ref`，选择支持图像输入和图像输出的多模态模型
- `--imageSize` 映射到 OpenRouter `imageGenerationOptions.size`；当仅给出 `--size <WxH>` 时，会转换为最近的 OpenRouter 尺寸并在可能时推断宽高比

### Replicate 模型

支持的模型格式：

- `owner/name`（推荐用于官方模型），例如 `google/nano-banana-pro`
- `owner/name:version`（社区模型按版本），例如 `stability-ai/sdxl:<version>`

示例：

```bash
# 使用 Replicate 默认模型
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate

# 显式覆盖模型
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate --model google/nano-banana
```

## 提供商选择

1. 提供了 `--ref` 且无 `--provider` → 自动选择 Google，然后 OpenAI，然后 OpenRouter，然后 Replicate（Jimeng 和 Seedream 不支持参考图）
2. 指定了 `--provider` → 使用它（如果使用 `--ref`，必须是 `google`、`openai`、`openrouter` 或 `replicate`）
3. 只有一个 API 密钥可用 → 使用该提供商
4. 多个可用 → 默认为 Google

## 质量预设

| 预设 | Google imageSize | OpenAI Size | OpenRouter size | Replicate resolution | 使用场景 |
|--------|------------------|-------------|-----------------|----------------------|----------|
| `normal` | 1K | 1024px | 1K | 1K | 快速预览 |
| `2k`（默认） | 2K | 2048px | 2K | 2K | 封面、插图、信息图 |

**Google/OpenRouter imageSize**：可用 `--imageSize 1K|2K|4K` 覆盖

## 宽高比

支持：`1:1`、`16:9`、`9:16`、`4:3`、`3:4`、`2.35:1`

- Google 多模态：使用 `imageConfig.aspectRatio`
- OpenAI：映射到最接近支持的尺寸
- OpenRouter：发送 `imageGenerationOptions.aspect_ratio`；如果仅给出 `--size <WxH>`，宽高比自动推断
- Replicate：传递 `aspect_ratio` 到模型；当提供 `--ref` 但没有 `--ar` 时，默认为 `match_input_image`

## 生成模式

**默认**：顺序生成。

**批处理并行生成**：当 `--batchfile` 包含 2 个或更多待处理任务时，脚本自动启用并行生成。

| 模式 | 使用时机 |
|------|-------------|
| 顺序（默认） | 正常使用、单图、小批量 |
| 并行批处理 | 2 个或更多任务的批处理模式 |

执行选择：

| 情况 | 首选方法 | 原因 |
|-----------|--------------------|-----|
| 一张图像，或 1-2 张简单图像 | 顺序 | 更低的协调开销和更容易调试 |
| 多个图像已有保存的提示词文件 | 批处理（`--batchfile`） | 重用最终确定的提示词，应用共享节流/重试，并提供可预测的吞吐量 |
| 每个图像仍需要单独推理、提示词编写或风格探索 | 子智能体 | 工作仍是探索性的，因此每张图像在生成前可能需要独立分析 |
| 输出来自 `baoyu-article-illustrator`，带有 `outline.md` + `prompts/` | 批处理（`build-batch.ts` -> `--batchfile`） | 该工作流已经产生提示词文件，因此直接批处理执行是预期路径 |

经验法则：

- 一旦提示词文件已保存且任务是"生成所有这些"，优先使用批处理而非子智能体
- 仅当生成与每图像思考、重写或发散创意探索相耦合时才使用子智能体

并行行为：

- 默认工作器数量是自动的，由配置限制，内置默认 10
- 提供商特定节流仅在批处理模式下应用，内置默认调优为更快吞吐量同时避免明显的 RPM 突发
- 可以用 `--jobs <count>` 覆盖工作器数量
- 每个图像自动重试最多 3 次
- 最终输出包括成功计数、失败计数和每图像失败原因

## 错误处理

- 缺少 API 密钥 → 带设置说明的错误
- 生成失败 → 每张图像自动重试最多 3 次
- 无效宽高比 → 警告，使用默认值继续
- 参考图与不支持的提供商/模型 → 带修复提示的错误

## 扩展支持

通过 EXTEND.md 自定义配置。参见**偏好设置**部分了解路径和支持的选项。
