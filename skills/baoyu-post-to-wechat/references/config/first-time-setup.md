---
name: first-time-setup
description: baoyu-post-to-wechat 偏好设置的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：此设置必须在任何其他工作流步骤之前完成。禁止：
- 询问要发布的内容或文件
- 询问主题或发布方式
- 继续内容转换或发布

仅询问此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
No EXTEND.md found
        |
        v
+---------------------+
| AskUserQuestion     |
| (all questions)     |
+---------------------+
        |
        v
+---------------------+
| Create EXTEND.md    |
+---------------------+
        |
        v
    Continue to Step 1
```

## 问题

**语言**：使用用户的输入语言或已保存的语言偏好。

使用 AskUserQuestion 在一次调用中提出所有问题：

### 问题 1：默认主题

```yaml
header: "Theme"
question: "Default theme for article conversion?"
options:
  - label: "default (Recommended)"
    description: "Classic layout - centered title with border, white-on-color H2 (default: blue)"
  - label: "grace"
    description: "Elegant - text shadows, rounded cards, refined blockquotes (default: purple)"
  - label: "simple"
    description: "Minimal modern - asymmetric rounded corners, clean whitespace (default: green)"
  - label: "modern"
    description: "Large rounded corners, pill headings, spacious (default: orange)"
```

### 问题 2：默认颜色

```yaml
header: "Color"
question: "Default color preset? (theme default if not set)"
options:
  - label: "Theme default (Recommended)"
    description: "Use the theme's built-in default color"
  - label: "blue"
    description: "#0F4C81 经典蓝"
  - label: "red"
    description: "#A93226 中国红"
  - label: "green"
    description: "#009874 翡翠绿"
```

注：用户可选择"Other"输入任何预设名称（vermilion、yellow、purple、sky、rose、olive、black、gray、pink、orange）或十六进制值。

### 问题 3：默认发布方式

```yaml
header: "Method"
question: "Default publishing method?"
options:
  - label: "api (Recommended)"
    description: "Fast, requires API credentials (AppID + AppSecret)"
  - label: "browser"
    description: "Slow, requires Chrome and login session"
```

### 问题 4：默认作者

```yaml
header: "Author"
question: "Default author name for articles?"
options:
  - label: "No default"
    description: "Leave empty, specify per article"
```

注：用户可能会选择"Other"输入作者名。

### 问题 5：开启评论

```yaml
header: "Comments"
question: "Enable comments on articles by default?"
options:
  - label: "Yes (Recommended)"
    description: "Allow readers to comment on articles"
  - label: "No"
    description: "Disable comments by default"
```

### 问题 6：粉丝专评

```yaml
header: "Fans only"
question: "Restrict comments to followers only?"
options:
  - label: "No (Recommended)"
    description: "All readers can comment"
  - label: "Yes"
    description: "Only followers can comment"
```

### 问题 7：保存位置

```yaml
header: "Save"
question: "Where to save preferences?"
options:
  - label: "Project (Recommended)"
    description: ".baoyu-skills/ (this project only)"
  - label: "User"
    description: "~/.baoyu-skills/ (all projects)"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| Project | `.baoyu-skills/baoyu-post-to-wechat/EXTEND.md` | 当前项目 |
| User | `~/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md` | 所有项目 |

## 设置后

1. 如需要则创建目录
2. 写入 EXTEND.md
3. 确认："Preferences saved to [path]"
4. 继续到 Step 0（加载保存的偏好设置）

## EXTEND.md 模板

### 单账号（默认）

```md
default_theme: [default/grace/simple/modern]
default_color: [preset name, hex, or empty for theme default]
default_publish_method: [api/browser]
default_author: [author name or empty]
need_open_comment: [1/0]
only_fans_can_comment: [1/0]
chrome_profile_path:
```

### 多账号

```md
default_theme: [default/grace/simple/modern]
default_color: [preset name, hex, or empty for theme default]

accounts:
  - name: [display name]
    alias: [short key, e.g. "baoyu"]
    default: true
    default_publish_method: [api/browser]
    default_author: [author name]
    need_open_comment: [1/0]
    only_fans_can_comment: [1/0]
    app_id: [WeChat App ID, optional]
    app_secret: [WeChat App Secret, optional]
  - name: [second account name]
    alias: [short key, e.g. "ai-tools"]
    default_publish_method: [api/browser]
    default_author: [author name]
    need_open_comment: [1/0]
    only_fans_can_comment: [1/0]
```

## 稍后添加更多账号

初始设置后，用户可通过编辑 EXTEND.md 添加账号：

1. 添加包含列表项的 `accounts:` 块
2. 将每账号设置（author、publish method、comments）移入每个账号条目
3. 将全局设置（theme、color）保留在顶层
4. 每个账号需要一个唯一 `alias`（用于 CLI `--account` 参数和 Chrome 配置文件命名）
5. 在主账号上设置 `default: true`

## 稍后修改偏好设置

用户可直接编辑 EXTEND.md 或删除它以重新触发设置。
