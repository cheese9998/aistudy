# 03 - LangChain / LangGraph

> LangChain 是 LLM 应用开发的事实标准框架，**80% 岗位 JD 会写**。LangGraph 是它的多智能体编排扩展，2026 年高级岗必学。

## LangChain 是什么

一个把 LLM、工具、数据库、记忆、外部 API 串起来的开发框架。核心抽象：
- **Model I/O**：统一调用不同 LLM（OpenAI、Anthropic、本地模型）
- **Prompt Templates**：可复用的提示词模板
- **Output Parsers**：把 LLM 输出转成结构化数据
- **Chains**：把多个步骤串起来
- **Agents**：让 LLM 自己决定调用哪些工具
- **Memory**：多轮对话记忆
- **Retrievers / Vector Stores**：RAG 检索

## LangChain 仓库

- **官方仓库**：https://github.com/langchain-ai/langchain
- **Stars**：95k+
- **学习路径**：
  - `libs/langchain/langchain/` - 核心库源码
  - `libs/community/` - 第三方集成（200+ 工具）
  - `cookbook/` - 实战代码片段
  - `docs/docs/tutorials/` - 官方教程

### 必看示例
```python
# 1. 基础 Chain
from langchain.chat_models import init_chat_model
from langchain.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

model = init_chat_model("gpt-4o", model_provider="openai")
prompt = ChatPromptTemplate.from_template("用一句话解释 {concept}")
chain = prompt | model | StrOutputParser()
print(chain.invoke({"concept": "量子纠缠"}))

# 2. RAG
from langchain_community.document_loaders import WebBaseLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import FAISS
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain

loader = WebBaseLoader("https://example.com")
docs = loader.load()
splits = RecursiveCharacterTextSplitter().split_documents(docs)
vectorstore = FAISS.from_documents(splits, OpenAIEmbeddings())
retriever = vectorstore.as_retriever()

prompt = ChatPromptTemplate.from_template("""
基于上下文回答：<context>{context}</context>
问题：{input}""")
qa_chain = create_stuff_documents_chain(model, prompt)
rag_chain = create_retrieval_chain(retriever, qa_chain)
print(rag_chain.invoke({"input": "总结要点"}))

# 3. Agent with Tools
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_community.tools import DuckDuckGoSearchRun

search = DuckDuckGoSearchRun()
tools = [search]
agent = create_tool_calling_agent(model, tools, prompt)
executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
print(executor.invoke({"input": "今天北京天气如何？"}))
```

## LangGraph 仓库

- **官方仓库**：https://github.com/langchain-ai/langgraph
- **Stars**：34k+
- **月下载量**：3450 万（企业采用率第一）
- **学什么**：用"图"（节点+边）编排多智能体，支持循环、条件分支、人机协同、状态持久化

### 为什么需要 LangGraph？
LangChain Agent 是"链式"的，每轮决定下一步动作。LangGraph 把整个流程建模成**有限状态机**，适合：
- 多 Agent 协作（研究员 → 写作者 → 审稿人）
- 需要回退/重试的复杂流程
- 长时任务 + 状态持久化
- 人机协同（Human-in-the-loop）

### 必看示例
```python
from langgraph.graph import StateGraph, START, END
from langgraph.checkpoint.memory import MemorySaver
from typing import TypedDict, Annotated
import operator

# 1. 定义状态
class State(TypedDict):
    messages: Annotated[list, operator.add]
    next_step: str

# 2. 定义节点
def researcher(state: State):
    """研究节点：搜集信息"""
    return {"messages": ["研究报告：xxx"]}

def writer(state: State):
    """写作节点：基于研究写文章"""
    return {"messages": ["初稿：xxx"]}

def reviewer(state: State):
    """审稿节点：检查质量"""
    return {"messages": ["审稿意见：xxx"]}

# 3. 构图
workflow = StateGraph(State)
workflow.add_node("researcher", researcher)
workflow.add_node("writer", writer)
workflow.add_node("reviewer", reviewer)

workflow.add_edge(START, "researcher")
workflow.add_edge("researcher", "writer")
workflow.add_edge("writer", "reviewer")
workflow.add_edge("reviewer", END)

# 4. 编译（带记忆）
app = workflow.compile(checkpointer=MemorySaver())

# 5. 运行
config = {"configurable": {"thread_id": "1"}}
result = app.invoke({"messages": [], "next_step": "research"}, config)
```

## 配套生态

- **LangSmith**：调试 + 监控 + 评估（生产必备）
- **LangServe**：把 Chain 部署成 REST API
- **LangChain Templates**：官方项目模板（开箱即用）

## 学习建议

| 周 | 任务 |
|---|---|
| W1 | 跑通 LangChain 官方 Quickstart + 基础 Chain |
| W2 | 做一个 RAG 文档问答（FAISS + Web 文档） |
| W3 | 实现带工具的 Agent（搜索 + 计算器） |
| W4 | 用 LangGraph 写多 Agent 协作流程 |
| W5 | 接入 LangSmith 做评估和调试 |

## 面试常问的 5 个 LangChain 题

1. **Chain 和 Agent 的本质区别？**
   - Chain 是预定义流程；Agent 让 LLM 动态决定步骤

2. **LangGraph 的 State 在哪存？**
   - Checkpoint（MemorySaver / Postgres / Redis），支持断点续跑

3. **如何让 Agent 调用外部 API？**
   - `@tool` 装饰器 + Tool 描述（name + description + args schema）

4. **RAG 中 retriever 怎么优化？**
   - MMR、Self-Query、MultiQuery、Hybrid Search、Reranker

5. **如何控制 Agent 成本？**
   - 限制迭代次数、缓存重复结果、小模型做预处理、token 计数
