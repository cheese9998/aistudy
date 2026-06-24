# 06 - Agent 框架

> 智能体（Agent）是 2026 年最热门的方向，BOSS直聘智能体岗位需求同比 +217%。高级岗起薪 25-50K+。

## 什么是 Agent

**Agent = LLM + 记忆 + 工具 + 规划能力**

让 LLM 自主决定：
1. 下一步该做什么（Planning）
2. 调用哪个工具（Tool Use）
3. 是否需要检索信息（RAG）
4. 何时结束（Termination）

## 6 个主流 Agent 框架对比

| 框架 | 厂商 | 特点 | 适用场景 | 仓库 |
|---|---|---|---|---|
| **LangGraph** | LangChain | 图编排、企业采用率第一 | 复杂多 Agent、生产级 | github.com/langchain-ai/langgraph |
| **AutoGen** | 微软 | 对话式多 Agent | 研究、代码生成 | github.com/microsoft/autogen |
| **CrewAI** | CrewAI | 角色化协作（仿人类团队） | 业务流程自动化 | github.com/crewAIInc/crewAI |
| **smolagents** | HuggingFace | 极简、HuggingFace 生态 | 快速实验 | github.com/huggingface/smolagents |
| **OpenAI Agents SDK** | OpenAI | 轻量、官方 | GPT 生态 | github.com/openai/openai-agents-python |
| **Mastra** | Mastra | TypeScript 原生 | JS/TS 团队 | github.com/mastra-ai/mastra |
| **Semantic Kernel** | 微软 | .NET/企业级集成 | 大企业 | github.com/microsoft/semantic-kernel |

## 1. LangGraph ⭐⭐⭐⭐⭐（最推荐）

详见 [03-langchain/README.md](../03-langchain/README.md) 的 LangGraph 部分。

**核心优势**：
- 34k Stars，3450 万月下载
- 状态机 + 持久化 + 人机协同
- LangSmith 全链路调试
- 企业生产首选

## 2. AutoGen ⭐⭐⭐⭐

- **仓库**：https://github.com/microsoft/autogen
- **Stars**：50k+
- **核心思想**：让多个 Agent **对话**解决问题

```python
from autogen import AssistantAgent, UserProxyAgent, GroupChat, GroupChatManager

# 1. 定义 Agent
researcher = AssistantAgent(
    name="researcher",
    llm_config={"model": "gpt-4o"},
    system_message="你是研究员，擅长搜集信息"
)

writer = AssistantAgent(
    name="writer",
    llm_config={"model": "gpt-4o"},
    system_message="你是写作者，擅长写文章"
)

critic = AssistantAgent(
    name="critic",
    llm_config={"model": "gpt-4o"},
    system_message="你是审稿人，擅长挑毛病"
)

# 2. 创建群聊
groupchat = GroupChat(agents=[researcher, writer, critic], messages=[], max_round=10)
manager = GroupChatManager(groupchat=groupchat, llm_config={"model": "gpt-4o"})

# 3. 启动
user = UserProxyAgent(name="user", human_input_mode="NEVER")
user.initiate_chat(manager, message="写一篇关于 RAG 的技术博客")
```

## 3. CrewAI ⭐⭐⭐⭐

- **仓库**：https://github.com/crewAIInc/crewAI
- **Stars**：25k+
- **核心思想**：仿照"团队协作"——给每个 Agent 分配**角色、目标、任务**

```python
from crewai import Agent, Task, Crew, Process

researcher = Agent(
    role="高级研究员",
    goal="搜集 AI 应用开发的最新趋势",
    backstory="你是 10 年经验的技术分析师",
    tools=[search_tool],
    llm=llm,
)

writer = Agent(
    role="技术作家",
    goal="把研究内容写成通俗文章",
    backstory="你是知名技术博主",
    llm=llm,
)

task1 = Task(description="调研 2026 年 AI 趋势", agent=researcher, expected_output="报告")
task2 = Task(description="基于报告写文章", agent=writer, expected_output="文章")

crew = Crew(
    agents=[researcher, writer],
    tasks=[task1, task2],
    process=Process.sequential,  # 顺序执行
)
result = crew.kickoff()
```

**优势**：DSL 设计直观，5 分钟搭一个 Agent 团队  
**劣势**：复杂流程不如 LangGraph 灵活

## 4. smolagents ⭐⭐⭐

- **仓库**：https://github.com/huggingface/smolagents
- **Stars**：20k+
- **核心思想**：HuggingFace 出品，**极简**，把 Agent 压缩到 1000 行代码

```python
from smolagents import CodeAgent, HfApiModel

agent = CodeAgent(tools=[search_tool, my_custom_tool], model=HfApiModel())
result = agent.run("北京今天天气如何？")
```

**特色**：Agent **写 Python 代码**而不是文本推理（CodeAct 范式），处理复杂任务更准

## 5. OpenAI Agents SDK ⭐⭐⭐

- **仓库**：https://github.com/openai/openai-agents-python
- **轻量级**、与 OpenAI 生态深度集成
- 支持 Handoffs（Agent 互相交接任务）

## 6. Semantic Kernel ⭐⭐⭐

- **仓库**：https://github.com/microsoft/semantic-kernel
- **Stars**：25k+
- **核心定位**：.NET / 微软技术栈企业的 Agent 平台

## 简历项目：多 Agent 协作平台

```
项目名：智能办公助手（AgentOS）

业务背景：销售团队每周花 8 小时写周报、查客户资料、汇总邮件

技术栈：
- 编排：LangGraph
- Agent：研究 Agent + 写周报 Agent + 审稿 Agent
- 工具：飞书 API、企查查、邮件 MCP
- 记忆：PostgreSQL + Redis
- 前端：React + 流式输出
- 部署：K8s

核心功能：
1. 用户说"帮我写这周周报"
2. 研究 Agent 自动读取飞书任务、销售数据
3. 写周报 Agent 生成初稿
4. 审稿 Agent 检查格式、可读性
5. 人工确认后发送给老板

成果：
- 销售周报时间：8h → 30min
- 月活用户 200+
```

## 面试常问的 5 个 Agent 题

1. **Agent 和 Chain 的本质区别？**
2. **ReAct 模式是什么？**
   - Thought → Action → Observation → Thought → ... 循环

3. **Function Calling 的原理？**
   - LLM 输出结构化 JSON（工具名+参数），应用层执行后回传

4. **MCP 协议解决了什么问题？**
   - 标准化 LLM 与工具的通信（类似 USB-C）
   - 一次开发、多模型复用

5. **多 Agent 协作如何避免"吵架"循环？**
   - 设定最大轮数、明确终止条件、加监督 Agent、人类兜底
