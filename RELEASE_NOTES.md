# Anything Analyzer v3.6.2

## 新功能

- **交互录制 UI 面板** — Inspector 视图新增"交互录制"Tab，可查看所有录制的点击、输入、滚动、鼠标移动轨迹事件，支持展开查看完整选择器和属性
- **浏览器 DevTools 按钮** — 浏览器面板新增 DevTools 开关按钮，点击可打开/关闭当前标签页的开发者工具（独立窗口模式）
- **录制状态指示器** — 底部状态栏显示交互录制事件计数，录制中时以红色脉冲圆点提示

## 修复

- **JS Hook 数据无法抓取** — 目标浏览器标签页缺少 preload 脚本，导致 hook-script 注入后 postMessage 无法转发到主进程。新增 target-preload 并配置到 WebContentsView
- **InteractionRecorder.pause/resume 崩溃** — `setRecordingState` 方法未实现导致运行时报错，已补充实现并正确广播到所有已注入的目标页面
- **交互事件监听无防抖** — 每次录制事件触发全量 DB 查询，快速操作时造成性能问题。已加入 500ms 防抖

## 下载

| 平台 | 文件 |
|------|------|
| Windows | Anything-Analyzer-Setup-3.6.2.exe |
| macOS (Apple Silicon) | Anything-Analyzer-3.6.2-arm64.dmg |
| macOS (Intel) | Anything-Analyzer-3.6.2-x64.dmg |
| Linux | Anything-Analyzer-3.6.2.AppImage |
