---
name: first-time-setup
description: baoyu-danger-x-to-markdown 偏好设置的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：此设置必须在任何其他工作流步骤之前完成。不要：
- 开始转换推文或文章
- 询问 URL 或输出路径
- 进行任何转换

只询问此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        |
        v
+---------------------+
| AskUserQuestion     |
| (所有问题)          |
+---------------------+
        |
        v
+---------------------+
| 创建 EXTEND.md      |
+---------------------+
        |
        v
    继续转换
```

## 问题

**语言**：使用用户的输入语言或已保存的语言偏好。

使用 `AskUserQuestion` 将所有问题在一次调用中提出：

### 问题 1：下载媒体

```yaml
header: "Media"
question: "如何处理推文中的图片和视频？"
options:
  - label: "每次询问（推荐）"
    description: "保存 Markdown 后，询问是否下载媒体"
  - label: "始终下载"
    description: "始终将媒体下载到本地 imgs/ 和 videos/ 目录"
  - label: "从不下载"
    description: "在 Markdown 中保留原始远程 URL"
```

### 问题 2：默认输出目录

```yaml
header: "Output"
question: "默认输出目录？"
options:
  - label: "x-to-markdown（推荐）"
    description: "保存到 ./x-to-markdown/{username}/{tweet-id}.md"
```

注意：用户很可能会选择"其他"来输入自定义路径。

### 问题 3：保存位置

```yaml
header: "Save"
question: "偏好设置保存位置？"
options:
  - label: "User（推荐）"
    description: "~/.baoyu-skills/（所有项目）"
  - label: "Project"
    description: ".baoyu-skills/（仅当前项目）"
```

## 保存位置

| 选择 | 路径 | 作用范围 |
|--------|------|-------|
| User | `~/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md` | 所有项目 |
| Project | `.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md` | 当前项目 |

## 设置完成后

1. 如需则创建目录
2. 写入 EXTEND.md
3. 确认："偏好设置已保存到 [路径]"
4. 使用已保存的偏好设置继续转换

## EXTEND.md 模板

```md
download_media: [ask/1/0]
default_output_dir: [path or empty]
```

## 后续修改偏好设置

用户可以直接编辑 EXTEND.md，或删除它以重新触发设置流程。
