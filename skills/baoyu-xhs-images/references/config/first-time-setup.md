# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：此设置必须在任何其他工作流程步骤之前完成。禁止：
- 询问内容/文章相关问题
- 询问风格或布局相关问题
- 询问目标受众相关问题
- 进入内容分析步骤

仅提出此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ （所有问题）         │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ 创建 EXTEND.md       │
└─────────────────────┘
        │
        ▼
    进入第 1 步
```

## 问题

**语言**：使用用户的输入语言或已保存的语言偏好。

使用单一的 AskUserQuestion 提问，包含多个问题（AskUserQuestion 会自动添加"其他"选项）：

### 问题 1：水印

```
header: "水印"
question: "生成图像的水印文字是什么？输入您的水印内容（例如：姓名、@handle）"
options:
  - label: "无水印（推荐）"
    description: "无水印，之后可在 EXTEND.md 中启用"
```

位置默认为右下角。

### 问题 2：偏好风格

```
header: "风格"
question: "默认视觉风格偏好？或输入其他风格名称或您的自定义风格"
options:
  - label: "无（推荐）"
    description: "根据内容分析自动选择"
  - label: "cute"
    description: "甜美、可爱 - 经典小红书风格"
  - label: "notion"
    description: "简约手绘、理性风格"
```

### 问题 3：保存位置

```
header: "保存"
question: "偏好设置保存到哪里？"
options:
  - label: "项目"
    description: ".baoyu-skills/（仅当前项目）"
  - label: "用户"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-xhs-images/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-xhs-images/EXTEND.md` | 所有项目 |

## 设置后

1. 按需创建目录
2. 使用 frontmatter 写入 EXTEND.md
3. 确认："偏好设置已保存至 [路径]"
4. 进入第 1 步

## EXTEND.md 模板

```yaml
---
version: 1
watermark:
  enabled: [true/false]
  content: "[用户输入或为空]"
  position: bottom-right
  opacity: 0.7
preferred_style:
  name: [所选风格或 null]
  description: ""
preferred_layout: null
language: null
custom_styles: []
---
```

## 后续修改偏好

用户可直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 以触发设置
- 编辑 YAML frontmatter 以快速修改
- 完整 schema：`config/preferences-schema.md`
