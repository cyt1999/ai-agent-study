# AI Agent 开发框架如何选型
Created: 2026-05-24

## 1. 先说结论

Agent 框架不是越“智能”越好，也不是越流行越好。企业落地时，应该先看这 6 个问题：

- 业务流程是否需要显式控制？
- 是否有长流程、暂停、恢复、人工审批？
- 是否需要多模型、多云、多供应商？
- 是否主要是文档/RAG/知识库问题？
- 是否需要企业级权限、审计、遥测和治理？
- 团队主要技术栈是 Python、TypeScript、.NET，还是云平台强绑定？

本课程第一版建议在 **OpenAI Agents SDK** 和 **LangGraph** 之间选择一个主框架：

- 如果目标是快速构建 OpenAI 生态下的 agent、tool calling、handoff、guardrails、trace，并希望尽快做出课程项目，优先 **OpenAI Agents SDK**。
- 如果目标是深入理解企业级 workflow、状态机、checkpoint、human-in-the-loop、可恢复执行，优先 **LangGraph**。

我的初步建议：**课程主线用 LangGraph，OpenAI Agents SDK 作为对照章节。**

理由是：我们这门课强调企业可落地、可测试、可恢复、可审批，而 LangGraph 的显式图、持久化、checkpoint 和 human-in-the-loop 更适合训练工程判断。OpenAI Agents SDK 很适合快速上手，但如果只用它，学习者容易把注意力放在 SDK 便利性，而不是 workflow 设计能力。

## 2. 不要先问“哪个框架最强”

更好的选型顺序是：

```text
业务风险
  ↓
流程复杂度
  ↓
状态和恢复要求
  ↓
工具和权限边界
  ↓
评测与观测要求
  ↓
团队技术栈
  ↓
框架选择
```

如果一个需求只是“给文本分类”，不需要 Agent 框架，普通 LLM API + schema validation 就够了。

如果一个需求是“查订单、判断政策、生成建议、人工审批、执行退款”，这时才需要 workflow/agent runtime。

## 3. 主流框架定位

### 3.1 OpenAI Agents SDK

适合：

- 想快速做 OpenAI 模型驱动的 Agent。
- 需要 tools、handoffs、guardrails、tracing。
- 希望少写框架胶水代码，直接围绕 agent loop 开发。
- 项目主要使用 OpenAI 生态。

优势：

- API 心智负担相对低。
- 官方支持 agent、tools、handoffs、guardrails、tracing。
- Trace 能记录模型调用、工具调用、handoff、guardrail 等运行过程。
- 2026 年版本开始强化 sandbox、文件、命令执行、checkpoint/rehydration 等长任务能力。

风险：

- 对 OpenAI 生态绑定更强。
- 如果业务流程非常复杂，显式状态机能力不如 LangGraph 直观。
- 学习者可能误以为“用了 SDK 就是生产级 Agent”，忽略 eval、权限和业务流程设计。

适合课程里的角色：

- 作为快速入门和对照框架。
- 用来讲 agent、tool、handoff、guardrail、trace 的最小闭环。

参考：

- OpenAI Agents SDK docs: https://platform.openai.com/docs/guides/agents-sdk/
- OpenAI Agent evals: https://developers.openai.com/api/docs/guides/agent-evals
- OpenAI Agents SDK tracing: https://openai.github.io/openai-agents-python/tracing/
- OpenAI Agents SDK guardrails: https://openai.github.io/openai-agents-js/guides/guardrails/

### 3.2 LangGraph

适合：

- 多步骤、长流程、有状态、需要暂停和恢复的企业 workflow。
- 需要 human-in-the-loop。
- 需要 checkpoint、time travel、fault-tolerant execution。
- 需要把业务流程显式建模成图，而不是交给模型自由发挥。

优势：

- 显式图结构：节点、边、条件、状态都清楚。
- 内置 persistence/checkpoint，支持中断、恢复、人工审批和调试。
- 更适合讲“deterministic workflow + model decision points”。
- 和 LangSmith 生态结合后，trace/eval/deploy 路径比较完整。

风险：

- 入门心智负担比 OpenAI Agents SDK 高。
- 图结构设计不好时，容易过度工程化。
- 如果只是简单 agent loop，LangGraph 可能显得重。

适合课程里的角色：

- 作为课程主框架候选。
- 用来训练企业级 Agent 的流程设计、状态设计和审批设计。

参考：

- LangGraph overview: https://docs.langchain.com/oss/python/langgraph/overview
- LangGraph persistence: https://docs.langchain.com/oss/javascript/langgraph/persistence
- LangGraph durable execution: https://docs.langchain.com/oss/python/langgraph/durable-execution

### 3.3 Microsoft Agent Framework

适合：

- 企业在 Microsoft/Azure/.NET 生态。
- 需要 C# / Python 双栈。
- 需要 type safety、middleware、telemetry、session state、MCP、graph-based workflows。
- 团队以前使用 Semantic Kernel 或 AutoGen。

优势：

- Microsoft 官方把它定位为 AutoGen 和 Semantic Kernel 的下一代统一方向。
- 同时提供 Agents 和 Workflows。
- Workflows 支持 graph-based orchestration、checkpointing、human-in-the-loop。
- 对 Azure 和 Microsoft 企业环境更友好。

风险：

- 截至 2026-05，官方文档仍标注 public preview。
- 对非 Microsoft 生态团队，学习和部署成本不一定划算。

适合课程里的角色：

- 作为企业生态选型章节介绍。
- 不建议作为第一版课程主框架，除非课程明确面向 Azure/.NET 企业开发者。

参考：

- Microsoft Agent Framework overview: https://learn.microsoft.com/en-us/agent-framework/overview/
- Microsoft Agent Framework workflows: https://learn.microsoft.com/en-us/agent-framework/workflows/workflows

### 3.4 CrewAI

适合：

- 业务自动化、研究报告、多角色协作 demo。
- 需要快速搭建 role / task / crew / flow。
- 团队希望低门槛进入多 agent 概念。

优势：

- 上手快。
- 抽象贴近“角色协作”。
- 文档强调 guardrails、memory、knowledge、observability、flows。

风险：

- 容易让初学者过早迷恋“多角色 Agent”。
- 对复杂状态、严格审批、长期可恢复工作流的训练价值不如 LangGraph。

适合课程里的角色：

- 可以作为扩展阅读或 bonus demo。
- 不建议作为主框架。

参考：

- CrewAI docs: https://docs.crewai.com/en/index

### 3.5 LlamaIndex

适合：

- 企业知识库、RAG、文档解析、检索、引用、数据连接。
- 文档、PDF、表格、多来源知识系统。

优势：

- RAG 和文档智能生态强。
- 提供 loading、indexing、storing、querying、evaluation 等完整 RAG 路径。
- 可以与 LangGraph/OpenAI Agents SDK 组合使用，把 RAG 作为 Agent 的一个工具。

风险：

- 它不是所有 Agent workflow 的默认答案。
- 如果主问题不是知识库/RAG，用它做总编排可能绕远。

适合课程里的角色：

- 作为 RAG/知识库模块的重点工具。
- 不一定作为整个课程主框架。

参考：

- LlamaIndex RAG docs: https://developers.llamaindex.ai/python/framework/understanding/rag/
- LlamaIndex workflows: https://developers.llamaindex.ai/python/llamaagents/workflows/

## 4. 选型矩阵

| 场景 | 首选 | 备选 |
|---|---|---|
| 快速做 OpenAI agent 原型 | OpenAI Agents SDK | LangGraph |
| 长流程、有状态、可恢复 workflow | LangGraph | Microsoft Agent Framework |
| Azure/.NET 企业项目 | Microsoft Agent Framework | Semantic Kernel 既有方案 |
| 知识库、文档、RAG | LlamaIndex | LangChain/LangGraph + 自建 RAG |
| 多角色研究/内容自动化 | CrewAI | OpenAI Agents SDK handoffs |
| 强审批、强状态、强可审计 | LangGraph | Microsoft Agent Framework |
| 多供应商模型和工具生态 | LangGraph | Microsoft Agent Framework |
| 教学入门但要求企业深度 | LangGraph 主线 + OpenAI Agents SDK 对照 | OpenAI Agents SDK 主线 + LangGraph 深入 |

## 5. 本课程建议路线

第一版课程可以这样安排：

```text
第 1 阶段：不用框架
LLM API + structured output + tool calling + simple eval

第 2 阶段：引入 OpenAI Agents SDK
理解 agent、tools、handoffs、guardrails、trace 的最小闭环

第 3 阶段：切换到 LangGraph 主线
把企业工单/知识库流程建成显式 workflow，加入状态、审批、恢复和观测

第 4 阶段：组合 LlamaIndex / MCP
把知识库和企业工具封装成 Agent 可调用能力

第 5 阶段：Capstone
完成 Enterprise Workflow Agent，并用 eval + trace + safety gate 证明它不是玩具
```

这样设计有两个好处：

- 学习曲线平滑：先理解 agent loop，再进入 workflow runtime。
- 工程深度足够：最终主线落在可控、可恢复、可评测的企业流程上。

## 6. 最终建议

如果我们只选一个主框架：

**选 LangGraph。**

如果课程允许对照：

**OpenAI Agents SDK 负责入门闭环，LangGraph 负责企业主线。**

这和课程目标最一致：不是教“某个 SDK 怎么用”，而是训练学习者做企业 Agent 系统时的工程判断。

