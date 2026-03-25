---
name: first-time-setup
description: baoyu-article-illustrator 首选项的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成首选项设置。

**⛔ 阻塞式操作（Blocking Operation）**: 此设置必须在任何其他工作流程步骤之前完成。禁止：
- 询问参考图片
- 询问内容/文章
- 询问类型或风格偏好
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
question: "为生成的插图添加水印？请输入水印内容（例如：名称、@handle）"
options:
  - label: "无水印（推荐）"
    description: "无水印，可稍后在 EXTEND.md 中启用"
```

位置默认为右下角。

### 问题 2：首选风格

```
header: "风格"
question: "默认插图风格偏好？或输入其他风格名称或您的自定义风格"
options:
  - label: "无（推荐）"
    description: "根据内容分析自动选择"
  - label: "notion"
    description: "极简手绘线条艺术"
  - label: "warm"
    description: "友好、平易近人、有个性"
```

### 问题 3：输出目录

```
header: "输出目录"
question: "为文章生成插图时保存到哪里？"
options:
  - label: "imgs-subdir（推荐）"
    description: "{article-dir}/imgs/ — 文章目录下的图片子目录"
  - label: "same-dir"
    description: "{article-dir}/ — 图片与文章文件并列"
  - label: "illustrations-subdir"
    description: "{article-dir}/illustrations/ — 独立的插图子目录"
  - label: "independent"
    description: "illustrations/{topic-slug}/ — 工作目录中的独立目录"
```

### 问题 4：保存位置

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
| 项目 | `.baoyu-skills/baoyu-article-illustrator/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-article-illustrator/EXTEND.md` | 所有项目 |

## 设置之后

1. 如有需要创建目录
2. 使用前置元数据写入 EXTEND.md
3. 确认："首选项已保存到 [路径]"
4. 继续执行步骤 1

## EXTEND.md 模板

```yaml
---
version: 1
watermark:
  enabled: [true/false]
  content: "[用户输入或空]"
  position: bottom-right
  opacity: 0.7
preferred_style:
  name: [所选风格或 null]
  description: ""
default_output_dir: imgs-subdir  # same-dir | imgs-subdir | illustrations-subdir | independent
language: null
custom_styles: []
---
```

## 后续修改首选项

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 触发设置
- 编辑 YAML 前置元数据进行快速更改
- 完整架构：`config/preferences-schema.md`
