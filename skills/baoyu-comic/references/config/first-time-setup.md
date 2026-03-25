---
name: first-time-setup
description: baoyu-comic 首选项的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成首选项设置。

**⛔ 阻塞式操作**: 此设置必须在任何其他工作流程步骤之前完成。禁止：
- 询问内容/源材料
- 询问艺术风格或基调
- 询问布局偏好
- 继续进行内容分析

仅询问此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ （所有问题）          │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ 创建 EXTEND.md      │
└─────────────────────┘
        │
        ▼
    继续执行步骤 1
```

## 问题

**语言**: 对所有问题使用用户的输入语言或首选语言。不要总是使用英语。

使用单个 AskUserQuestion 包含多个问题（AskUserQuestion 自动添加"其他"选项）：

### 问题 1：水印

```
header: "水印"
question: "为生成的漫画页面添加水印？请输入您的水印内容（例如：名称、@handle）"
options:
  - label: "无水印（推荐）"
    description: "无水印，可稍后在 EXTEND.md 中启用"
```

位置默认为右下角。

### 问题 2：首选艺术风格

```
header: "艺术"
question: "默认艺术风格偏好？或输入其他风格名称"
options:
  - label: "自动选择（推荐）"
    description: "根据内容分析自动选择"
  - label: "ligne-claire"
    description: "均匀线条、平坦色彩、欧洲漫画（丁丁风格）"
  - label: "manga"
    description: "日本漫画风格，表情丰富的眼睛和情感"
  - label: "realistic"
    description: "数字绘画，精湛专业"
```

### 问题 3：首选基调

```
header: "基调"
question: "默认基调/氛围偏好？"
options:
  - label: "自动选择（推荐）"
    description: "根据内容信号自动选择"
  - label: "neutral"
    description: "平衡、理性、教育"
  - label: "warm"
    description: "怀旧、个人、舒适"
  - label: "dramatic"
    description: "高对比、强烈、有力"
```

### 问题 4：语言

```
header: "语言"
question: "漫画文本的输出语言？"
options:
  - label: "自动检测（推荐）"
    description: "匹配源内容语言"
  - label: "zh"
    description: "中文（中文）"
  - label: "en"
    description: "英语"
```

### 问题 5：保存位置

```
header: "保存"
question: "在哪里保存首选项？"
options:
  - label: "项目"
    description: ".baoyu-skills/（仅当前项目）"
  - label: "用户"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-comic/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-comic/EXTEND.md` | 所有项目 |

## 设置之后

1. 如有需要创建目录
2. 使用前置元数据写入 EXTEND.md
3. 确认："首选项已保存到 [路径]"
4. 继续执行步骤 1

## EXTEND.md 模板

```yaml
---
version: 2
watermark:
  enabled: [true/false]
  content: "[用户输入或空]"
  position: bottom-right
  opacity: 0.5
preferred_art: [所选艺术风格或 null]
preferred_tone: [所选基调或 null]
preferred_layout: null
preferred_aspect: null
language: [所选语言或 null]
character_presets: []
---
```

## 后续修改首选项

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 触发设置
- 编辑 YAML 前置元数据进行快速更改
- 完整架构：`config/preferences-schema.md`
