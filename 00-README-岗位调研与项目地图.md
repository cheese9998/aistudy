# 2026 年 AI 应用开发岗位知识图谱与 GitHub 项目清单

> 调研时间：2026-06-24  
> 数据来源：Boss直聘、猎聘、牛客网、智联招聘、csdn、掘金、Firecrawl 博客等公开 JD  
> 目的：为 AI 应用开发 / 智能体开发岗的求职者提供「技能 → 项目」对照地图

---

## 一、2026 年市场到底在招什么人？

### 1.1 岗位趋势

- BOSS直聘 2026 上半年：**智能体开发岗位需求同比 +217%**
- 主流岗位名称：`AI 应用开发工程师` / `Agent 开发工程师` / `大模型应用开发` / `LLM 应用工程师` / `Dify 开发工程师` / `AI 产品工程师`
- 薪资范围（社招 1-3 年）：15K-30K 起步；3-5 年：25K-50K；资深/架构师：50K-75K+；14-17 薪是常态
- 学历门槛：本科及以上为主流（大厂校招/技术专家岗要求硕士）
- 关键判断：**企业不缺"调包侠"，缺的是能把 LLM 真正落地到业务、解决幻觉/成本/工程化问题的工程师**

### 1.2 高频关键词（来自 50+ 真实 JD）

| 排名 | 关键词 | 出现频次 | 性质 |
|---|---|---|---|
| 1 | Python | 100% | 必备 |
| 2 | LLM / 大模型 | 95% | 必备 |
| 3 | RAG / 检索增强 | 85% | 必备 |
| 4 | Prompt Engineering | 85% | 必备 |
| 5 | LangChain | 80% | 必备 |
| 6 | Agent / 智能体 | 75% | 必备 |
| 7 | Docker | 70% | 必备 |
| 8 | FastAPI / Flask | 65% | 必备 |
| 9 | Dify | 55% | 高频 |
| 10 | 向量数据库 (Milvus/Chroma/Qdrant/FAISS) | 50% | 高频 |
| 11 | LlamaIndex | 40% | 高频 |
| 12 | Vue/React (前端) | 40% | 加分 |
| 13 | vLLM / 推理优化 | 35% | 加分 |
| 14 | MCP 协议 | 30% | 新趋势 |
| 15 | LoRA / 微调 | 25% | 加分 |
| 16 | Claude Code / Cursor / Copilot | 100% | **重中之重** |

---

## 二、核心技能栈分层（P0/P1/P2）

### P0 - 必学（1-2 个月拿下）

1. **Python 进阶** - 异步IO、装饰器、类型注解、Pydantic
2. **Prompt Engineering** - Zero/Few-shot、CoT、ToT、ReAct、系统提示词设计
3. **LLM API 调用** - OpenAI / Anthropic / DeepSeek / Qwen / Gemini 兼容协议
4. **FastAPI** - 异步接口、依赖注入、中间件、文档自动生成
5. **Docker 基础** - Dockerfile 编写、Compose 多容器、镜像优化
6. **AI 编程工具** - Claude Code / Cursor / Copilot / Codex（**2026 年 JD 必提**）
7. **LangChain 基础** - Chain / Tool / Agent / Memory / RAG 核心 API

### P1 - 进阶（3-6 个月）

1. **RAG 全链路** - 文档解析 → 切片 → Embedding → 检索 → 重排 → 生成 → 评估
2. **LangGraph** - 多智能体编排、状态机、人机协同
3. **LlamaIndex** - 数据连接器、索引、查询引擎
4. **向量数据库** - Milvus / Chroma / Qdrant / Weaviate 至少精通一种
5. **Dify / Coze / n8n** - 低代码 AI 应用平台二次开发
6. **数据库** - PostgreSQL + pgvector、Redis 缓存
7. **前端基础** - Vue/React 组件化、能用 Streamlit/Gradio 快速搭 demo

### P2 - 加分（6-12 个月）

1. **推理优化** - vLLM / TensorRT-LLM / ONNX / FlashAttention
2. **模型微调** - LoRA / QLoRA / DeepSpeed / Hugging Face Transformers
3. **多模态** - 图像/语音/视频理解与生成
4. **MCP 协议** - 工具调用标准化
5. **Kubernetes** - 生产部署
6. **LLMOps** - 监控、灰度、成本治理、CI/CD
7. **AutoGen / CrewAI** - 多智能体框架
8. **安全合规** - 数据脱敏、内容审核、Prompt 注入防御

---

## 三、对应的 GitHub 热门项目清单

> 按"JD 提及频次 × GitHub 活跃度"排序，**★ 表示必学**

### 3.1 大模型应用框架层

| 项目 | 仓库 | Stars | 用途 | 学习路径 |
|---|---|---|---|---|
| ★ LangChain | github.com/langchain-ai/langchain | ~95k | LLM 应用开发事实标准 | `libs/langchain` 看 Chain/Agent/RAG 实现 |
| ★ LangGraph | github.com/langchain-ai/langgraph | ~34k | 多智能体编排 | `examples/` 下有完整 multi-agent 案例 |
| ★ LlamaIndex | github.com/run-llama/llama_index | ~40k | RAG / 数据连接 | `docs/docs/getting_started` |
| Dify | github.com/langgenius/dify | ~140k | 生产级 AI 应用平台 | 直接 docker compose 跑起来用 |
| Langflow | github.com/langflow-ai/langflow | ~147k | 拖拽式 Agent 构建 | 看 `src/backend/base/flow` |

### 3.2 Agent / 智能体专项

| 项目 | 仓库 | Stars | 用途 |
|---|---|---|---|
| AutoGPT | github.com/Significant-Gravitas/AutoGPT | ~182k | 经典自主智能体 |
| CrewAI | github.com/crewAIInc/crewAI | ~25k | 多智能体协作（角色+任务） |
| AutoGen | github.com/microsoft/autogen | ~50k | 微软出品，多智能体对话 |
| smolagents | github.com/huggingface/smolagents | ~20k | HuggingFace 极简 Agent |
| OpenHands | github.com/All-Hands-AI/OpenHands | ~60k | AI 软件工程师 Agent |

### 3.3 RAG 实战资源

| 项目 | 仓库 | 用途 |
|---|---|---|
| rag-from-scratch | github.com/langchain-ai/rag-from-scratch | LangChain 官方 RAG 教程（从0到1） |
| GraphRAG | github.com/microsoft/graphrag | 微软知识图谱 RAG |
| txtai | github.com/neuml/txtai | 语义搜索全家桶 |
| verba | github.com/weaviate/Verba | Weaviate 出品的 RAG UI |

### 3.4 推理优化 / 模型服务

| 项目 | 仓库 | 用途 |
|---|---|---|
| vLLM | github.com/vllm-project/vllm | 高吞吐推理（PagedAttention） |
| TensorRT-LLM | github.com/NVIDIA/TensorRT-LLM | NVIDIA 推理加速 |
| llama.cpp | github.com/ggerganov/llama.cpp | 本地 CPU/GPU 推理 |
| Ollama | github.com/ollama/ollama | 本地模型一键运行 |
| LiteLLM | github.com/BerriAI/litellm | 统一 100+ LLM API 入口 |

### 3.5 向量数据库

| 项目 | 仓库 | 特点 |
|---|---|---|
| Milvus | github.com/milvus-io/milvus | 国产、分布式、生产首选 |
| Qdrant | github.com/qdrant/qdrant | Rust 写、性能强 |
| Chroma | github.com/chroma-core/chroma | 原型首选、嵌入式 |
| Weaviate | github.com/weaviate/weaviate | 内置向量化模块 |
| pgvector | github.com/pgvector/pgvector | PostgreSQL 扩展，最轻量 |

### 3.6 学习资源（按推荐顺序）

| 项目 | 仓库 | 适合 |
|---|---|---|
| ★ awesome-llm-apps | github.com/Shubhamsaboo/awesome-llm-apps | 100+ 可运行 RAG/Agent 示例（最推荐入门） |
| ★ microsoft/generative-ai-for-beginners | github.com/microsoft/generative-ai-for-beginners | 微软官方 21 课时课程 |
| ★ Hands-On-Large-Language-Models | github.com/HandsOnLLM/Hands-On-Large-Language-Models | Jay Alammar 经典书配套代码 |
| LLMs-from-scratch | github.com/rasbt/LLMs-from-scratch | Sebastian Raschka 从零实现 LLM |
| prompt-engineering-guide | github.com/dair-ai/Prompt-Engineering-Guide | DAIR.AI 的 Prompt 圣经 |
| system-prompts-and-models-of-ai-tools | github.com/x1xhlol/system-prompts-and-models-of-ai-tools | 130k+ Star，研究大厂系统提示词 |

### 3.7 前端 / 应用界面

| 项目 | 仓库 | 用途 |
|---|---|---|
| Open WebUI | github.com/open-webui/open-webui | ChatGPT 风格前端，对接 Ollama |
| LibreChat | github.com/danny-avila/LibreChat | 多模型聊天界面 |
| Lobe Chat | github.com/lobehub/lobe-chat | 国产、UI 漂亮、支持多模型 |
| ChatGPT-Next-Web | github.com/ChatGPTNextWeb/ChatGPT-Next-Web | 国内可访问版本 |

### 3.8 工作流 / 低代码

| 项目 | 仓库 | 特点 |
|---|---|---|
| n8n | github.com/n8n-io/n8n | 179k Stars，AI Workflow 编排 |
| Flowise | github.com/FlowiseAI/Flowise | 拖拽 LangChain |
| dify | (见上) | 国产，开箱即用 |
| coze | coze.com (官方) | 字节出品 |

### 3.9 AI 编程工具（2026 必学）

| 工具 | 用途 |
|---|---|
| Claude Code | 终端 AI 编程（**JD 出现率最高**） |
| Cursor | AI IDE 标杆 |
| GitHub Copilot | 老牌 AI 补全 |
| Codex / GPT-5-Codex | OpenAI 代码专用模型 |
| Trae | 字节出品的 AI IDE（你正在用的） |
| 通义灵码 | 阿里免费 AI 编程 |
| Cline / Continue | 开源 AI 编程插件 |

---

## 四、按岗位方向的「技能 → 项目」对照表

### 方向 A：AI 应用后端工程师（11-20K）

- 必学：Python / FastAPI / Docker / Prompt / OpenAI API
- 重点仓库：`Shubhamsaboo/awesome-llm-apps` 选 3-5 个 simple_chat 跑通
- 简历项目：基于 FastAPI 的多模型对话网关

### 方向 B：RAG 工程师（20-40K）

- 必学：LangChain / LlamaIndex / 向量数据库 / 文档解析
- 重点仓库：`langchain-ai/rag-from-scratch` + `microsoft/graphrag`
- 简历项目：企业知识库 / 文档问答系统（含 rerank、混合检索、评估）

### 方向 C：Agent 工程师（25-50K）

- 必学：LangGraph / AutoGen / CrewAI / Function Calling / MCP
- 重点仓库：`langchain-ai/langgraph` examples + `microsoft/autogen` samples
- 简历项目：多 Agent 协作系统（如自动写标书 / 智能面试 / 办公自动化）

### 方向 D：Dify 平台工程师（15-30K）

- 必学：Dify 工作流编排 / 知识库 / 插件二次开发
- 重点仓库：`langgenius/dify` 源码（看 api + docker 部署）
- 简历项目：Dify + 企业系统集成（OA/ERP 对接）

### 方向 E：AI 架构师（50-75K+）

- 必学：vLLM / K8s / 微调 / 分布式 / LLMOps
- 重点仓库：`vllm-project/vllm` + `NVIDIA/TensorRT-LLM`
- 简历项目：LLM 推理服务平台 / 多模型路由

---

## 五、4 个月学习路线（求职向）

| 周数 | 内容 | 输出物 |
|---|---|---|
| W1-2 | Python 进阶 + FastAPI + Docker | Todo API 容器化部署 |
| W3-4 | Prompt 工程 + OpenAI/Claude API | 提示词模板库 + CLI 工具 |
| W5-6 | LangChain 基础 + 第一个 RAG | 文档问答 demo（用 Chroma） |
| W7-8 | LlamaIndex + 高级 RAG（rerank/混合） | 企业知识库项目（写简历） |
| W9-10 | LangGraph + Function Calling | 多工具 Agent demo |
| W11-12 | Dify / Langflow + 工作流编排 | Dify 二次开发 demo |
| W13-14 | vLLM 部署 + 向量库选型 | 推理服务 demo |
| W15-16 | 准备简历 + 项目包装 + 刷题 | Boss 投递 + 模拟面试 |

---

## 六、简历项目包装建议（核心：可量化、有难点、有截图）

每个项目用 STAR 法则描述，重点突出：
- **S**ituation：什么业务场景（最好贴近目标公司）
- **T**ask：你的角色和负责模块
- **A**ction：用了什么技术、解决了什么难点（幻觉/并发/成本/检索不准）
- **R**esult：可量化指标（响应时间 ↓40%、准确率 ↑25%、节省 20 人时/周）

推荐 3 个高性价比项目（写 1-2 个到简历即可）：

1. **企业知识库问答系统** - RAG + 向量库 + Web UI
2. **多 Agent 协作平台** - LangGraph + MCP + 工具链
3. **AI 数据分析助手** - Text-to-SQL + Dify + 多模型路由

---

## 七、面试高频题（提前准备）

1. RAG 的完整链路？切片策略有哪些？
2. 如何解决 LLM 幻觉问题？
3. Agent 和 Chain 的区别？什么是 ReAct？
4. Function Calling 原理？MCP 协议解决了什么问题？
5. 向量数据库如何选型？HNSW 原理？
6. Prompt 注入如何防御？
7. vLLM 为什么快？PagedAttention 原理？
8. RAG 评估怎么做？RAGAS 指标？
9. LoRA 和 QLoRA 区别？什么时候需要微调？
10. 线上 LLM 服务如何做成本治理？（缓存/降级/路由）

---

## 八、参考资源链接

- [50+ 真实 JD 拆解 - 掘金](https://juejin.cn/post/7644822313692692518)
- [2026 金三银四求职指南 - CSDN](https://blog.csdn.net/qq_44866828/article/details/159516001)
- [AI 全栈开发学习路线 - 掘金](https://juejin.cn/post/7651528402427609122)
- [Best Open Source Agent Frameworks 2026 - Firecrawl](https://www.firecrawl.dev/blog/best-open-source-agent-frameworks)
- [Top 20 AI Projects on GitHub 2026 - NocoBase](https://www.nocobase.com/en/blog/best-open-source-ai-projects-github-2026)
- [Personal AI Agent GitHub - OneClaw](https://www.oneclaw.net/blog/personal-ai-agent-github)
