---
name: langchain_oss_python
description: LangChain OSS Python 文档综合技能：覆盖 LangChain、LangGraph、DeepAgents、Integrations 以及 MCP 相关文档，用于检索 API、概念、示例与最佳实践。
---

# LangChain OSS Python 综合技能

基于 LangChain 官方文档（`docs.langchain.com`）的 `llms.txt` 产物构建，覆盖以下主题：LangChain、LangGraph、DeepAgents、Integrations、MCP。

## When to Use This Skill（适用场景/触发条件）

- 你在写/改 LangChain（LCEL、Runnables、Prompts、Tools、Retrievers、Memory 等）代码
- 你在用 LangGraph 构建有状态/多节点/工具调用的 Agent 流程
- 你在用 DeepAgents 做多代理协作、路由、技能/记忆编排
- 你在查某个 Integrations（模型/向量库/检索器/工具等）的安装与配置方式
- 你在对接 MCP（协议概念、Server/Client、在 LangChain 中如何集成）

## 如何高效使用

1. 先告诉我：你用的是 `langchain` / `langgraph` / `deepagents` / `integrations` / `mcp` 里的哪块 + 你的目标（例如“多步推理 + 工具调用 + 记忆”）。
2. 我会优先从 `references/index.md` 和 `references/llms-full.md` 定位到相关章节，再给你可执行步骤、关键 API、注意事项与示例代码。

## 示例提问（推荐直接复制改写）

- “用 LangGraph 写一个带工具调用的多步 Agent：如何定义 State、节点、边、并运行？”
- “LangChain 的 LCEL（Runnable 管道）怎么把 `prompt + model + parser` 串起来并支持流式输出？”
- “DeepAgents 的技能路由（router）怎么做？如何把不同工具/子代理组合到一个系统？”
- “Integrations 里某个 Provider（OpenAI/Anthropic/Azure 等）如何配置环境变量与客户端？”
- “MCP 是什么？如何在 LangChain 中把 MCP server 暴露的能力当成工具使用？”

## 代码骨架（最小可运行形态）

以下示例是“最小骨架”，具体类名/参数以 `references/` 中官方文档为准。

### LangChain：LCEL 串联 Prompt → Model

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_openai import ChatOpenAI

prompt = ChatPromptTemplate.from_messages(
    [("system", "你是一个严谨的助手"), ("human", "{input}")]
)
model = ChatOpenAI(model="gpt-4o-mini")

chain = prompt | model
result = chain.invoke({"input": "解释一下 LangChain 的 Runnable 是什么？"})
print(result)
```

### LangGraph：StateGraph 最小图

```python
from typing import TypedDict
from langgraph.graph import StateGraph, END

class State(TypedDict):
    input: str
    output: str

def echo(state: State) -> State:
    return {"output": f"你说：{state['input']}"}

graph = StateGraph(State)
graph.add_node("echo", echo)
graph.set_entry_point("echo")
graph.add_edge("echo", END)

app = graph.compile()
print(app.invoke({"input": "hello"}))
```

## 参考资料（权威来源）

- `references/index.md`：章节索引（先看它定位主题）
- `references/llms-full.md`：完整版条目（覆盖面最全）
- `references/llms.md`：精简版条目
- `references/llms-txt.md`：抓取摘要与结构说明

## 更新方式

如需刷新到最新官方文档，可在 `D:\\Skill_Seekers` 下重新运行：

```powershell
Set-Location D:\Skill_Seekers
$env:PYTHONPATH = 'D:\Skill_Seekers\src'
python -m skill_seekers.cli.main scrape --config configs\langchain_oss_python.json --async --workers 8
```
