# 自动选择

内容信号决定默认画风、基调、版面布局（或预设）。

## 内容信号矩阵

| 内容信号 | 画风 | 基调 | 版面布局 | 预设 |
|-----------------|-----------|------|--------|--------|
| 教程、指南、初学者 | manga | neutral | webtoon | **ohmsha** |
| 计算机、AI、编程 | manga | neutral | dense | **ohmsha** |
| 技术解释、教育 | manga | neutral | webtoon | **ohmsha** |
| 1950年前、古典、古代 | realistic | vintage | cinematic | - |
| 个人故事、导师 | ligne-claire | warm | standard | - |
| 冲突、突破 | （继承） | dramatic | splash | - |
| 葡萄酒、美食、商业、生活方式 | realistic | neutral | cinematic | - |
| 武侠、玄幻、修仙 | ink-brush | action | splash | **wuxia** |
| 爱情、恋爱、校园生活 | manga | romantic | standard | **shoujo** |
| 传记、平衡 | ligne-claire | neutral | mixed | - |

## 预设推荐规则

**当预设被推荐时**：加载 `presets/{preset}.md` 并应用所有特殊规则。

### ohmsha
- **触发条件**：教程、技术、教育、计算、编程、指南、初学者
- **特殊规则**：视觉隐喻、禁止头像对话、小工具展示、哆啦A梦风格角色
- **基础**：manga + neutral + webtoon/dense

### wuxia
- **触发条件**：武侠、玄幻、修仙、修炼、剑术
- **特殊规则**：气效果、战斗视觉、氛围元素
- **基础**：ink-brush + action + splash

### shoujo
- **触发条件**：爱情、恋爱故事、校园生活、情感戏剧
- **特殊规则**：装饰元素、眼睛细节、浪漫节拍
- **基础**：manga + romantic + standard

## 兼容性矩阵

画风与基调组合在匹配适当时能达到最佳效果：

| 画风 | 最佳搭配 | 可用搭配 | 应避免 |
|-----------|---------|---------|---------|
| ligne-claire | neutral, warm | dramatic, vintage, energetic | romantic, action |
| manga | neutral, romantic, energetic, action | warm, dramatic | vintage |
| realistic | neutral, warm, dramatic, vintage | action | romantic, energetic |
| ink-brush | neutral, dramatic, action, vintage | warm | romantic, energetic |
| chalk | neutral, warm, energetic | vintage | dramatic, action, romantic |

**注意**：画风、基调、版面布局可以自由组合。不兼容的组合可以使用，但可能产生意外结果。

## 优先级顺序

1. 用户指定的选项（`--art`、`--tone`、`--style`）
2. EXTEND.md 默认值
3. 内容信号分析 → 自动选择
4. 回退：ligne-claire + neutral + standard
