# 第 0 讲：为什么我们要学企业级 Agent 开发
Created: 2026-05-24

## 1. 这门课解决什么问题

很多 Agent 入门内容会从“让模型调用几个工具”开始，然后很快做出一个看起来很聪明的 demo。但企业真正关心的不是 demo 是否惊艳，而是：

- 能不能稳定完成真实任务？
- 能不能解释每一步为什么这么做？
- 能不能知道什么时候不该自动化？
- 能不能控制权限和风险？
- 能不能通过测试证明能力没有退化？
- 能不能灰度上线，而不是一次性接入生产系统？

所以这门课的目标不是“学会写 Agent”，而是：

**学会设计、实现、测试和交付一个企业可落地的 Agent 工作流。**

## 2. 学完你应该具备的能力

完成课程后，你应该能做到：

- 判断一个场景适不适合 Agent。
- 把业务流程拆成 deterministic workflow 和 model decision points。
- 设计模型可调用的工具，而不是让模型直接碰数据库、支付、订单或 shell。
- 构建带引用、可追溯、可拒答的企业知识库 Agent。
- 用 LangGraph 建立有状态、可恢复、可审批的 Agent workflow。
- 用 eval 和 trace 判断改动是变好还是变差。
- 为高风险动作设计 human-in-the-loop。
- 写出上线前 readiness report，说明哪些能力已验证、哪些只能 shadow/copilot、哪些不能自动化。

## 3. 课程为什么主线选 LangGraph

我们主线使用 LangGraph，不是因为它“最流行”，而是因为它适合训练企业 Agent 的核心工程能力。

LangGraph 官方把它定位为面向 long-running、stateful agents 的低层编排框架和 runtime，并强调 durable execution、human-in-the-loop、memory、debugging 和 production deployment。它不会把流程藏在黑盒里，而是鼓励你显式表达状态、节点、边、条件和恢复点。

这和我们的课程目标一致：

- 企业流程需要显式控制。
- 高风险动作需要人工审批。
- 长流程需要 checkpoint 和恢复。
- 故障需要 trace 和 replay。
- 评测不能只看最终答案，还要看中间路径。

## 4. OpenAI Agents SDK 在课程里的角色

OpenAI Agents SDK 不作为主线框架，但会作为入门闭环和对照框架。

它适合帮助你快速理解：

- agent 是什么。
- tool calling 如何工作。
- handoff 和 guardrails 是什么。
- trace 如何记录一次 agent run。
- eval 如何围绕 agent workflow 建立。

我们会先用它建立直觉，再切换到 LangGraph 训练企业 workflow 设计能力。

## 5. 我们不会直接追求“全自动 Agent”

企业落地更稳的路径是：

```text
单次 LLM 调用
  ↓
结构化输出
  ↓
工具调用
  ↓
确定性 workflow + 局部模型决策
  ↓
copilot
  ↓
semi-auto
  ↓
limited autonomous
```

第一版项目不会追求完全 autonomous。原因很简单：越自动，越需要测试、权限、审计、灰度、熔断和回滚。没有这些，自动化越强，风险越高。

## 6. Agent 能力怎么测试

这门课会把测试放在主线，而不是最后补一节。

Agent 至少要经过 7 层测试：

```text
1. 单元测试
2. 工具契约测试
3. RAG / 检索测试
4. Agent 行为评测
5. Trace 级工作流评测
6. 安全与越权测试
7. 灰度上线与持续评测
```

这意味着每个模块都会同时交付“功能”和“验证方式”。比如：

- 结构化输出模块要测 invalid JSON、schema mismatch、retry。
- Tool calling 模块要测工具选择、参数正确性、越权调用。
- RAG 模块要测引用准确性、拒答、旧文档误用。
- Workflow 模块要测状态转换、审批路径、失败恢复。
- 安全模块要测 prompt injection、tool poisoning、PII 泄露。

## 7. 最终项目

最终项目暂定为 **Enterprise Workflow Agent**。

它模拟一个企业内部工单/知识库/业务工具场景：

- 用户提交问题或工单。
- Agent 分类意图和风险等级。
- Agent 检索知识库、历史案例和业务数据。
- Agent 生成处理建议、证据和下一步动作。
- 低风险动作可以执行或 dry-run。
- 高风险动作进入人工审批。
- 系统记录 trace、eval、成本、延迟和失败原因。

这个项目的重点不是功能多，而是工程完整：

- 有工具边界。
- 有状态。
- 有 eval 数据集。
- 有 trace。
- 有人工审批。
- 有安全测试。
- 有上线门禁。
- 有 README 和架构说明。

## 8. 课程产出

每一讲尽量包含 5 个部分：

- 企业问题：为什么这个能力在真实业务里重要。
- 核心概念：本讲到底学什么。
- 实战任务：写一个可运行的小模块。
- 测试门禁：怎么证明它不是“看起来能用”。
- 作品集增量：这节课如何沉淀到最终项目。

## 9. 当前还需要后续确认的事

我们已经确认：

- 主线框架：LangGraph。
- 入门和对照框架：OpenAI Agents SDK。
- 课程目标：企业可落地，不做玩具。
- Agent 测试和上线门禁：作为课程主线。

后续还需要确认：

- 最终项目是否只做工单/知识库 Agent，还是把代码库维护 Agent 作为扩展项目。
- 每节课最终形态是长文教程、视频脚本、直播课大纲，还是三者都保留。
- 是否从第一版就做配套代码仓库模板。
- 是否加入飞书、企微、钉钉、语雀等中文企业环境集成。

## 10. 参考依据

- Anthropic: Building effective agents (https://www.anthropic.com/engineering/building-effective-agents)
- OpenAI Agents SDK docs (https://developers.openai.com/api/docs/guides/agents)
- OpenAI Agent evals (https://developers.openai.com/api/docs/guides/agent-evals)
- OpenAI Evaluation best practices (https://developers.openai.com/api/docs/guides/evaluation-best-practices)
- LangGraph overview (https://docs.langchain.com/oss/python/langgraph/overview)
- LangGraph durable execution (https://docs.langchain.com/oss/python/langgraph/durable-execution)
- Model Context Protocol architecture (https://modelcontextprotocol.io/docs/learn/architecture)
- OWASP MCP Top 10 (https://owasp.org/www-project-mcp-top-10/)

