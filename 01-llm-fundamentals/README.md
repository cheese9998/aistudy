# 01 - LLM 基础理论

> 学 AI 应用开发前需要掌握的"地基"——理解 Transformer、注意力机制、Token、上下文窗口等概念。不需要深究数学，但要能讲清楚原理。

## 核心概念速查

| 概念 | 一句话解释 |
|---|---|
| Token | 模型处理的最小文本单位（中文约 1 字 = 1-2 tokens） |
| Embedding | 把文本转成向量的过程，相似的文本向量距离近 |
| Attention | 让模型"看上下文"时聚焦关键信息 |
| Transformer | 当前所有 LLM 的基础架构（Encoder-Decoder / Decoder-only） |
| Context Window | 模型一次能看多长（4k / 32k / 128k / 200k） |
| Temperature | 控制输出随机性（0=确定，1=创造） |
| Top-p / Top-k | 采样策略，控制输出多样性 |
| Fine-tuning | 在小数据集上微调预训练模型 |
| LoRA | 低秩适配微调，省显存 |
| RAG | 检索增强生成，解决幻觉和时效问题 |
| Agent | 能感知、规划、调用工具的自主系统 |
| Function Calling | 让 LLM 调用外部 API 的机制 |
| MCP | Model Context Protocol，Anthropic 提出的工具调用标准 |

## 推荐学习路径

### 1. 入门（1周）
- 3Blue1Brown 视频：**[深度学习可视化](https://www.youtube.com/watch?v=aircAruvnKk)**
- Andrej Karpathy 视频：**[Let's build GPT](https://www.youtube.com/watch?v=kCc8FmEb1nY)** —— 从零手搓 GPT

### 2. 原理（2-3周）
- Jay Alammar 的 **[The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)**
- Sebastian Raschka 的 **[LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch)** —— 边写代码边学原理

### 3. 进阶（按需）
- 苏剑林的 **[科学空间](https://spaces.ac.cn/)** —— 中文圈最硬核的 LLM 原理博客
- HuggingFace NLP Course

## 必读仓库

### LLMs-from-scratch
- **仓库**：https://github.com/rasbt/LLMs-from-scratch
- **作者**：Sebastian Raschka（Lightning AI 科学家）
- **Stars**：50k+
- **学什么**：从 0 实现 GPT 架构 → 预训练 → 微调 → RAG
- **建议**：跟着 `ch02` 到 `ch07` 一章一章敲代码

## 面试常问的 3 个原理题

1. **Self-Attention 公式是什么？为什么除以 √d？**
   - Q·K^T / √d_k → softmax → ·V；除以 √d 防止点积过大导致 softmax 饱和

2. **Decoder-only 和 Encoder-Decoder 的区别？现在为什么都是 Decoder-only？**
   - Decoder-only（GPT）用 Causal Mask 单向；统一了理解和生成任务，规模化效果好

3. **KV Cache 是什么？为什么推理时要缓存？**
   - 自回归生成时缓存已计算的 K/V，避免重复计算；显存换速度
