# 术语表
Created: 2026-05-24

## Agent

围绕目标使用模型、工具、数据和状态来完成多步骤任务的软件系统。企业语境下，Agent 必须可测试、可观测、可约束。

## Chatbot

以对话为核心的系统，主要回答问题或生成文本。Chatbot 不一定有工具、状态或业务流程能力。

## Workflow

显式定义的业务流程。节点、路径和状态由系统控制，模型只在部分节点做理解、生成或判断。

## Autonomous Agent

由模型动态决定下一步做什么、调用什么工具、何时停止的 Agent。灵活性更高，但测试、审计和安全难度更大。

## Multi-agent

多个 Agent 分工协作的系统。适合复杂任务，但初学阶段不应过早追求多 Agent。

## Structured Output

让模型按预定义 schema 输出结构化数据，例如 JSON。它让后续路由、工具调用、测试和审批更可控。

## Tool Calling

模型选择并调用外部工具的机制。工具可以查询数据、检索文档、创建工单、提交审批等。

## Tool Boundary

工具边界。指工具能做什么、不能做什么、需要什么权限、是否需要审批、失败时如何处理。

## RAG

Retrieval-Augmented Generation，检索增强生成。先从外部知识库检索证据，再让模型基于证据回答。

## Citation

引用。企业 RAG 中，答案需要指向具体来源、版本、章节或片段，方便审查。

## Faithfulness

忠实度。指答案是否被检索到的证据支持，而不是模型自己编造。

## Research Agent

面向研究和决策支持的 Agent。它不只是回答问题，而是检索多来源、整理证据、发现冲突、表达未知并给出建议。

## State

状态。记录一个任务从开始到结束的关键信息，例如请求、分类结果、证据、工具调用、审批状态和最终输出。

## Checkpoint

检查点。保存 workflow 中间状态，使长流程可以暂停、恢复和回放。

## Human-in-the-loop

人工介入。高风险、低置信度、证据冲突或权限敏感场景下，让人参与确认或审批。

## Guardrails

护栏。用于限制模型或 Agent 行为的机制，例如输入检查、输出检查、工具权限、内容安全和审批规则。

## Eval

评测。用数据集和指标系统地测试 Agent 能力，而不是靠手工试几次。

## Golden Dataset

黄金测试集。一组经过人工设计或标注的样例，用于稳定评估 Agent 的表现。

## LLM-as-judge

用模型作为评委评估输出质量的方法。适合评价开放性输出，但高风险判断不能只依赖它。

## Trace

一次 Agent run 的完整路径记录，包括模型调用、工具调用、检索、状态变化、审批和最终输出。

## Observability

可观测性。通过 trace、日志、指标和告警理解 Agent 为什么成功或失败。

## Prompt Injection

提示注入。用户或外部内容试图覆盖系统规则，例如“忽略之前所有规则”。

## Indirect Prompt Injection

间接提示注入。恶意指令藏在检索文档、网页、邮件或工具返回中，诱导 Agent 执行不该执行的动作。

## Tool Poisoning

工具污染。恶意或错误工具描述、工具返回、第三方工具诱导 Agent 调用危险能力。

## MCP

Model Context Protocol。用于连接 AI 应用与外部工具、资源和上下文的开放协议。它不是 Agent 框架。

## Shadow Mode

影子模式。Agent 接真实请求副本，但不影响用户和业务系统，只用于对比和评估。

## Copilot Mode

辅助模式。Agent 给员工建议，由员工确认后执行。

## Semi-auto Mode

半自动模式。低风险动作自动处理，中高风险动作进入审批或人工处理。

## Limited Autonomous

有限自主模式。只在极窄、低风险、高确定性的场景中允许 Agent 自动执行。

## Production Readiness

生产就绪。Agent 在测试、权限、观测、安全、灰度、回滚和人工接管方面达到进入生产的条件。

