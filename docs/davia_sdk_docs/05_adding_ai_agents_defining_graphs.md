# Adding AI Agents - Davia Starter Kit

Use @app.graph to connect LangGraph-based agents with real-time UI in Davia.

### See it in action

Here’s a quick demo:

Davia supports **LangGraph-style AI agents** through the `@app.graph` decorator.

A **graph** represents an AI workflow — a sequence of steps connected through logic and memory (called **state**). It’s useful for building advanced agents like chatbots, assistants, or tool-using flows.

Davia connects directly to the graph you define and builds a UI so users can interact with it — with real-time inputs, state handling, and streaming output.

---

## 1. What is a graph in AI?

A graph is an orchestration framework for building controllable AI agents. It enables complex workflows with customizable architectures, long-term memory, and human-in-the-loop capabilities to handle sophisticated tasks reliably.

### How Davia works with graphs

Davia seamlessly integrates with LangGraph-style AI agents through the `@app.graph` decorator. When you expose a graph to Davia:

*   It reads your graph’s structure and state
*   Builds an interactive UI that matches your workflow
*   Handles real-time inputs and streaming outputs
*   Manages state transitions automatically

If your agent is conversational, **Davia can generate a chat-style interface** to let users interact with it naturally.

---

## 2. Full example: A minimal graph

Here’s a full example of a simple one-turn chatbot using LangGraph and Davia.

First, install Davia with LangGraph support (requires Python 3.11+):

```bash
pip install davia langgraph "langgraph-api==0.0.38"
```

Then create your agent:

```python
from davia import Davia
from langgraph.graph import StateGraph, START, END, MessagesState

# Define a simple node
def start(state: MessagesState) -> MessagesState:
    return {"messages": [{"role": "ai", "content": "Hello, how can I help you today?"}]}

# Create and expose the graph
app = Davia()

@app.graph
def hello_graph():
    """
    A minimal LangGraph agent that returns a greeting.
    """
    graph = StateGraph(MessagesState)
    graph.add_node("start", start)
    graph.add_edge(START, "start")
    graph.add_edge("start", END)
    return graph

#  Launch the server
if __name__ == "__main__":
    app.run()
```

When you run this file, Davia opens the editor at:

🔗 <https://dev.davia.ai/dashboard>

You’ll see a simple interface where:

*   Users can submit a message
*   The graph runs and returns a greeting
*   Responses are streamed back to the UI in real time

---

## 3. How Davia integrates with LangGraph

When you expose a LangGraph agent:

*   Davia reads the **state class** (e.g. `MessagesState`) and builds an entire UI based on it,
*   It connects to the graph’s lifecycle: `START → node(s) → END`
*   It can **stream the output** live as each node in the graph runs
*   The entire UI is automatically adapted to your graph’s logic

---

## 4. Best practices

*   ✅ Always return a Graph from your decorated function
*   ✅ Add a helpful docstring to describe your agent
*   ❌ Don’t return the graph directly at the top level — it must be wrapped in a function

---
*Navigation links from original page (for reference, may not be active locally):*
*   [Defining Tasks](https://docs.davia.ai/develop/defining-tasks)
*   [Debugging Tasks](https://docs.davia.ai/develop/debug)
