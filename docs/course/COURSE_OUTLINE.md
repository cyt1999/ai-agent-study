# 企业级 AI Agent 开发入门课程大纲
Created: 2026-05-24

## 课程定位

这是一门面向有工程经验开发者的企业级 Agent 入门课。课程不追求玩具 demo，而是围绕企业内部工单与知识库 Agent，系统讲清楚 Agent 如何连接模型、工具、数据、状态、评测、观测、安全和人工审批。

课程形态：

- 长文教程。
- 第一版不写代码模板。
- 先建立理论、架构、测试和落地判断。
- 后续学习实践阶段再围绕课程做项目。

主线项目：

**Enterprise Support Workflow Agent：企业内部工单与知识库 Agent**

进阶作品集方向：

**Repo Maintenance Agent：代码库维护 Agent**

中文企业场景参照：

- 飞书群聊 / 机器人：用户入口。
- 飞书知识库 / 文档：RAG 数据源。
- 飞书多维表格：轻量业务数据与工单记录。
- 飞书审批：human-in-the-loop。
- 飞书服务台：正式工单流。

## 课程总览

| 讲次 | 标题 | 核心问题 | 主要产出 |
|---|---|---|---|
| 第 0 讲 | 为什么学企业级 Agent 开发 | 为什么不是玩具 demo，课程如何学习 | 学习路线与课程标准 |
| 第 1 讲 | Agent 到底解决什么企业问题 | Agent、chatbot、workflow、多 Agent 的区别 | Agent 场景分析表 |
| 第 2 讲 | 从 LLM API 到结构化输出 | 企业系统为什么不能依赖自由文本 | 工单结构化输出设计 |
| 第 3 讲 | Tool Calling 与业务工具边界 | Agent 如何安全调用业务系统 | 工具清单与权限边界 |
| 第 4 讲 | 企业知识库 RAG | 如何让 Agent 基于文档回答并给引用 | 知识库检索与引用设计 |
| 第 5 讲 | Research Agent：从问答到可验证报告 | 如何处理多来源、冲突和未知 | 可验证报告结构 |
| 第 6 讲 | Workflow 优先，而不是盲目 Autonomous | 如何把 Agent 设计成可控业务流程 | 主线 Agent 流程图 |
| 第 7 讲 | OpenAI Agents SDK 与 LangGraph 选型 | 为什么 SDK 入门、LangGraph 主线 | 框架选型决策 |
| 第 8 讲 | LangGraph 企业工作流设计 | 状态、节点、边、checkpoint、人工审批怎么设计 | LangGraph 状态模型草图 |
| 第 9 讲 | Agent 能力测试与 Evals | 怎么证明 Agent 能用，而不是感觉不错 | Eval 数据集设计 |
| 第 10 讲 | Observability：Trace、日志、成本、延迟 | 如何定位 Agent 失败发生在哪里 | Trace 与指标设计 |
| 第 11 讲 | 安全、权限与上线门禁 | 如何防越权、prompt injection 和危险自动化 | 上线门禁清单 |
| 第 12 讲 | MCP、飞书集成思路与最终项目设计 | 如何连接企业系统并沉淀作品集 | 最终项目方案 |

## 推进原则

每一讲固定包含：

- 企业问题：这一讲解决哪个真实痛点。
- 核心概念：必须掌握的 Agent 工程概念。
- 主线项目映射：放到企业内部工单与知识库 Agent 里如何设计。
- 飞书参照：在中文企业协作环境中如何理解。
- 测试与上线门禁：如何证明它可靠。
- 作业：沉淀为最终项目的一部分。

## 学习顺序

不要跳过前半段直接学 LangGraph。

推荐顺序：

```text
业务场景判断
  ↓
结构化输出
  ↓
工具边界
  ↓
RAG 与引用
  ↓
Workflow
  ↓
LangGraph
  ↓
Evals
  ↓
Observability
  ↓
Security
  ↓
MCP 与最终项目
```

课程的核心不是某个框架，而是企业 Agent 的工程判断。

