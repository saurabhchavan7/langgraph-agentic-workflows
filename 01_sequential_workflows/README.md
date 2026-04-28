# 01 - Sequential Workflows in LangGraph

This folder contains three basic LangGraph notebooks focused on sequential workflows.

A sequential workflow runs steps one after another in a fixed order.

Example:

START → Step 1 → Step 2 → END

LangGraph workflows are organized using state, nodes, and edges. State stores the workflow data, nodes perform tasks, and edges define the order in which nodes run.

## Files

### bmi_workflow.ipynb

This notebook contains a simple non-LLM LangGraph workflow.

It takes height and weight as input, calculates BMI, and assigns a BMI category.

Workflow:

START → calculate_bmi → label_bmi → END

Main purpose:

- Basic LangGraph workflow structure
- State definition
- Node creation
- Edge connection
- Graph compilation and execution
- Non-LLM sequential workflow example

### simple_llm_workflow.ipynb

This notebook contains a simple LLM-based LangGraph workflow.

It takes a user question, sends it to an LLM, and stores the generated answer in the workflow state.

Workflow:

START → llm_qa → END

Main purpose:

- LLM call inside a LangGraph node
- LangGraph and LangChain integration
- Question and answer state handling
- Basic LLM workflow structure

### prompt_chaining.ipynb

This notebook contains a prompt chaining workflow.

It takes a blog topic, generates an outline first, and then generates the final blog using that outline.

Workflow:

START → create_outline → create_blog → END

Main purpose:

- Multiple LLM calls in sequence
- Passing output from one node to the next node
- Storing intermediate output in state
- Prompt chaining workflow pattern

## Folder Summary

This folder demonstrates the basic LangGraph pattern:

Define state → Add nodes → Add edges → Compile graph → Invoke graph

The examples start with a simple non-LLM workflow, then move to a basic LLM workflow, and finally show a multi-step LLM prompt chaining workflow.

This section is the foundation for later LangGraph patterns such as parallel workflows, conditional workflows, iterative workflows, chatbots, RAG, tool calling, and memory-based agents.