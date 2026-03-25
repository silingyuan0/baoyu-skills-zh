# 常规帖子 - 详细指南

发布文本和图片到 X 的详细文档。

## 手动工作流

如您偏好逐步控制：

### Step 1: 将图片复制到剪贴板

```bash
${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts image /path/to/image.png
```

### Step 2: 从剪贴板粘贴

```bash
# 简单粘贴到前台应用
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts

# 带重试粘贴到 Chrome
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts --app "Google Chrome" --retries 5

# 快速粘贴（较短延迟）
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts --delay 200
```

### Step 3: 使用 Playwright MCP（如 Chrome 会话可用）

```bash
# 导航
mcp__playwright__browser_navigate url="https://x.com/compose/post"

# 获取元素引用
mcp__playwright__browser_snapshot

# 输入文本
mcp__playwright__browser_click element="editor" ref="<ref>"
mcp__playwright__browser_type element="editor" ref="<ref>" text="Your content"

# 粘贴图片（复制到剪贴板后）
mcp__playwright__browser_press_key key="Meta+v"  # macOS
# 或
mcp__playwright__browser_press_key key="Control+v"  # Windows/Linux

# 截图验证
mcp__playwright__browser_take_screenshot filename="preview.png"
```

## 图片支持

- 格式：PNG、JPEG、GIF、WebP
- 每帖最多 4 张图片
- 图片复制到系统剪贴板，然后通过键盘快捷键粘贴

## 示例会话

```
User: /post-to-x "Hello from Claude!" --image ./screenshot.png

Claude:
1. Runs: ${BUN_X} {baseDir}/scripts/x-browser.ts "Hello from Claude!" --image ./screenshot.png
2. Chrome opens with X compose page
3. Text is typed into editor
4. Image is copied to clipboard and pasted
5. Browser stays open 30s for preview
6. Reports: "Post composed. Use --submit to post."
```

## 故障排除

- **Chrome 未找到**：设置 `X_BROWSER_CHROME_PATH` 环境变量
- **未登录**：首次运行打开 Chrome — 手动登录，cookies 保存
- **图片粘贴失败**：
  - 验证剪贴板脚本：`${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts image <path>`
  - 在 macOS 上，在系统设置 > 隐私与安全 > 辅助功能中授予终端/iTerm"辅助功能"权限
  - 粘贴操作期间保持 Chrome 窗口可见且在前
- **osascript 权限被拒绝**：在系统偏好设置中授予终端辅助功能权限
- **触发速率限制**：等待几分钟后重试

## 工作原理

`x-browser.ts` 脚本使用 Chrome DevTools Protocol（CDP）来：
1. 启动真实 Chrome（而非 Playwright）并使用 `--disable-blink-features=AutomationControlled`
2. 使用持久化配置文件目录保存登录会话
3. 通过 CDP 命令与 X 交互（Runtime.evaluate、Input.dispatchKeyEvent）
4. **使用 osascript 粘贴图片**（macOS）：向 Chrome 发送真实 Cmd+V 按键，绕过 CDP 可检测的合成事件

此方法绕过 X 的反自动化检测，该检测会阻止 Playwright/Puppeteer。

### 图片粘贴机制（macOS）

CDP 的 `Input.dispatchKeyEvent` 发送"合成"键盘事件，网站可以检测到。X 会忽略合成的粘贴事件以保证安全。解决方案：

1. 通过 Swift/AppKit 将图片复制到系统剪贴板（`copy-to-clipboard.ts`）
2. 通过 `osascript` 将 Chrome 置于前台
3. 通过 `osascript` 和系统事件发送真实 Cmd+V 按键
4. 等待上传完成

这需要终端在系统设置中具有"辅助功能"权限。
