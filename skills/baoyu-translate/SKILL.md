---
sourceName: baoyu-translate
sourceDescription: 在语言之间翻译文章和文档，提供三种模式——快速（直接翻译）、普通（先分析后翻译）和精细（分析、翻译、审校、润色）。通过 EXTEND.md 支持自定义术语表和术语一致性。当用户要求"translate"、"翻译"、"精翻"、"translate article"、"translate to Chinese/English"、"改成中文"、"改成英文"、"convert to Chinese"、"localize"、"本地化"，或需要任何文档翻译时使用。也适用于"refined translation"、"精细翻译"、"proofread translation"、"快速翻译"、"快翻"、"这篇文章翻译一下"，或当提供了带有翻译意图的 URL 或文件时触发。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-translate
    requires:
      anyBins:
        - bun
        - npx
---

# 翻译器

三模式翻译技能：**快速模式**用于直接翻译，**普通模式**用于分析驱动的翻译，**精细模式**用于包含审校和润色的完整出版级工作流。

## 脚本目录

脚本位于 `scripts/` 子目录。`{baseDir}` = 本 SKILL.md 所在目录路径。解析 `${BUN_X}` 运行时：如果已安装 `bun` 则使用 `bun`；如果 `npx` 可用则使用 `npx -y bun`；否则提示安装 bun。将 `{baseDir}` 和 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | CLI 入口点。默认操作将 Markdown 拆分为分块；也支持显式的 `chunk` 子命令 |
| `scripts/chunk.ts` | Markdown 分块实现，被 `main.ts` 调用，同时保持兼容以供直接使用 |

## 偏好设置（EXTEND.md）

检查 EXTEND.md 是否存在（按优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-translate/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-translate/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-translate/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-translate/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-translate/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-translate/EXTEND.md") { "user" }
```

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-translate/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-translate/EXTEND.md` | 用户主目录 |

| 结果 | 操作 |
|--------|--------|
| 找到 | 读取、解析、应用设置。首次在会话中使用时，简要提示："Using preferences from [path]. You can edit EXTEND.md to customize glossary, audience, etc." |
| 未找到 | **必须**运行首次设置（见下文）——不得静默使用默认值 |

**EXTEND.md 支持的配置项**：默认目标语言 | 默认模式 | 目标受众 | 自定义术语表（内联或文件路径） | 翻译风格 | 分块设置

Schema 文档：[references/config/extend-schema.md](references/config/extend-schema.md)

### 首次设置（阻塞操作）

**关键**：当 EXTEND.md 未找到时，你**必须**在执行任何翻译之前运行首次设置。这是一项**阻塞操作**。

完整参考：[references/config/first-time-setup.md](references/config/first-time-setup.md)

使用 `AskUserQuestion` 在一次调用中提出所有问题（目标语言、模式、受众、风格、保存位置）。用户回答后，在选定位置创建 EXTEND.md，确认"Preferences saved to [path]"，然后继续。

## 默认值

所有可配置值集中管理。EXTEND.md 覆盖这些默认值；CLI 标志覆盖 EXTEND.md。

| 设置项 | 默认值 | EXTEND.md 键 | CLI 标志 | 说明 |
|---------|---------|---------------|----------|-------------|
| 目标语言 | `zh-CN` | `target_language` | `--to` | 翻译目标语言 |
| 模式 | `normal` | `default_mode` | `--mode` | 翻译模式 |
| 受众 | `general` | `audience` | `--audience` | 目标读者画像 |
| 风格 | `storytelling` | `style` | `--style` | 翻译风格偏好 |
| 分块阈值 | `4000` | `chunk_threshold` | — | 触发分块翻译的词数 |
| 分块最大词数 | `5000` | `chunk_max_words` | — | 每个分块的最大词数 |

## 模式

| 模式 | 标志 | 步骤 | 适用场景 |
|------|------|-------|-------------|
| 快速 | `--mode quick` | 翻译 | 短文本、非正式内容、快速任务 |
| 普通 | `--mode normal`（默认） | 分析 → 翻译 | 文章、博客文章、一般内容 |
| 精细 | `--mode refined` | 分析 → 翻译 → 审校 → 润色 | 出版级、重要文档 |

**默认模式**：普通模式（可在 EXTEND.md 的 `default_mode` 设置中覆盖）。

**风格预设**——控制翻译的语调和语气（与受众独立）：

| 值 | 说明 | 效果 |
|-------|-------------|--------|
| `storytelling` | 引人入胜的叙事流畅性（默认） | 吸引读者，过渡平滑，措辞生动 |
| `formal` | 专业、结构化 | 中性语调，组织清晰，不使用口语化表达 |
| `technical` | 精确、文档风格 | 简洁，术语密集，最小化修饰 |
| `literal` | 接近原文结构 | 最小化重组，保留源语言句式 |
| `academic` | 学术、严谨 | 正式语域，允许复杂从句，注意引文格式 |
| `business` | 简洁、结果导向 | 行动导向，适合管理层阅读，要点思维 |
| `humorous` | 保留并改编幽默 | 机智俏皮，在目标语言中重现喜剧效果 |
| `conversational` | 轻松、口语化 | 友好平易，如同向朋友解释 |
| `elegant` | 文学、精炼的散文 | 美学精炼，富有节奏感，措辞讲究 |

也接受自定义风格描述，例如 `--style "poetic and lyrical"`。

**自动检测**：
- "快翻"、"quick"、"直接翻译" → 快速模式
- "精翻"、"refined"、"publication quality"、"proofread" → 精细模式
- 其他 → 默认模式（普通模式）

**升级提示**：普通模式完成后，显示：
> Translation saved. To further review and polish, reply "继续润色" or "refine".

如果用户回应，则继续执行审校 → 润色步骤（与精细模式 refined-workflow.md 中的步骤 4-6 相同），基于已有输出进行操作。

## 用法

```
/translate [--mode quick|normal|refined] [--from <lang>] [--to <lang>] [--audience <audience>] [--style <style>] [--glossary <file>] <source>
```

- `<source>`：文件路径、URL 或内联文本
- `--from`：源语言（省略时自动检测）
- `--to`：目标语言（来自 EXTEND.md 或默认 `zh-CN`）
- `--audience`：目标读者画像（来自 EXTEND.md 或默认 `general`）
- `--style`：翻译风格（来自 EXTEND.md 或默认 `storytelling`）
- `--glossary`：额外的术语表文件，与 EXTEND.md 术语表合并

**受众预设**：

| 值 | 说明 | 效果 |
|-------|-------------|--------|
| `general` | 普通读者（默认） | 通俗语言，对术语添加更多译者注 |
| `technical` | 开发者 / 工程师 | 对常见技术术语减少注释 |
| `academic` | 研究者 / 学者 | 正式语域，精确术语 |
| `business` | 商业专业人士 | 商务友好语调，解释技术概念 |

也接受自定义受众描述，例如 `--audience "AI感兴趣的普通读者"`。

## 工作流

### 步骤 1：加载偏好设置

1.1 检查 EXTEND.md（见上方偏好设置部分）

1.2 如果可用，加载语言对的内置术语表：
- EN→ZH：[references/glossary-en-zh.md](references/glossary-en-zh.md)

1.3 合并术语表：EXTEND.md `glossary`（内联）+ EXTEND.md `glossary_files`（外部文件，路径相对于 EXTEND.md 位置）+ 内置术语表 + `--glossary` 文件（CLI 覆盖所有）

### 步骤 2：准备源文件并创建输出目录

准备源文件（文件原样使用，内联文本/URL 保存到 `translate/{slug}.md`），然后创建输出目录：`{source-dir}/{source-basename}-{target-lang}/`。如果未指定 `--from`，自动检测源语言。

完整详情：[references/workflow-mechanics.md](references/workflow-mechanics.md)

**输出目录内容**（所有中间文件和最终文件均存放于此）：

| 文件 | 模式 | 说明 |
|------|------|-------------|
| `translation.md` | 所有 | 最终翻译（始终使用此文件名） |
| `01-analysis.md` | 普通、精细 | 内容分析（领域、语调、术语） |
| `02-prompt.md` | 普通、精细 | 组装的翻译提示词 |
| `03-draft.md` | 精细 | 审校前的初稿 |
| `04-critique.md` | 精细 | 审校发现（仅诊断） |
| `05-revision.md` | 精细 | 基于审校的修订翻译 |
| `chunks/` | 分块模式 | 源分块 + 已翻译分块 |

### 步骤 3：评估内容长度

快速模式不分块——无论长度如何都直接翻译。翻译前估算词数。如果内容超过分块阈值（默认 4000 词），主动提醒："This article is ~{N} words. Quick mode translates in one pass without chunking — for long content, `--mode normal` produces better results with terminology consistency." 如果用户未切换模式则继续执行。

对于普通模式和精细模式：

| 内容 | 操作 |
|---------|--------|
| < 分块阈值 | 作为整体翻译 |
| >= 分块阈值 | 分块翻译（见步骤 3.1） |

**3.1 长内容准备**（普通/精细模式，仅当 >= 分块阈值时适用）

翻译分块之前：

1. **提取术语**：扫描整个文档中的专有名词、技术术语和重复出现的短语
2. **构建会话语义词表**：将提取的术语与已加载的术语表合并，确立一致的翻译
3. **拆分为分块**：使用 `${BUN_X} {baseDir}/scripts/main.ts <file> [--max-words <chunk_max_words>] [--output-dir <output-dir>]`
   - 解析 Markdown 块（标题、段落、列表、代码块、表格等）
   - 在 Markdown 块边界处拆分以保留结构
   - 如果单个块超过阈值，回退到按行拆分，然后按词拆分
4. **组装翻译提示词**：
   - 主代理读取 `01-analysis.md`（如果存在），使用 [references/subagent-prompt-template.md](references/subagent-prompt-template.md) 的第一部分组装共享上下文——内联解析后的风格预设（来自 `--style` 标志、EXTEND.md 的 `style` 设置或默认的 `storytelling`）、内容背景、合并的术语表和理解挑战
   - 保存为输出目录中的 `02-prompt.md`（仅共享上下文，不含任务指令）
5. **通过子代理起草翻译**（如果 Agent 工具可用）：
   - **每个分块**生成一个子代理，全部并行执行（模板第二部分）
   - 每个子代理读取 `02-prompt.md` 获取共享上下文，翻译其分块，保存到 `chunks/chunk-NN-draft.md`
   - 术语一致性由共享的 `02-prompt.md`（术语表 + 分析中的理解挑战）保证
   - 如果没有分块（内容低于阈值）：为整个源文件生成一个子代理
   - 如果 Agent 工具不可用，使用 `02-prompt.md` 按顺序内联翻译各分块
6. **合并**：所有子代理完成后，按顺序合并已翻译的分块。如果 `chunks/frontmatter.md` 存在，将其前置。保存为 `03-draft.md`（精细模式）或 `translation.md`（普通模式）
7. 所有中间文件（源分块 + 已翻译分块）保留在 `chunks/` 目录中

**分块初稿合并后**，将控制权交回主代理进行审校、修订和润色（步骤 4）。

### 步骤 4：翻译与润色

**翻译原则**（适用于所有模式）：

- **准确性优先**：事实、数据和逻辑必须与原文完全一致
- **意译优先**：翻译作者要表达的意思，而非逐字翻译。当直译不自然或未能传达预期效果时，自由重组以用地道的目标语言表达相同的意思
- **比喻语言**：按其含义而非逐字翻译来处理比喻、习语和比喻性表达。当源语言图像在目标语言中没有相同含义时，用自然表达替换以传达相同的思想和情感效果
- **情感忠实**：保留词语选择中的情感内涵，而非仅仅保留字典含义。带有主观感受的词语（如 "alarming"、"haunting"）应在目标语言读者中唤起相同的反应
- **自然流畅**：使用地道的目标语言语序和句式；当源语言结构在目标语言中不够自然时，自由拆分或重组句子
- **术语**：使用标准翻译；首次出现时在括号中注释原文
- **保留格式**：保持所有 Markdown 格式（标题、粗体、斜体、图片、链接、代码块）
- **图片语言感知**：翻译过程中精确保留图片引用，但翻译完成后，检查引用的图片并确认其主要文本语言是否仍与翻译后的文章语言匹配
- **前置元数据转换**：如果源文件有 YAML 前置元数据，在翻译中保留并做以下修改：(1) 重命名描述*源文章*的元数据字段——`url`→`sourceUrl`、`title`→`sourceTitle`、`description`→`sourceDescription`、`author`→`sourceAuthor`、`date`→`sourceDate`，以及任何类似的来源元数据字段——通过添加 `source` 前缀（驼峰命名）。(2) 翻译文本字段的值（title、description 等），并作为新的顶级字段添加。(3) 其他字段（tags、categories、自定义字段）保持原样，适当地翻译其值
- **尊重原文**：保持原始含义和意图；不添加、删除或编辑观点——但句子结构和意象可自由调整以服务于含义
- **译者注**：对于目标读者可能不理解的概念或文化引用（由于术语、文化差异或领域特定知识），在术语后立即添加简明的括号注释。注释应解释其*含义*，而非仅仅提供英文原文。格式：`译文（English original，通俗解释）`。根据目标受众调整注释深度：普通读者比技术读者需要更多注释。对于短文本（< 5 句），进一步减少注释——仅为目标读者不太可能知道的非常见术语添加注释；跳过广为人知或在上下文中不言自明的术语。仅在真正需要时添加注释；不要过度注释显而易见的术语

#### 快速模式

直接翻译，保存到 `translation.md`。不生成分析文件，但仍需应用上述所有翻译原则——尤其是：按含义（而非逐字）解释比喻语言，保留情感内涵，重组句子以实现自然的目标语言流畅性。

#### 普通模式

1. **分析** → `01-analysis.md`（领域、语调、受众、术语、读者理解挑战、比喻语言与隐喻映射）
2. **组装提示词** → `02-prompt.md`（内联风格预设、内容背景、术语表和理解挑战的翻译指令）
3. **翻译**（按照 `02-prompt.md`）→ `translation.md`

完成后提示用户："Translation saved. To further review and polish, reply **继续润色** or **refine**."

如果用户继续，执行审校 → 修订 → 润色（与下方精细模式的步骤 4-6 相同），保存 `03-draft.md`（将当前 `translation.md` 重命名）、`04-critique.md`、`05-revision.md` 和更新后的 `translation.md`。

#### 精细模式

面向出版质量的完整工作流。每个步骤的详细指南见 [references/refined-workflow.md](references/refined-workflow.md)。

子代理（如果在步骤 3.1 中使用）仅处理初稿。所有后续步骤（审校、修订、润色）由主代理处理，主代理可自行决定是否委托给子代理。

步骤和保存的文件（全部在输出目录中）：
1. **分析** → `01-analysis.md`（领域、语调、术语、读者理解挑战、比喻语言与隐喻映射）
2. **组装提示词** → `02-prompt.md`（内联上下文的翻译指令）
3. **初稿** → `03-draft.md`（包含译者注的初始翻译；分块模式下由子代理生成）
4. **审校** → `04-critique.md`（仅诊断：准确性、翻译腔、策略执行、表达问题）
5. **修订** → `05-revision.md`（应用所有审校发现以生成修订翻译）
6. **润色** → `translation.md`（最终出版级翻译）

每个步骤读取上一步骤的文件并在此基础上构建。

### 步骤 5：输出

最终翻译始终位于输出目录中的 `translation.md`。

最终翻译写入后，执行轻量级的图片语言检查：

1. 从翻译后的文章中收集图片引用
2. 识别可能包含大量文字的图片，如封面、截图、图表、框架图和信息图
3. 如果任何图片可能包含与翻译后文章语言不匹配的主要文本语言，主动提醒用户
4. 提醒必须仅为列表形式。除非用户要求，否则不要自动本地化这些图片

提醒格式（使用文章已有的图片语法——标准 Markdown 或 wikilink）：
```text
Possible image localization needed:
- ![example cover](attachments/example-cover.png): likely still contains source-language text while the article is now in target language
- ![example diagram](attachments/example-diagram.png): likely text-heavy framework graphic, check whether labels need translation
```

显示摘要：
```
**Translation complete** ({mode} mode)

Source: {source-path}
Languages: {from} → {to}
Output dir: {output-dir}/
Final: {output-dir}/translation.md
Glossary terms applied: {count}
```

如果发现语言不匹配的图片候选，在摘要后追加一条简短说明，告知用户某些嵌入图片可能仍需要图片文本本地化，后跟候选列表。

## 扩展支持

通过 EXTEND.md 进行自定义配置。路径和支持的选项见**偏好设置**部分。
