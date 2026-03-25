---
name: baoyu-cover-image
description: 生成具有5个维度（类型、调色板、渲染方式、文本、氛围）的文章封面图，融合10种色彩调色板和7种渲染风格。支持电影级（2.35:1）、宽屏（16:9）和方形（1:1）比例。当用户要求"生成封面图"、"创建文章封面"或"制作封面"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-cover-image
---

# 封面图生成器

生成具有5维度定制功能的优雅文章封面图。

## 使用方法

```bash
# 根据内容自动选择尺寸
/baoyu-cover-image path/to/article.md

# 快速模式：跳过确认
/baoyu-cover-image article.md --quick

# 指定尺寸
/baoyu-cover-image article.md --type conceptual --palette warm --rendering flat-vector

# 风格预设（调色板 + 渲染方式的简写）
/baoyu-cover-image article.md --style blueprint

# 使用参考图像
/baoyu-cover-image article.md --ref style-ref.png

# 直接输入内容
/baoyu-cover-image --palette mono --aspect 1:1 --quick
[粘贴内容]
```

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--type <名称>` | hero, conceptual, typography, metaphor, scene, minimal |
| `--palette <名称>` | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone |
| `--rendering <名称>` | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print |
| `--style <名称>` | 预设简写（参见 [风格预设](references/style-presets.md)） |
| `--text <级别>` | none, title-only, title-subtitle, text-rich |
| `--mood <级别>` | subtle, balanced, bold |
| `--font <名称>` | clean, handwritten, serif, display |
| `--aspect <比例>` | 16:9（默认）, 2.35:1, 4:3, 3:2, 1:1, 3:4 |
| `--lang <代码>` | 标题语言（en, zh, ja 等） |
| `--no-title` | `--text none` 的别名 |
| `--quick` | 跳过确认，使用自动选择 |
| `--ref <文件...>` | 用于风格/构图指导的参考图像 |

## 五维度

| 维度 | 值 | 默认 |
|-----------|--------|---------|
| **类型** | hero, conceptual, typography, metaphor, scene, minimal | auto |
| **调色板** | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone | auto |
| **渲染方式** | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print | auto |
| **文本** | none, title-only, title-subtitle, text-rich | title-only |
| **氛围** | subtle, balanced, bold | balanced |
| **字体** | clean, handwritten, serif, display | clean |

自动选择规则：[references/auto-selection.md](references/auto-selection.md)

## 画廊

**类型**: hero, conceptual, typography, metaphor, scene, minimal
→ 详情：[references/types.md](references/types.md)

**调色板**: warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone
→ 详情：[references/palettes/](references/palettes/)

**渲染方式**: flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print
→ 详情：[references/renderings/](references/renderings/)

**文本级别**: none（纯视觉） | title-only（默认） | title-subtitle | text-rich（带标签）
→ 详情：[references/dimensions/text.md](references/dimensions/text.md)

**氛围级别**: subtle（低对比度） | balanced（默认） | bold（高对比度）
→ 详情：[references/dimensions/mood.md](references/dimensions/mood.md)

**字体**: clean（无衬线） | handwritten | serif | display（粗体装饰）
→ 详情：[references/dimensions/font.md](references/dimensions/font.md)

## 文件结构

每个 `default_output_dir` 偏好的输出目录：
- `same-dir`: `{article-dir}/`
- `imgs-subdir`: `{article-dir}/imgs/`
- `independent`（默认）: `cover-image/{topic-slug}/`

```
<output-dir>/
├── source-{slug}.{ext}    # 源文件
├── refs/                  # 参考图像（如果提供）
│   ├── ref-01-{slug}.{ext}
│   └── ref-01-{slug}.md   # 描述文件
├── prompts/cover.md       # 生成提示词
└── cover.png              # 输出图像
```

**Slug**: 2-4个单词，kebab-case。冲突时附加 `-YYYYMMDD-HHMMSS`

## 工作流程

### 进度清单

```
封面图进度：
- [ ] 步骤0：检查偏好设置（EXTEND.md）⛔ 阻塞
- [ ] 步骤1：分析内容 + 保存参考 + 确定输出目录
- [ ] 步骤2：确认选项（6个维度）⚠️ 除非 --quick
- [ ] 步骤3：创建提示词
- [ ] 步骤4：生成图像
- [ ] 步骤5：完成报告
```

### 流程

```
输入 → [步骤0：偏好设置] ─┬─ 找到 → 继续
                          └─ 未找到 → 首次设置 ⛔ 阻塞 → 保存 EXTEND.md → 继续
        ↓
分析 + 保存参考 → [输出目录] → [确认：6个维度] → 提示词 → 生成 → 完成
                                              ↓
                                     （如果 --quick 或全部指定则跳过）
```

### 步骤0：加载偏好设置 ⛔ 阻塞

检查 EXTEND.md 存在性（优先级：项目 → 用户）：
```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-cover-image/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-cover-image/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-cover-image/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-cover-image/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-cover-image/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-cover-image/EXTEND.md") { "user" }
```

| 结果 | 动作 |
|--------|-------------|
| 找到 | 加载，显示摘要 → 继续 |
| 未找到 | ⛔ 运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 保存 → 继续 |

**关键**: 如果未找到，在任何其他步骤或问题之前完成设置。

### 步骤1：分析内容

1. **保存参考图像**（如果提供）→ [references/workflow/reference-images.md](references/workflow/reference-images.md)
2. **保存源内容**（如果粘贴，保存到 `source.md`）
3. **分析内容**: 主题、语气、关键词、视觉隐喻
4. **深度分析参考** ⚠️: 提取具体、具象的元素（参见 reference-images.md）
   - 如果参考包含**人物** → 设置 `usage: direct` 以便模型查看参考图像，描述角色特征以进行风格化保留（参见 reference-images.md § 角色分析）
5. **检测语言**: 比较源、用户输入、EXTEND.md 偏好
6. **确定输出目录**: 按文件结构规则

### 步骤2：确认选项 ⚠️

完整确认流程：[references/workflow/confirm-options.md](references/workflow/confirm-options.md)

| 条件 | 跳过 | 仍需询问 |
|-----------|---------|-------------|
| `--quick` 或 `quick_mode: true` | 6个维度 | 宽高比（除非指定 `--aspect`） |
| 指定了全部6个 + `--aspect` | 全部 | 无 |

### 步骤3：创建提示词

保存到 `prompts/cover.md`。模板：[references/workflow/prompt-template.md](references/workflow/prompt-template.md)

**关键 - 前置元数据中的参考**:
- 保存到 `refs/` 的文件 → 添加到前置元数据 `references` 列表
- 口头提取的风格（无文件）→ 省略 `references`，在正文中描述
- 写入前验证: `test -f refs/ref-NN-{slug}.{ext}`

正文中的参考元素**必须**详细，带"MUST"/"REQUIRED"前缀，并说明整合方式。

### 步骤4：生成图像

1. **备份现有** `cover.png`（如果重新生成）
2. **检查图像生成技能**；如果多个，询问偏好
3. **处理提示词前置元数据中的参考**:
   - `direct` 使用 → 通过 `--ref` 传递（使用支持 ref 的后端）
   - `style`/`palette` → 提取特征，追加到提示词
4. **生成**: 使用提示词文件、输出路径、宽高比调用技能
5. 失败时：自动重试一次

### 步骤5：完成报告

```
封面已生成！

主题: [主题]
类型: [类型] | 调色板: [调色板] | 渲染方式: [渲染方式]
文本: [文本] | 氛围: [氛围] | 字体: [字体] | 宽高比: [比例]
标题: [标题或"仅视觉"]
语言: [语言] | 水印: [启用/禁用]
参考: [N张图像或"提取的风格"或"无"]
位置: [目录路径]

文件:
✓ source-{slug}.{ext}
✓ prompts/cover.md
✓ cover.png
```

## 图像修改

| 动作 | 步骤 |
|--------|-------|
| **重新生成** | 备份 → 先更新提示词文件 → 重新生成 |
| **更改维度** | 备份 → 确认新值 → 更新提示词 → 重新生成 |

## 组合原则

- **留白**: 40-60% 的呼吸空间
- **视觉锚点**: 主元素居中或偏左
- **人物**: 简化的剪影；不要逼真的人物
- **标题**: 使用用户/源提供的准确标题；绝不杜撰

## 扩展支持

通过 EXTEND.md 进行自定义配置。参见**步骤0**了解路径。

支持: 水印 | 首选尺寸 | 默认宽高比/输出 | 快速模式 | 自定义调色板 | 语言

模式：[references/config/preferences-schema.md](references/config/preferences-schema.md)

## 参考资料

**维度**: [text.md](references/dimensions/text.md) | [mood.md](references/dimensions/mood.md) | [font.md](references/dimensions/font.md)
**调色板**: [references/palettes/](references/palettes/)
**渲染方式**: [references/renderings/](references/renderings/)
**类型**: [references/types.md](references/types.md)
**自动选择**: [references/auto-selection.md](references/auto-selection.md)
**风格预设**: [references/style-presets.md](references/style-presets.md)
**兼容性**: [references/compatibility.md](references/compatibility.md)
**视觉元素**: [references/visual-elements.md](references/visual-elements.md)
**工作流程**: [confirm-options.md](references/workflow/confirm-options.md) | [prompt-template.md](references/workflow/prompt-template.md) | [reference-images.md](references/workflow/reference-images.md)
**配置**: [preferences-schema.md](references/config/preferences-schema.md) | [first-time-setup.md](references/config/first-time-setup.md) | [watermark-guide.md](references/config/watermark-guide.md)
