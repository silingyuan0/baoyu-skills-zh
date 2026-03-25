# 部分工作流

运行工作流特定部分的选项。

## 选项概览

| 选项 | 执行的步骤 | 输出 |
|--------|----------------|--------|
| `--storyboard-only` | 1-3 | `storyboard.md` + `characters/` |
| `--prompts-only` | 1-5 | + `prompts/*.md` |
| `--images-only` | 7-9 | + 图像 + PDF |
| `--regenerate N` | 7（部分） | 指定页面 + PDF |

---

## 使用 `--storyboard-only`

生成分镜故事板和角色，不包含提示词或图像：

```bash
/baoyu-comic content.md --storyboard-only
```

**工作流**：仅步骤 1-3（分镜故事板 + 角色后停止）

**输出**：
- `analysis.md`
- `storyboard.md`
- `characters/characters.md`

**使用场景**：在生成图像之前审核和编辑分镜故事板。适用于：
- 获取有关叙事结构的反馈
- 手动调整面板布局
- 定义自定义角色

---

## 使用 `--prompts-only`

生成分镜故事板、角色和提示词，不包含图像：

```bash
/baoyu-comic content.md --prompts-only
```

**工作流**：步骤 1-5（生成提示词，跳过图像）

**输出**：
- `analysis.md`
- `storyboard.md`
- `characters/characters.md`
- `prompts/*.md`

**使用场景**：在图像生成之前审核和编辑提示词。适用于：
- 微调图像生成提示词
- 在提交生成之前确保视觉一致性
- 在提示词级别进行风格调整

---

## 使用 `--images-only`

从现有提示词生成图像（从步骤 7 开始）：

```bash
/baoyu-comic comic/topic-slug/ --images-only
```

**工作流**：跳至步骤 7，然后 8-9

**前置条件**（必须存在）：
- 包含页面提示词文件的 `prompts/` 目录
- 包含样式信息的 `storyboard.md`
- 包含角色定义的 `characters/characters.md`

**输出**：
- `characters/characters.png`（如果不存在）
- `NN-{cover|page}-[slug].png` 图像
- `{topic-slug}.pdf`

**使用场景**：编辑提示词后重新生成图像。适用于：
- 从失败的图像生成中恢复
- 尝试不同的图像生成设置
- 手动编辑提示词后重新生成

---

## 使用 `--regenerate`

仅重新生成特定页面：

```bash
# 单页
/baoyu-comic comic/topic-slug/ --regenerate 3

# 多页
/baoyu-comic comic/topic-slug/ --regenerate 2,5,8

# 封面
/baoyu-comic comic/topic-slug/ --regenerate 0
```

**工作流**：
1. 读取指定页面的现有提示词
2. 仅重新生成那些页面的图像
3. 重新生成 PDF

**前置条件**（必须存在）：
- 指定页面的 `prompts/NN-{cover|page}-[slug].md`
- `characters/characters.png`（作为参考）

**输出**：
- 重新生成指定页面的 `NN-{cover|page}-[slug].png`
- 更新的 `{topic-slug}.pdf`

**使用场景**：重新生成部分页面而非整个漫画。适用于：
- 修复单个有问题的页面
- 迭代特定视觉效果
- 编辑提示词后重新生成页面

**页码编号**：
- `0` = 封面
- `1-N` = 内容页
