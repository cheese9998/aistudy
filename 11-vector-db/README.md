# 11 - 向量数据库

> 50% 岗位 JD 明确要求向量库经验。RAG 的核心组件。

## 主流向量库对比

| 数据库 | Stars | 特点 | 适用场景 | 仓库 |
|---|---|---|---|---|
| **Milvus** | 32k+ | 国产、分布式、生产首选 | 大规模、企业级 | github.com/milvus-io/milvus |
| **Qdrant** | 22k+ | Rust 写、性能强 | 中小规模、高性能 | github.com/qdrant/qdrant |
| **Chroma** | 15k+ | 嵌入式、原型首选 | 学习、POC | github.com/chroma-core/chroma |
| **Weaviate** | 12k+ | 内置向量化、GraphQL | 多模态、欧洲企业 | github.com/weaviate/weaviate |
| **pgvector** | 5k+ | PostgreSQL 扩展 | 已有 PG 栈、轻量 | github.com/pgvector/pgvector |
| **FAISS** | 32k+ | Meta 出品、库而非 DB | 内存级检索、嵌入 | github.com/facebookresearch/faiss |
| **Pinecone** | 闭源 | 托管服务 | 不想运维 | pinecone.io |
| **Elasticsearch** | - | 8.x 支持向量 | 已有 ES 栈 | elastic.co |

## 1. Chroma（入门首选）

```bash
pip install chromadb
```

```python
import chromadb

client = chromadb.PersistentClient(path="./chroma_db")
collection = client.get_or_create_collection("my_docs")

# 添加文档
collection.add(
    documents=["苹果是一种水果", "香蕉是黄色的", "汽车是交通工具"],
    ids=["1", "2", "3"],
    metadatas=[{"category": "fruit"}, {"category": "fruit"}, {"category": "vehicle"}],
)

# 检索
results = collection.query(
    query_texts=["什么能吃？"],
    n_results=2,
    where={"category": "fruit"},  # 元数据过滤
)
print(results)
```

## 2. Milvus（生产首选）

```python
from pymilvus import MilvusClient, DataType

client = MilvusClient(uri="http://localhost:19530")

# 创建集合
client.create_collection(
    collection_name="my_docs",
    dimension=1536,  # OpenAI embedding 维度
    metric_type="COSINE",
)

# 插入
data = [
    {"id": 1, "vector": [0.1, 0.2, ...], "text": "苹果", "category": "fruit"},
    {"id": 2, "vector": [0.2, 0.3, ...], "text": "汽车", "category": "vehicle"},
]
client.insert(collection_name="my_docs", data=data)

# 检索
results = client.search(
    collection_name="my_docs",
    data=[[0.1, 0.2, ...]],
    limit=5,
    filter="category == 'fruit'",
    output_fields=["text", "category"],
)
```

## 3. Qdrant（性能之王）

```python
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams, PointStruct

client = QdrantClient(url="http://localhost:6333")

client.create_collection(
    collection_name="my_docs",
    vectors_config=VectorParams(size=1536, distance=Distance.COSINE),
)

client.upsert(
    collection_name="my_docs",
    points=[
        PointStruct(id=1, vector=[0.1, 0.2, ...], payload={"text": "苹果"}),
    ],
)

results = client.search(
    collection_name="my_docs",
    query_vector=[0.1, 0.2, ...],
    limit=5,
    with_payload=True,
)
```

## 4. pgvector（已有 PG 栈首选）

```sql
-- 启用扩展
CREATE EXTENSION vector;

-- 建表
CREATE TABLE documents (
    id BIGSERIAL PRIMARY KEY,
    content TEXT,
    embedding vector(1536)
);

-- 创建索引（HNSW 算法）
CREATE INDEX ON documents USING hnsw (embedding vector_cosine_ops);

-- 检索（找最近的 5 个）
SELECT content FROM documents
ORDER BY embedding <=> '[0.1, 0.2, ...]'
LIMIT 5;
```

## 核心算法

| 算法 | 原理 | 速度 | 准确度 |
|---|---|---|---|
| **Flat**（暴力） | 全量比对 | ❌ 慢 | ✅ 100% |
| **IVF**（倒排） | 聚类 + 桶内搜索 | ⚡ 中 | ✅ 高 |
| **HNSW**（图） | 分层导航小世界图 | ✅✅ 快 | ✅ 接近 100% |
| **PQ**（量化） | 向量量化压缩 | ✅✅ 快 | ⚠️ 略低 |

**生产环境推荐 HNSW**（Milvus/Qdrant/pgvector 都支持）

## 选型指南

| 场景 | 推荐 |
|---|---|
| 学习/POC | Chroma |
| 中小规模生产 | Qdrant / pgvector |
| 大规模分布式 | Milvus |
| 已有 ES 栈 | Elasticsearch 8.x |
| 不想运维 | Pinecone（托管） |

## 简历加分

会熟练使用至少 1-2 个向量库，了解 HNSW、量化、混合检索原理。
