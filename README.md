<p align = "center" draggable="false" ><img src="https://github.com/AI-Maker-Space/LLM-Dev-101/assets/37101144/d1343317-fa2f-41e1-8af1-1dbb18399719"
     width="200px"
     height="auto"/>
</p>

<h1 align="center" id="heading">Session 3: Agentic RAG with LangGraph and LangChain</h1>

### [Quicklinks]()

| 📰 Session Sheet | ⏺️ Recording | 🖼️ Slides | 👨‍💻 Repo | 📁 Feedback |
|:-----------------|:-------------|:----------|:----------|:------------|
| | | | | |

## ⚡ Lightning Session Overview

This lightning session shows how to turn a fixed RAG pipeline into an agentic one with LangChain and LangGraph.

Session 2 showed a predictable two-step RAG flow:

```text
question -> retrieve -> generate
```

This session makes retrieval agentic:

```text
question -> agent decides whether to retrieve -> optional retriever tool call -> answer
```

The point is not to add a complicated retrieval pipeline. The point is to give the agent a retrieval tool so it can retrieve when it decides retrieval is useful.

You will build that same loop two ways:

1. With LangChain `create_agent`, which gives you the agent loop quickly.
2. With LangGraph `StateGraph`, `ToolNode`, and `tools_condition`, so you can see how the loop works.

The main notebook is:

```text
01_Cat_Health_Agentic_RAG_LangGraph_LangChain.ipynb
```

The notebook uses the bundled cat health corpus:

```text
data/cat_health_guidelines.md
```

## 🛠️ Setup

From this folder, install the environment with uv:

```bash
uv sync
```

Then open the notebook in Cursor or VS Code and select the Python/Jupyter environment created by uv.

You will need an OpenAI API key available when running the notebook.

Optional LangSmith tracing:

```bash
export LANGSMITH_TRACING=true
export LANGSMITH_API_KEY="your-key"
```

---

## 🏗️ Activity #1: From RAG Pipeline to RAG Tool

Run the notebook sections that load, split, embed, and index the cat health corpus in Qdrant.

Then run the `retrieve_cat_health_guidelines` tool directly.

#### ❓Discussion Prompt

What changes when retrieval becomes a tool instead of a mandatory first step?

---

## 🏗️ Activity #2: Build the Agent with `create_agent`

Run the notebook section that creates a LangChain agent with:

1. A chat model
2. The `retrieve_cat_health_guidelines` tool
3. A system prompt that tells the agent when to retrieve

#### ❓Discussion Prompt

Why is this agentic RAG even though we did not write an explicit retrieval step before generation?

---

## 🏗️ Activity #3: Visualize and Stream the `create_agent` Agent

Render the agent graph and run the streaming examples.

Watch whether the retriever tool is called for:

- A cat urinary warning-sign question
- A cat preventive-care question
- An unrelated sports question

#### ❓Discussion Prompt

For each example, did the agent call the retrieval tool? Why or why not?

---

## 🏗️ Activity #4: Build the Same Loop with LangGraph

Run the notebook section that builds the same agent loop with:

1. `StateGraph`
2. A model node with the retriever tool bound
3. `ToolNode`
4. `tools_condition`
5. An edge from tools back to the model

#### ❓Discussion Prompt

What parts did `create_agent` hide that the explicit LangGraph version made visible?

---

## 🏗️ Activity #5: Compare and Tune Both Agents

Improve the agent by changing one or more of:

- Retrieval `k`
- Chunk size or overlap
- The retriever tool name or description
- The system prompt rules for when to retrieve
- The source citation instructions

Run at least one cat health question and one unrelated question through both the `create_agent` version and the explicit LangGraph version.

---

## 🚧 Optional Extensions

Extend the notebook in one meaningful way.

Suggestions:

- Add a second retrieval tool for another cat health document
- Add a document relevance grader after retrieval
- Add a query rewrite node when retrieval is weak
- Add a loop limit so the agent cannot retry forever
- Add LangSmith tracing tags or metadata
- Persist Qdrant locally instead of using in-memory Qdrant
