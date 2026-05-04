# 04 - Iterative Workflows in LangGraph

This folder contains a LangGraph example that demonstrates an iterative workflow.

An iterative workflow is a workflow where some steps repeat until a stopping condition is met.

Example:

START → Generate → Evaluate → Optimize → Evaluate → END

The goal of this folder is to show how LangGraph can create loops using edges and conditional routing.

## File

### tweet_optimizer_workflow.ipynb

This notebook builds a tweet generation and optimization workflow.

The workflow takes a topic as input and generates a short, funny, original tweet for that topic.

The tweet is then evaluated by another LLM. If the tweet is approved, the workflow ends. If the tweet needs improvement, it is sent to an optimizer node. The optimizer improves the tweet using evaluator feedback and sends it back for evaluation.

Workflow:

START → generate → evaluate → END

If improvement is needed:

evaluate → optimize → evaluate

## Workflow Components

### generate

Generates the first tweet from the input topic.

The generated tweet is stored in state.

### evaluate

Evaluates the tweet based on criteria such as originality, humor, viral potential, tweet format, and length.

The evaluator returns:

- evaluation result
- feedback

The evaluation result can be:

- approved
- needs_improvement

### optimize

Improves the tweet using the feedback from the evaluator.

The optimized tweet is stored back into state and sent again for evaluation.

## State Fields

The workflow state contains:

- topic
- tweet
- evaluation
- feedback
- iteration
- max_iteration
- tweet_history
- feedback_history

## Why max_iteration is used

The workflow can loop multiple times between the evaluator and optimizer.

To avoid an infinite loop, a maximum iteration limit is used.

The workflow stops when:

- the tweet is approved
- or the maximum iteration limit is reached

## History Tracking

The workflow also stores tweet history and feedback history.

This helps track how the tweet changed during each iteration.

Example:

tweet_history:

- first generated tweet
- optimized tweet
- final approved tweet

feedback_history:

- feedback from first evaluation
- feedback from second evaluation

## Concepts Covered

This folder demonstrates the following LangGraph concepts:

- iterative workflows
- loops in graph execution
- conditional routing
- evaluator-optimizer pattern
- structured output
- state updates
- reducers for history tracking
- max iteration stopping condition

## Folder Summary

This workflow shows how LangGraph can be used when the first LLM output is not final.

Instead of accepting the first generated tweet, the workflow evaluates it, improves it, and repeats the process until the tweet is good enough or the loop limit is reached.

This pattern is useful for many agentic AI systems, such as:

- content generation
- answer refinement
- code improvement
- email drafting
- resume bullet improvement
- RAG answer validation

## Key Idea

The main idea of this folder is:

Generate → Evaluate → Improve → Evaluate again

LangGraph makes this possible by connecting nodes in a loop and using a routing function to decide whether to continue or stop.