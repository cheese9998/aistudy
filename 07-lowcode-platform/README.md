# 07 - 低代码/工作流平台

> **Dify 方向**的岗位 2026 年非常多（很多公司招"AI 开发工程师"实际就是 Dify 二次开发）。低代码平台让你快速搭建 AI 应用，简历写"Dify 项目经验"直接加分。

## 主流平台对比

| 平台 | 类型 | Stars | 国产 | 特点 | 仓库 |
|---|---|---|---|---|---|
| **Dify** | 开源 + 商业 | 140k+ | ✅ | 生产级 AI 应用平台 | github.com/langgenius/dify |
| **Langflow** | 开源 | 147k+ | ❌ | 拖拽式 LangChain | github.com/langflow-ai/langflow |
| **n8n** | 开源 | 179k+ | ❌ | 通用工作流 + AI | github.com/n8n-io/n8n |
| **Flowise** | 开源 | 35k+ | ❌ | 拖拽 LangChain 早期版 | github.com/FlowiseAI/Flowise |
| **Coze** | 闭源 | - | ✅ | 字节出品、C 端友好 | coze.com |
| **MaxKB** | 开源 | 17k+ | ✅ | 知识库问答 | github.com/1Panel-dev/MaxKB |
| **Bisheng** | 开源 | 10k+ | ✅ | 必胜科技，企业 BFF | github.com/dataelement/bisheng |

## 1. Dify ⭐⭐⭐⭐⭐（最推荐学）

- **官方仓库**：https://github.com/langgenius/dify
- **官网**：https://dify.ai
- **定位**：生产级 LLM 应用开发平台
- **核心技术**：BaaS + LLMOps，DSL 定义工作流

### 为什么 Dify 是 JD 高频词？
- 开箱即用、文档好、活跃度高
- 支持私有化部署（企业刚需）
- 大量公司基于 Dify 做二次开发

### Dify 核心能力
1. **可视化工作流编排** - 拖拽式画 DAG
2. **知识库管理** - 文档上传 → 自动切片 → 向量化
3. **多模型支持** - 接入 100+ LLM（OpenAI、Claude、通义、DeepSeek）
4. **Agent 节点** - ReAct、Function Calling
5. **API 发布** - 一键把应用变成 OpenAI 兼容 API
6. **监控运维** - 调用日志、成本统计、效果评估

### 快速跑起来
```bash
# 1. 克隆
git clone --depth 1 https://github.com/langgenius/dify.git
cd dify/docker
cp .env.example .env

# 2. 启动（需要 Docker + Docker Compose）
docker compose up -d

# 3. 访问 http://localhost/install 完成初始化
```

### 二次开发
- 后端：Python (Flask) + Celery
- 前端：React + TypeScript
- 插件：基于 Plugin SDK 自定义工具

### 简历项目：基于 Dify 的企业知识助手

```
项目名：Dify 企业知识助手

业务：公司内部 1000+ 份产品手册、HR 政策、报销规则，员工查询困难

实施：
1. 部署 Dify 私有化版
2. 接入公司 Confluence / Notion 数据
3. 配置工作流：用户提问 → 意图识别 → 知识库检索 → 生成答案 → 引用原文
4. 对接企业微信机器人
5. 基于 Dify Plugin SDK 开发"工单创建"工具

成果：
- 30+ 部门使用、日均 2000+ 次查询
- HR 重复咨询工作量下降 60%
```

## 2. Langflow ⭐⭐⭐⭐

- **仓库**：https://github.com/langflow-ai/langflow
- **特点**：把 LangChain 包成可视化拖拽工具
- **优势**：PM / 运营也能搭 Agent
- **劣势**：复杂逻辑还是要写代码

## 3. n8n ⭐⭐⭐⭐

- **仓库**：https://github.com/n8n-io/n8n
- **Stars**：179k
- **定位**：通用工作流自动化，AI 是子集
- **优势**：250+ 集成（Slack、Notion、Salesforce、数据库）
- **适用**：企业流程自动化 + AI 增强

## 4. Flowise ⭐⭐⭐

- **仓库**：https://github.com/FlowiseAI/Flowise
- **定位**：拖拽式 LangChain / LlamaIndex
- **优势**：上手最快、社区大
- **劣势**：作者精力分散，发展慢于 Langflow

## 5. Coze（闭源，字节出品）

- 适合做 C 端产品（Bot 商店、插件市场）
- 抖音/飞书生态深度集成
- 学习路径：直接上 [coze.com](https://www.coze.cn) 玩

## 面试常问的 3 个低代码题

1. **Dify 和 Langflow 选哪个？**
   - Dify：生产级、知识库完善、私有化友好
   - Langflow：原型快、可直接看底层 LangChain 代码

2. **Dify 的工作流 DSL 怎么写？**
   - YAML 定义节点（节点类型、参数、连接）
   - 支持变量、循环、条件分支

3. **Dify 怎么对接企业微信/钉钉？**
   - 用 Webhook 节点接收消息
   - 用 HTTP 节点调用企业微信 API 发送
