# 完整工作流

生成知识漫画的完整工作流。

## 进度检查清单

复制并跟踪进度：

```
漫画进度：
- [ ] 步骤 1：设置与分析
  - [ ] 1.1 加载偏好设置
  - [ ] 1.2 分析内容
  - [ ] 1.3 检查现有 ⚠️ 必需
- [ ] 步骤 2：确认 1 - 风格与选项 ⚠️ 必需
- [ ] 步骤 3：生成分镜故事板 + 角色
- [ ] 步骤 4：审核大纲（条件性）
- [ ] 步骤 5：生成提示词
- [ ] 步骤 6：审核提示词（条件性）
- [ ] 步骤 7：生成图像
- [ ] 步骤 8：合并为 PDF
- [ ] 步骤 9：完成报告
```

## 流程图

```
输入 → 偏好设置 → 分析 → [检查现有？] → [确认 1：风格 + 审核] → 分镜故事板 → [审核大纲？] → 提示词 → [审核提示词？] → 图像 → PDF → 完成
```

---

## 步骤 1：设置与分析

### 1.1 加载偏好设置（EXTEND.md）

检查 EXTEND.md 存在性（优先级顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-comic/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-comic/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-comic/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-comic/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-comic/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-comic/EXTEND.md") { "user" }
```

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-comic/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-comic/EXTEND.md` | 用户主目录 |

**当找到 EXTEND.md 时** → 读取、解析、**向用户输出摘要**：

```
📋 已从 [完整路径] 加载偏好设置
├─ 水印：[启用/禁用] [如果启用则显示内容]
├─ 画风：[风格名称或"自动选择"]
├─ 基调：[基调名称或"自动选择"]
├─ 版面布局：[布局或"自动选择"]
├─ 语言：[语言或"自动检测"]
└─ 角色预设：已定义 [数量] 个
```

**必须输出此摘要** 以便用户了解当前配置。不要跳过或静默加载。

**当未找到 EXTEND.md 时** → 首次设置：

1. 通知用户："未找到偏好设置。让我们设置您的默认值。"
2. 使用 AskUserQuestion 收集偏好设置（见 `config/first-time-setup.md`）
3. 在用户选择的位置创建 EXTEND.md
4. 确认："✓ 偏好设置已保存到 [路径]"

**EXTEND.md 支持**：水印 | 偏好的画风/基调/布局 | 自定义风格定义 | 角色预设 | 语言偏好

Schema：`config/preferences-schema.md`

**重要**：一旦 EXTEND.md 存在，水印、语言和风格默认值不会在确认 1 或 2 中再次询问。这些是会话持久设置。

### 1.2 分析内容 → `analysis.md`

读取源内容，保存（如需要），并执行深度分析。

**操作**：
1. **保存源内容**（如果不是文件）：
   - 如果用户提供文件路径：按原样使用
   - 如果用户粘贴内容：保存到目标目录的 `source.md`
   - **备份规则**：如果 `source.md` 存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 读取源内容
3. **深度分析** 遵循 `analysis-framework.md`：
   - 目标受众识别
   - 对读者的价值主张
   - 核心主题和叙事潜力
   - 关键人物及其故事弧线
4. 检测源语言
5. **确定语言**：
   - 如果 EXTEND.md 有 `language` → 使用它
   - 否则如果提供了 `--lang` 选项 → 使用它
   - 否则 → 使用检测到的源语言
6. 确定推荐的页数：
   - 短故事：5-8 页
   - 中等复杂度：9-15 页
   - 完整传记：16-25 页
7. 分析内容信号以获得画风/基调/布局推荐
8. **保存到 `analysis.md`**

**analysis.md 格式**：YAML 前置元数据（title, topic, time_span, source_language, user_language, aspect_ratio, recommended_page_count, recommended_art, recommended_tone）+ 目标受众、价值主张、核心主题、关键人物与故事弧线、内容信号、推荐方法等部分。参见 `analysis-framework.md` 完整模板。

### 1.3 检查现有内容 ⚠️ 必需

**在继续步骤 2 之前必须执行。**

使用 Bash 检查输出目录是否存在：

```bash
test -d "comic/{topic-slug}" && echo "exists"
```

**如果目录存在**，使用 AskUserQuestion：

```
header: "Existing"
question: "找到现有内容。如何继续？"
options:
  - label: "重新生成分镜故事板"
    description: "保留图像，仅重新生成分镜故事板和角色"
  - label: "重新生成图像"
    description: "保留分镜故事板，仅重新生成图像"
  - label: "备份并重新生成"
    description: "备份到 {slug}-backup-{timestamp}，然后重新生成所有"
  - label: "退出"
    description: "取消，保持现有内容不变"
```

保存结果并相应处理：
- **重新生成分镜故事板**：跳至步骤 3，保留 `prompts/` 和图像
- **重新生成图像**：跳至步骤 7，使用现有提示词
- **备份并重新生成**：移动目录，从步骤 2 开始全新
- **退出**：立即结束工作流

---

## 步骤 2：确认 1 - 风格与选项 ⚠️

**目的**：选择视觉风格 + 决定是否在校对前审核大纲。**不要跳过。**

**注意**：水印和语言已在 EXTEND.md（步骤 1）中配置。

**显示摘要**：
- 识别到的内容类型 + 主题
- 提取的关键人物
- 检测到的时间跨度
- 推荐页数
- 语言：[来自 EXTEND.md 或检测到的]
- **推荐风格**：[画风] + [基调]（基于内容信号）

使用 AskUserQuestion 提问：

### 问题 1：视觉风格

如果推荐了预设（见 `auto-selection.md`），首先显示它：

```
header: "Style"
question: "这个漫画使用哪种视觉风格？"
options:
  - label: "[预设名称] 预设（推荐）"       # 如果推荐了预设
    description: "[预设描述] - 包含特殊规则"
  - label: "[推荐画风] + [推荐基调]（推荐）"  # 如果没有预设
    description: "基于分析，为您的内容最佳匹配"
  - label: "ligne-claire + neutral"
    description: "经典教育风格，Logicomix 风格"
  - label: "ohmsha 预设"
    description: "教育漫画，视觉隐喻，小工具，禁止头像对话"
  - label: "自定义"
    description: "指定您自己的画风 + 基调或预设"
```

**预设 vs 画风+基调**：预设包含超越画风+基调的特殊规则。`ohmsha` = manga + neutral + 视觉隐喻规则 + 角色角色 + 禁止头像对话。普通的 `manga + neutral` 不包含这些规则。

### 问题 2：叙事焦点（multiSelect: true）

```
header: "Focus"
question: "漫画应该强调什么？（选择所有适用项）"
options:
  - label: "传记/人生故事"
    description: "通过关键人生事件追随一个人的旅程"
  - label: "概念解释"
    description: "视觉化分解复杂概念"
  - label: "历史事件"
    description: "戏剧化重要的历史时刻"
  - label: "教程/指南"
    description: "逐步教育指南"
```

### 问题 3：目标受众

```
header: "Audience"
question: "谁是主要读者？"
options:
  - label: "一般读者"
    description: "广泛吸引力，易于理解的内容"
  - label: "学生/学习者"
    description: "教育重点，清晰解释"
  - label: "行业专业人士"
    description: "技术深度领域知识"
  - label: "儿童/年轻读者"
    description: "简化的语言，引人入胜的视觉"
```

### 问题 4：大纲审核

```
header: "Review"
question: "要在图像生成前审核大纲吗？"
options:
  - label: "是的，我要审核（推荐）"
    description: "在生成图像前审核分镜故事板和角色"
  - label: "否，直接生成"
    description: "跳过大纲审核，立即开始生成"
```

### 问题 5：提示词审核

```
header: "Prompts"
question: "要在生成图像前审核提示词吗？"
options:
  - label: "是的，审核提示词（推荐）"
    description: "在生成图像前审核图像生成提示词"
  - label: "否，跳过提示词审核"
    description: "直接进入图像生成"
```

**响应后**：
1. 更新 `analysis.md` 包含用户偏好
2. 基于问题 4 响应存储 `skip_outline_review` 标志
3. 基于问题 5 响应存储 `skip_prompt_review` 标志
4. → 步骤 3

---

## 步骤 3：生成分镜故事板 + 角色

使用步骤 2 确认的样式创建分镜故事板和角色定义。

**加载样式参考**：
- 画风：`art-styles/{art}.md`
- 基调：`tones/{tone}.md`
- 如果是预设（ohmsha/wuxia/shoujo）：也加载 `presets/{preset}.md`

**生成**：

1. **分镜故事板**（`storyboard.md`）：
   - 带 art_style、tone、layout、aspect_ratio 的 YAML 前置元数据
   - 封面设计
   - 每页：布局、面板分解、视觉提示词
   - **以用户首选语言撰写**（来自步骤 1）
   - 参考：`storyboard-template.md`
   - **如果使用预设**：从 `presets/` 加载并应用预设规则

2. **角色定义**（`characters/characters.md`）：
   - 与画风匹配的视觉规格（用户首选语言）
   - 包含用于后续图像生成的参考表提示词
   - 参考：`character-template.md`
   - **如果使用 ohmsha 预设**：使用默认哆啦A梦角色（见下文）

**Ohmsha 默认角色**（除非用户指定 `--characters`，否则使用这些）：

| 角色 | 角色 | 视觉描述 |
|------|-----------|-------------------|
| 学生 | 大雄 (Nobita) | 日本男孩，10岁，圆眼镜，中分黑发，黄色衬衫，藏青色短裤 |
| 导师 | 哆啦A梦 (Doraemon) | 圆蓝色机器猫，大白眼睛，红鼻子，胡须，白色肚子带4D口袋，金色铃铛，没有耳朵 |
| 挑战者 | 胖虎 (Gian) | 矮胖男孩，粗犷特征，小眼睛，橙色衬衫 |
| 支持者 | 静香 (Shizuka) | 可爱女孩，黑色短发，粉色连衣裙，表情温柔 |

这些是规范的 ohmsha 风格角色。除非明确要求，否则不要为 ohmsha 创建自定义角色。

**生成后**：
- 如果 `skip_outline_review` 为 true → 跳过步骤 4，直接进入步骤 5
- 如果 `skip_outline_review` 为 false → 继续步骤 4

---

## 步骤 4：审核大纲（条件性）

**如果用户在步骤 2 中选择"否，直接生成"，则跳过此步骤。**

**目的**：用户在生成前审核和确认分镜故事板 + 角色。

**显示**：
- 页数和结构
- 画风 + 基调组合
- 页-by-页摘要（封面 → P1 → P2...）
- 角色列表及简要描述

使用 AskUserQuestion：

```
header: "Confirm"
question: "准备好用此大纲生成图像了吗？"
options:
  - label: "是的，继续（推荐）"
    description: "生成角色表和漫画页面"
  - label: "先编辑分镜故事板"
    description: "我将在继续前修改 storyboard.md"
  - label: "先编辑角色"
    description: "我将在继续前修改 characters/characters.md"
  - label: "两者都编辑"
    description: "我将在继续前修改两个文件"
```

**响应后**：
1. 如果用户想要编辑 → 等待用户完成编辑，然后再次提问
2. 如果用户确认 → 继续步骤 5

---

## 步骤 5：生成提示词

为所有页面创建图像生成提示词。

**样式参考加载**：
- 阅读 `art-styles/{art}.md` 获取渲染指南
- 阅读 `tones/{tone}.md` 获取基调/颜色调整
- 如果是预设：阅读 `presets/{preset}.md` 获取特殊规则

**对于每个页面（封面 + 页面）**：
1. 遵循画风 + 基调指南创建提示词
2. 包含用于一致性的角色视觉描述
3. 保存到 `prompts/NN-{cover|page}-[slug].md`
   - **备份规则**：如果提示词文件存在，重命名为 `prompts/NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.md`

**提示词文件格式**：
```markdown
# Page NN: [Title]

## Visual Style
Art: [画风] | Tone: [基调] | Layout: [布局类型]

## Character Reference
[来自 characters/characters.md 的角色描述]

## Panel Breakdown
[来自 storyboard.md - 面板描述、动作、对话]

## Generation Prompt
[用于图像生成智能体的组合提示词]
```

**水印应用**（如果在偏好设置中启用）：
添加到每个提示词：
```
包含一个位于 [位置] 的微妙水印 "[内容]"，可见度约为 [opacity*100]%。
水印应清晰可见但不应分散漫画面板和叙事的注意力。
确保水印不与对话气泡或关键动作重叠。
```
参考：`config/watermark-guide.md`

**生成后**：
- 如果 `skip_prompt_review` 为 true → 跳过步骤 6，直接进入步骤 7
- 如果 `skip_prompt_review` 为 false → 继续步骤 6

---

## 步骤 6：审核提示词（条件性）

**如果用户在步骤 2 中选择"否，跳过提示词审核"，则跳过此步骤。**

**目的**：用户在图像生成前审核和确认提示词。

**显示提示词摘要表**：

| 页面 | 标题 | 关键元素 |
|------|-------|--------------|
| Cover | [标题] | [主要视觉] |
| P1 | [标题] | [关键元素] |
| ... | ... | ... |

使用 AskUserQuestion：

```
header: "Confirm"
question: "准备好用这些提示词生成图像了吗？"
options:
  - label: "是的，继续（推荐）"
    description: "生成所有漫画页面图像"
  - label: "先编辑提示词"
    description: "我将在继续前修改 prompts/*.md"
  - label: "重新生成提示词"
    description: "用不同方法重新生成所有提示词"
```

**响应后**：
1. 如果用户想要编辑 → 等待用户完成编辑，然后再次提问
2. 如果用户想要重新生成 → 返回步骤 5
3. 如果用户确认 → 继续步骤 7

---

## 步骤 7：生成图像

使用步骤 5/6 确认的提示词：

### 7.1 生成角色参考表（首先）

1. 使用 `characters/characters.md` 中的参考表提示词
2. **备份规则**：如果 `characters/characters.png` 存在，重命名为 `characters/characters-backup-YYYYMMDD-HHMMSS.png`
3. 生成 → `characters/characters.png`
4. 这确保了所有后续页面的视觉一致性

### 7.2 生成漫画页面

**关键：角色参考对所有页面的视觉一致性是必需的。**

**在任何页面生成之前**：
1. 阅读图像生成智能体的 SKILL.md
2. 检查它是否支持参考图像输入（`--ref`、`--reference` 等）
3. 根据以下适当策略选择

**角色参考策略**：

| 智能体能力 | 策略 | 操作 |
|------------------|----------|--------|
| 支持 `--ref` | **策略 A** | 对每个页面传递 `characters/characters.png` |
| 不支持 `--ref` | **策略 B** | 在每个提示词前添加角色描述 |

**策略 A：使用 `--ref` 参数**（例如 baoyu-image-gen）

- 阅读所选图像生成智能体的 `SKILL.md`
- 通过其记录的接口调用已安装的智能体，而非直接调用其脚本
- 对于每个页面，使用 `prompts/01-page-xxx.md` 作为提示词文件输入
- 保存输出到 `01-page-xxx.png`
- 使用宽高比 `3:4`
- 在每个页面生成时传递 `characters/characters.png` 作为 `--ref`

**策略 B：在提示词中嵌入角色描述**

当智能体不支持参考图像时，创建组合提示词文件：

```markdown
# prompts/01-page-xxx.md（嵌入角色参考）

## Character Reference (maintain consistency)
[从这里复制 characters/characters.md 的相关部分]
- 大雄：日本男孩，圆眼镜，黄色衬衫，藏青色短裤...
- 哆啦A梦：圆蓝色机器猫，白色肚子，红鼻子，金色铃铛...

## Page Content
[原始页面提示词在这里]
```

**对于每个页面（封面 + 页面）**：
1. 从 `prompts/NN-{cover|page}-[slug].md` 读取提示词
2. **备份规则**：如果图像文件存在，重命名为 `NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.png`
3. 使用策略 A 或 B（基于智能体能力）生成图像
4. 保存到 `NN-{cover|page}-[slug].png`
5. 每次生成后报告进度："已生成 X/N：[页面标题]"

**会话管理**：
如果图像生成智能体支持 `--sessionId`：
1. 生成唯一会话 ID：`comic-{topic-slug}-{timestamp}`
2. 对所有页面使用相同的会话 ID
3. 确保跨生成图像的视觉一致性

---

## 步骤 8：合并为 PDF

所有图像生成后：

```bash
${BUN_X} {baseDir}/scripts/merge-to-pdf.ts <comic-dir>
```

创建包含所有页面为全页图像的 `{topic-slug}.pdf`。

---

## 步骤 9：完成报告

```
漫画完成！
标题：[标题] | 画风：[画风] | 基调：[基调] | 页数：[数量] | 宽高比：[比例] | 语言：[语言]
水印：[启用/禁用]
位置：[路径]
✓ analysis.md
✓ characters.png
✓ 00-cover-[slug].png ... NN-page-[slug].png
✓ {topic-slug}.pdf
```

---

## 页面修改

| 操作 | 步骤 |
|--------|-------|
| **编辑** | 更新提示词 → 重新生成图像 → 重新生成 PDF |
| **添加** | 在位置创建提示词 → 生成图像 → 重新编号后续（NN+1） → 更新分镜故事板 → 重新生成 PDF |
| **删除** | 删除文件 → 重新编号后续（NN-1） → 更新分镜故事板 → 重新生成 PDF |

**文件命名**：`NN-{cover|page}-[slug].png`（例如 `03-page-enigma-machine.png`）
- Slug：kebab-case、唯一、从内容派生
- 重新编号：仅更新 NN 前缀，slug 不变
