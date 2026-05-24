# 企业级 AI Agent 开发入门课程 PRD v1
Created: 2026-05-24

## 1. 课程定位

课程名称暂定：

**企业级 AI Agent 开发入门：从工具调用到可上线工作流**

这不是“写几个 prompt + 调工具”的玩具课，而是一门面向有工程经验开发者的 Agent 落地课程。学习者完成后，应能判断一个企业场景是否适合 Agent，并能做出一个可观测、可评测、可控、有业务边界的 Agent 系统原型。

## 2. 目标学习者

主要面向：

- 有 3-8 年后端、全栈、平台工程或数据工程经验的程序员。
- 熟悉 HTTP、API、JSON、数据库、命令行和基本部署概念。
- 没有系统做过 Agent，但希望进入 AI Agent、AI 应用工程、AI 平台工程或 DevEx Agent 方向。

不面向：

- 完全不会编程的零基础用户。
- 只想使用现成 AI 工具、不关心工程实现的人。
- 只追求短视频式 demo、不关心企业落地约束的人。

## 3. 课程目标

学习完成后，学习者应能：

- 区分 chatbot、workflow、agent、多 agent，不把所有 LLM 应用都叫 Agent。
- 使用 LLM API 完成结构化输出、工具调用、流式响应和上下文管理。
- 设计有明确边界的工具层，避免让模型直接操作高风险系统。
- 构建企业知识库 Research Agent，支持引用、来源追踪和可信度判断。
- 构建工单/流程类 Workflow Agent，支持状态、审批、失败恢复和人工介入。
- 理解 MCP 在企业工具连接中的价值和风险。
- 为 Agent 建立 eval、trace、日志、成本、延迟和安全测试。
- 做出一个能展示求职能力的企业级 Agent 作品集项目。

## 4. 核心判断

课程必须反复强调这些工程判断：

- **能用 workflow，就不要先做 fully autonomous agent。**
- **Agent 的核心价值不是聊天，而是连接工具、数据和业务流程。**
- **企业落地的难点是可靠性、权限、评测、观测、失败处理和组织流程。**
- **多 Agent 不是入门目标，单 Agent + 工具 + 状态 + eval 先做好。**
- **没有 eval 和 trace 的 Agent 不算工程完成。**
- **没有测试集、灰度策略、人工审批和回滚方案的 Agent 不能直接上生产。**

## 5. 企业真实案例素材

课程会围绕 5 类企业落地案例讲解，而不是从抽象概念开始。

### 5.1 客服 / 工单 / 售后 Agent

代表案例：Decagon、Parloa、企业呼叫中心和售后系统。

真实痛点：

- 用户问题重复但表达方式高度不一致。
- 机器人不能只给帮助文档，要能查用户、查订单、判断政策、调用后端工具。
- 高风险动作如退款、改签、改权限必须有审批或置信度阈值。
- 语音场景还要处理延迟、ASR 错误、打断、多语言和实时评测。

课程映射：

- Tool calling
- RAG
- 人工审批
- eval-first
- latency / cost / escalation

### 5.2 企业知识库 / Research Agent

真实痛点：

- 企业知识分散在文档、IM、工单、CRM、数据库和代码库里。
- 普通 RAG 只能“看起来回答了”，但企业需要引用、可追溯、权限隔离和冲突检测。
- 内部资料会过期，答案必须知道来源版本和更新时间。

课程映射：

- 文档 ingestion
- chunking / retrieval / rerank
- 引用与来源
- 多来源交叉验证
- 回答可信度
- 权限过滤

### 5.3 业务流程自动化 / Back-office Agent

代表案例：Choco 订单处理 Agent。

真实痛点：

- 订单、申请、报销、合同、表单来自邮件、短信、图片、PDF、语音和手写内容。
- 信息需要转成 ERP、CRM、工单系统可处理的结构化数据。
- 难点不是抽取字段，而是业务上下文、客户偏好、历史订单、SKU 映射和异常处理。

课程映射：

- 多模态输入
- structured output
- schema validation
- confidence threshold
- human review queue
- ground-truth eval

### 5.4 代码库维护 / DevEx Agent

代表案例：Ramp、Virgin Atlantic、Codex 类企业开发工作流。

真实痛点：

- 大型代码库上下文多，人工 review、测试补齐、遗留重构成本高。
- on-call、事故排查、迁移和代码审查需要跨文件、跨系统理解。
- Agent 可以提速，但必须保留人类审查、测试和风险说明。

课程映射：

- 代码检索
- repo-aware agent
- patch generation
- test execution
- code review
- risk report
- human-in-the-loop

### 5.5 企业 Agent 平台与治理

代表案例：Microsoft Agent 365、OpenAI 企业 agent platform、MCP/A2A 生态。

真实痛点：

- 企业会出现 agent sprawl：很多团队各自做 Agent，没有统一权限、审计、版本、监控。
- Agent 需要像员工或服务账号一样被注册、授权、审计和停用。
- 多 Agent 协作需要清晰的边界、协议和治理，而不是随意互相调用。

课程映射：

- agent registry
- permissions
- observability
- MCP
- A2A 概念
- security review

## 6. 课程结构

建议第一版做 12 个模块，每个模块包含：

- 一个企业问题
- 一个核心概念
- 一个可运行实验
- 一个工程化检查点
- 一个作业或作品集增量

### 模块 1：Agent 到底解决什么企业问题

学习目标：

- 区分 chatbot、copilot、workflow agent、autonomous agent、多 agent。
- 理解企业落地为什么先从窄场景开始。
- 看懂 5 类真实企业案例。

实战任务：

- 选一个企业场景，写出“输入、工具、状态、输出、风险、人工介入点”。

产出：

- Agent 场景分析表。

### 模块 2：LLM API、结构化输出与可控生成

学习目标：

- 掌握 prompt、system/developer/user 指令、JSON schema、structured output。
- 理解为什么企业系统不能依赖自由文本。

实战任务：

- 做一个“工单分类与字段抽取器”，把自然语言请求转成结构化 ticket。

工程检查点：

- schema validation
- retry
- invalid output handling
- 单元测试样例

### 模块 3：Tool Calling 与业务工具边界

学习目标：

- 掌握 function/tool calling。
- 理解工具设计比 prompt 更重要。
- 学会给工具加最小权限、参数校验和错误返回。

实战任务：

- 为工单 Agent 接入 3 个模拟业务工具：查询用户、查询订单、创建工单。

工程检查点：

- 工具白名单
- 参数类型约束
- 错误码
- 工具调用日志

### 模块 4：企业知识库 RAG

学习目标：

- 理解 ingestion、chunking、embedding、retrieval、rerank、context packing。
- 知道普通 RAG 在企业场景为什么不够。

实战任务：

- 构建一个内部知识库问答 Agent，必须返回引用来源。

工程检查点：

- 文档版本
- 来源链接
- 引用片段
- 无答案时拒答

### 模块 5：Research Agent：从问答到可验证报告

学习目标：

- 让 Agent 跨多来源检索、归纳、发现冲突。
- 区分 answer、evidence、assumption、unknown。

实战任务：

- 做一个“企业政策研究 Agent”，输出带引用的处理建议。

工程检查点：

- 多来源引用
- 冲突检测
- 置信度说明
- 人工复核提示

### 模块 6：Workflow 优先，而不是盲目 autonomous

学习目标：

- 掌握 routing、parallelization、evaluator-optimizer、orchestrator-workers。
- 理解确定性流程和模型决策的分工。

实战任务：

- 把工单处理拆成显式 workflow：分类、检索、建议、校验、审批、执行。

工程检查点：

- 状态流转图
- 失败重试
- 可恢复 checkpoint
- 人工审批节点

### 模块 7：Agent Runtime：OpenAI Agents SDK / LangGraph

学习目标：

- 选择一个主框架深入。
- 理解 agent loop、state、handoff、guardrails、tracing。

实战任务：

- 用框架重构前面的 workflow agent。

工程检查点：

- trace 可查看
- run state 可恢复
- tool call 可审计
- 输出可回放

### 模块 8：Agent 能力测试与 Evals

学习目标：

- 理解为什么 eval 是 Agent 开发的主循环。
- 掌握 golden dataset、LLM-as-judge、deterministic checks、simulation。
- 建立从单元测试、工具契约测试、RAG 测试、trace grading 到安全测试的完整测试观念。
- 理解“感觉能用”是 Agent 工程里的反模式。

实战任务：

- 为知识库和工单 Agent 建 100 条 eval case，并形成上线前能力评估报告。

工程检查点：

- 正常样例
- 边界样例
- 工具失败样例
- 越权样例
- RAG 引用准确性
- tool selection accuracy
- escalation accuracy
- trace-level workflow grading
- 回归报告

### 模块 9：Observability：trace、日志、成本与延迟

学习目标：

- 能定位 Agent 失败发生在模型、检索、工具、权限还是业务规则。
- 设计生产可用的监控指标。

实战任务：

- 给 Agent 加 trace dashboard 或结构化日志分析。

工程检查点：

- 每次 run 的输入输出
- 工具调用链
- token/cost
- latency
- failure reason

### 模块 10：安全、权限与人工审批

学习目标：

- 理解 prompt injection、tool poisoning、过度授权、敏感信息泄露。
- 设计最小权限工具和高风险动作审批。
- 理解 Agent 上线前的 shadow mode、copilot mode、semi-auto mode 和 limited autonomous mode。

实战任务：

- 给 Agent 加安全测试集、审批队列和上线门禁清单。

工程检查点：

- 高风险动作拦截
- 权限校验
- prompt injection 测试
- 审计日志
- dry-run 模式
- 灰度发布策略
- 回滚方案

### 模块 11：MCP 与企业工具连接

学习目标：

- 理解 MCP 是工具/上下文协议，不是 Agent 框架。
- 学会把内部服务封装成 Agent 可用工具。
- 理解第三方 MCP server 的安全边界。

实战任务：

- 写一个简单 MCP server，暴露知识库查询或工单查询工具。

工程检查点：

- 工具描述清晰
- 权限隔离
- 输入校验
- 调用日志

### 模块 12：Capstone：企业内部工作流 Agent 系统

学习目标：

- 串联知识库、工单、工具调用、workflow、eval、observability、安全和审批。
- 做出可用于求职展示的完整项目。

最终项目建议：

**Enterprise Workflow Agent**

功能范围：

- 用户提交一个企业内部问题或工单。
- Agent 判断问题类型。
- 检索知识库和历史案例。
- 调用模拟业务工具查询状态。
- 生成处理建议和依据。
- 对低风险动作给出可执行计划。
- 对高风险动作进入人工审批。
- 记录 trace、工具调用、成本、延迟和 eval 结果。

进阶扩展：

- 加入代码库维护 Agent：读取 issue、检索代码、生成 patch 计划、运行测试、输出风险说明。

## 7. 作品集标准

课程最终项目必须能展示这些能力：

- README 清楚说明业务场景和边界。
- 有架构图或流程图。
- 有可运行 demo。
- 有测试数据和 eval 报告。
- 有 trace/log 示例。
- 有安全测试样例。
- 有人工审批流程。
- 有成本和延迟分析。
- 有上线门禁清单和灰度发布策略。
- 有 shadow mode / copilot mode / semi-auto mode 的边界说明。
- 有“哪些场景不应该自动化”的说明。

## 8. 已确认决策

- 课程主线采用 **LangGraph**，用于训练企业级 workflow、状态、checkpoint、human-in-the-loop、可恢复执行和可观测能力。
- **OpenAI Agents SDK** 作为入门闭环和对照框架，用于讲清 agent、tools、handoffs、guardrails、trace 和 eval 的最小可运行路径。
- Agent 能力测试是课程主线能力，不是附录；每个实战模块都要带测试样例、失败样例或上线门禁。
- 课程形态先做 **长文教程**，暂不写代码模板；后续学习阶段再围绕课程做项目实践。
- 第一版课程先以理论、案例、架构、测试和落地判断为主，代码实现延后。
- 最终项目主线确定为 **Enterprise Support Workflow Agent：企业内部工单与知识库 Agent**。
- 代码库维护 Agent 作为课程最后的进阶作品集方向，不作为第一版主线项目。
- 中文企业场景会结合飞书来讲，重点是群聊入口、机器人、知识库、文档、多维表格、审批、服务台等能力如何映射到 Agent 系统设计。

## 9. 第一版技术栈建议

主线建议：

- Python
- LangGraph 作为主框架
- OpenAI Agents SDK 作为入门和对照框架
- FastAPI 作为 API 层
- SQLite/Postgres 作为状态和业务数据模拟
- 向量库可先用 Chroma/FAISS/pgvector 之一
- pytest 做单元测试和 eval runner
- 结构化日志或 OpenTelemetry/LangSmith 类工具做观测
- MCP Python SDK 做工具协议实验

选择原则：

- 第一版课程不要同时深入太多框架。
- 框架只是载体，核心是 Agent 工程能力。
- 所有工具都可以替换，但 eval、trace、安全边界不能省。
- 框架选型文章单独维护在 `docs/articles/2026-05-24-agent-framework-selection.md`。

## 10. 成功标准

课程成功不是“学生能跑通 demo”，而是：

- 学生能解释为什么这个场景适合或不适合 Agent。
- 学生能把业务流程拆成 deterministic workflow + model decision points。
- 学生能设计工具 schema 和权限边界。
- 学生能写 eval case 验证改动是否变好。
- 学生能通过 trace 定位失败原因。
- 学生能说明高风险动作为什么需要 human-in-the-loop。
- 学生能给 Agent 制定上线前测试门禁，说明哪些能力已经验证、哪些只能 shadow/copilot，哪些不能自动化。
- 学生能拿最终项目去面试 Agent 工程相关岗位。

## 11. 需要继续明确的问题

- 是否需要在后续课程中对比企微、钉钉、语雀等飞书之外的中文企业协作环境。

## 12. 参考依据

企业案例：

- Choco automates food distribution with AI agents (https://openai.com/index/choco/)
- Ramp engineers accelerate code review with Codex (https://openai.com/index/ramp/)
- Virgin Atlantic ships faster with Codex (https://openai.com/index/virgin-atlantic/)
- Parloa service agents (https://openai.com/index/parloa/)
- LangChain State of Agent Engineering (https://www.langchain.com/state-of-agent-engineering)

框架与工程实践：

- Anthropic: Building effective agents (https://www.anthropic.com/engineering/building-effective-agents)
- OpenAI Agents SDK docs (https://developers.openai.com/api/docs/guides/agents)
- OpenAI Agent evals (https://developers.openai.com/api/docs/guides/agent-evals)
- OpenAI Evaluation best practices (https://developers.openai.com/api/docs/guides/evaluation-best-practices)
- LangGraph overview (https://docs.langchain.com/oss/python/langgraph/overview)
- LangGraph durable execution (https://docs.langchain.com/oss/python/langgraph/durable-execution)
- Microsoft Agent Framework overview (https://learn.microsoft.com/en-us/agent-framework/overview/)
- LlamaIndex RAG docs (https://developers.llamaindex.ai/python/framework/understanding/rag/)
- Model Context Protocol architecture (https://modelcontextprotocol.io/docs/learn/architecture)
- OWASP MCP Top 10 (https://owasp.org/www-project-mcp-top-10/)
