# My AI Memory 隐私政策 / Privacy Policy

**生效日期 / Effective Date:** 2026-04-23  
**最后更新 / Last Updated:** 2026-04-23

---

## 中文版

### 1. 概述

My AI Memory（以下简称"本扩展"）是一款帮助用户在浏览网页和使用 AI 平台时采集、整理个人记忆信息的 Chrome 扩展。我们高度重视用户隐私，本政策说明本扩展会采集哪些数据、如何使用，以及用户如何控制自己的数据。

**核心原则：本扩展不会将任何用户数据上传至开发者服务器或任何第三方云端服务。所有数据仅保存在用户本机。**

### 2. 本扩展会采集的数据

仅在用户**主动触发**（点击按钮、使用快捷键、确认右键菜单）时，才会采集以下内容：

| 数据类型 | 触发方式 | 用途 |
|---------|---------|-----|
| 选中的文字 | 用户点击浮动工具栏 / 快捷键 `Alt+Shift+L` | 保存到本地作为记忆条目 |
| 页面 HTML | 用户点击"记住此页" / 快捷键 `Alt+Shift+K` | 保存到本地作为记忆条目 |
| 页面截图 | 用户点击截图快捷键 `Alt+Shift+Y` / `Alt+Shift+U` | 保存到本地作为记忆条目 |
| 页面表单内容 | 随选中文字一并采集 | 辅助记忆上下文 |
| AI 平台输入框内容 | 用户在支持的 AI 平台提交 prompt 时 | 本地增强后重新注入输入框 |

**密码字段（`<input type="password">`）不会被采集。**

### 3. 数据如何处理

- **数据目的地：** 所有采集到的内容仅发送至用户本机的 Memory Server（默认 `http://localhost:9527`）
- **不上传云端：** 本扩展不会向开发者或任何第三方服务器传输用户数据
- **本地存储位置：** 用户本机硬盘的数据目录（默认 `memory_data/`），完全由用户掌控
- **Chrome Storage：** 仅用于保存用户偏好设置（快捷键、AI 增强开关状态），不包含任何采集到的网页内容

### 4. 第三方服务

- 本扩展本身**不集成任何第三方分析、广告或追踪 SDK**
- 用户可以在后端服务中自行配置 LLM API（如 DeepSeek、OpenAI 等）。此时记忆内容会发送至用户自己配置的 LLM 服务商——**这是用户自主选择并自行承担的行为**，与本扩展无关
- 本扩展不代为上传任何数据到 LLM 服务商

### 5. 权限说明

| 权限 | 用途 |
|-----|------|
| `activeTab` | 在用户触发时访问当前标签页 |
| `scripting` | 在页面注入采集脚本和浮动工具栏 |
| `tabs` | 读取标签页 URL 以识别 AI 平台 |
| `contextMenus` | 创建右键菜单 |
| `storage` | 保存用户偏好设置 |
| `host_permissions` | 在用户访问的网页上运行采集功能 |

### 6. 用户控制

- 用户可以随时通过 Chrome 扩展管理页面卸载本扩展
- 卸载后，Chrome Storage 中的偏好设置将被清除
- 本地 `memory_data/` 目录中的数据由用户自行管理和删除
- 用户可以随时在扩展 Popup 中关闭 AI 增强功能

### 7. 儿童隐私

本扩展不面向 13 岁以下儿童，也不会有意采集儿童个人信息。

### 8. 政策变更

如本政策有更新，我们会在本文件中修改"最后更新"日期。重大变更将在扩展说明页公告。

### 9. 联系方式

如有疑问，请通过以下方式联系：

- GitHub Issues: https://github.com/l18846758281-pixel/ai_memory_tools/issues

---

## English Version

### 1. Overview

My AI Memory (the "Extension") is a Chrome extension that helps users capture and organize personal memory notes while browsing the web and interacting with AI platforms. We take user privacy seriously.

**Core principle: The Extension does NOT upload any user data to developer-controlled or any third-party cloud servers. All data stays on the user's local machine.**

### 2. Data We Collect

The Extension collects data **only upon explicit user action** (clicking a button, using a keyboard shortcut, or confirming a context menu item):

| Data Type | Trigger | Purpose |
|-----------|---------|---------|
| Selected text | Floating toolbar / `Alt+Shift+L` | Save locally as a memory entry |
| Page HTML | "Memorize this page" / `Alt+Shift+K` | Save locally as a memory entry |
| Page screenshot | `Alt+Shift+Y` / `Alt+Shift+U` | Save locally as a memory entry |
| Form field content | Along with selected text | Provide context for memory |
| AI platform prompt | When user submits a prompt on supported platforms | Enhance locally and re-inject |

**Password fields (`<input type="password">`) are never collected.**

### 3. How Data Is Processed

- **Destination:** Collected content is sent only to the user's local Memory Server (default `http://localhost:9527`)
- **No cloud upload:** The Extension does not transmit user data to the developer or any third party
- **Local storage:** Data is saved in a local directory (default `memory_data/`) under the user's full control
- **Chrome Storage:** Used only for user preferences (shortcuts, AI enhancement toggle); never stores collected web content

### 4. Third-Party Services

- The Extension itself **does not integrate any third-party analytics, ads, or tracking SDKs**
- Users may configure an LLM API (e.g., DeepSeek, OpenAI) in the backend service. In that case, memory content will be sent to the LLM provider chosen by the user — this is **entirely the user's own choice and responsibility**, unrelated to the Extension itself
- The Extension does not upload any data to LLM providers on its own

### 5. Permissions

| Permission | Purpose |
|------------|---------|
| `activeTab` | Access the active tab only when triggered by user |
| `scripting` | Inject collection scripts and floating toolbar |
| `tabs` | Read tab URL to detect AI platforms |
| `contextMenus` | Create right-click menu |
| `storage` | Save user preferences |
| `host_permissions` | Run collection features on pages the user visits |

### 6. User Control

- Uninstall the Extension at any time via Chrome's extension management page
- Uninstalling clears preferences stored in Chrome Storage
- Local `memory_data/` is managed and deletable by the user directly
- AI enhancement can be toggled off at any time in the popup

### 7. Children's Privacy

The Extension is not directed to children under 13, and we do not knowingly collect personal information from children.

### 8. Policy Changes

We will update the "Last Updated" date above when this policy changes. Significant changes will be announced on the extension's store listing.

### 9. Contact

- GitHub Issues: https://github.com/l18846758281-pixel/ai_memory_tools/issues
