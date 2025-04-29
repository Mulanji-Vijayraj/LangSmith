# LangSmith - Short Notes

LangSmith is a developer platform designed to help you observe, debug, test, and evaluate applications built with large language models (LLMs). It provides tools for tracing, monitoring, and fine-tuning production-grade LLM applications.

---

## Overview

LangSmith aids users in:

- Tracking LLM chains, agents, and tools.
- Visualizing inputs, outputs, and internal reasoning steps.
- Creating datasets and evaluating prompts over multiple inputs.

It integrates seamlessly with the LangChain framework but can also be used independently via API.

---

## Packages and Classes

Below are the core packages and classes used when working with LangSmith, along with their basic functions and syntax:

### `langsmith.client`

- `Client` – Interface to interact with the LangSmith API.

```python
  from langsmith.client import Client
  client = Client()
```

---

### `langsmith.tracing`

- `traceable` –  Decorator to auto-trace functions for debugging and analytics.

```python
from langsmith.tracing import traceable

@traceable(name="FunctionName")
def your_function():
    pass
```

---

### `langsmith.evaluation`
- `RunEvaluator` – Helps auto-score and rank outputs using criteria.
- `evaluate` – Evaluates runs using specified evaluators.

```python
from langsmith.evaluation import RunEvaluator, evaluate
```

---

## Basic Example
```python
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain
from langsmith.tracing import traceable

llm = OpenAI(temperature=0.7)

prompt = PromptTemplate(
    input_variables=["topic"],
    template="Write a short story about {topic}."
)

chain = LLMChain(llm=llm, prompt=prompt)

@traceable(name="ShortStoryChain")
def generate_story(topic):
    return chain.run(topic)

# Example call
print(generate_story("a lonely robot on Mars"))
```

This trace will be visible in your LangSmith dashboard.

---

##  Creating and Using a Dataset
- LangSmith supports creating datasets to test prompts over multiple inputs

```python
langsmith dataset create "my-dataset" --description "Test stories"
langsmith dataset add "my-dataset" --input '{"topic": "a friendly dragon"}'
langsmith dataset add "my-dataset" --input '{"topic": "a haunted forest"}'
```

## Evaluation and Scoring

```python
from langsmith.evaluation import RunEvaluator, evaluate

# Use built-in evaluators like "criteria" or "embedding_distance"
results = evaluate(
    run_ids=["run_id_1", "run_id_2"],
    evaluators=["criteria"],
    criteria={"helpfulness": "How helpful is the output?"}
)
```

---

# Thank you for reading!
