# 偏好设置架构

## 完整架构

```yaml
---
version: 1

default_provider: null      # google|openai|openrouter|dashscope|replicate|null (null = auto-detect)

default_quality: null       # normal|2k|null (null = use default: 2k)

default_aspect_ratio: null  # "16:9"|"1:1"|"4:3"|"3:4"|"2.35:1"|null

default_image_size: null    # 1K|2K|4K|null (Google/OpenRouter, overrides quality)

default_model:
  google: null              # e.g., "gemini-3-pro-image-preview", "gemini-3.1-flash-image-preview"
  openai: null              # e.g., "gpt-image-1.5", "gpt-image-1"
  openrouter: null          # e.g., "google/gemini-3.1-flash-image-preview"
  dashscope: null           # e.g., "qwen-image-2.0-pro"
  replicate: null           # e.g., "google/nano-banana-pro"

batch:
  max_workers: 10
  provider_limits:
    replicate:
      concurrency: 5
      start_interval_ms: 700
    google:
      concurrency: 3
      start_interval_ms: 1100
    openai:
      concurrency: 3
      start_interval_ms: 1100
    openrouter:
      concurrency: 3
      start_interval_ms: 1100
    dashscope:
      concurrency: 3
      start_interval_ms: 1100
---
```

## 字段参考

| 字段 | 类型 | 默认 | 描述 |
|-------|------|---------|-------------|
| `version` | int | 1 | 架构版本 |
| `default_provider` | string\|null | null | 默认提供商（null = 自动检测） |
| `default_quality` | string\|null | null | 默认质量（null = 2k） |
| `default_aspect_ratio` | string\|null | null | 默认宽高比 |
| `default_image_size` | string\|null | null | Google/OpenRouter 图像尺寸（覆盖质量） |
| `default_model.google` | string\|null | null | Google 默认模型 |
| `default_model.openai` | string\|null | null | OpenAI 默认模型 |
| `default_model.openrouter` | string\|null | null | OpenRouter 默认模型 |
| `default_model.dashscope` | string\|null | null | DashScope 默认模型 |
| `default_model.replicate` | string\|null | null | Replicate 默认模型 |
| `batch.max_workers` | int\|null | 10 | 批处理工作器上限 |
| `batch.provider_limits.<provider>.concurrency` | int\|null | provider default | 每个提供商最大并发请求数 |
| `batch.provider_limits.<provider>.start_interval_ms` | int\|null | provider default | 每个提供商请求启动之间的最小间隔 |

## 示例

**最小**：
```yaml
---
version: 1
default_provider: google
default_quality: 2k
---
```

**完整**：
```yaml
---
version: 1
default_provider: google
default_quality: 2k
default_aspect_ratio: "16:9"
default_image_size: 2K
default_model:
  google: "gemini-3-pro-image-preview"
  openai: "gpt-image-1.5"
  openrouter: "google/gemini-3.1-flash-image-preview"
  dashscope: "qwen-image-2.0-pro"
  replicate: "google/nano-banana-pro"
batch:
  max_workers: 10
  provider_limits:
    replicate:
      concurrency: 5
      start_interval_ms: 700
    openrouter:
      concurrency: 3
      start_interval_ms: 1100
---
```
