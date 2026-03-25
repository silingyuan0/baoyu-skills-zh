---
name: baoyu-article-illustrator
description: 分析文章结构，识别需要配图的位置，使用类型 x 风格二维方式生成插图。当用户要求"illustrate article"、"add images"、"generate images for article"或"为文章配图"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-article-illustrator
---

# 文章插图生成器

分析文章，识别插图位置，生成类型 x 风格一致的图片。

## 双维度

| 维度 | 控制内容 | 示例 |
|------|---------|------|
| **类型** | 信息结构 | 信息图、场景、流程图、对比、框架、时间线 |
| **风格** | 视觉美学 | notion、暖色调、极简、蓝图、水彩、优雅 |

自由组合：`--type infographic --style blueprint`

或使用预设：`--preset tech-explainer` → 一个标志同时设置类型和风格。参见[风格预设](references/style-presets.md)。

## 类型

| 类型 | 最适用于 |
|------|---------|
| `infographic` | 数据、指标、技术内容 |
| `scene` | 叙事、情感表达 |
| `flowchart` | 流程、工作流 |
| `comparison` | 对比分析、选项比较 |
| `framework` | 模型、架构 |
| `timeline` | 历史、演进 |

## 风格

参见 [references/styles.md](references/styles.md) 了解核心风格、完整风格库以及类型 x 风格兼容性矩阵。

## 工作流程

```
- [ ] 步骤 1：预检（EXTEND.md、引用、配置）
- [ ] 步骤 2：分析内容
- [ ] 步骤 3：确认设置（AskUserQuestion）
- [ ] 步骤 4：生成大纲
- [ ] 步骤 5：生成图片
- [ ] 步骤 6：完成
```

### 步骤 1：预检

**1.5 加载偏好配置（EXTEND.md） ⛔ 阻塞**

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-article-illustrator/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "user"
```

```powershell
# PowerShell（Windows）
if (Test-Path .baoyu-skills/baoyu-article-illustrator/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|------|------|
| 找到 | 读取、解析、显示摘要 |
| 未找到 | ⛔ 执行[首次设置](references/config/first-time-setup.md) |

完整流程：[references/workflow.md](references/workflow.md#step-1-pre-check)

### 步骤 2：分析

| 分析项 | 输出 |
|--------|------|
| 内容类型 | 技术 / 教程 / 方法论 / 叙事 |
| 目的 | 信息传达 / 可视化 / 想象力 |
| 核心论点 | 2-5 个主要观点 |
| 位置 | 插图能增加价值的位置 |

**关键**：比喻 → 可视化底层概念，而非字面图像。

完整流程：[references/workflow.md](references/workflow.md#step-2-setup--analyze)

### 步骤 3：确认设置 ⚠️

**一次 AskUserQuestion，最多 4 个问题。Q1-Q2 必选。选择预设时 Q3 可跳过。**

| 问题 | 选项 |
|------|------|
| **Q1：预设或类型** | [推荐预设]、[备选预设]，或手动选择：infographic、scene、flowchart、comparison、framework、timeline、mixed |
| **Q2：密度** | minimal（1-2 张）、balanced（3-5 张）、per-section（推荐）、rich（6+ 张） |
| **Q3：风格** | [推荐]、minimal-flat、sci-fi、hand-drawn、editorial、scene、poster、其他 — **选择预设时跳过** |
| Q4：语言 | 当文章语言与 EXTEND.md 设置不同时 |

完整流程：[references/workflow.md](references/workflow.md#step-3-confirm-settings-)

### 步骤 4：生成大纲

保存 `outline.md`，包含前置元数据（type、density、style、image_count）和条目：

```yaml
## 插图 1
**位置**：[章节/段落]
**目的**：[为什么需要此图]
**视觉内容**：[展示什么]
**文件名**：01-infographic-concept-name.png
```

完整模板：[references/workflow.md](references/workflow.md#step-4-generate-outline)

### 步骤 5：生成图片

⛔ **阻塞：提示词文件必须在任何图片生成之前保存。**

**执行策略**：当多张插图已保存提示词文件且当前任务仅为生成时，优先使用 `baoyu-image-gen` 批量模式（`build-batch.ts` → `--batchfile`）而非派生子代理。仅当每张图片仍需独立的提示词迭代或创意探索时，才使用子代理。

1. 为每张插图，按 [references/prompt-construction.md](references/prompt-construction.md) 创建提示词文件
2. 保存为 `prompts/NN-{type}-{slug}.md`，含 YAML 前置元数据
3. 提示词**必须**使用类型专用模板，包含结构化段落（ZONES / LABELS / COLORS / STYLE / ASPECT）
4. LABELS **必须**包含文章具体数据：实际数字、术语、指标、引文
5. **不要**在未保存提示词文件的情况下直接向 `--prompt` 传递临时内联提示词
6. 选择生成技能，处理引用（`direct`/`style`/`palette`）
7. 如 EXTEND.md 启用，添加水印
8. 基于已保存的提示词文件生成图片；失败时重试一次

完整流程：[references/workflow.md](references/workflow.md#step-5-generate-images)

### 步骤 6：完成

在段落后插入 `![描述]({relative-path}/NN-{type}-{slug}.png)`。路径根据输出目录设置，基于文章文件计算相对路径。

```
Article Illustration Complete!
Article: [path] | Type: [type] | Density: [level] | Style: [style]
Images: X/N generated
```

## 输出目录

输出目录由 EXTEND.md 中的 `default_output_dir` 决定（在首次设置时配置）：

| `default_output_dir` | 输出路径 | Markdown 插入路径 |
|----------------------|---------|-------------------|
| `imgs-subdir`（默认） | `{article-dir}/imgs/` | `imgs/NN-{type}-{slug}.png` |
| `same-dir` | `{article-dir}/` | `NN-{type}-{slug}.png` |
| `illustrations-subdir` | `{article-dir}/illustrations/` | `illustrations/NN-{type}-{slug}.png` |
| `independent` | `illustrations/{topic-slug}/` | `illustrations/{topic-slug}/NN-{type}-{slug}.png`（相对于当前工作目录） |

所有辅助文件（大纲、提示词）保存在输出目录内：

```
{output-dir}/
├── outline.md
├── prompts/
│   └── NN-{type}-{slug}.md
└── NN-{type}-{slug}.png
```

当输入为**粘贴内容**（无文件路径）时，始终使用 `illustrations/{topic-slug}/`，并在旁边保存 `source-{slug}.{ext}`。

**短标识**：2-4 个单词，kebab-case 格式。**冲突处理**：追加 `-YYYYMMDD-HHMMSS`。

## 修改

| 操作 | 步骤 |
|------|------|
| 编辑 | 更新提示词 → 重新生成 → 更新引用 |
| 添加 | 确定位置 → 创建提示词 → 生成 → 更新大纲 → 插入 |
| 删除 | 删除文件 → 移除引用 → 更新大纲 |

## 参考资料

| 文件 | 内容 |
|------|------|
| [references/workflow.md](references/workflow.md) | 详细流程 |
| [references/usage.md](references/usage.md) | 命令语法 |
| [references/styles.md](references/styles.md) | 风格库 |
| [references/style-presets.md](references/style-presets.md) | 预设快捷方式（类型 + 风格） |
| [references/prompt-construction.md](references/prompt-construction.md) | 提示词模板 |
| [references/config/first-time-setup.md](references/config/first-time-setup.md) | 首次设置 |
