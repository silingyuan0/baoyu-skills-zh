---
name: baoyu-comic
description: 知识漫画创建器，支持多种画风和基调。创建原创教育漫画，具有详细的面板布局和顺序图像生成。当用户要求创建"知识漫画"、"教育漫画"、"传记漫画"、"教程漫画"或"Logicomix风格漫画"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-comic
    requires:
      anyBins:
        - bun
        - npx
---

# 知识漫画创建器

创建具有灵活画风与基调组合的原创知识漫画。

## 使用方法

```bash
/baoyu-comic posts/turing-story/source.md
/baoyu-comic article.md --art manga --tone warm
/baoyu-comic  # 然后粘贴内容
```

## 选项

### 视觉维度

| 选项 | 值 | 描述 |
|--------|--------|-------------|
| `--art` | ligne-claire (默认), manga, realistic, ink-brush, chalk | 画风/渲染技术 |
| `--tone` | neutral (默认), warm, dramatic, romantic, energetic, vintage, action | 基调/氛围 |
| `--layout` | standard (默认), cinematic, dense, splash, mixed, webtoon | 面板排列方式 |
| `--aspect` | 3:4 (默认, 竖版), 4:3 (横版), 16:9 (宽屏) | 页面宽高比 |
| `--lang` | auto (默认), zh, en, ja 等 | 输出语言 |

### 部分工作流选项

| 选项 | 描述 |
|--------|-------------|
| `--storyboard-only` | 仅生成分镜故事板，跳过提示词和图像 |
| `--prompts-only` | 生成分镜故事板与提示词，跳过图像 |
| `--images-only` | 从现有提示词目录生成图像 |
| `--regenerate N` | 仅重新生成指定页面（如 `3` 或 `2,5,8`） |

详情：[references/partial-workflows.md](references/partial-workflows.md)

### 画风

| 风格 | 中文 | 描述 |
|-------|------|-------------|
| `ligne-claire` | 清线 | 均匀线条、平涂色彩，欧洲漫画传统（丁丁、Logicomix） |
| `manga` | 日漫 | 大眼睛、漫画惯例、表情丰富 |
| `realistic` | 写实 | 数字绘画、写实比例、精湛 |
| `ink-brush` | 水墨 | 中国毛笔笔触、水墨效果 |
| `chalk` | 粉笔 | 黑板美学、手绘温暖 |

### 基调

| 基调 | 中文 | 描述 |
|------|------|-------------|
| `neutral` | 中性 | 平衡、理性、教育性 |
| `warm` | 温馨 | 怀旧、个人、舒适 |
| `dramatic` | 戏剧 | 高对比度、强烈、有力 |
| `romantic` | 浪漫 | 柔和、优美、装饰元素 |
| `energetic` | 活力 | 明亮、动态、激动人心 |
| `vintage` | 复古 | 历史感、年代感、时期真实性 |
| `action` | 动作 | 速度线、冲击效果、战斗 |

### 预设快捷方式

具有超越画风+基调特殊规则的预设：

| 预设 | 等效于 | 特殊规则 |
|--------|-----------|---------------|
| `--style ohmsha` | `--art manga --tone neutral` | 视觉隐喻、禁止头像对话、小工具展示 |
| `--style wuxia` | `--art ink-brush --tone action` | 气效果、战斗视觉、氛围元素 |
| `--style shoujo` | `--art manga --tone romantic` | 装饰元素、眼睛细节、浪漫节拍 |

### 兼容性矩阵

| 画风 | 最佳搭配 | 可用搭配 | 应避免 |
|-----------|---------|---------|---------|
| ligne-claire | neutral, warm | dramatic, vintage, energetic | romantic, action |
| manga | neutral, romantic, energetic, action | warm, dramatic | vintage |
| realistic | neutral, warm, dramatic, vintage | action | romantic, energetic |
| ink-brush | neutral, dramatic, action, vintage | warm | romantic, energetic |
| chalk | neutral, warm, energetic | vintage | dramatic, action, romantic |

详情：[references/auto-selection.md](references/auto-selection.md)

## 自动选择

内容信号决定默认画风、基调、版面布局（或预设）：

| 内容信号 | 推荐方案 |
|-----------------|-------------|
| 教程、指南、编程、教育 | **ohmsha** 预设 |
| 1950年前、古典、古代 | realistic + vintage |
| 个人故事、导师 | ligne-claire + warm |
| 武侠、玄幻 | **wuxia** 预设 |
| 爱情、校园生活 | **shoujo** 预设 |
| 传记、平衡 | ligne-claire + neutral |

**当预设被推荐时**：加载 `references/presets/{preset}.md` 并应用所有特殊规则。

详情：[references/auto-selection.md](references/auto-selection.md)

## 脚本目录

**重要**：所有脚本位于本技能的 `scripts/` 子目录中。

**智能体执行指令**：
1. 将本 SKILL.md 文件的目录路径确定为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<脚本名称>.ts`
3. 将本文档中的所有 `{baseDir}` 替换为实际路径
4. 解析 `${BUN_X}` 运行时：如果已安装 `bun` → `bun`；如果 `npx` 可用 → `npx -y bun`；否则建议安装 bun

**脚本参考**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/merge-to-pdf.ts` | 将漫画页面合并为 PDF |

## 文件结构

输出目录：`comic/{topic-slug}/`
- Slug：来自主题的 2-4 个单词的 kebab-case 格式（如 `alan-turing-bio`）
- 冲突时：附加时间戳（如 `turing-story-20260118-143052`）

**内容**：
| 文件 | 描述 |
|------|-------------|
| `source-{slug}.{ext}` | 源文件 |
| `analysis.md` | 内容分析 |
| `storyboard.md` | 带面板分解的分镜故事板 |
| `characters/characters.md` | 角色定义 |
| `characters/characters.png` | 角色参考表 |
| `prompts/NN-{cover|page}-[slug].md` | 生成提示词 |
| `NN-{cover|page}-[slug].png` | 生成的图像 |
| `{topic-slug}.pdf` | 最终合并的 PDF |

## 语言处理

**检测优先级**：
1. `--lang` 标志（显式指定）
2. EXTEND.md 中的 `language` 设置
3. 用户的对话语言
4. 源内容语言

**规则**：对所有交互使用用户输入语言或保存的语言偏好：
- 分镜故事板大纲和场景描述
- 图像生成提示词
- 用户选择选项和确认
- 进度更新、问题、错误、摘要

技术术语保留英文。

## 工作流

### 进度检查清单

```
漫画进度：
- [ ] 步骤 1：设置与分析
  - [ ] 1.1 偏好设置（EXTEND.md）⛔ 阻塞
    - [ ] 找到 → 加载偏好 → 继续
    - [ ] 未找到 → 运行首次设置 → 必须在其他步骤之前完成
  - [ ] 1.2 分析，1.3 检查现有
- [ ] 步骤 2：确认 - 风格与选项 ⚠️ 必需
- [ ] 步骤 3：生成分镜故事板与角色
- [ ] 步骤 4：审核大纲（条件性）
- [ ] 步骤 5：生成提示词
- [ ] 步骤 6：审核提示词（条件性）
- [ ] 步骤 7：生成图像 ⚠️ 需要角色参考
  - [ ] 7.1 先生成角色表 → characters/characters.png
  - [ ] 7.2 使用 --ref characters/characters.png 生成页面
- [ ] 步骤 8：合并为 PDF
- [ ] 步骤 9：完成报告
```

### 流程

```
输入 → [偏好设置] ─┬─ 找到 → 继续
                       │
                       └─ 未找到 → 首次设置 ⛔ 阻塞
                                      │
                                      └─ 完成设置 → 保存 EXTEND.md → 继续
                                                                              │
        ┌─────────────────────────────────────────────────────────────────────┘
        ↓
分析 → [检查现有？] → [确认：风格 + 审核] → 分镜 → [审核？] → 提示词 → [审核？] → 图像 → PDF → 完成
```

### 步骤摘要

| 步骤 | 操作 | 关键输出 |
|------|--------|------------|
| 1.1 | 加载 EXTEND.md 偏好 ⛔ 如未找到则阻塞 | 已加载配置 |
| 1.2 | 分析内容 | `analysis.md` |
| 1.3 | 检查现有目录 | 处理冲突 |
| 2 | 确认风格、焦点、受众、审核 | 用户偏好 |
| 3 | 生成分镜故事板与角色 | `storyboard.md`、`characters/` |
| 4 | 审核大纲（如果请求） | 用户批准 |
| 5 | 生成提示词 | `prompts/*.md` |
| 6 | 审核提示词（如果请求） | 用户批准 |
| **7.1** | **先生成角色表** | `characters/characters.png` |
| **7.2** | **使用角色参考生成页面** | `*.png` 文件 |
| 8 | 合并为 PDF | `{slug}.pdf` |
| 9 | 完成报告 | 摘要 |

### 步骤 7：图像生成 ⚠️ 关键

**角色参考对视觉一致性是必需的。**

**7.1 先生成角色表**：
- **备份规则**：如果 `characters/characters.png` 存在，重命名为 `characters/characters-backup-YYYYMMDD-HHMMSS.png`
- 调用已安装的图像生成智能体（如 `baoyu-image-gen`）
- 阅读该智能体的 `SKILL.md` 并遵循其记录的接口，而非直接调用其脚本
- 使用 `characters/characters.md` 作为提示词文件输入
- 保存输出到 `characters/characters.png`
- 使用宽高比 `4:3`

**压缩角色表**（推荐）：
- 压缩以减少用作参考图像时的 token 使用量
- 使用可用的图像压缩智能体（如有）
- 或使用系统工具：`pngquant`、`optipng`、`sips`（macOS）
- **保持 PNG 格式**，首选无损压缩

**7.2 使用角色参考生成每个页面**：

| 智能体能力 | 策略 |
|------------------|----------|
| 支持 `--ref` | 对每个页面传递 `characters/characters.png` |
| 不支持 `--ref` | 在每个提示词文件前添加角色描述 |

**页面生成的备份规则**：
- 如果提示词文件存在：重命名为 `prompts/NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.md`
- 如果图像文件存在：重命名为 `NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.png`
- 为每个页面调用已安装的图像生成智能体
- 使用 `prompts/01-page-xxx.md` 作为提示词文件输入
- 保存输出到 `01-page-xxx.png`
- 使用宽高比 `3:4`
- 如果选定的智能体支持参考图像，传递 `characters/characters.png` 作为 `--ref`

**完整工作流详情**：[references/workflow.md](references/workflow.md)

### EXTEND.md 路径 ⛔ 阻塞

**关键**：如果未找到 EXTEND.md，必须在任何其他问题或步骤之前完成首次设置。不要进行内容分析，不要询问画风，不要询问基调——仅先完成偏好设置。

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-comic/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-comic/EXTEND.md` | 用户主目录 |

| 结果 | 操作 |
|--------|--------|
| 找到 | 读取、解析、向用户显示摘要 → 继续 |
| 未找到 | ⛔ **阻塞**：仅运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 完成并保存 EXTEND.md → 然后继续 |

**EXTEND.md 支持**：水印 | 偏好的画风/基调/布局 | 自定义风格定义 | 角色预设 | 语言偏好

Schema：[references/config/preferences-schema.md](references/config/preferences-schema.md)

## 参考资料

**核心模板**：
- [analysis-framework.md](references/analysis-framework.md) - 深度内容分析
- [character-template.md](references/character-template.md) - 角色定义格式
- [storyboard-template.md](references/storyboard-template.md) - 分镜故事板结构
- [ohmsha-guide.md](references/ohmsha-guide.md) - Ohmsha 漫画详情

**风格定义**：
- `references/art-styles/` - 画风（ligne-claire, manga, realistic, ink-brush, chalk）
- `references/tones/` - 基调（neutral, warm, dramatic, romantic, energetic, vintage, action）
- `references/presets/` - 具有特殊规则的预设（ohmsha, wuxia, shoujo）
- `references/layouts/` - 版面布局（standard, cinematic, dense, splash, mixed, webtoon）

**工作流**：
- [workflow.md](references/workflow.md) - 完整工作流详情
- [auto-selection.md](references/auto-selection.md) - 内容信号分析
- [partial-workflows.md](references/partial-workflows.md) - 部分工作流选项

**配置**：
- [config/preferences-schema.md](references/config/preferences-schema.md) - EXTEND.md Schema
- [config/first-time-setup.md](references/config/first-time-setup.md) - 首次设置
- [config/watermark-guide.md](references/config/watermark-guide.md) - 水印配置

## 页面修改

| 操作 | 步骤 |
|--------|-------|
| **编辑** | **首先更新提示词文件** → `--regenerate N` → 重新生成 PDF |
| **添加** | 在指定位置创建提示词 → 使用角色参考生成 → 重新编号后续页面 → 更新分镜故事板 → 重新生成 PDF |
| **删除** | 删除文件 → 重新编号后续页面 → 更新分镜故事板 → 重新生成 PDF |

**重要**：更新页面时，**始终**先更新提示词文件（`prompts/NN-{cover|page}-[slug].md`），然后再重新生成。这确保更改被记录且可重现。

## 注意事项

- 图像生成：每页 10-30 秒
- 生成失败时自动重试一次
- 对敏感公众人物使用风格化替代方案
- 通过会话 ID 保持风格一致性
- **步骤 2 确认是必需的** - 不要跳过
- **步骤 4/6 是条件性的** - 仅在用户在步骤 2 中请求时执行
- **步骤 7.1 角色表必须在页面之前生成** - 确保一致性
- **步骤 7.2 每个页面必须引用角色** - 使用 `--ref` 或嵌入描述
- 水印/语言在 EXTEND.md 中配置一次
