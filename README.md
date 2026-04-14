# Anything Analyzer

> **操作一遍网站或应用，AI 就把协议逆向、加密分析、安全审计全干了。**

[![Electron](https://img.shields.io/badge/Electron-35-blue)](https://www.electronjs.org/)
[![React](https://img.shields.io/badge/React-19-61dafb)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178c6)](https://www.typescriptlang.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

<img alt="Anything Analyzer Screenshot" src="https://github.com/user-attachments/assets/87f24186-ea00-4a03-9634-4d7af4b224d4" />

---

## 为什么用 Anything Analyzer？

传统的协议分析流程是这样的：

1. 开 Chrome DevTools 或 Fiddler/Charles 抓包
2. 手动翻几十上百条请求，找出关键接口
3. 一条条复制 headers、body，整理成文档
4. 自己分析鉴权流程、加密逻辑、API 模式
5. 手写 Python 复现代码

**Anything Analyzer 只需一步：在浏览器或应用里操作一遍，AI 搞定剩下所有。**

---

## 三大核心能力

### 1. 双通道抓包 — 浏览器 + 系统流量全覆盖

| 通道 | 原理 | 适用场景 |
|------|------|---------|
| **内嵌浏览器** | Chrome DevTools Protocol (CDP) | 网页操作、OAuth 流程、前端加密 |
| **MITM 代理** | HTTPS 中间人代理，自动签发 TLS 证书 | curl、Postman、Python 脚本、移动端 App |

两个通道的请求**统一汇入同一会话**，AI 分析时一起处理。

### 2. AI 智能分析 — 不只是抓包，是自动理解协议

- **两阶段分析** — Phase 1 智能过滤噪声请求 → Phase 2 聚焦深度分析
- **5 种分析模式** — 自动识别 / API 逆向 / 安全审计 / 性能分析 / JS 加密逆向
- **JS Hook 注入** — 自动拦截 fetch、XHR、crypto.subtle、CryptoJS、SM2/3/4 等加密调用
- **加密代码提取** — 从 JS 文件中提取加密相关代码片段，三级匹配优先级
- **流式输出 + 多轮追问** — 报告实时流式显示，可继续追问细节

### 3. MCP 生态集成 — AI Agent 的抓包工具

- **MCP Client** — 接入外部 MCP Server（stdio + StreamableHTTP），扩展 AI 分析能力
- **内置 MCP Server** — 将抓包和分析能力暴露为 MCP 工具，可被 Claude Desktop、Cursor 等直接调用

---

## 使用场景

| 场景 | 你能得到什么 |
|------|-------------|
| **逆向 API 协议** | API 端点文档 + 鉴权流程 + Python 复现代码 |
| **JS 加密逆向** | 加密算法识别 + 流程还原 + Python 实现 |
| **安全审计** | Token 泄露、CSRF/XSS 漏洞、敏感数据暴露清单 |
| **性能分析** | 请求瀑布图 + 冗余请求 + 缓存策略建议 |
| **学习 Web 协议** | OAuth/SSO 认证链路 + Token 流转解读 |

---

## 快速开始

### 下载安装

从 [Releases](https://github.com/Mouseww/anything-analyzer/releases) 下载对应平台安装包：

| 平台 | 文件 |
|------|------|
| Windows | `Anything-Analyzer-Setup-x.x.x.exe` |
| macOS (Apple Silicon) | `Anything-Analyzer-x.x.x-arm64.dmg` |
| macOS (Intel) | `Anything-Analyzer-x.x.x-x64.dmg` |
| Linux | `Anything-Analyzer-x.x.x.AppImage` |

### 基本用法

1. **配置 LLM** — Settings → LLM，填入 API Key（支持 OpenAI / Anthropic / 任何兼容 API）
2. **新建 Session** — 输入名称和目标 URL
3. **操作抓包** — 在内嵌浏览器中操作网站，点击 Start Capture
4. **AI 分析** — 停止捕获，点击 Analyze，选择分析模式

### MITM 代理（捕获外部应用流量）

不止浏览器 — curl、Postman、Python 脚本、移动端 App 的流量也能抓：

1. Settings → MITM 代理 → **安装 CA 证书**
2. **启用代理**（默认端口 `8888`）
3. 开启「设为系统代理」或手动指定：

```bash
# curl
curl -x http://127.0.0.1:8888 https://api.example.com/data

# Python
proxies = {"http": "http://127.0.0.1:8888", "https": "http://127.0.0.1:8888"}
requests.get("https://api.example.com/data", proxies=proxies)

# Node.js
HTTP_PROXY=http://127.0.0.1:8888 HTTPS_PROXY=http://127.0.0.1:8888 node app.js
```

> **纯代理模式：** 新建 Session 时 URL 留空，专注捕获外部应用流量。

<details>
<summary>CA 证书详细说明</summary>

- 证书存储：`%APPDATA%/anything-analyzer/certs/`（Windows）/ `~/Library/Application Support/anything-analyzer/certs/`（macOS）
- 首次安装需管理员权限（Windows UAC / macOS 密码）
- Settings 中可随时卸载、重新生成或导出证书
- 根 CA 有效期 10 年，子证书 825 天（符合 Apple 要求）
- MITM 代理为**只读捕获**，不修改请求/响应内容
- WebSocket 流量隧道转发，不做解密
- 单个 body 上限 1MB，二进制内容自动跳过

</details>

---

## 全部功能

<details>
<summary>展开查看完整功能列表</summary>

**抓包引擎**
- 全量网络捕获 — CDP Fetch 拦截，所有 HTTP 请求/响应（含 headers、body）
- SSE / WebSocket 识别 — 自动检测流式通信和 WebSocket 升级请求
- 存储快照 — 定时采集 Cookie、localStorage、sessionStorage 变化
- Domain 过滤 — 请求列表按域名分组过滤，支持部分匹配搜索
- 导出请求 — 原始请求数据导出为 JSON 文件

**浏览器**
- 多标签页 — 支持弹窗自动捕获为内部标签（OAuth 流程友好）
- 标签页防护 — 阻止 `window.close()` 关闭标签页，最后一个标签页意外销毁时自动恢复
- 一键清除环境 — Cookies、localStorage、sessionStorage、缓存一键清空

**AI 分析**
- 两阶段分析 — Phase 1 智能过滤 → Phase 2 深度分析，AI 按需查看请求详情
- 手动多选分析 — 勾选指定请求直接分析，跳过预过滤
- 自定义 Prompt 模板 — 内置多种模板，支持自定义
- 流式输出 + 追问 — 报告实时显示，支持多轮对话

**系统**
- 全局代理 — 支持 SOCKS5/HTTP/HTTPS 代理
- 自动更新 — 内置 electron-updater
- 暗色主题 — 基于 Ant Design 的现代界面

</details>

---

## 从源码构建

```bash
git clone https://github.com/MouseWW/anything-analyzer.git
cd anything-analyzer
pnpm install
pnpm dev        # 开发模式
pnpm test       # 运行测试
pnpm build && npx electron-builder --win  # 构建 Windows 安装包
```

**环境要求：** Node.js >= 18 · pnpm · Visual Studio Build Tools (Windows)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Electron 35 + electron-vite |
| Frontend | React 19 + Ant Design 5 + TypeScript |
| Database | better-sqlite3 (local SQLite) |
| Protocol | Chrome DevTools Protocol (CDP) |
| AI | OpenAI / Anthropic / Custom LLM（Chat Completions + Responses API） |
| AI 扩展 | MCP Client（stdio + StreamableHTTP）+ 内置 MCP Server |

<details>
<summary>项目结构</summary>

```
src/
├── main/                    # Electron main process
│   ├── ai/                  # AI 分析流水线（两阶段编排、prompt、LLM 路由）
│   ├── capture/             # 抓包引擎（CDP Fetch + JS Hook + 存储快照）
│   ├── cdp/                 # Chrome DevTools Protocol 管理
│   ├── proxy/               # MITM 代理（CA 管理、证书签发、系统代理）
│   ├── mcp/                 # MCP Client + 内置 MCP Server
│   ├── db/                  # SQLite 数据层
│   └── session/             # 会话生命周期管理
├── preload/                 # Context bridge + Hook 注入脚本
├── renderer/                # React UI（组件 + Hooks）
└── shared/                  # 共享类型定义
```

</details>

---

本项目`不具备`以下能力：
- 不具备【非法获取计算机数据】的功能
- 不具备【非法修改计算机数据】的功能
- 不具备【非法控制计算机系统】的功能
- 不具备【破坏计算机系统】的功能
- 不具备【内置AI模型】（AI 模型由用户自行配置，请按照《生成式人工智能服务管理暂行办法》合规使用大模型）

**务必不要使用本工具进行任何违反中国法律的行为！！！**

---

Thanks to everyone on [LinuxDo](https://linux.do/) for their support!

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Mouseww/anything-analyzer&type=Date)](https://star-history.com/#Mouseww/anything-analyzer&Date)

## License

MIT
