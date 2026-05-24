# 第 7 讲：OpenAI Agents SDK 与 LangGraph 选型
Created: 2026-05-24

## 1. 框架不是起点，是放大器

前面几讲我们一直没有急着进入框架。原因是：

**如果你还没有想清楚业务流程、工具边界、RAG、状态和测试，换任何框架都只是把混乱包装起来。**

框架的价值是放大已经清楚的设计：

- 帮你组织 agent loop。
- 帮你管理状态。
- 帮你接工具。
- 帮你做 trace。
- 帮你实现 human-in-the-loop。
- 帮你部署和观测。

但框架不能替你决定什么该自动化、什么必须审批、什么应该拒答。

## 2. 本课程的框架路线

我们已经确定：

- **OpenAI Agents SDK**：入门闭环和对照。
- **LangGraph**：企业主线。

这不是二选一的宗教问题，而是学习阶段不同。

OpenAI Agents SDK 适合快速理解：

- agent 是什么。
- tools 怎么暴露给模型。
- handoffs 怎么做。
- guardrails 怎么介入。
- traces 如何记录一次运行。
- agent workflow 如何评测。

LangGraph 适合训练企业落地能力：

- 显式 workflow。
- 状态管理。
- checkpoint。
- durable execution。
- human-in-the-loop。
- 中断、恢复、回放。
- 长流程和复杂路由。

## 3. 为什么不直接从 LangGraph 开始

可以直接学，但学习曲线会陡。

如果你还没建立 agent loop 的直觉，LangGraph 的节点、边、状态、编译和 checkpoint 可能会显得抽象。

所以更稳的路线是：

```text
LLM API
  ↓
structured output
  ↓
tool calling
  ↓
OpenAI Agents SDK 入门闭环
  ↓
LangGraph 企业 workflow
```

这样你知道每一层解决什么问题。

## 4. OpenAI Agents SDK 适合什么

适合：

- 快速做 OpenAI 生态 agent。
- 工具调用比较清晰。
- 需要 handoff、guardrail、trace 的项目。
- 希望快速验证想法。

不适合：

- 一上来就承载复杂企业流程。
- 把 SDK 当成生产级保证。
- 忽略自己的 eval 和权限边界。

在课程里，它承担“看清 agent 最小闭环”的角色。

## 5. LangGraph 适合什么

适合：

- 长流程。
- 多步骤。
- 有状态。
- 需要暂停和恢复。
- 需要人工审批。
- 需要明确可视化流程。
- 需要把 Agent 行为纳入工程测试。

主线项目为什么适合 LangGraph？

因为企业内部工单与知识库 Agent 天然是一个 workflow：

```text
接收请求 → 分类 → 检索 → 查询 → 研究报告 → 风险判断 → 审批 → 回复/工单
```

这些步骤不应该都交给模型自由决定。它们应该被显式建模。

## 6. 其他框架怎么看

### Microsoft Agent Framework

适合 Azure、.NET、Microsoft 企业生态。它强调 agents、workflows、telemetry、state、MCP 等企业能力。

如果课程面向微软生态团队，它可以作为主线候选。但我们当前课程更适合 Python + LangGraph。

### CrewAI

适合快速做多角色协作 demo、研究报告、内容自动化。

但它容易让初学者过早关注“多个角色”，而不是工具边界、状态、测试和审批。

### LlamaIndex

适合 RAG、文档解析、知识库、检索增强。

在本课程里，它更适合作为 RAG 能力组件，而不是整个 workflow 的主编排框架。

## 7. 选型矩阵

| 场景 | 推荐 |
|---|---|
| 快速理解 agent loop | OpenAI Agents SDK |
| 企业长流程、有状态、审批 | LangGraph |
| Azure / .NET 企业 | Microsoft Agent Framework |
| 文档和知识库 | LlamaIndex |
| 多角色研究 demo | CrewAI |
| 工具协议和企业连接 | MCP |

## 8. 主线项目中的框架分工

理论设计阶段：

- 不依赖框架。
- 先定义流程、工具、状态、测试和权限。

入门闭环阶段：

- 用 OpenAI Agents SDK 理解 agent + tools + trace。

企业主线阶段：

- 用 LangGraph 表达 workflow。
- 每个节点有明确输入输出。
- 高风险节点支持 interrupt / human approval。
- 状态可 checkpoint。
- trace 可用于评测。

## 9. 测试门禁

框架选型也要有测试标准：

| 标准 | 问题 |
|---|---|
| 可控性 | 能否显式控制流程？ |
| 可恢复 | 失败后能否从中间状态恢复？ |
| 可观测 | 是否能看到模型、工具、状态变化？ |
| 可审批 | 是否支持人工介入？ |
| 可测试 | 是否能构造回归测试？ |
| 可替换 | 业务逻辑是否被框架锁死？ |

## 10. 作业

写一份框架选型说明。

要求：

- 为什么课程主线选 LangGraph。
- OpenAI Agents SDK 在课程中的作用。
- 为什么不把 CrewAI 作为主线。
- LlamaIndex 在主线项目中扮演什么角色。
- 如果企业使用 Azure/.NET，选型会如何变化。

## 11. 本讲结论

- 框架不是起点，业务流程和测试边界才是起点。
- OpenAI Agents SDK 适合入门闭环。
- LangGraph 更适合作为企业 workflow 主线。
- 选型要围绕状态、审批、恢复、观测、评测，而不是围绕流行度。

