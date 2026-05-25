# 企业级 AI Agent 开发入门课程
Created: 2026-05-24

## 课程主线

本课程面向有工程经验、但没有系统做过 Agent 的程序员。目标不是做玩具 chatbot，而是做一个具备企业落地意识的 Agent 系统：有工具边界、状态、评测、trace、安全、人工审批和上线门禁。

课程形态：

- 第一版先写成长文教程。
- 暂不提供代码仓库模板。
- 后续学习实践阶段再围绕课程内容独立做项目。

主线技术路线：

- 第一阶段：不用框架，理解 LLM API、结构化输出、tool calling 和 eval。
- 第二阶段：用 OpenAI Agents SDK 做入门闭环和对照，理解 agent、tools、handoffs、guardrails、trace。
- 第三阶段：用 LangGraph 作为主线框架，构建可控、可恢复、可审批的企业 workflow agent。
- 第四阶段：加入 RAG、MCP、安全测试、灰度上线和作品集项目。

## 课程文件

- `COURSE_OUTLINE.md`：完整课程大纲与推进方式。
- `GLOSSARY.md`：课程术语表。
- `00-course-orientation.md`：课程导论、学习目标、企业落地标准和路线。
- `01-what-problems-do-agents-solve.md`：Agent 解决的企业问题、5 类落地场景、飞书参照和主线项目设定。
- `02-structured-output.md`：从 LLM API 到结构化输出。
- `03-tool-calling-and-boundaries.md`：Tool Calling 与业务工具边界。
- `04-enterprise-rag.md`：企业知识库 RAG。
- `05-research-agent.md`：Research Agent：从问答到可验证报告。
- `06-workflow-first.md`：Workflow 优先，而不是盲目 Autonomous。
- `07-framework-selection.md`：OpenAI Agents SDK 与 LangGraph 选型。
- `08-langgraph-workflow-design.md`：LangGraph 企业工作流设计。
- `09-agent-evals.md`：Agent 能力测试与 Evals。
- `10-observability.md`：Observability：Trace、日志、成本、延迟。
- `11-security-and-production-gates.md`：安全、权限与上线门禁。
- `12-mcp-feishu-and-final-project.md`：MCP、飞书集成思路与最终项目设计。

## 课程配套材料

- `final-project-template.md`：最终项目设计模板。
- `eval-dataset-examples.md`：Agent eval 数据集样例。
- `production-readiness-checklist.md`：Agent 上线门禁清单。
- `feishu-enterprise-agent-mapping.md`：飞书企业 Agent 场景映射。

## 配套文档

- `docs/brainstorms/2026-05-24-enterprise-agent-course-requirements.md`：课程 PRD。
- `docs/articles/2026-05-24-agent-framework-selection.md`：框架选型文章。
- `docs/articles/2026-05-24-agent-capability-testing-and-production-readiness.md`：Agent 能力测试与上线门禁。

## 默认项目方向

课程最终项目确定为 **Enterprise Support Workflow Agent：企业内部工单与知识库 Agent**。

它会结合中文企业常见协作环境来讲，尤其是飞书这类一体化办公平台。课程不会在第一版写飞书 API 代码，但会用飞书的群聊、机器人、知识库、文档、多维表格、审批、服务台等能力作为场景参照。

- 输入企业内部问题或工单。
- 判断问题类型和风险等级。
- 检索知识库和历史案例。
- 调用模拟业务工具。
- 生成处理建议和证据。
- 低风险动作可执行，高风险动作进入人工审批。
- 记录 trace、eval、成本、延迟和失败原因。

后续可加入 DevEx Agent 扩展：读 issue、检索代码、生成 patch 计划、运行测试、输出风险说明。
