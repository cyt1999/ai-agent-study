# 第 8 讲：LangGraph 企业工作流设计
Created: 2026-05-24

## 1. LangGraph 的核心价值

LangGraph 的关键不是“让 Agent 更智能”，而是让 Agent 更可控。

它适合把企业 Agent 设计成图：

- 节点：一个处理步骤。
- 边：步骤之间的流转。
- 状态：贯穿整个流程的数据。
- 条件：根据结果选择路径。
- 中断：等待人工审批或外部事件。
- checkpoint：保存进度，支持恢复。

这正好对应企业流程。

## 2. 先设计状态，再设计节点

很多人会先想节点：

```text
分类节点、检索节点、审批节点...
```

但企业 workflow 更应该先想状态。

主线项目的状态可以包括：

| 状态字段 | 说明 |
|---|---|
| `request_id` | 请求唯一 ID |
| `user_id` | 发起人 |
| `source` | 飞书群聊、服务台、网页等入口 |
| `raw_message` | 原始输入 |
| `triage_result` | 结构化分类结果 |
| `retrieved_evidence` | 检索到的证据 |
| `business_context` | 客户、订单、历史工单等业务信息 |
| `research_report` | 研究报告 |
| `risk_assessment` | 风险判断 |
| `approval_status` | 审批状态 |
| `final_response` | 最终回复 |
| `errors` | 失败记录 |

状态设计清楚，节点才知道读什么、写什么。

## 3. 主线项目的图结构

可以先设计成：

```text
START
  ↓
normalize_request
  ↓
triage
  ↓
route_by_intent
  ├─ simple_knowledge_question → retrieve_knowledge → answer_with_citations
  ├─ support_case → retrieve_knowledge → query_business_context → research_report
  └─ unsafe_or_unknown → human_triage

research_report
  ↓
risk_assessment
  ├─ low_risk → draft_response → final_review → END
  ├─ medium_risk → create_ticket → notify_owner → END
  └─ high_risk → request_approval → wait_for_approval → approved_or_rejected
```

这个图里，模型不是到处自由行动。它只在特定节点做特定事情。

## 4. 节点应该小而清楚

每个节点应该回答：

- 输入是什么？
- 输出是什么？
- 是否调用模型？
- 是否调用工具？
- 失败怎么处理？
- 是否需要记录 trace？
- 是否影响状态？

例如 `triage` 节点：

输入：

- raw_message
- user_id
- source

输出：

- request_type
- risk_level
- missing_information
- next_route

失败处理：

- schema invalid：重试或进入人工分类。
- 信息不足：进入追问节点。

## 5. 条件边是企业流程的关键

条件边决定下一步路径。

例如：

```text
if risk_level == "high":
  request_approval
elif missing_information:
  ask_clarifying_question
elif request_type == "knowledge_question":
  answer_with_citations
else:
  create_support_ticket
```

条件边应该尽量基于结构化字段，而不是基于一段自由文本解释。

## 6. Human-in-the-loop 不是异常路径

企业 Agent 里，人工介入是正常节点。

需要人工介入的情况：

- 高金额退款。
- 合同承诺冲突。
- 客户投诉升级。
- 权限变更。
- 证据不足但影响重大。
- 工具返回异常。
- 模型置信度低。

人工节点的输出也要写入状态：

- approver
- decision
- comment
- decision_time
- follow_up_action

## 7. Checkpoint 和恢复

长流程不能假设一次完成。

例如审批可能隔天才处理。如果没有 checkpoint，Agent 无法恢复上下文。

LangGraph 的 durable execution 思路适合这种场景：流程保存关键状态，中断后可以从上次位置继续，而不是重新跑所有步骤。

这对企业很重要：

- 避免重复调用昂贵工具。
- 避免重复发消息。
- 保留审计历史。
- 支持人工审批后继续。

## 8. 飞书参照

飞书环境中，LangGraph 的节点可以这样理解：

| LangGraph 节点 | 飞书参照 |
|---|---|
| normalize_request | 群聊消息、机器人私聊、服务台工单统一格式 |
| retrieve_knowledge | 飞书知识库 / 云文档检索 |
| query_business_context | 多维表格查询客户和工单 |
| request_approval | 飞书审批 |
| notify_owner | 飞书机器人消息 |
| create_ticket | 飞书服务台 |
| wait_for_approval | 等待审批事件回调 |

课程第一版不写 API，但这个映射有助于理解真实企业集成。

## 9. 测试门禁

LangGraph workflow 测试重点：

| 测试项 | 目标 |
|---|---|
| 节点输入输出 | 每个节点状态变化正确 |
| 条件边 | 不同风险进入正确路径 |
| checkpoint | 中断后能恢复 |
| 人工审批 | 审批前不执行高风险动作 |
| 工具失败 | 失败被记录并进入正确路径 |
| trace | 每次运行可回放 |

## 10. 作业

为主线项目写一份 LangGraph 设计草图。

不用写代码，只写：

- 状态字段表。
- 节点列表。
- 条件边。
- 人工介入节点。
- 失败恢复策略。
- 哪些节点需要 eval。

## 11. 本讲结论

- LangGraph 的价值是可控编排，不是魔法。
- 企业 workflow 应该先设计状态。
- 节点要小，边要清楚，条件要结构化。
- Human-in-the-loop 是正常流程。
- Checkpoint 和恢复是生产级 Agent 的核心能力。

