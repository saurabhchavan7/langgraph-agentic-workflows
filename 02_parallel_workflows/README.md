# 02 - Parallel Workflows in LangGraph

This folder contains LangGraph examples where multiple independent tasks run in parallel and their outputs are merged later.

## Files

### batsman_workflow.ipynb

A non-LLM parallel workflow that calculates cricket batting metrics such as strike rate, boundary percentage, and balls per boundary.

Workflow:

START → calculate_strike_rate / calculate_boundary_percentage / calculate_balls_per_boundary → summary → END

### essay_workflow.ipynb

An LLM-based parallel workflow that evaluates an essay from three perspectives: language quality, depth of analysis, and clarity of thought.

Workflow:

START → evaluate_language / evaluate_analysis / evaluate_thought → final_evaluation → END

This example also uses structured output and reducers.

## Concepts Covered

- Parallel node execution
- Partial state updates
- Reducers for merging parallel outputs
- Structured LLM output
- Final aggregation node