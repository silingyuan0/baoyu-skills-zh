---
name: watermark-guide
description: baoyu-comic 水印配置指南
---

# 水印指南

## 位置图

```
┌─────────────────────────────┐
│                  [top-right]│ ← 避免（与页码冲突）
│                             │
│                             │
│       漫画页面内容            │
│                             │
│                             │
│[bottom-left][bottom-center][bottom-right]│
└─────────────────────────────┘
```

## 位置推荐

| 位置 | 适用于 | 避免情况 |
|----------|----------|------------|
| `bottom-right` | 默认选择，适合大多数面板布局 | 右下角有关键面板 |
| `bottom-left` | 右重布局 | 左下角有关键面板 |
| `bottom-center` | 网页漫画垂直滚动，居中设计 | 底部区域文字密集 |
| `top-right` | **漫画不推荐** | 始终 - 与页码冲突 |

## 内容格式

| 格式 | 示例 | 风格 |
|--------|---------|-------|
| 账号 | `@username` | 社交媒体风格 |
| 文字 | `Studio Name` | 专业品牌 |
| 中文 | `漫画工作室` | 中国市场 |
| 首字母 | `ABC` | 简约干净 |

## 漫画最佳实践

1. **面板感知放置**：避免放置在对话气泡或关键动作上
2. **一致性**：漫画所有页面使用相同水印
3. **大小**：保持微妙 - 不应分散故事叙述
4. **风格匹配**：水印风格应补充漫画的视觉风格
5. **网页漫画特殊**：垂直滚动格式使用 `bottom-center`

## 提示词整合

当启用水印时，添加到图像生成提示词：

```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the comic panels
and storytelling. Ensure watermark does not overlap speech bubbles or key action.
```

## 常见问题

| 问题 | 解决方案 |
|-------|----------|
| 水印在深色面板上不可见 | 调整对比度或添加微妙轮廓 |
| 水印与对话气泡重叠 | 更改位置或降低页面位置 |
| 页面间水印不一致 | 使用会话 ID 确保一致性 |
| 水印过于突出 | 更改位置或减小大小 |
| 与页码冲突 | 从不使用 top-right 位置 |
