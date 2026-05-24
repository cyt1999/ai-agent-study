# 第 10 讲：Observability：Trace、日志、成本、延迟
Created: 2026-05-24

## 1. 为什么 Agent 更需要可观测性

传统后端服务失败时，你通常能看：

- 请求日志。
- 错误栈。
- 数据库日志。
- 指标和告警。

Agent 失败更复杂。它可能失败在：

- 用户意图理解错了。
- 检索错了文档。
- 工具选错了。
- 参数抽错了。
- 模型被 prompt injection 带偏。
- 审批节点被跳过。
- 输出没有引用。
- 成本或延迟失控。

所以 Agent 需要的不只是日志，而是完整 trace。

## 2. Trace 要记录什么

一次 Agent run 至少记录：

- run id。
- 用户输入。
- 入口来源。
- 模型调用。
- prompt / system instruction 版本。
- structured output。
- 工具调用。
- 工具参数。
- 工具返回。
- 检索 query。
- 检索结果。
- workflow 节点路径。
- guardrail 结果。
- 审批状态。
- 最终输出。
- token、成本、延迟。
- 错误和失败分类。

没有这些信息，出了问题只能猜。

## 3. 日志和 Trace 的区别

日志是事件记录。

Trace 是一次任务的完整路径。

例如一个工单 Agent 的 trace：

```text
run_001
  normalize_request: success
  triage: refund_policy_conflict / high
  retrieve_knowledge: found refund_policy_v3
  query_business_context: missing contract_id
  research_report: conflict found
  risk_assessment: high
  request_approval: submitted
  final_response: waiting for approval
```

你能看到它为什么没有直接回复，也能看到它为什么提交审批。

## 4. 失败分类

Agent 的失败要分类，否则无法改进。

建议分类：

| 类型 | 例子 |
|---|---|
| intent_error | 意图分类错误 |
| retrieval_error | 没召回正确文档 |
| citation_error | 引用不支持结论 |
| tool_selection_error | 调错工具 |
| tool_argument_error | 参数错误 |
| permission_error | 权限不足或越权 |
| workflow_error | 路由或状态错误 |
| safety_error | 没拦住危险输入 |
| latency_error | 超时 |
| cost_error | 成本异常 |

每次 eval 和线上观察都应该归因到这些类别。

## 5. 成本和延迟

企业 Agent 不只是要准确，还要可用。

需要关注：

- 平均延迟。
- P95 延迟。
- 单次 run token。
- 单次 run 成本。
- 工具调用次数。
- 检索次数。
- 重试次数。
- 人工介入率。

如果一个简单知识库问题每次要跑 8 个模型调用、查 5 个工具、耗时 30 秒，即使答案正确，也不一定可用。

## 6. 飞书参照

飞书环境里，observability 还要考虑：

- 哪个群聊触发。
- 哪个用户触发。
- 是否涉及敏感群或敏感文档。
- 机器人是否发过消息。
- 是否创建了服务台工单。
- 是否提交审批。
- 审批人是谁。

这些都影响审计。

## 7. Trace 如何服务测试

Trace 不只是线上排障，也服务 eval。

例如测试期发现：

```text
最终回答正确，但没有引用知识库。
```

或者：

```text
最终回复说已提交审批，但 trace 里没有 request_approval 节点。
```

这些都应该判失败。

## 8. 告警策略

企业 Agent 可以设置告警：

- unsafe action attempt > 0。
- 高风险请求未进入审批。
- RAG 无引用回答。
- 工具错误率升高。
- P95 延迟超过阈值。
- 单次成本超过阈值。
- 某个 prompt 版本失败率升高。
- 某个文档版本导致回答冲突。

告警不是为了追求完美，而是为了知道系统什么时候正在变坏。

## 9. 作业

设计主线项目的 observability plan。

包括：

- 一次 run 的 trace 字段。
- 失败分类。
- 关键指标。
- 成本指标。
- 延迟指标。
- 安全告警。
- 你会如何用 trace debug 一个错误答案。

## 10. 本讲结论

- Agent 失败不是一个点，而是一条路径。
- Trace 是 Agent 工程的核心资产。
- 最终答案正确不代表过程安全。
- 成本、延迟、工具调用和审批路径都应该被观测。
- 没有 observability，就没有生产级 Agent。

