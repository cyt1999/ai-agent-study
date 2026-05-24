# 企业级 AI Agent 开发入门课程
Created: 2026-05-24

## 课程主线

本课程面向有工程经验、但没有系统做过 Agent 的程序员。目标不是做玩具 chatbot，而是做一个具备企业落地意识的 Agent 系统：有工具边界、状态、评测、trace、安全、人工审批和上线门禁。

主线技术路线：

- 第一阶段：不用框架，理解 LLM API、结构化输出、tool calling 和 eval。
- 第二阶段：用 OpenAI Agents SDK 做入门闭环和对照，理解 agent、tools、handoffs、guardrails、trace。
- 第三阶段：用 LangGraph 作为主线框架，构建可控、可恢复、可审批的企业 workflow agent。
- 第四阶段：加入 RAG、MCP、安全测试、灰度上线和作品集项目。

## 课程文件

- `00-course-orientation.md`：课程导论、学习目标、企业落地标准和路线。

## 配套文档

- `docs/brainstorms/2026-05-24-enterprise-agent-course-requirements.md`：课程 PRD。
- `docs/articles/2026-05-24-agent-framework-selection.md`：框架选型文章。
- `docs/articles/2026-05-24-agent-capability-testing-and-production-readiness.md`：Agent 能力测试与上线门禁。

## 默认项目方向

课程最终项目暂定为 **Enterprise Workflow Agent**：

- 输入企业内部问题或工单。
- 判断问题类型和风险等级。
- 检索知识库和历史案例。
- 调用模拟业务工具。
- 生成处理建议和证据。
- 低风险动作可执行，高风险动作进入人工审批。
- 记录 trace、eval、成本、延迟和失败原因。

后续可加入 DevEx Agent 扩展：读 issue、检索代码、生成 patch 计划、运行测试、输出风险说明。

