# Enterprise Support Workflow Agent 项目设计模板
Created: 2026-05-24

## 使用方式

这个模板用于沉淀课程最终项目：

**Enterprise Support Workflow Agent：企业内部工单与知识库 Agent**

第一版课程不写代码，但你应该用这个模板完成一份能指导后续实现的项目设计文档。每一讲的作业都可以填入这里。

## 1. 业务背景

说明企业为什么需要这个 Agent。

建议回答：

- 当前问题是什么？
- 谁受影响？
- 现在怎么处理？
- 当前处理方式有什么成本？
- 为什么 Agent 适合这个场景？
- 哪些部分不应该自动化？

示例：

```text
企业内部客服和运营经常在飞书群里收到退款、权限、产品故障、合同政策等问题。答案分散在飞书知识库、云文档、多维表格和服务台历史工单中。人工处理需要反复查资料、问负责人、确认政策版本。Agent 的目标是先做辅助决策和工单分流，而不是直接执行业务动作。
```

## 2. 用户与入口

| 项目 | 内容 |
|---|---|
| 主要用户 | 客服、运营、销售、内部员工 |
| 次要用户 | 审批人、工单负责人、管理员 |
| 入口 | 飞书机器人、群聊 @、服务台、网页表单 |
| 主要任务 | 咨询政策、查询工单、处理退款争议、权限申请、故障上报 |

## 3. Agent 场景分析

| 项目 | 内容 |
|---|---|
| 目标用户 |  |
| 主要入口 |  |
| 输入类型 |  |
| 需要的数据 |  |
| 需要的工具 |  |
| 状态 |  |
| 输出 |  |
| 风险动作 |  |
| 人工介入点 |  |
| 测试方式 |  |
| 上线方式 |  |

## 4. 结构化输出 Schema

定义 Agent 第一步 triage 的输出。

建议字段：

| 字段 | 类型 | 说明 | 示例 |
|---|---|---|---|
| `request_type` | enum | 请求类型 | `refund_policy_conflict` |
| `urgency` | enum | 紧急程度 | `normal` |
| `risk_level` | enum | 风险等级 | `high` |
| `entities` | object | 客户、订单、合同等实体 | `{ "customer": "客户A" }` |
| `missing_information` | array | 缺失信息 | `["contract_id"]` |
| `required_context` | array | 需要检索的资料 | `["refund_policy", "customer_contract"]` |
| `required_tools` | array | 可能调用的工具 | `["search_knowledge_base"]` |
| `should_escalate` | boolean | 是否升级人工 | `true` |
| `next_route` | enum | 下一步路由 | `research_report` |
| `reasoning_summary` | string | 给人看的简短解释 | `涉及合同与退款政策冲突` |

枚举建议：

```text
request_type:
- knowledge_question
- refund_inquiry
- refund_policy_conflict
- permission_request
- product_issue
- contract_question
- billing_question
- unknown

risk_level:
- low
- medium
- high

next_route:
- answer_with_knowledge
- ask_clarifying_question
- retrieve_context
- research_report
- create_ticket
- human_triage
- reject_unsafe_request
```

## 5. 工具清单与权限矩阵

| 工具 | 读/写 | 风险 | 权限 | 是否审批 | 失败处理 |
|---|---|---|---|---|---|
| `search_knowledge_base` | 读 | low | read_public_kb | 否 | 返回无证据并拒答或追问 |
| `get_customer_profile` | 读 | medium | read_customer_summary | 否 | 降级为人工处理 |
| `get_order_summary` | 读 | medium | read_order_summary | 否 | 记录失败并追问 |
| `search_ticket_history` | 读 | low | read_ticket_history | 否 | 不使用历史案例 |
| `create_support_ticket` | 写 | medium | create_ticket | 视情况 | 创建失败则通知人工 |
| `send_internal_message` | 写 | medium | send_internal_message | 否 | 记录失败 |
| `submit_approval_request` | 写 | high | submit_approval | 是 | 失败则转人工 |
| `execute_refund` | 写 | high | 不开放 | 必须 | 第一版禁止 Agent 调用 |

原则：

- 读写分离。
- 写操作默认 dry-run 或审批。
- 高风险动作不自动执行。
- 所有工具调用必须可审计。

## 6. 知识库与 RAG 设计

### 6.1 数据源

| 数据源 | 内容 | 权限 | 更新频率 | 是否可引用 |
|---|---|---|---|---|
| 飞书知识库 | 政策、SOP、产品文档 | 部门权限 | 每周/变更时 | 是 |
| 飞书云文档 | 合同模板、流程说明 | 文档权限 | 变更时 | 是 |
| 多维表格 | 客户、订单、工单摘要 | 业务权限 | 实时/每日 | 部分 |
| 服务台历史工单 | 历史处理案例 | 客服权限 | 实时 | 是 |
| 群聊消息 | 非正式上下文 | 群权限 | 实时 | 谨慎使用 |

### 6.2 引用规则

每条答案必须包含：

- 文档名。
- 版本或更新时间。
- 章节或片段。
- 为什么该证据支持结论。

### 6.3 拒答规则

必须拒答或升级的情况：

- 没有检索到证据。
- 证据冲突且影响业务动作。
- 用户无权查看相关资料。
- 问题要求导出敏感信息。
- 请求涉及高风险写操作。

## 7. Research Agent 报告模板

```text
问题重述：

结论摘要：

关键事实：
1.
2.
3.

证据引用：
1. 来源 / 版本 / 片段 / 支持的结论
2.

冲突或不确定性：

风险等级：

建议动作：

必须人工确认的问题：
```

## 8. Workflow 设计

建议主流程：

```text
START
  ↓
normalize_request
  ↓
triage
  ↓
route_by_intent
  ├─ knowledge_question → retrieve_knowledge → answer_with_citations
  ├─ support_case → retrieve_knowledge → query_business_context → research_report
  ├─ missing_info → ask_clarifying_question
  └─ unsafe_or_unknown → human_triage

research_report
  ↓
risk_assessment
  ├─ low_risk → draft_response → final_review → END
  ├─ medium_risk → create_ticket → notify_owner → END
  └─ high_risk → submit_approval → wait_for_approval → approved_or_rejected
```

## 9. LangGraph 状态草图

| 状态字段 | 说明 |
|---|---|
| `request_id` | 请求唯一 ID |
| `user_id` | 发起用户 |
| `source` | 来源，如飞书群聊、服务台 |
| `raw_message` | 原始输入 |
| `triage_result` | 分类结果 |
| `retrieved_evidence` | 检索证据 |
| `business_context` | 业务数据 |
| `research_report` | 研究报告 |
| `risk_assessment` | 风险判断 |
| `approval_status` | 审批状态 |
| `final_response` | 最终输出 |
| `errors` | 失败记录 |
| `trace_id` | Trace ID |

## 10. Eval 计划

| 类型 | 数量 | 目标 |
|---|---:|---|
| 正常知识库问题 | 20 | 能基于证据回答 |
| 普通工单问题 | 20 | 能正确分类和建工单 |
| 高风险问题 | 20 | 能进入审批 |
| 信息不足问题 | 15 | 能追问 |
| 工具失败问题 | 15 | 不编造结果 |
| 恶意/越权问题 | 20 | 能拒绝或升级 |
| 冲突文档问题 | 10 | 能指出冲突 |

关键指标：

- intent accuracy
- tool selection accuracy
- citation accuracy
- escalation accuracy
- refusal accuracy
- unsafe action rate
- task completion rate

## 11. Observability 计划

一次 run 记录：

- run id
- 用户输入
- 入口来源
- prompt / model 版本
- structured output
- workflow 节点路径
- 检索 query 和结果
- 工具调用和参数
- guardrail 结果
- 审批状态
- 最终输出
- token / cost / latency
- failure reason

失败分类：

- intent_error
- retrieval_error
- citation_error
- tool_selection_error
- tool_argument_error
- permission_error
- workflow_error
- safety_error
- latency_error
- cost_error

## 12. 安全与上线门禁

上线前必须满足：

- 有 eval dataset。
- 高风险动作不会自动执行。
- 所有写工具有审批或 dry-run。
- RAG 答案必须有引用。
- 无证据时能拒答。
- 工具调用有审计。
- Trace 可回放。
- 成本和延迟有阈值。
- 有 shadow / copilot / semi-auto 灰度计划。
- 有人工接管和回滚方案。

## 13. 不做什么

第一版明确不做：

- 不自动退款。
- 不自动修改权限。
- 不导出敏感客户信息。
- 不读取无权限文档。
- 不用群聊消息替代正式政策。
- 不让 Agent 直接执行生产写操作。
- 不做完全 autonomous。

## 14. 后续代码实践计划

后续真正实现时可以分阶段：

1. 结构化输出 demo。
2. 工具调用 demo。
3. 知识库 RAG demo。
4. 显式 workflow demo。
5. LangGraph 状态与审批。
6. Eval runner。
7. Trace 与日志。
8. 安全样例和上线门禁。

