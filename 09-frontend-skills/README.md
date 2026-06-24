# 09 - 前端技能

> AI 应用开发 40% 岗位要求前端能力（Vue/React）。Dify 方向、Agent 平台、对话产品都需要。

## 必备技能

### 1. 基础三件套
- **HTML5** - 语义化标签、表单、Canvas
- **CSS3** - Flex/Grid、动画、响应式
- **JavaScript** - ES6+、异步、模块化

### 2. 主流框架（必须会一个）
| 框架 | 国内用 | 特点 |
|---|---|---|
| **Vue 3** | ⭐⭐⭐⭐⭐ | 国内主流、Composition API、上手快 |
| **React 18** | ⭐⭐⭐⭐ | 国外主流、生态最大 |
| **Next.js** | ⭐⭐⭐⭐ | React 全栈框架、SSR |
| **Nuxt 3** | ⭐⭐⭐ | Vue 全栈框架 |

### 3. UI 组件库
- **Element Plus** - Vue 3 国内最常用
- **Ant Design** - 中后台首选
- **shadcn/ui** - 2026 新趋势、可复制源码
- **Tailwind CSS** - 原子化 CSS 必备

### 4. 状态管理
- Pinia (Vue) / Zustand (React) / Redux Toolkit

### 5. 路由 / 请求
- Vue Router / React Router
- Axios / Fetch / TanStack Query

## AI 应用专属前端技术

### 1. 流式输出（Streaming）
```javascript
// 前端 EventSource 接收 SSE
const eventSource = new EventSource('/api/chat');
eventSource.onmessage = (event) => {
  const chunk = JSON.parse(event.data);
  appendToUI(chunk.content);
};
eventSource.onerror = () => eventSource.close();
```

```python
# 后端 FastAPI 流式
from fastapi.responses import StreamingResponse

async def stream_generator():
    async for chunk in llm.astream(prompt):
        yield f"data: {chunk}\n\n"

@app.get("/api/chat")
async def chat():
    return StreamingResponse(stream_generator(), media_type="text/event-stream")
```

### 2. Markdown / 代码高亮渲染
```javascript
// React 示例
import ReactMarkdown from 'react-markdown';
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';

<ReactMarkdown
  components={{
    code({node, inline, className, children}) {
      return (
        <SyntaxHighlighter language="python">
          {String(children)}
        </SyntaxHighlighter>
      );
    }
  }}
>
  {content}
</ReactMarkdown>
```

### 3. 对话历史管理
- 虚拟滚动（react-window / vue-virtual-scroller）
- 消息分页加载
- 会话切换

## 快速搭 Demo 的工具

| 工具 | 用途 | 仓库 |
|---|---|---|
| **Streamlit** | Python 一句话搭 Web | streamlit.io |
| **Gradio** | ML 模型展示首选 | github.com/gradio-app/gradio |
| **Chainlit** | 专为 LLM 对话设计 | github.com/Chainlit/chainlit |
| **Open WebUI** | ChatGPT 风格界面 | github.com/open-webui/open-webui |
| **Lobe Chat** | 国产、开源、多模型 | github.com/lobehub/lobe-chat |

### 推荐：Chainlit（AI 对话最专业）
```python
import chainlit as cl

@cl.on_message
async def main(message: cl.Message):
    # 流式回复
    msg = cl.Message(content="")
    await msg.send()

    async for chunk in llm.astream(message.content):
        await msg.stream_token(chunk)
    
    await msg.update()
```

## 简历项目：AI 对话应用前端

```
项目名：智言 - AI 知识助手 Web App

技术栈：Vue 3 + TypeScript + Pinia + Vite + Tailwind
亮点：
1. 流式打字机效果（SSE + EventSource）
2. Markdown + 代码高亮（marked + highlight.js）
3. 多会话管理（侧边栏切换）
4. 消息复制、点赞、重新生成
5. 暗色/亮色主题
6. 移动端响应式
7. PWA 离线访问

性能优化：
- 路由懒加载
- 虚拟滚动处理长对话
- IndexedDB 缓存历史
- 首屏 < 1.5s
```

## 面试常问的前端题

1. **流式输出怎么实现？SSE 和 WebSocket 区别？**
2. **大列表卡顿怎么优化？** - 虚拟滚动、分页
3. **前后端鉴权怎么做？** - JWT、Cookie、OAuth
4. **如何做错误边界？** - try/catch、ErrorBoundary、全局拦截器
