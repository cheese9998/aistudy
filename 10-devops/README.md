# 10 - DevOps / 工程化

> 70% 岗位要求 Docker，35% 要求 K8s。AI 应用上生产必学。

## 必备技能

### 1. Git & GitHub（必会）
- 分支策略：GitFlow / Trunk-Based
- PR / Code Review
- GitHub Actions CI/CD
- Conventional Commits

### 2. Linux 基础
```bash
# 必备命令
ls / cd / pwd / mkdir / rm / cp / mv
cat / less / head / tail / grep
ps / top / htop / kill
chmod / chown
ssh / scp / rsync
systemctl / journalctl
```

### 3. Docker（必会）
```dockerfile
# Python + FastAPI 示例
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    depends_on:
      - postgres
      - redis
  
  postgres:
    image: pgvector/pgvector:pg16
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
  
  redis:
    image: redis:7-alpine

volumes:
  pgdata:
```

### 4. Kubernetes（加分）
- Pod / Deployment / Service / Ingress
- ConfigMap / Secret
- HPA 自动扩缩容
- Helm Chart

### 5. CI/CD
- GitHub Actions（最常用）
- GitLab CI
- Jenkins

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: pytest
      
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build & Push Docker
        run: |
          docker build -t myapp:${{ github.sha }} .
          docker push myapp:${{ github.sha }}
      - name: Deploy to K8s
        run: kubectl set image deployment/api api=myapp:${{ github.sha }}
```

### 6. 监控（生产必备）
- **Prometheus** - 指标采集
- **Grafana** - 可视化
- **Langfuse / LangSmith** - LLM 专项监控（token、成本、效果）
- **Sentry** - 错误追踪
- **ELK / Loki** - 日志

### 7. 反向代理 & 网关
- **Nginx** - 必备
- **Caddy** - 自动 HTTPS
- **Kong / APISIX** - API 网关
- **Cloudflare** - CDN + 防护

## LLMOps 专项

### 核心问题
- 多个 LLM 服务的成本怎么统计？
- 线上效果怎么监控？
- 失败 case 怎么定位和复现？
- 怎么 A/B 测试新模型？

### 推荐工具
| 工具 | 用途 | 仓库 |
|---|---|---|
| **Langfuse** | 开源 LLMOps、trace 追踪 | github.com/langfuse/langfuse |
| **LangSmith** | LangChain 官方 | smith.langchain.com |
| **Helicone** | LLM 代理、监控 | github.com/Helicone/helicone |
| **Phoenix (Arize)** | 可观测性 + 评估 | github.com/Arize-ai/phoenix |
| **RAGAS** | RAG 评估 | github.com/explodinggradients/ragas |

### Langfuse 自部署（推荐）
```bash
git clone https://github.com/langfuse/langfuse.git
cd langfuse
docker compose up -d
```

```python
from langfuse import Langfuse
langfuse = Langfuse(public_key="...", secret_key="...", host="http://localhost:3000")

# 追踪 LLM 调用
trace = langfuse.trace(name="chat-completion")
generation = trace.generation(
    name="gpt-4o",
    model="gpt-4o",
    input=messages,
    output=response,
    usage={"prompt_tokens": 100, "completion_tokens": 50},
)
```

## 简历加分项

- ✅ 部署过 Dify / LangChain 应用到 K8s
- ✅ 接入 Langfuse 做线上监控
- ✅ 用 GitHub Actions 跑 CI/CD
- ✅ 写过 Dockerfile 优化（多阶段构建、镜像 < 500MB）
- ✅ 配过 Nginx 反代 + HTTPS

## 面试常问的 DevOps 题

1. **Docker 多阶段构建？为什么用？**
   - 减小镜像体积、分离构建/运行环境

2. **K8s Pod 和 Deployment 区别？**
   - Pod 是最小调度单位；Deployment 管理 Pod 副本

3. **CI/CD 流水线怎么设计？**
   - 提 PR → 跑测试 → 构建镜像 → 推镜像 → 自动部署

4. **线上服务挂了怎么排查？**
   - 看监控（Grafana）→ 看日志（ELK）→ 看 Trace（Langfuse）→ 复现
