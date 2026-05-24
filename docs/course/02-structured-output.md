# 第 2 讲：从 LLM API 到结构化输出
Created: 2026-05-24

## 1. 为什么第一步不是 Agent 框架

很多人学 Agent 会直接从框架开始：LangChain、LangGraph、OpenAI Agents SDK、CrewAI。这样学容易忽略一个更基础的问题：

**企业系统不能消费“看起来合理的自然语言”，企业系统需要稳定、明确、可校验的数据结构。**

比如用户在飞书群里说：

> 客户 A 说合同里承诺 30 天退款，但知识库写的是 7 天。帮我看看怎么处理。

人能理解这句话，但业务系统不能直接理解。系统真正需要的是：

- 这是什么类型的问题？
- 涉及哪个客户？
- 是否涉及退款？
- 风险等级是什么？
- 是否需要查合同？
- 是否需要人工审批？
- 缺少哪些信息？

所以企业 Agent 的第一步不是“让模型自由发挥”，而是让模型把自然语言变成结构化中间结果。

## 2. 自由文本为什么危险

自由文本有几个问题。

第一，**不可校验**。

模型说“这是一个高风险问题”，但系统不知道这个字段是否存在、取值是否合法、是否能用于后续路由。

第二，**不可稳定路由**。

如果模型这次输出“建议升级人工”，下次输出“需要人工进一步确认”，再下次输出“建议由主管处理”，语义接近，但程序很难稳定判断。

第三，**容易误入业务动作**。

如果模型直接生成：

> 建议立即给客户退款。

而后续系统没有风险判断和审批，就可能把建议误当成执行指令。

第四，**难以测试**。

自由文本很难做确定性测试。结构化输出可以检查字段、枚举、置信度、缺失项和风险等级。

## 3. 什么是结构化输出

结构化输出就是让模型按照预定义 schema 输出。

例如一个工单分类结果可以设计成：

```json
{
  "intent": "refund_policy_conflict",
  "risk_level": "high",
  "customer_name": "客户A",
  "requires_human_review": true,
  "missing_fields": ["contract_id"],
  "next_step": "retrieve_policy_and_contract"
}
```

这里每个字段都有意义：

- `intent` 决定后续流程。
- `risk_level` 决定是否能自动处理。
- `customer_name` 用于查询业务数据。
- `requires_human_review` 控制审批。
- `missing_fields` 决定是否追问。
- `next_step` 让 workflow 知道下一步。

这不是为了让输出好看，而是为了让后续系统可控。

## 4. 主线项目中的结构化输出

在 Enterprise Support Workflow Agent 里，第一步叫 **Intake & Triage**。

它负责把飞书消息、服务台问题或网页表单转成标准工单输入。

建议字段：

| 字段 | 说明 |
|---|---|
| `request_type` | 问题类型，如退款、权限、故障、合同、产品咨询 |
| `urgency` | 紧急程度 |
| `risk_level` | low / medium / high |
| `entities` | 客户、订单、合同、产品、人员等实体 |
| `required_context` | 需要检索哪些资料 |
| `required_tools` | 可能需要调用哪些工具 |
| `missing_information` | 缺失信息 |
| `should_escalate` | 是否需要转人工 |
| `reasoning_summary` | 简短原因，供人审查，不作为程序决策唯一依据 |

注意：`reasoning_summary` 只是给人看的解释，程序决策应该依赖枚举字段和布尔字段。

## 5. 飞书参照

如果放到飞书环境里，结构化输出通常来自这些入口：

- 用户在群里 @机器人。
- 用户私聊机器人。
- 用户提交服务台工单。
- 用户在多维表格里新增一条记录。
- 用户在审批备注里补充信息。

Agent 不应该把这些入口当成完全不同的系统。第一步应该统一成一个内部请求结构：

```text
Feishu message / service desk ticket / bitable record
  ↓
normalized request
  ↓
triage result
  ↓
workflow route
```

这样后续流程不关心请求来自哪里，只关心它被标准化后的结构。

## 6. 如何处理低置信度和缺失信息

结构化输出不能假装什么都知道。

如果用户说：

> 帮我看看那个客户能不能退款。

这里缺少客户、订单、合同和退款原因。正确输出不是编造字段，而是：

```json
{
  "request_type": "refund_inquiry",
  "risk_level": "medium",
  "entities": {},
  "missing_information": ["customer_id", "order_id", "refund_reason"],
  "should_escalate": false,
  "next_step": "ask_clarifying_question"
}
```

企业 Agent 的专业性不在于“什么都答”，而在于知道什么时候应该追问。

## 7. 测试门禁

结构化输出至少要测这些能力：

| 测试项 | 目标 |
|---|---|
| 字段完整性 | 必填字段不能缺失 |
| 枚举合法性 | `risk_level` 不能输出 schema 外的值 |
| 意图分类 | 相似问题能稳定归类 |
| 缺失信息识别 | 信息不足时能追问 |
| 高风险识别 | 退款、权限、合同承诺等不能判低风险 |
| prompt injection 抵抗 | 用户要求忽略规则时仍按 schema 输出 |

第一版可以准备 30 条样例：

- 10 条正常请求。
- 5 条信息不足请求。
- 5 条高风险请求。
- 5 条模糊请求。
- 5 条恶意或越权请求。

## 8. 作业

为主线项目设计一份“工单结构化输出 schema”。

至少包含：

- 请求类型。
- 风险等级。
- 业务实体。
- 缺失信息。
- 需要检索的资料。
- 需要调用的工具。
- 是否需要人工审批。
- 下一步 workflow 路由。

然后写 10 条测试样例，覆盖正常、模糊、高风险和恶意输入。

## 9. 本讲结论

- 企业 Agent 的第一步是结构化，不是框架。
- 自由文本不能直接驱动业务流程。
- 结构化输出让后续 routing、tool calling、eval 和审批变得可控。
- 模型不确定时应该追问、拒答或升级，而不是编造字段。
- 结构化输出本身就需要测试。

