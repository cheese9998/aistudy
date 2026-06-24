# 02 - Prompt Engineering 提示词工程

> 2026 年 JD 必提技能，P0 优先级。**不要小看提示词**——面试必问、工作中 80% 时间都在调提示词。

## 核心技巧矩阵

| 技巧 | 用途 | 示例 |
|---|---|---|
| Zero-shot | 简单任务直接提问 | "把这段话翻译成英文：" |
| Few-shot | 给 2-3 个示例让模型模仿 | 输入→输出，输入→输出，输入→ |
| CoT (Chain of Thought) | 复杂推理任务 | "Let's think step by step" |
| ToT (Tree of Thoughts) | 多路径探索 | 让模型生成多方案再选最优 |
| ReAct | Agent 推理+行动 | Thought → Action → Observation |
| Self-Consistency | 多次采样取多数 | 同一问题问 5 次取最一致答案 |
| Role Prompting | 设定专家角色 | "你是一个 10 年经验的 Python 工程师" |
| Structured Output | 指定 JSON/XML 格式 | "返回 JSON：{summary, score}" |
| System Prompt | 全局设定 | 角色、风格、约束、工具说明 |
| Prompt Chaining | 多步提示词串联 | 上一步输出作为下一步输入 |

## 提示词模板黄金结构

```text
# Role（角色）
你是一位资深的[领域]专家，擅长[技能]。

# Context（背景）
当前业务场景是[描述]。

# Task（任务）
请完成[具体任务]。

# Constraints（约束）
- 输出格式：JSON
- 长度限制：200字以内
- 禁止：[不允许的内容]

# Examples（示例）
输入：xxx
输出：xxx

# Workflow（步骤）
1. 第一步...
2. 第二步...

# Initialization（开场）
请先理解上述设定，等待我给出具体输入。
```

## 必读仓库

### 1. prompt-engineering-guide
- **仓库**：https://github.com/dair-ai/Prompt-Engineering-Guide
- **作者**：DAIR.AI 团队
- **Stars**：55k+
- **学什么**：从基础到高级的完整提示词教程，含论文和示例
- **必读章节**：RAG、ReAct、Program-Aided Language Models

### 2. system-prompts-and-models-of-ai-tools
- **仓库**：https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools
- **Stars**：130k+
- **学什么**：看 v0、Cursor、Devin、Manus、Perplexity 等产品的真实系统提示词
- **价值**：直接抄大厂的提示词设计范式

### 3. prompts.chat
- **仓库**：https://github.com/f/prompts.chat
- **Stars**：151k
- **学什么**：开源提示词社区，按场景分类（编程/写作/营销/教育）

## 实战练习

```python
# 1. 角色 + 约束 + 格式 三件套
prompt = """
你是一位资深 HR 招聘官，擅长快速筛选简历。
请根据以下简历内容判断是否匹配"AI 应用开发工程师"岗位。
- 必备：Python、LLM、LangChain
- 加分：RAG、Agent、Dify

简历：
{resume}

按 JSON 格式输出：{"match": "high/medium/low", "reason": "...", "missing_skills": [...]}
"""

# 2. CoT 推理
prompt = """
判断下面这段代码是否有 bug，先一步步分析：
{code}

请按以下步骤：
1. 逐行阅读
2. 列出潜在问题
3. 给出修复建议
"""

# 3. Few-shot 分类
prompt = """
判断情感倾向：

示例：
"这个产品太棒了" → 正面
"一般般吧" → 中性
"难用死了" → 负面

待分类："{text}" →
"""
```

## 面试常问的 3 个 Prompt 题

1. **如何让 LLM 输出稳定的 JSON？**
   - 指定格式 + Pydantic 校验 + `response_format={"type": "json_object"}`

2. **如何减少幻觉？**
   - 提供上下文（RAG）、降低 temperature、要求引用来源、加 system 约束

3. **长 prompt 如何优化 token 成本？**
   - 模板化复用、压缩示例、只传关键变量、用小模型做预处理
