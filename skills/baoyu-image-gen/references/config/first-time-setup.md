# 首次设置

## 概述

触发条件：
1. 未找到 EXTEND.md → 完整设置（提供商 + 模型 + 偏好设置）
2. 找到 EXTEND.md 但 `default_model.[provider]` 为 null → 仅选择模型

## 设置流程

```
No EXTEND.md found          EXTEND.md found, model null
        │                            │
        ▼                            ▼
┌─────────────────────┐    ┌──────────────────────┐
│ AskUserQuestion     │    │ AskUserQuestion      │
│ (full setup)        │    │ (model only)         │
└─────────────────────┘    └──────────────────────┘
        │                            │
        ▼                            ▼
┌─────────────────────┐    ┌──────────────────────┐
│ Create EXTEND.md    │    │ Update EXTEND.md     │
└─────────────────────┘    └──────────────────────┘
        │                            │
        ▼                            ▼
    Continue                     Continue
```

## 流程 1：无 EXTEND.md（完整设置）

**语言**：使用用户的输入语言或保存的语言偏好。

使用 AskUserQuestion 在一次调用中提出所有问题：

### 问题 1：默认提供商

```yaml
header: "Provider"
question: "Default image generation provider?"
options:
  - label: "Google (Recommended)"
    description: "Gemini multimodal - high quality, reference images, flexible sizes"
  - label: "OpenAI"
    description: "GPT Image - consistent quality, reliable output"
  - label: "OpenRouter"
    description: "Router for Gemini/FLUX/OpenAI-compatible image models"
  - label: "DashScope"
    description: "Alibaba Cloud - Qwen-Image, strong Chinese/English text rendering"
  - label: "Replicate"
    description: "Community models - nano-banana-pro, flexible model selection"
```

### 问题 2：默认 Google 模型

仅在用户选择 Google 或自动检测（无显式提供商）时显示。

```yaml
header: "Google Model"
question: "Default Google image generation model?"
options:
  - label: "gemini-3-pro-image-preview (Recommended)"
    description: "Highest quality, best for production use"
  - label: "gemini-3.1-flash-image-preview"
    description: "Fast generation, good quality, lower cost"
  - label: "gemini-3-flash-preview"
    description: "Fast generation, balanced quality and speed"
```

### 问题 2b：默认 OpenRouter 模型

仅在用户选择 OpenRouter 时显示。

```yaml
header: "OpenRouter Model"
question: "Default OpenRouter image generation model?"
options:
  - label: "google/gemini-3.1-flash-image-preview (Recommended)"
    description: "Best general-purpose OpenRouter image model with reference-image workflows"
  - label: "google/gemini-2.5-flash-image-preview"
    description: "Fast Gemini preview model on OpenRouter"
  - label: "black-forest-labs/flux.2-pro"
    description: "Strong text-to-image quality through OpenRouter"
```

### 问题 3：默认质量

```yaml
header: "Quality"
question: "Default image quality?"
options:
  - label: "2k (Recommended)"
    description: "2048px - covers, illustrations, infographics"
  - label: "normal"
    description: "1024px - quick previews, drafts"
```

### 问题 4：保存位置

```yaml
header: "Save"
question: "Where to save preferences?"
options:
  - label: "Project (Recommended)"
    description: ".baoyu-skills/ (this project only)"
  - label: "User"
    description: "~/.baoyu-skills/ (all projects)"
```

### 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-image-gen/EXTEND.md` | 当前项目 |
| 用户 | `$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 所有项目 |

### EXTEND.md 模板

```yaml
---
version: 1
default_provider: [selected provider or null]
default_quality: [selected quality]
default_aspect_ratio: null
default_image_size: null
default_model:
  google: [selected google model or null]
  openai: null
  openrouter: [selected openrouter model or null]
  dashscope: null
  replicate: null
---
```

## 流程 2：EXTEND.md 存在，模型为 null

当 EXTEND.md 存在但 `default_model.[current_provider]` 为 null 时，仅询问当前提供商的模型问题。

### Google 模型选择

```yaml
header: "Google Model"
question: "Choose a default Google image generation model?"
options:
  - label: "gemini-3-pro-image-preview (Recommended)"
    description: "Highest quality, best for production use"
  - label: "gemini-3.1-flash-image-preview"
    description: "Fast generation, good quality, lower cost"
  - label: "gemini-3-flash-preview"
    description: "Fast generation, balanced quality and speed"
```

### OpenAI 模型选择

```yaml
header: "OpenAI Model"
question: "Choose a default OpenAI image generation model?"
options:
  - label: "gpt-image-1.5 (Recommended)"
    description: "Latest GPT Image model, high quality"
  - label: "gpt-image-1"
    description: "Previous generation GPT Image model"
```

### OpenRouter 模型选择

```yaml
header: "OpenRouter Model"
question: "Choose a default OpenRouter image generation model?"
options:
  - label: "google/gemini-3.1-flash-image-preview (Recommended)"
    description: "Recommended for image output and reference-image edits"
  - label: "google/gemini-2.5-flash-image-preview"
    description: "Fast preview-oriented image generation"
  - label: "black-forest-labs/flux.2-pro"
    description: "High-quality text-to-image through OpenRouter"
```

### DashScope 模型选择

```yaml
header: "DashScope Model"
question: "Choose a default DashScope image generation model?"
options:
  - label: "qwen-image-2.0-pro (Recommended)"
    description: "Best DashScope model for text rendering and custom sizes"
  - label: "qwen-image-2.0"
    description: "Faster 2.0 variant with flexible output size"
  - label: "qwen-image-max"
    description: "Legacy Qwen model with five fixed output sizes"
  - label: "qwen-image-plus"
    description: "Legacy Qwen model, same current capability as qwen-image"
  - label: "z-image-turbo"
    description: "Legacy DashScope model for compatibility"
  - label: "z-image-ultra"
    description: "Legacy DashScope model, higher quality but slower"
```

DashScope 设置注意事项：

- 当用户需要自定义 `--size`、不常见比例如 `21:9` 或强大的中英文文字渲染时，优先使用 `qwen-image-2.0-pro`。
- `qwen-image-max` / `qwen-image-plus` / `qwen-image` 仅支持五种固定尺寸：`1664*928`、`1472*1104`、`1328*1328`、`1104*1472`、`928*1664`。
- 在 `baoyu-image-gen` 中，`quality` 是一个兼容性预设。它不是原生 DashScope 参数。

### Replicate 模型选择

```yaml
header: "Replicate Model"
question: "Choose a default Replicate image generation model?"
options:
  - label: "google/nano-banana-pro (Recommended)"
    description: "Google's fast image model on Replicate"
  - label: "google/nano-banana"
    description: "Google's base image model on Replicate"
```

### 更新 EXTEND.md

用户选择模型后：

1. 读取现有 EXTEND.md
2. 如果 `default_model:` 部分存在 → 更新提供商特定键
3. 如果 `default_model:` 部分缺失 → 添加完整部分：

```yaml
default_model:
  google: [value or null]
  openai: [value or null]
  openrouter: [value or null]
  dashscope: [value or null]
  replicate: [value or null]
```

仅设置所选提供商的模型；将其他保留为其当前值或 null。

## 设置后

1. 如需要则创建目录
2. 使用前置元数据写入/更新 EXTEND.md
3. 确认："偏好设置已保存到 [path]"
4. 继续图像生成
