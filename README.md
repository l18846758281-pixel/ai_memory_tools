# AI Memory Server 后端服务

> 本地记忆管理后端，为 Chrome 扩展提供采集、OCR、LLM 抽取、向量检索、Prompt 增强能力。
> 基于 Python 标准库实现，默认监听 `http://localhost:9527`。

---

## 📁 模块结构

| 文件 | 说明 |
|------|------|
| `memory_server.py` | 服务入口，初始化所有组件并启动 HTTPServer |
| `handler.py` | HTTP 路由与请求处理（`MemoryHandler`） |
| `store.py` | 双层文件存储（raw + formatted） |
| `structured_memory.py` | 按 domain/thread 组织的结构化记忆 + 分层用户画像 |
| `vector_store.py` | ChromaDB 向量库封装（可选依赖） |
| `ocr_engine.py` | PaddleOCR 3.x 封装（可选依赖） |
| `ocr_batch.py` | 批量 OCR 脚本 |
| `llm_client.py` | OpenAI 兼容 API 客户端（抽取 / 路由 / Prompt 优化） |
| `worker.py` | 后台处理线程：OCR → 格式化 → LLM 抽取 |
| `mcp_server.py` | Model Context Protocol 服务端（可选，接入 Claude Desktop 等） |
| `monitor.py` | 运行状态监控工具 |
| `memory_console.html` | 浏览器管理控制台（单文件，无需构建） |
| `conf/` | 配置文件目录（LLM / 端口 / OCR 语言等） |
| `skill/` | Prompt 模板（抽取 / 路由 / 优化 / 画像合并） |

---

## 🚀 启动服务

### 基础启动（无 OCR / 无向量搜索）
```bash
cd server
python memory_server.py
```

### 完整功能
```bash
cd server
pip install -r requirements.txt
python memory_server.py --port 9527 --data-dir ./memory_data
```

### 启动参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `-p / --port` | `9527` | 监听端口 |
| `-d / --data-dir` | `./memory_data` | 数据存储目录 |
| `--host` | `0.0.0.0` | 绑定地址 |
| `--lang` | `ch` | OCR 语言（`ch`/`en`/`japan`/`korean` 等） |

---

## ⚙️ 配置 LLM

编辑 `server/conf/llm`（OpenAI 兼容 API，支持 DeepSeek / 智谱 / OpenAI / 本地 Ollama 等）：

```
key: sk-xxxxxxxxxxxxxxxx
url: https://api.deepseek.com/v1
model: deepseek-chat
```

保存后重启服务即可生效。LLM 用于：
- 从原始内容中抽取结构化记忆（extractor）
- Prompt 路由分类（query_router）
- Prompt 优化增强（query_opt）
- 用户画像合并（profile_consolidator）

---

## 🗂️ 数据目录结构

```
memory_data/
├── raw/{id}/                             # 原始数据（文本 / 图片）
├── formatted/{id}/                       # 格式化文本、标签、LLM 抽取结果
├── my_memory/
│   ├── {domain}/{thread}/{id}.json       # 结构化记忆条目
│   ├── _profile/stable_profile.json      # 分层用户画像
│   └── _index.json                       # 结构化索引
└── index.json                            # 全局记忆索引
vector_data/                              # ChromaDB 向量库持久化目录
logs/                                     # 按天滚动的日志文件
```

> 所有数据完全保存在本机，**不上传任何外部服务**。

---

## 🌐 API 接口

### 记忆采集

| 方法 | 路径 | 说明 |
|------|------|------|
| `POST` | `/api/memory/text` | 上传文本 |
| `POST` | `/api/memory/image` | 上传图片（自动触发 OCR） |
| `POST` | `/api/memory` | 自动检测类型上传 |

### Prompt 增强

| 方法 | 路径 | 说明 |
|------|------|------|
| `POST` | `/api/prompt/optimize` | 向量匹配 → 路由 → Prompt 优化 |

### 查询

| 方法 | 路径 | 说明 |
|------|------|------|
| `GET` | `/api/memories?page=1&size=10` | 分页列出记忆 |
| `GET` | `/api/memories/{id}` | 查看单条记忆详情 |
| `GET` | `/api/search?q=关键词` | 关键词搜索 |
| `DELETE` | `/api/memories/{id}` | 删除记忆 |

### 结构化记忆 / 画像

| 方法 | 路径 | 说明 |
|------|------|------|
| `GET` | `/api/my_memory/index` | 结构化记忆索引（按 domain/thread） |
| `GET` | `/api/my_memory/profile` | 分层用户画像 |

### 向量检索

| 方法 | 路径 | 说明 |
|------|------|------|
| `GET` | `/api/vector/search?q=` | 向量相似度搜索 |
| `GET` | `/api/vector/status` | 向量数据库状态 |

### 系统

| 方法 | 路径 | 说明 |
|------|------|------|
| `GET` | `/health` | 服务健康检查 |
| `GET` | `/console` | 打开管理控制台 |

---

## 🖥️ 管理控制台

服务启动后，浏览器访问：

```
http://localhost:9527/console
```

或直接双击 `server/memory_console.html` 通过 `file://` 打开。

### 功能区域

| 区域 | 说明 |
|------|------|
| **Server 连接栏** | 填写服务地址，点击「连接」校验 |
| **📥 记忆输入** | 发送文本 / 拖拽图片，展示 OCR、标签、摘要、LLM 抽取结果 |
| **✨ Prompt 优化** | 输入原始 Prompt，自动执行向量匹配 → 路由 → 优化 |
| **📂 My Memory** | Index 标签：按 Domain/Thread/Action 树状浏览结构化记忆<br>Profile 标签：查看分层用户画像（不变层/缓变层/演进层）及待合并条目 |

---

## 📦 依赖说明

| 依赖 | 必需 | 用途 |
|------|:----:|------|
| Python 3.8+ | ✅ | 标准库驱动的 HTTPServer |
| `paddlepaddle` + `paddleocr` | 可选 | 图片 OCR（首次运行会下载 ~200MB 模型） |
| `chromadb` | 可选 | 向量相似度搜索 |
| `mcp` | 可选 | MCP Server，用于接入 Claude Desktop 等 |
| `pillow` / `numpy` | 可选 | OCR 图像处理依赖 |

**最小运行：** 只需 Python 3.8+ 标准库，文本记忆 / 关键词搜索 / LLM 抽取均可工作。

---

## 🧩 可选：MCP Server

`mcp_server.py` 可作为 [Model Context Protocol](https://modelcontextprotocol.io/) 服务端运行，将本地记忆作为工具暴露给 Claude Desktop / Cline 等 MCP 客户端。

```bash
python mcp_server.py
```

具体接入配置视客户端而定，参见 `mcp_server.py` 源码说明。

---

## 🔧 开发 & 调试

### 查看日志
```bash
# 按天滚动的日志
tail -f logs/memory_server_2026-04-23.log
```

### 运行状态监控
```bash
python monitor.py
```

### 批量 OCR 工具
```bash
python ocr_batch.py --input ./images --output ./ocr_results
```

---

## ❓ 常见问题

### Q1: 端口 9527 被占用？
```bash
python memory_server.py --port 9528
```
同时更新 Chrome 扩展 Popup 中的服务器地址。

### Q2: OCR 功能报错 / 模型下载失败？
- PaddleOCR 首次运行需要下载模型，确保网络畅通
- 可手动下载模型后放入 `~/.paddleocr/`

### Q3: 向量搜索不生效？
- 确认已安装 `chromadb`：`pip install chromadb`
- 确认 `vector_data/` 目录可写
- 新数据需触发一次 `/api/memory/*` 上传后才会入库

### Q4: LLM 调用失败？
- 检查 `conf/llm` 中的 `key` / `url` / `model` 是否正确
- 确认本机能访问配置的 API 地址
- 日志中搜索 `LLMClient` 获取详细错误

### Q5: 如何完全清空记忆数据？
```bash
# 停止服务后执行
rm -rf memory_data/ vector_data/ logs/
```

---

## 📄 License

MIT License
