# 11 - LangSmith Observability for LangGraph Chatbot

This folder contains a Streamlit + LangGraph chatbot integrated with LangSmith for observability.

The goal of this module is to trace chatbot execution so each user interaction can be inspected in LangSmith.

## Files

### langgraph_database_backend.py

This file contains the LangGraph chatbot backend.

It includes:

- chatbot state
- chat node
- LLM call
- SQLite checkpointer
- compiled chatbot graph
- helper function to retrieve saved chat threads

The backend continues to use SQLite-backed persistence from the previous module.

### streamlit_frontend_langsmith.py

This file contains the Streamlit frontend with LangSmith tracing enabled.

It includes:

- Streamlit chat UI
- streaming assistant responses
- sidebar chat threads
- resume chat functionality
- LangGraph chatbot calls
- LangSmith trace grouping using thread metadata

## Problem Solved

Before this module, the chatbot worked but there was no observability dashboard.

That means it was difficult to inspect:

- user inputs
- model outputs
- latency
- token usage
- internal LangGraph node execution
- individual conversation turns

This module adds LangSmith tracing so each chatbot interaction can be monitored and debugged.

## LangSmith Setup

LangSmith is enabled using environment variables in the `.env` file.

Required variables:

LANGSMITH_TRACING=true  
LANGSMITH_ENDPOINT=https://api.smith.langchain.com  
LANGSMITH_API_KEY=your_api_key_here  
LANGSMITH_PROJECT=chatbot-project

After these variables are set, chatbot runs appear in the LangSmith project dashboard.

## What LangSmith Captures

For each chatbot turn, LangSmith captures a trace.

A trace can show:

- user message
- assistant response
- LangGraph node name
- LLM model call
- start time
- end time
- latency
- time to first token
- token usage
- run status

## Project, Trace, and Thread

LangSmith organizes chatbot activity using these levels:

Project:

The top-level workspace for traces.

Trace:

One chatbot turn.

Thread:

A full multi-turn conversation made of multiple traces.

Example:

Project: chatbot-project

Thread 1:

- Trace 1: user says hi
- Trace 2: user gives name
- Trace 3: user asks follow-up

Thread 2:

- Trace 1: user asks recipe question
- Trace 2: user asks cooking time

## Thread Grouping

The chatbot already uses thread IDs for LangGraph memory.

This module also passes the same thread ID as LangSmith metadata.

This allows LangSmith to group traces by conversation.

The config includes:

- configurable.thread_id
- metadata.thread_id
- run_name

Purpose:

configurable.thread_id is used by LangGraph persistence.

metadata.thread_id is used by LangSmith to group traces.

run_name gives a cleaner name to each trace.

## Why Observability Matters

Observability is important for debugging and monitoring LLM applications.

It helps answer questions like:

- What did the user ask?
- What did the model answer?
- Which node ran?
- How long did it take?
- How many tokens were used?
- Did the response fail?
- Which conversation did this turn belong to?

## Current Chatbot Features

At this stage, the chatbot includes:

- LangGraph backend
- Streamlit UI
- streaming responses
- multiple chat threads
- SQLite persistence
- resume chat
- LangSmith observability

## Future Use

LangSmith becomes more useful as the chatbot becomes more complex.

For future modules such as tools, RAG, MCP, and agents, LangSmith can help inspect:

- tool calls
- retrieved documents
- intermediate steps
- routing decisions
- final model response

## Folder Summary

This module adds observability to the LangGraph chatbot.

The main idea is:

Enable LangSmith tracing with environment variables.  
Send each chatbot turn to LangSmith as a trace.  
Use metadata.thread_id to group traces into conversation threads.  
Use LangSmith dashboard to inspect latency, token usage, inputs, outputs, and internal execution.

This makes the chatbot easier to debug, monitor, and extend.