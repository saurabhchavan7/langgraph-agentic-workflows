# 03 - Conditional Workflows in LangGraph

This folder contains LangGraph examples where the workflow path changes based on a condition.

A conditional workflow means the graph has multiple possible branches, but only one branch is selected during execution.

Example:

START → Check condition → Branch A or Branch B → END

Conditional workflows are useful when the next step depends on the current state, similar to `if-else` logic in normal programming.

## Files

### quadratic_equation_workflow.ipynb

This notebook contains a non-LLM conditional workflow.

It solves a quadratic equation using the values of `a`, `b`, and `c`.

The workflow first calculates the discriminant:

D = b² - 4ac

Then it selects one of three branches:

- If D > 0, calculate two real roots
- If D == 0, calculate one repeated root
- If D < 0, return no real roots

Workflow:

START → show_equation → calculate_discriminant → real_roots / repeated_roots / no_real_roots → END

Main purpose:

- Demonstrates basic conditional routing
- Uses a routing function to choose the next node
- Shows how one workflow can have multiple possible paths
- Provides a simple non-LLM example of conditional edges

### customer_review_workflow.ipynb

This notebook contains an LLM-based conditional workflow.

It takes a customer review as input and generates an appropriate response based on the sentiment of the review.

The workflow first detects whether the review sentiment is positive or negative.

If the review is positive, it directly generates a warm thank-you response.

If the review is negative, it first diagnoses the issue and then generates a more specific support response.

Workflow:

START → find_sentiment → positive_response / run_diagnosis → negative_response → END

For negative reviews, the diagnosis step extracts:

- issue type
- customer tone
- urgency level

Main purpose:

- Demonstrates conditional routing with LLM output
- Uses structured output for reliable sentiment classification
- Routes positive and negative reviews to different paths
- Uses diagnosis before generating a support response for negative feedback
- Shows how LangGraph can support customer support automation workflows

## Concepts Covered

This folder demonstrates the following LangGraph concepts:

- Conditional workflows
- Routing functions
- Conditional edges
- State-based decision making
- LLM-based routing
- Structured output
- Pydantic schema usage
- Positive and negative branch handling
- Multi-step response generation

## Folder Summary

The first notebook shows conditional routing using a simple math example.

The second notebook applies the same idea to an LLM workflow for customer review handling.

Together, these examples show how LangGraph can route execution based on state values.

The core pattern used in this folder is:

Define state → Run first node → Check condition → Route to one branch → Continue workflow → END

## When to Use Conditional Workflows

Use conditional workflows when only one path should run based on a decision.

Examples:

- Route customer queries based on intent
- Send positive and negative reviews to different response flows
- Choose whether to retrieve more documents in a RAG workflow
- Decide whether an answer should be accepted or regenerated
- Route a user request to billing, technical support, refund, or sales

## Key Takeaway

Conditional workflows are one of the most important LangGraph patterns because real agentic AI systems often need decision-making.

Instead of running every step, the graph checks the current state and chooses the correct next step.