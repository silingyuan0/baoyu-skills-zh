# 详细工作流程步骤

## 步骤 1：预检

### 1.0 检测并保存参考图片 ⚠️ 如果提供图片则必须

检查用户是否提供了参考图片。根据输入类型处理：

| 输入类型 | 操作 |
|------------|--------|
| 提供了图片文件路径 | 复制到 `references/` 子目录 → 可使用 `--ref` |
| 对话中的图片（无路径） | 使用 AskUserQuestion **询问用户文件路径** |
| 用户无法提供路径 | 口头提取风格/调色板 → 追加到提示词（不引用 frontmatter）|

**⚠️ 关键**：仅在文件实际保存到 `references/` 目录时，才将 `references` 添加到提示词 frontmatter。

**如果用户提供文件路径**：
1. 复制到 `references/NN-ref-{slug}.png`
2. 创建描述：`references/NN-ref-{slug}.md`
3. 验证文件存在后再继续

**如果用户无法提供路径**（口头提取）：
1. 视觉分析图片，提取：颜色、风格、构图
2. 创建 `references/extracted-style.md` 包含提取的信息
3. 不将 `references` 添加到提示词 frontmatter
4. 而是将提取的风格/颜色直接追加到提示词文本

**描述文件格式**（仅当文件保存时）：
```yaml
---
ref_id: NN
filename: NN-ref-{slug}.png
---
[用户的描述或自动生成的描述]
```

**验证**（仅针对保存的文件）：
```
参考图片已保存：
- 01-ref-{slug}.png ✓（可使用 --ref）
- 02-ref-{slug}.png ✓（可使用 --ref）
```

**或者对于提取的风格**：
```
参考风格已提取（无文件）：
- 颜色：#E8756D 珊瑚色，#7ECFC0 薄荷绿...
- 风格：极简扁平矢量，干净线条...
→ 将追加到提示词文本（不是 --ref）
```

---

### 1.1 确定输入类型

| 输入 | 输出目录 | 下一步 |
|-------|------------------|------|
| 文件路径 | EXTEND.md `default_output_dir`（默认：`imgs-subdir`）。如未配置，在 1.2 中确认。 | → 1.2 |
| 粘贴内容 | `illustrations/{topic-slug}/` | → 1.4 |

**粘贴内容备份规则**：如果目标目录存在 `source.md`，在保存前重命名为 `source-backup-YYYYMMDD-HHMMSS.md`。

### 1.2-1.4 配置（仅文件路径输入）

检查首选项和现有状态，然后在一个 AskUserQuestion 调用中询问所有需要的问题（最多 4 个问题）。

**要包含的问题**（如果存在首选项或不可用则跳过）：

| 问题 | 何时询问 | 选项 |
|----------|-------------|---------|
| 输出目录 | EXTEND.md 中没有 `default_output_dir` | `{article-dir}/imgs/`（推荐）、`{article-dir}/`、`{article-dir}/illustrations/`、`illustrations/{topic-slug}/` |
| 现有图片 | 目标目录有 `.png/.jpg/.webp` 文件 | `supplement`、`overwrite`、`regenerate` |
| 文章更新 | 始终（文件路径输入） | `update`、`copy` |

**首选项值**（如果已配置，跳过询问）：

| `default_output_dir` | 路径 |
|----------------------|------|
| `same-dir` | `{article-dir}/` |
| `imgs-subdir` | `{article-dir}/imgs/` |
| `illustrations-subdir` | `{article-dir}/illustrations/` |
| `independent` | `illustrations/{topic-slug}/` |

### 1.5 加载首选项（EXTEND.md）⛔ 阻塞式操作

**⚠️ 关键**：如果未找到 EXTEND.md，必须在任何其他问题或步骤之前完成首次设置。不要继续进行参考图片，不要询问内容，不要询问类型/风格 — 仅先完成首选项设置。

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-article-illustrator/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-article-illustrator/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|--------|--------|
| 找到 | 读取、解析、显示摘要 → 继续 |
| 未找到 | ⛔ **阻塞式操作**：仅运行首次设置（[config/first-time-setup.md](config/first-time-setup.md)）→ 完成并保存 EXTEND.md → 然后继续 |

**支持**：水印 | 首选类型/风格 | 自定义风格 | 语言 | 输出目录

---

## 步骤 2：设置与分析

### 2.1 分析内容

| 分析 | 描述 |
|----------|-------------|
| 内容类型 | 技术 / 教程 / 方法论 / 叙事 |
| 插图目的 | 信息 / 可视化 / 想象 |
| 核心论点 | 2-5 个要可视化主要点 |
| 视觉机会 | 插图增加价值的位置 |
| 推荐类型 | 基于内容信号和目的 |
| 推荐密度级别 | 基于长度和复杂性 |

### 2.2 提取核心论点

- 主要论点
- 读者需要的关键概念
- 比较/对比
- 提出的框架/模型

**⚠️ 关键**：如果文章使用隐喻（例如"电锯切西瓜"），不要字面说明。将底层概念可视化。

### 2.3 识别位置

**要说明**：
- 核心论点（必须）
- 抽象概念
- 数据比较
- 流程、工作流

**不要说明**：
- 字面隐喻
- 装饰性场景
- 通用插图

### 2.4 分析参考图片（如果在步骤 1.0 中提供）

对于每个参考图片：

| 分析 | 描述 |
|----------|-------------|
| 视觉特征 | 风格、颜色、构图 |
| 内容/主题 | 参考描述的内容 |
| 合适位置 | 哪些部分匹配此参考 |
| 风格匹配 | 哪些插图类型/风格对齐 |
| 使用建议 | `direct` / `style` / `palette` |

| 用途 | 何时使用 |
|-------|-------------|
| `direct` | 参考与期望输出密切匹配 |
| `style` | 仅提取视觉风格特征 |
| `palette` | 仅提取配色方案 |

---

## 步骤 3：确认设置 ⚠️

**不要跳过。** 使用一个 AskUserQuestion 调用，最多 4 个问题。**Q1、Q2、Q3 都是必需的。**

### Q1：预设或类型 ⚠️ 必须

基于步骤 2 内容分析，首先推荐预设（设置类型和风格）。查阅 [style-presets.md](style-presets.md) "内容类型 → 预设推荐"表。

- [推荐的预设] — [简要：类型 + 风格 + 原因]（推荐）
- [替代预设] — [简要]
- 或手动选择类型：信息图 / 场景 / 流程图 / 比较 / 框架 / 时间线 / 混合

**如果用户选择预设 → 跳过 Q3**（类型和风格都解决了）。
**如果用户选择类型 → Q3 是必需的。**

### Q2：密度级别 ⚠️ 必须 - 不要跳过
- minimal (1-2) - 仅核心概念
- balanced (3-5) - 主要部分
- per-section - 每个部分/章节至少 1 个（推荐）
- rich (6+) - 全覆盖

### Q3：风格 ⚠️ 如果在 Q1 中选择预设则跳过

如果 EXTEND.md 有 `preferred_style`：
- [自定义风格名称 + 简要描述]（推荐）
- [最兼容的核心风格 1]
- [最兼容的核心风格 2]
- 其他（见完整风格库）

如果没有 `preferred_style`（首先展示核心风格）：
- [最兼容的核心风格]（推荐）
- [其他兼容核心风格 1]
- [其他兼容核心风格 2]
- 其他（见完整风格库）

**核心风格**（简化选择）：

| 核心风格 | 对应 | 适用于 |
|------------|---------|----------|
| `minimal-flat` | notion | 一般、知识分享、SaaS |
| `sci-fi` | blueprint | AI、前沿技术、系统设计 |
| `hand-drawn` | sketch/warm | 放松、反思、休闲 |
| `editorial` | editorial | 流程、数据、新闻 |
| `scene` | warm/watercolor | 叙事、情感、生活方式 |
| `poster` | screen-print | 观点、社论、文化、电影 |

基于类型 × 风格兼容性矩阵选择风格（styles.md）。
完整规格：`styles/<style>.md`

### Q4：图片文本语言 ⚠️ 当文章语言 ≠ EXTEND.md `language` 时必须

从内容检测文章语言。如果与 EXTEND.md `language` 设置不同，必须询问：
- 文章语言（匹配文章内容）（推荐）
- EXTEND.md 语言（用户的一般偏好）

**仅在以下情况跳过**：文章语言匹配 EXTEND.md `language`，或 EXTEND.md 没有 `language` 设置。

### 显示参考使用情况（如果在步骤 1.0 中检测到参考）

向用户展示大纲预览时，显示参考分配：

```
参考图片：
| Ref | 文件名 | 推荐用途 |
|-----|----------|-------------------|
| 01 | 01-ref-diagram.png | direct → 插图 1、3 |
| 02 | 02-ref-chart.png | palette → 插图 2 |
```

---

## 步骤 4：生成大纲

保存为 `{output-dir}/outline.md`（以下所有路径相对于步骤 1.1/1.2 确定的输出目录）：

```yaml
---
type: infographic
density: balanced
style: blueprint
image_count: 4
references:                    # 仅当提供参考时
  - ref_id: 01
    filename: 01-ref-diagram.png
    description: "显示系统架构的技术图"
  - ref_id: 02
    filename: 02-ref-chart.png
    description: "带品牌调色板的颜色图表"
---

## 插图 1

**位置**：[部分] / [段落]
**目的**：[为何有帮助]
**视觉内容**：[显示什么]
**类型应用**：[类型如何应用]
**参考**：[01]                    # 可选：使用的 ref_id
**参考用途**：direct             # direct | style | palette
**文件名**：01-infographic-concept-name.png

## 插图 2
...
```

**要求**：
- 每个位置都有内容需要的理由
- 类型应用一致
- 风格在大纲中反映
- 数量与密度匹配
- 参考基于步骤 2.4 分析分配

---

## 步骤 5：生成图片

### 5.1 创建提示词 ⛔ 阻塞式操作

**每个插图在生成开始前必须有保存的提示词文件。不要跳过此步骤。**

对于大纲中的每个插图：

1. **创建提示词文件**：`{output-dir}/prompts/NN-{type}-{slug}.md`
2. **包含 YAML frontmatter**：
   ```yaml
   ---
   illustration_id: 01
   type: infographic
   style: custom-flat-vector
   ---
   ```
3. 遵循 [prompt-construction.md](prompt-construction.md) 中的类型特定模板
4. **提示词质量要求**（全部必需）：
   - `Layout`：描述整体构图（网格 / 径向 / 层级 / 左右 / 上下）
   - `ZONES`：用具体内容描述每个视觉区域，而非模糊描述
   - `LABELS`：使用**实际数字、术语、指标、文章引用** — 而非通用占位符
   - `COLORS`：指定带有语义含义的十六进制代码（例如 `Coral (#E07A5F) 用于强调`）
   - `STYLE`：描述线条处理、纹理、氛围、角色渲染
   - `ASPECT`：指定比例（例如 `16:9`）
5. **应用默认值**：构图要求、角色渲染、文本指南、水印
6. **备份规则**：如果提示词文件存在，重命名为 `prompts/NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.md`

**验证 ⚠️**：在继续 5.2 之前，确认所有提示词文件存在：
```
提示词文件：
- prompts/01-infographic-overview.md ✓
- prompts/02-infographic-distillation.md ✓
...
```

**不要**传递临时内联文本到 `--prompt`。生成命令应使用 `--promptfiles prompts/NN-{type}-{slug}.md` 或读取保存的文件内容用于 `--prompt`。

**执行选择**：
- 如果多个插图已有保存的提示词文件且任务现在是纯生成，优先使用 `baoyu-image-gen` 批量模式（`build-batch.ts` -> `main.ts --batchfile`）
- 仅当每个插图仍需要单独的提示词重写、风格探索或其他每图推理时，才使用子代理

**关键 - Frontmatter 中的参考**：
- 仅当 `references/` 目录中文件实际存在时，才添加 `references` 字段
- 如果风格/调色板是口头提取的（无文件），将信息追加到提示词正文
- 写入 frontmatter 前验证：`test -f references/NN-ref-{slug}.png`

### 5.2 选择生成技能

检查可用技能。如果多个，询问用户。

### 5.3 处理参考 ⚠️ 如果在步骤 1.0 中保存了参考则必须

**如果用户提供参考图片，不要跳过。** 对于每个有参考的插图：

1. **首先验证文件存在**：
   ```bash
   test -f references/NN-ref-{slug}.png && echo "exists" || echo "MISSING"
   ```
   - 如果文件缺失但出现在 frontmatter → 错误，修复 frontmatter 或移除 references 字段
   - 如果文件存在 → 继续处理

2. 读取提示词 frontmatter 获取参考信息
3. 根据用途类型处理：

| 用途 | 操作 | 示例 |
|-------|--------|---------|
| `direct` | 将参考路径添加到 `--ref` 参数 | `--ref references/01-ref-brand.png` |
| `style` | 分析参考，将风格特征追加到提示词 | "风格：干净线条、渐变背景..." |
| `palette` | 从参考提取颜色，追加到提示词 | "颜色：#E8756D 珊瑚色、#7ECFC0 薄荷绿..." |

4. 检查图像生成技能能力：

| 技能支持 `--ref` | 操作 |
|------------------------|--------|
| 是（例如 baoyu-image-gen 使用 Google） | 通过 `--ref` 传递参考图片 |
| 否 | 转换为文本描述，追加到提示词 |

**验证**：生成前确认参考处理：
```
参考处理：
- 插图 1：使用 01-ref-brand.png（direct）✓
- 插图 2：从 02-ref-style.png 提取调色板 ✓
```

### 5.4 应用水印（如果启用）

添加：`Include a subtle watermark "[content]" at [position].`

### 5.5 生成

1. 对于每个插图：
   - **备份规则**：如果图片文件存在，重命名为 `NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.md`
   - 如果有 `direct` 用法的参考：包含 `--ref` 参数
   - 生成图片
2. 每次后："已生成 X/N"
3. 失败时：重试一次，然后记录并继续

---

## 步骤 6：完成

### 6.1 更新文章

在相应段落之后插入，使用相对于文章文件的路径：

| `default_output_dir` | 插入路径 |
|----------------------|-------------|
| `imgs-subdir` | `![description](imgs/NN-{type}-{slug}.png)` |
| `same-dir` | `![description](NN-{type}-{slug}.png)` |
| `illustrations-subdir` | `![description](illustrations/NN-{type}-{slug}.png)` |
| `independent` | `![description](illustrations/{topic-slug}/NN-{type}-{slug}.png)`（相对于 cwd）|

Alt 文本：用文章语言的简洁描述。

### 6.2 输出摘要

```
文章插图完成！

文章：[路径]
类型：[类型] | 密度级别：[级别] | 风格：[风格]
位置：[目录]
图片：X/N 已生成

位置：
- 01-xxx.png → "[部分]"之后
- 02-yyy.png → "[部分]"之后

[如果失败]
失败：
- NN-zzz.png：[原因]
```
