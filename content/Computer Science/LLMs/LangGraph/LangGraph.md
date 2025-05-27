---
aliases:
  - Lang graph
  - lang graph
  - Langgraph
tags:
  - LLM
---
# What is LangGraph?
LangGraph is an orchestration framework for complex agentic systems and is more low-level and controllable than LangChain agents. On the other hand, LangChain provides a standard interface to interact with models and other components, useful for straight-forward chains and retrieval flows.

# Why use LangGraph?
Other agentic frameworks can work for simple, generic tasks but fall short for complex tasks bespoke to a company’s needs. LangGraph provides a more expressive framework to handle companies’ unique tasks without restricting users to a single black-box cognitive architecture.


# Basic Principles

## States
States represent the current data available at any point in the workflow. It is a shared memory bank for all nodes in the graph. It is used to hold intermediate values as your graph executes node-by-node. 

## Nodes
Nodes execute based on the instructions given to them when they are instantiated. 

### Linking Nodes and Functions
You can bind the nodes to different functions using `llm.bindtools([*function name*])`, where the `llm` object is an instantiation of the LLM API. For example:

``` python
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="gpt-4o")

def multiply(a: int, b: int) -> int:
    """Multiply a and b.

    Args:
        a: first int
        b: second int
    """
    return a * b

llm_with_tools = llm.bind_tools([multiply])

```

## Edges
Edges determine the flow of the application. 
