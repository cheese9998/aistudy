# 04 - LlamaIndex

> LlamaIndex 专注于 **RAG（检索增强生成）** 场景，提供丰富的数据连接器、索引策略和查询引擎。是 RAG 工程师的核心工具之一。

## LlamaIndex 仓库

- **官方仓库**：https://github.com/run-llama/llama_index
- **Stars**：40k+
- **核心定位**：数据 → 知识 → 智能查询 的全流程框架
- **学习路径**：`docs/docs/getting_started/starter_example/` + `examples/`

## 核心概念

| 模块 | 作用 |
|---|---|
| **Data Connectors** | 从 PDF、Word、Notion、数据库、网站等加载数据 |
| **Documents / Nodes** | 文档和切片节点 |
| **Transformations** | 文本切片、Embedding、元数据提取 |
| **Indexes** | 索引策略（VectorIndex、ListIndex、TreeIndex、KeywordIndex） |
| **Query Engines** | 多种查询模式（语义、混合、子问题） |
| **Retrievers** | 检索器（基础、融合、自查询） |
| **Agents / Workflows** | 多步骤 Agent 编排 |
| **LlamaParse** | 商业级 PDF 解析（业界最强） |

## 必看示例

```python
# 1. 最简 RAG（5 行代码）
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader("data").load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query("文档主要讲了什么？")
print(response)

# 2. 高级 RAG：带 Reranker 的混合检索
from llama_index.core import VectorStoreIndex, Settings
from llama_index.core.node_parser import SentenceSplitter
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.postprocessor.cohere_rerank import CohereRerank

# 配置全局
Settings.embed_model = OpenAIEmbedding(model="text-embedding-3-small")
Settings.node_parser = SentenceSplitter(chunk_size=512, chunk_overlap=50)

index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine(
    similarity_top_k=10,
    node_postprocessors=[CohereRerank(top_n=3)],  # 取 10 个再用 reranker 选 3 个
)
response = query_engine.query("xxx")

# 3. 多文档 Agent
from llama_index.core.agent import ReActAgent

agent = ReActAgent.from_tools([...], llm=model, verbose=True)
response = agent.chat("对比 A 和 B 文档的差异")
```

## 核心优势

1. **数据连接器丰富** - 200+ Loader，Notion/Slack/Discord/PDF/Excel 全支持
2. **LlamaParse** - 处理复杂 PDF（表格、公式、图片）行业最强
3. **索引策略多样** - 同一份数据可建多种索引适应不同查询
4. **与 LangChain 互操作** - 可作为 LangChain 的 Retriever 使用

## 与 LangChain 的取舍

| 场景 | 推荐 |
|---|---|
| 复杂 RAG 流程、多数据源 | LlamaIndex |
| Agent 编排、工具链 | LangChain |
| 都要 | 组合用（LlamaIndex 做数据层，LangChain 做 Agent 层） |

## 学习建议

1. 先跑官方 `starter_example`（5 分钟）
2. 看 `examples/retrievers/` 了解各种检索策略
3. 试 `examples/agent/` 多文档 Agent
4. 进阶看 `examples/low_level/` 理解底层 API

## 面试常问的 3 个 LlamaIndex 题

1. **VectorIndex 和 SummaryIndex 区别？**
   - VectorIndex 基于 Embedding 相似度；SummaryIndex 让 LLM 遍历所有节点

2. **如何处理大文档的检索效率问题？**
   - 分层索引（父文档+子切片）、HyDE、查询改写、压缩

3. **RAG 准确率上不去怎么办？**
   - 加 Reranker、混合检索（BM25+向量）、查询扩展、HyDE、Fine-tune Embedding
