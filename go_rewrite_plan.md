# OpenManus Go 重写计划

## 1. 项目概述
OpenManus 是一个基于 LLM 的多智能体框架，支持工具调用、流程控制、安全沙箱等功能。本计划旨在用 Go 重写核心模块，同时保留与 Python 生态的兼容性。

## 2. 项目结构
```
openmanus-go/
├── cmd/                    # 命令行入口
│   ├── main.go             # 主程序入口
│   ├── run_mcp.go          # MCP 工具版本入口
│   └── run_flow.go         # 多智能体版本入口
├── internal/               # 内部包
│   ├── agent/              # 智能体实现
│   │   ├── base.go         # 基础智能体接口
│   │   ├── mcp.go          # MCP 智能体
│   │   ├── manus.go        # Manus 智能体
│   │   ├── react.go        # React 智能体
│   │   ├── swe.go          # SWE 智能体
│   │   ├── browser.go      # 浏览器智能体
│   │   └── data_analysis.go # 数据分析智能体
│   ├── tool/               # 工具集成
│   │   ├── base.go         # 工具接口
│   │   ├── web_search.go   # 网页搜索工具
│   │   ├── str_replace.go  # 字符串替换工具
│   │   ├── python_exec.go  # Python 执行工具（通过 RPC/HTTP 调用 Python 服务）
│   │   ├── file_ops.go     # 文件操作工具
│   │   └── browser_tool.go # 浏览器工具（通过 Playwright 调用）
│   ├── flow/               # 流程控制
│   │   ├── base.go         # 流程接口
│   │   ├── planning.go     # 规划器
│   │   └── flow_factory.go # 流程工厂
│   ├── mcp/                # 多智能体控制
│   │   ├── server.go       # MCP 服务器
│   │   └── client.go       # MCP 客户端
│   ├── prompt/             # 提示词工程
│   │   ├── template.go     # 提示词模板
│   │   └── prompt.go       # 提示词生成
│   ├── sandbox/            # 安全沙箱
│   │   ├── sandbox.go      # 沙箱实现
│   │   ├── terminal.go     # 终端模拟
│   │   └── manager.go      # 沙箱管理器
│   ├── config/             # 配置管理
│   │   └── config.go       # 配置加载（支持 toml/yaml/json）
│   └── llm/                # LLM 接口
│       ├── client.go       # LLM 客户端（OpenAI/Anthropic 等）
│       └── model.go        # LLM 模型定义
├── pkg/                    # 公共包
│   ├── logger/             # 日志
│   └── utils/              # 工具函数
├── config/                 # 配置文件
│   └── config.toml         # 示例配置
├── examples/               # 示例
├── tests/                  # 测试
├── go.mod                  # Go 模块定义
├── go.sum                  # 依赖版本锁定
└── README.md               # 项目说明
```

## 3. 技术选型
- **框架**：标准库 + 轻量级依赖
- **HTTP 客户端**：`net/http` 或 `go-resty`
- **配置解析**：`github.com/spf13/viper`
- **命令行工具**：`github.com/spf13/cobra`
- **Web 服务**：`github.com/gin-gonic/gin`
- **并发控制**：`context`、`sync`、`channel`
- **日志**：`github.com/sirupsen/logrus`
- **测试**：`testing` + `github.com/stretchr/testify`

## 4. 迁移步骤
### 4.1 基础框架搭建
- 初始化 Go 模块
- 搭建项目结构
- 实现配置加载
- 实现日志系统

### 4.2 核心模块迁移
#### 4.2.1 Agent 模块
- 定义基础 Agent 接口
- 实现 MCP、Manus、React、SWE、Browser、DataAnalysis 等智能体
- 实现智能体间通信（基于 channel 或 RPC）

#### 4.2.2 Tool 模块
- 定义工具接口
- 实现 Web 搜索、文件操作等纯 Go 工具
- 通过 HTTP/RPC 调用 Python 服务实现 AI/数据分析工具

#### 4.2.3 Flow 模块
- 实现流程控制与规划器
- 支持任务流定义、执行、监控

#### 4.2.4 MCP 模块
- 实现 MCP 服务器与客户端
- 支持多智能体调度与通信

#### 4.2.5 Prompt 模块
- 实现提示词模板系统
- 支持多语言提示词生成

#### 4.2.6 Sandbox 模块
- 实现安全沙箱（进程隔离、资源限制）
- 实现终端模拟与命令执行

### 4.3 入口与集成
- 实现 `main.go`、`run_mcp.go`、`run_flow.go` 等入口
- 集成所有模块，支持命令行参数与配置

## 5. 注意事项
- **Python 生态兼容**：AI/LLM/数据分析等强依赖 Python 的功能，通过微服务或插件机制与 Python 交互。
- **安全性**：沙箱模块需严格限制资源访问，防止恶意代码执行。
- **可扩展性**：设计插件机制，支持动态加载工具与智能体。
- **配置灵活**：支持多种配置格式（toml/yaml/json），便于用户自定义。

## 6. 后续计划
- 支持更多 LLM 模型（如 Claude、Gemini 等）
- 增强工具链（如代码生成、数据分析）
- 提供 Web UI 或 CLI 界面
- 社区贡献与生态建设
