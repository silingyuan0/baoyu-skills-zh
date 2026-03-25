# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：此设置必须在任何翻译之前完成。禁止：
- 开始翻译内容
- 询问文件或输出路径
- 进入任何工作流程步骤

仅提出此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
+---------------------+
| AskUserQuestion     |
| （所有问题）         |
+---------------------+
        │
        ▼
+---------------------+
| 创建 EXTEND.md       |
+---------------------+
        │
        ▼
    继续翻译
```

## 问题

**语言**：使用用户的输入语言或已保存的语言偏好。

使用 AskUserQuestion 在一次调用中提出所有问题：

### 问题 1：目标语言

```yaml
header: "目标语言"
question: "默认目标语言是什么？"
options:
  - label: "简体中文 zh-CN（推荐）"
    description: "翻译为简体中文"
  - label: "繁體中文 zh-TW"
    description: "翻译为繁体中文"
  - label: "English en"
    description: "翻译为英语"
  - label: "日本語 ja"
    description: "翻译为日语"
```

注：用户可输入自定义语言代码。

### 问题 2：翻译模式

```yaml
header: "模式"
question: "默认翻译模式是什么？"
options:
  - label: "正常（推荐）"
    description: "先分析内容，再翻译"
  - label: "快速"
    description: "直接翻译，无需分析"
  - label: "精细"
    description: "完整工作流：分析→翻译→审校→润色"
```

### 问题 3：目标受众

```yaml
header: "受众"
question: "默认目标受众是什么？"
options:
  - label: "普通读者（推荐）"
    description: "通俗语言，更多术语译注"
  - label: "技术人员"
    description: "开发/工程师，减少技术术语注解"
  - label: "学术人员"
    description: "正式语域，精确术语"
  - label: "商务人员"
    description: "商务友好语气，解释技术概念"
```

注：用户可输入自定义受众描述。

### 问题 4：翻译风格

```yaml
header: "风格"
question: "翻译风格是什么？"
options:
  - label: "叙事风格（推荐）"
    description: "引人入胜的叙事流程，流畅过渡"
  - label: "正式"
    description: "专业、结构化、中性语气"
  - label: "技术"
    description: "精确、文档风格、简洁"
  - label: "直译"
    description: "贴近原始结构"
  - label: "学术"
    description: "学术性、严谨、正式语域"
  - label: "商务"
    description: "简洁、结果导向、行动导向"
  - label: "幽默"
    description: "保留幽默、诙谐、活泼"
  - label: "会话"
    description: "休闲、友好、口语化"
  - label: "优雅"
    description: "文学感、精致、审美考究"
```

注：用户可输入自定义风格描述。

### 问题 5：保存位置

```yaml
header: "保存"
question: "偏好设置保存到哪里？"
options:
  - label: "用户（推荐）"
    description: "$HOME/.baoyu-skills/（所有项目）"
  - label: "项目"
    description: ".baoyu-skills/（仅当前项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 用户 | `$HOME/.baoyu-skills/baoyu-translate/EXTEND.md` | 所有项目 |
| 项目 | `.baoyu-skills/baoyu-translate/EXTEND.md` | 当前项目 |

## 设置后

1. 按需创建目录
2. 使用所选值写入 EXTEND.md
3. 确认："偏好设置已保存至 [路径]"
4. 说明："您可随时向 EXTEND.md 添加自定义术语表。参见文件中的 `glossary` 部分了解格式。"
5. 使用保存的偏好继续翻译

## EXTEND.md 模板

```yaml
target_language: [zh-CN/zh-TW/en/ja/...]
default_mode: [quick/normal/refined]
audience: [general/technical/academic/business/custom]
style: [storytelling/formal/technical/literal/academic/business/humorous/conversational/elegant]

# 自定义术语表（可选）— 在此添加您自己的术语翻译
# glossary:
#   - from: "Term"
#     to: "翻译"
#   - from: "Another Term"
#     to: "另一个翻译"
#     note: "使用语境"
```

## 后续修改偏好

用户可直接编辑 EXTEND.md 或删除它以重新触发设置。
