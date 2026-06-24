# 05 - RAG 检索增强生成

> RAG 是 2026 年 AI 应用开发**最高频的面试题**和最常见的落地场景。85% 岗位 JD 明确要求。

## 什么是 RAG

**RAG（Retrieval-Augmented Generation）**：让 LLM 在回答前先检索外部知识库，解决"幻觉"和"知识陈旧"问题。

```
用户问题 → Embedding → 向量数据库检索 → 拼接 Prompt → LLM 生成答案
```

## RAG 完整链路

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 文档加载     │ →  │ 文档切片      │ →  │ Embedding    │ →  │ 向量库存储    │
│ (PDF/Word)  │    │ (chunk)      │    │ 向量化       │    │              │
└─────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                                                   ↓
┌─────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ LLM 生成    │ ←  │ Prompt 拼接   │ ←  │ 重排/Rerank  │ ←  │ 检索 Top-K   │
│             │    │ + 上下文      │    │              │    │              │
└─────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

## 核心模块

### 1. 文档加载（Loader）
- PDF: PyPDF2, pdfplumber, **LlamaParse**（复杂 PDF 首选）
- Word: python-docx
- 网页: BeautifulSoup, Playwright
- 数据库: SQLAlchemy, pgvector
- 结构化: Pandas, CSV reader

### 2. 文档切片（Chunking）
- **固定长度切片** - 简单但可能切断语义
- **递归字符切片** - LangChain 默认，按段落/句子/词递归
- **语义切片** - 按主题边界切（成本高）
- **Markdown 结构切片** - 按标题层级
- **父子切片** - 检索小块，返回大块（提升上下文完整性）
- **滑动窗口切片** - 重叠避免信息丢失

### 3. Embedding 模型
- OpenAI `text-embedding-3-small/large` - 闭源最稳
- BGE（BAAI）- 中文开源 SOTA
- M3E - 轻量中文
- Cohere embed-v3 - 多语言
- 本地：`bge-small-zh-v1.5`、`bge-large-zh-v1.5`

### 4. 向量数据库
见 `11-vector-db/` 目录专题。

### 5. 检索策略

| 策略 | 原理 | 适用 |
|---|---|---|
| 相似度检索 | 余弦相似度 | 默认 |
| MMR | 最大边际相关性 | 减少结果重复 |
| 混合检索 | 向量 + BM25 关键词 | 提升专有名词召回 |
| HyDE | 用假设答案去检索 | 短查询场景 |
| Self-Query | LLM 提取元数据过滤 | 结构化文档 |
| MultiQuery | 多查询扩展再合并 | 复杂问题 |
| 父文档检索 | 检索子块、返回父块 | 上下文完整性 |
| GraphRAG | 知识图谱 + RAG | 多跳推理 |

### 6. Reranker（重排）
- Cohere Rerank
- BGE Reranker
- Jina Reranker
- 原理：用 cross-encoder 精细打分，Top-K → Top-N

### 7. 评估体系

| 指标 | 含义 |
|---|---|
| Context Precision | 检索到的相关文档占比 |
| Context Recall | 应检索到的文档中检索到了多少 |
| Faithfulness | 答案是否基于上下文（无幻觉） |
| Answer Relevancy | 答案与问题相关度 |
| **RAGAS** | 综合评估框架（事实标准） |

## 必读仓库

### 1. rag-from-scratch ⭐⭐⭐⭐⭐
- **仓库**：https://github.com/langchain-ai/rag-from-scratch
- **作者**：LangChain 官方 + Greg Kamradt
- **Stars**：6k+
- **为什么必读**：从 0 讲 RAG 全部技巧，每集一个 Jupyter Notebook
- **章节**：
  - 基础 RAG → 块大小优化 → 检索后处理 → Reranking → 检索前查询改写 → 路由 → GraphRAG

### 2. GraphRAG ⭐⭐⭐⭐
- **仓库**：https://github.com/microsoft/graphrag
- **作者**：微软
- **学什么**：基于知识图谱的 RAG，处理多跳推理问题
- **适用**：法律、医疗、金融等需要关联推理的场景

### 3. txtai ⭐⭐⭐
- **仓库**：https://github.com/neuml/txtai
- **Stars**：11k+
- **学什么**：一体化语义搜索和 RAG 框架，开箱即用

### 4. RAGAS ⭐⭐⭐⭐
- **仓库**：https://github.com/explodinggradients/ragas
- **学什么**：RAG 评估的事实标准

## 简历项目建议：企业知识库问答

```
项目名：智库问答助手（KnowledgeQ&A）

业务背景：公司内部 5000+ 份产品文档、API 文档、会议纪要，工程师检索效率低

技术栈：
- 前端：Vue 3 + Element Plus
- 后端：FastAPI + PostgreSQL (pgvector) + Redis
- RAG：LangChain + BGE Embedding + BGE Reranker
- LLM：DeepSeek-V3 / Qwen2.5
- 部署：Docker Compose + Nginx

核心难点与解决：
1. PDF 表格解析不准 → 引入 LlamaParse
2. 检索结果相关性差 → 加 Reranker + 混合检索（BM25 + 向量）
3. 答案有幻觉 → 强制要求引用原文段落 + 拒答机制
4. 长文档超出上下文 → 父子切片 + 上下文压缩

成果指标：
- 文档检索准确率：78% → 92%（上线 1 个月）
- 平均响应时间：2.3s
- 节省工程师查阅时间：约 15 人时/周
```

## 面试必问的 8 个 RAG 题

1. **RAG 完整流程？每一步的优化点？**
2. **Chunk Size 怎么选？太大太小会怎样？**
3. **Reranker 为什么能提升准确率？**
4. **向量检索和关键词检索怎么结合？**
5. **RAG 系统的幻觉如何缓解？**
6. **如何评估 RAG 质量？RAGAS 的指标？**
7. **多文档场景如何处理权限隔离？**
8. **GraphRAG 和传统 RAG 的区别？何时用？**
