# 05 - Chatbot with Memory in LangGraph

This folder contains a LangGraph chatbot example.

The goal of this module is to build a simple chatbot first and then add memory so the chatbot can remember previous messages in the same conversation.

## File

### basic_chatbot.ipynb

This notebook builds a basic LangGraph chatbot using a single chat node.

The chatbot starts as a simple one-turn workflow:

START → chat_node → END

The user sends a message, the chat node sends it to the LLM, and the LLM response is returned.

## Workflow

The chatbot workflow has one main node:

### chat_node

The chat node reads the current message history from state, sends it to the LLM, receives the AI response, and appends that response back into state.

Basic workflow:

START → chat_node → END

## State

The chatbot state contains one main field:

- messages

The `messages` field stores the conversation history between the user and the AI.

Messages are stored as LangChain message objects instead of plain strings.

Examples of message types:

- HumanMessage
- AIMessage
- SystemMessage
- ToolMessage

## Message Reducer

The chatbot uses `add_messages` as a reducer for the `messages` field.

This is needed because chatbot messages should be appended to the existing conversation history instead of replacing the previous messages.

Without this reducer, the latest message would replace the old messages.

## First Version: Basic Chatbot

The first version of the chatbot can answer a single user message.

Example:

User: What is the capital of India?  
AI: The capital of India is New Delhi.

This confirms that the graph structure works.

## Console Chat Loop

A Python loop is added to make the chatbot feel interactive.

The loop keeps asking for user input until the user types:

- exit
- quit
- bye

For each user message, the chatbot graph is invoked and the latest AI response is printed.

## Problem Without Persistence

The chatbot initially forgets previous messages.

Example:

User: Hi, my name is Nitish  
AI: Hello Nitish.

User: What is my name?  
AI: I do not know your name.

This happens because every call to `invoke()` starts a new graph execution with fresh state.

The previous conversation is not automatically available in the next invocation.

## Adding Memory with Persistence

To solve this, the notebook adds LangGraph persistence using a checkpointer.

The checkpointer used in this notebook is:

- MemorySaver

MemorySaver stores the graph state in RAM.

The graph is compiled with the checkpointer so LangGraph can save and reload the conversation state.

## Thread ID

A thread ID is used to identify one conversation.

Example:

thread_id = "1"

When the chatbot is invoked with the same thread ID, LangGraph loads the previous state for that conversation and appends the new message.

This allows the chatbot to remember earlier messages.

## Chatbot After Adding Memory

After adding persistence, the chatbot can answer follow-up questions correctly.

Example:

User: Hi, my name is Nitish  
AI: Hello Nitish.

User: What is my name?  
AI: Your name is Nitish.

Another example:

User: Add 10 to 100  
AI: 110

User: Multiply the result by 2  
AI: 220

## Important Note

MemorySaver stores state only while the program is running.

If the notebook kernel or Python process restarts, the saved memory is lost.

For production systems, a database-backed checkpointer should be used instead of in-memory storage.

## Concepts Covered

This folder demonstrates the following LangGraph concepts:

- chatbot workflow
- chat node
- message-based state
- HumanMessage and AIMessage
- BaseMessage
- add_messages reducer
- console-based chat loop
- persistence
- checkpointer
- MemorySaver
- thread_id
- retrieving saved state
- short-term conversation memory

## Folder Summary

This module shows how to build a simple LangGraph chatbot and then add memory using persistence.

The main idea is:

User message → chat node → AI response → save state → next user message uses previous state

This is the foundation for more advanced chatbot features such as RAG, tools, UI, LangSmith observability, human-in-the-loop, and long-term memory.