# 12 - Tools in LangGraph Chatbot

This folder contains a Streamlit + LangGraph chatbot with tool-calling support.

The goal of this module is to allow the chatbot to perform actions using external tools instead of only generating normal LLM responses.

## Files

### langgraph_tool_backend.py

This file contains the LangGraph chatbot backend with tools.

It includes:

- chatbot state
- chat node
- LLM with tools
- calculator tool
- DuckDuckGo search tool
- stock price tool
- ToolNode
- tools_condition
- SQLite checkpointer
- compiled LangGraph chatbot
- helper function to retrieve saved chat threads

### streamlit_frontend_tools.py

This file contains the Streamlit frontend for the tool-enabled chatbot.

It includes:

- chat UI
- streaming assistant response
- sidebar chat threads
- resume chat functionality
- tool-aware streaming
- filtering tool messages from final UI output
- optional status display when tools are used

## Problem Solved

Before this module, the chatbot could only respond using the LLM.

It could not perform external actions such as:

- exact calculations
- internet search
- stock price lookup
- multi-step tool workflows

This module adds tool-calling support so the chatbot can decide when a tool is needed and use the correct tool.

## Tools Added

### Calculator Tool

Used for numerical calculations.

Example:

User: What is the product of 654 and 713?

The chatbot uses the calculator tool and returns the result.

### DuckDuckGo Search Tool

Used for internet search.

Example:

User: What are the top news stories in India today?

The chatbot uses the search tool to fetch recent information.

### Stock Price Tool

Used to fetch stock price information.

Example:

User: What is the current stock price of Tesla?

The chatbot uses the stock price tool and returns the result.

## Main LangGraph Concepts

### ToolNode

ToolNode is a prebuilt LangGraph node used to execute tools.

Instead of creating one node for each tool, all tools are placed inside ToolNode.

ToolNode receives the tool call from the LLM, executes the correct tool, and returns the tool output.

### tools_condition

tools_condition is a prebuilt conditional routing function.

It checks whether the latest AI response contains a tool call.

If a tool call exists, execution goes to ToolNode.

If no tool call exists, the graph ends normally.

### bind_tools

The LLM must be bound with the available tools.

This allows the model to decide when to call a tool and what arguments to pass.

## Workflow

The final workflow is:

START → chat_node → tools or END  
tools → chat_node

This creates a loop between the LLM and the tools.

## Why tools → chat_node is Needed

The graph should not end immediately after a tool runs.

Tool output may be raw, technical, or incomplete.

By sending tool output back to the chat node, the LLM can:

- read the tool result
- generate a clean final answer
- decide whether another tool is needed
- handle multi-step tool workflows

Example:

User: What is the stock price of Apple and how much would 50 shares cost?

Flow:

1. LLM calls stock price tool
2. Tool returns stock price
3. LLM calls calculator tool
4. Calculator returns total cost
5. LLM gives final answer to the user

## Frontend Change

After adding tools, the backend may stream different message types:

- AIMessage
- ToolMessage

The frontend should display only user-facing AI messages.

Tool messages are intermediate outputs and should not be directly shown to the user.

This avoids showing raw JSON or technical tool output.

## Tool Status Display

The frontend can show a status message when a tool is being used.

Example:

- Using calculator
- Using DuckDuckGo search
- Using stock price tool

This gives better user experience because the user can see what the chatbot is doing.

## Current Chatbot Features

At this stage, the chatbot supports:

- Streamlit UI
- streaming responses
- multiple chat threads
- resume chat
- SQLite persistence
- LangSmith observability
- tool calling
- calculator
- web search
- stock price lookup

## Concepts Covered

This folder demonstrates:

- tool calling in LangGraph
- prebuilt tools
- custom tools
- ToolNode
- tools_condition
- LLM tool binding
- tool execution loop
- multi-step tool use
- filtering ToolMessage from UI
- Streamlit status updates for tool execution

## Folder Summary

This module upgrades the chatbot from a normal LLM chatbot to a tool-enabled chatbot.

The main idea is:

User asks a question.  
The LLM decides whether a tool is needed.  
If needed, ToolNode executes the tool.  
The tool output goes back to the LLM.  
The LLM returns a clean final answer.

This is an important step toward building agentic AI systems with LangGraph.