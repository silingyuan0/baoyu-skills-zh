# Style Presets

`--preset X` expands to a type + style combination. Users can override either dimension.

| --preset | Type | Style |
|----------|------|-------|
| `tech-explainer` | `infographic` | `blueprint` |
| `knowledge-base` | `infographic` | `vector-illustration` |
| `saas-guide` | `infographic` | `notion` |
| `data-report` | `infographic` | `editorial` |
| `science-paper` | `infographic` | `scientific` |
| `tutorial` | `flowchart` | `vector-illustration` |
| `process-flow` | `flowchart` | `notion` |
| `system-design` | `framework` | `blueprint` |
| `architecture` | `framework` | `vector-illustration` |
| `versus` | `comparison` | `vector-illustration` |
| `business-compare` | `comparison` | `elegant` |
| `storytelling` | `scene` | `warm` |
| `lifestyle` | `scene` | `watercolor` |
| `history` | `timeline` | `elegant` |
| `evolution` | `timeline` | `warm` |
| `opinion-piece` | `scene` | `screen-print` |
| `editorial-poster` | `comparison` | `screen-print` |
| `cinematic` | `scene` | `screen-print` |

## Override Examples

- `--preset tech-explainer --style notion` = infographic type with notion style
- `--preset storytelling --type timeline` = timeline type with warm style

Explicit `--type`/`--style` flags always override preset values.
