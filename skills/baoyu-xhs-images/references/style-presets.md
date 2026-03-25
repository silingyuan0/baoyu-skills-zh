# 风格预设

`--preset X` 展开为风格 + 布局组合。用户可覆盖任一维度。

| --preset | 风格 | 布局 |
|----------|-------|--------|
| `knowledge-card` | `notion` | `dense` |
| `checklist` | `notion` | `list` |
| `concept-map` | `notion` | `mindmap` |
| `swot` | `notion` | `quadrant` |
| `tutorial` | `chalkboard` | `flow` |
| `classroom` | `chalkboard` | `balanced` |
| `study-guide` | `study-notes` | `dense` |
| `cute-share` | `cute` | `balanced` |
| `girly` | `cute` | `sparse` |
| `cozy-story` | `warm` | `balanced` |
| `product-review` | `fresh` | `comparison` |
| `nature-flow` | `fresh` | `flow` |
| `warning` | `bold` | `list` |
| `versus` | `bold` | `comparison` |
| `clean-quote` | `minimal` | `sparse` |
| `pro-summary` | `minimal` | `balanced` |
| `retro-ranking` | `retro` | `list` |
| `throwback` | `retro` | `balanced` |
| `pop-facts` | `pop` | `list` |
| `hype` | `pop` | `sparse` |
| `poster` | `screen-print` | `sparse` |
| `editorial` | `screen-print` | `balanced` |
| `cinematic` | `screen-print` | `comparison` |

## 覆盖示例

- `--preset knowledge-card --style chalkboard` = 带密集布局的粉笔风格
- `--preset poster --layout quadrant` = 带四象限布局的丝网印刷风格

显式的 `--style`/`--layout` 参数始终覆盖预设值。
