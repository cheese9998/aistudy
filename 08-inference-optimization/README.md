# 08 - 推理优化 / 模型部署

> 高级岗（25K+）加分项。如果你想做 AI 架构师或 LLM 平台工程师，这是必修。

## 推理优化 4 大方向

1. **量化（Quantization）** - FP16 → INT8/INT4，显存 ↓ 4x
2. **批处理（Batching）** - 同时处理多个请求，提升吞吐
3. **KV Cache 优化** - PagedAttention、FlashAttention
4. **算子融合** - TensorRT、ONNX Runtime

## 必读仓库

### 1. vLLM ⭐⭐⭐⭐⭐（最重要）
- **仓库**：https://github.com/vllm-project/vllm
- **Stars**：30k+
- **核心创新**：**PagedAttention** —— 把操作系统的分页内存管理思想引入 KV Cache
- **效果**：相比 HuggingFace Transformers，**吞吐提升 14-24 倍**

#### 核心概念
- **PagedAttention**：KV Cache 分页存储，避免连续内存浪费
- **Continuous Batching**：动态批处理，token 级调度
- **Chunked Prefill**：长 prompt 分块处理
- **Speculative Decoding**：小模型草稿 + 大模型验证，加速生成

#### 快速使用
```bash
# 启动 OpenAI 兼容 API 服务
python -m vllm.entrypoints.openai.api_server \
  --model Qwen/Qwen2.5-7B-Instruct \
  --port 8000 \
  --tensor-parallel-size 1

# 调用
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen2.5-7B-Instruct",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

#### 在 Python 中使用
```python
from vllm import LLM, SamplingParams

llm = LLM(model="Qwen/Qwen2.5-7B-Instruct", tensor_parallel_size=1)
sampling_params = SamplingParams(temperature=0.7, max_tokens=512)

outputs = llm.generate(["介绍 vLLM"], sampling_params)
for output in outputs:
    print(output.outputs[0].text)
```

### 2. TensorRT-LLM ⭐⭐⭐⭐
- **仓库**：https://github.com/NVIDIA/TensorRT-LLM
- **NVIDIA 官方**：性能最强（针对 H100 / A100 优化）
- **适用**：超大规模生产部署
- **学习成本**：高，需要懂 CUDA

### 3. llama.cpp ⭐⭐⭐⭐
- **仓库**：https://github.com/ggerganov/llama.cpp
- **Stars**：75k+
- **核心**：纯 C++ 实现，**CPU 也能跑**
- **适用**：本地推理、Mac (M1/M2/M3/M4) 跑模型

```bash
# Mac 上跑 Qwen 7B
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp && make
./main -m qwen2.5-7b-instruct-q4_k_m.gguf \
  -p "你好" -n 512 -c 4096
```

### 4. Ollama ⭐⭐⭐⭐⭐（最易上手）
- **仓库**：https://github.com/ollama/ollama
- **Stars**：100k+
- **定位**：本地一键运行开源模型

```bash
# 装好 ollama 后
ollama pull qwen2.5:7b
ollama run qwen2.5:7b "你好"
```

```python
# Python 调用
import ollama
response = ollama.chat(model='qwen2.5:7b', messages=[
    {'role': 'user', 'content': '你好'}
])
print(response['message']['content'])
```

### 5. LiteLLM ⭐⭐⭐⭐
- **仓库**：https://github.com/BerriAI/litellm
- **Stars**：13k+
- **定位**：**统一 100+ LLM API 调用**（OpenAI 兼容协议）
- **企业必备**：路由、限流、成本统计

```python
from litellm import completion

# 一个接口调用所有模型
response = completion(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Hi"}]
)

# 自动切换不同厂商
response = completion(
    model="claude-3-5-sonnet-20241022",
    messages=[{"role": "user", "content": "Hi"}]
)
```

## 模型微调（LoRA 系列）

### Hugging Face Transformers + PEFT
- **PEFT 仓库**：https://github.com/huggingface/peft
- **方法**：LoRA / QLoRA / AdaLoRA / DoRA

```python
from peft import LoraConfig, get_peft_model

config = LoraConfig(
    r=8,                    # 低秩维度
    lora_alpha=16,
    target_modules=["q_proj", "v_proj"],  # 要插入 LoRA 的层
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM",
)
model = get_peft_model(base_model, config)
model.print_trainable_parameters()  # 只训练 0.1% 参数
```

## 部署架构

### 1. 单机部署
- Ollama + Open WebUI（个人用）

### 2. 容器化部署
- vLLM + Docker + Nginx

### 3. 集群部署
- vLLM + K8s + Ray Serve
- 多模型路由 + 灰度发布

### 4. Serverless
- Modal / Replicate / Hugging Face Inference Endpoints

## 简历项目：LLM 推理服务平台

```
项目名：MindForge 统一推理平台

背景：公司接入 GPT-4、Claude、Qwen、DeepSeek 等多模型，每个业务线各自对接成本高

技术栈：
- 推理引擎：vLLM（自部署 Qwen 2.5-72B）
- 路由层：LiteLLM
- 监控：Langfuse / Prometheus + Grafana
- 队列：Redis + Celery
- 部署：K8s + Helm
- 前端：内部 Web UI（调用统计、成本看板）

核心功能：
1. OpenAI 兼容 API 统一接入
2. 按业务路由（敏感业务走本地 Qwen、通用业务走 GPT-4o）
3. 自动降级（GPT 不可用时切 DeepSeek）
4. Token 限流 + 用户配额
5. 全链路 Trace

成果：
- 调用成本 ↓35%（智能路由）
- 响应 P99 < 3s
- 接入 20+ 业务线
```

## 面试常问的 5 个推理题

1. **vLLM 为什么比 Transformers 快？**
   - PagedAttention 减少 KV Cache 显存碎片
   - Continuous Batching 提升 GPU 利用率

2. **量化的 trade-off？**
   - INT4 显存 ↓75%，速度 ↑2-4x，准确率 ↓1-3%
   - 常用：AWQ、GPTQ、GGUF

3. **GPU 显存不够怎么办？**
   - 量化（INT8/INT4）
   - 卸载到 CPU（offload）
   - 张量并行（多卡）
   - 流水线并行
   - 序列并行

4. **LoRA 和全量微调区别？**
   - LoRA 冻结原模型，只训练低秩矩阵（< 1% 参数）
   - 节省 90% 显存，训练快 3-5x，效果接近全量

5. **Qwen、DeepSeek、Llama 选哪个？**
   - 中文场景：Qwen 2.5、DeepSeek-V3
   - 英文/代码：Llama 3.1、DeepSeek-Coder
   - 私有化：Qwen 2.5（中文最强）
