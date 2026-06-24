# 12 - 学习资源汇总

> 这些都是公开的优质学习资源，按"必学 → 选学"排序。

## 必读仓库（直接 clone 下来读）

### 1. Shubhamsaboo/awesome-llm-apps ⭐⭐⭐⭐⭐
- **链接**：https://github.com/Shubhamsaboo/awesome-llm-apps
- **Stars**：115k
- **为什么是 TOP 1**：100+ 个**可运行**的 AI Agent / RAG 示例项目
- **学什么**：
  - 跑通 3-5 个 ChatGPT 风格应用 → 理解 LLM API
  - 跑通 3-5 个 RAG 应用 → 理解文档问答
  - 跑通 3-5 个 Agent 应用 → 理解工具调用
- **优势**：每个项目自包含、独立 README、Apache 2.0 协议
- **学习方式**：不要全看！按你目标岗位挑 5-10 个跑通

### 2. langchain-ai/rag-from-scratch ⭐⭐⭐⭐⭐
- **链接**：https://github.com/langchain-ai/rag-from-scratch
- **作者**：LangChain 官方 + Greg Kamradt
- **学什么**：从 0 讲 RAG 全部技巧，每集一个 Jupyter Notebook
- **章节**：
  - Part 1-5: 基础 RAG → chunk size → retrieval 后处理 → reranking
  - Part 6-10: 查询改写 → 路由 → GraphRAG
- **学习方式**：跟着敲，每章 30-60 分钟

### 3. microsoft/generative-ai-for-beginners ⭐⭐⭐⭐
- **链接**：https://github.com/microsoft/generative-ai-for-beginners
- **Stars**：92k
- **学什么**：微软官方 21 课时生成式 AI 课程
- **适合**：零基础入门

### 4. HandsOnLLM/Hands-On-Large-Language-Models ⭐⭐⭐⭐⭐
- **链接**：https://github.com/HandsOnLLM/Hands-On-Large-Language-Models
- **配套书**：《Hands-On Large Language Models》
- **作者**：Jay Alammar、Maarten Grootendorst
- **学什么**：9 章实战，覆盖 LLM 全栈
- **特色**：图解 + 代码 + Colab

### 5. rasbt/LLMs-from-scratch ⭐⭐⭐⭐
- **链接**：https://github.com/rasbt/LLMs-from-scratch
- **Stars**：35k
- **作者**：Sebastian Raschka（权威）
- **学什么**：从 0 实现 GPT 架构 → 训练 → 微调 → RAG
- **特色**：图解详细、代码可读性强

## 必读文档/教程

### 概念扫盲
- Jay Alammar 的可视化系列：
  - [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
  - [How GPT3 Works](https://jalammar.github.io/how-gpt3-works-visualizations-animations/)
  - [Visualizing RAG](https://jalammar.github.io/visualizing-rag/)

### 官方文档
- **LangChain 文档**：https://python.langchain.com
- **LangGraph 文档**：https://langchain-ai.github.io/langgraph/
- **LlamaIndex 文档**：https://docs.llamaindex.ai
- **Dify 文档**：https://docs.dify.ai
- **vLLM 文档**：https://docs.vllm.ai

### Prompt 学习
- [DAIR.AI Prompt Engineering Guide](https://www.promptingguide.ai/zh)
- [Anthropic Prompt Engineering](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

## 必看视频

### 中文
- **李沐的 AI 论文精读**（B 站） - 原理深入
- **跟李沐学 AI**（Bilibili） - 系统课
- **DataWhale 开源教程**（GitHub + 公众号）
- **机器之心 / 量子位**（公众号） - 行业资讯

### 英文
- **Andrej Karpathy**：
  - "Let's build GPT"（强烈推荐，从 0 写 GPT）
  - "Intro to Large Language Models"
  - "State of GPT"（MS Build 演讲）
- **3Blue1Brown**：深度学习可视化
- **Yannic Kilcher**：论文精读
- **Sam Witteveen**（YouTube） - LangChain/LlamaIndex 实战

## 必关注社区

| 平台 | 用途 |
|---|---|
| **HuggingFace** | 模型 / Dataset / Spaces |
| **LangChain Hub** | 提示词 / Chain 共享 |
| **arXiv** | 最新论文（cs.CL、cs.AI） |
| **Papers with Code** | 论文 + 代码 |
| **Reddit r/LocalLLaMA** | 本地模型讨论 |
| **X (Twitter) AI 圈** | Andrew Ng、Andrej K、Yann LeCun |
| **即刻 AI 圈** | 中文 AI 动态 |
| **GitHub Trending** | 发现热门项目 |

## 必装工具

| 工具 | 用途 |
|---|---|
| **Claude Code** | 终端 AI 编程（JD 出现率最高） |
| **Cursor** | AI IDE |
| **Trae** | 字节 AI IDE（你正在用） |
| **GitHub Copilot** | AI 补全 |
| **Continue** | 开源 AI 编程插件 |
| **LM Studio** | 本地模型 GUI |
| **Ollama** | 本地模型 CLI |
| **RAGAS** | RAG 评估 |
| **Langfuse** | LLMOps 自部署 |
| **Dify** | 私有化 AI 平台 |

## 论文精读推荐（按主题）

### RAG
- [RAG 原始论文 (Lewis et al., 2020)](https://arxiv.org/abs/2005.11401)
- [Self-RAG](https://arxiv.org/abs/2310.11511)
- [GraphRAG (Microsoft)](https://arxiv.org/abs/2404.16130)

### Agent
- [ReAct](https://arxiv.org/abs/2210.03629)
- [Toolformer](https://arxiv.org/abs/2302.04761)
- [Reflexion](https://arxiv.org/abs/2303.11381)
- [AutoGPT / BabyAGI 综述]

### Prompt
- [Chain-of-Thought Prompting](https://arxiv.org/abs/2201.11903)
- [Tree of Thoughts](https://arxiv.org/abs/2305.10601)

### 推理优化
- [PagedAttention (vLLM)](https://arxiv.org/abs/2309.06180)
- [FlashAttention](https://arxiv.org/abs/2205.14135)

## 学习顺序建议（4 个月求职向）

```
W1-2  Python 进阶 + FastAPI + Docker 基础
       → 跟 awesome-llm-apps 的 simple_chat 跑通

W3-4  Prompt 工程 + OpenAI/Claude API + Cursor 练手
       → 读 dair-ai/Prompt-Engineering-Guide

W5-6  LangChain 基础 + 第一个 RAG
       → 跟 langchain-ai/rag-from-scratch Part 1-5

W7-8  高级 RAG（rerank / 混合 / HyDE）
       → 看 jalammar/visualizing-rag
       → 看 generative-ai-for-beginners 课时 5-8

W9-10 LangGraph + Agent 实战
       → 跑 awesome-llm-apps 的 agent 目录
       → 做一个 multi-agent demo

W11-12 Dify 部署 + 二次开发
       → 本地 docker compose 跑起来
       → 做一个企业内部知识助手

W13-14 vLLM 部署 + 推理优化入门
       → Ollama 本地跑通 Qwen
       → vLLM 起 OpenAI 兼容服务

W15-16 准备简历 + 项目包装
       → 看 13-resume-materials
       → 投递 + 模拟面试
```
