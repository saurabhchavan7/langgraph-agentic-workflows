# 10 - SQLite Persistence for LangGraph Chatbot

This folder contains a Streamlit + LangGraph chatbot with database-backed persistence using SQLite.

The goal of this module is to store chatbot conversations permanently so previous chats can be restored even after the app is restarted.

## Files

### langgraph_database_backend.py

This file contains the LangGraph chatbot backend with SQLite persistence.

It includes:

- chatbot state
- message-based state
- chat node
- LLM call
- SQLite database connection
- SqliteSaver checkpointer
- compiled chatbot graph
- helper function to retrieve all existing chat threads

The backend workflow is:

START → chat_node → END

### streamlit_frontend_database.py

This file contains the Streamlit frontend connected to the SQLite-backed LangGraph chatbot.

It includes:

- sidebar UI
- New Chat button
- previous chat thread list
- active thread management
- resume chat functionality
- streaming response display
- loading existing threads from the database

## Problem Solved

Before this module, the chatbot used in-memory persistence.

That meant conversations were lost when the app stopped or refreshed.

This module replaces in-memory persistence with SQLite persistence.

Old setup:

LangGraph chatbot → InMemorySaver → RAM

New setup:

LangGraph chatbot → SqliteSaver → chatbot.db

## SQLite Database

The chatbot stores checkpoints in a local SQLite database file.

Database file:

chatbot.db

If this file does not exist, it is created automatically.

The database stores LangGraph checkpoints for each conversation thread.

## Checkpointer

The backend uses SqliteSaver as the LangGraph checkpointer.

The checkpointer saves graph state after execution steps.

This allows the chatbot to restore old conversations using thread IDs.

## Thread ID

Each conversation has its own thread ID.

Example:

thread_1 → Saurabh conversation  
thread_2 → AI conversation  
thread_3 → Pizza recipe conversation

When the same thread ID is used again, LangGraph loads the previous conversation from the SQLite database.

## Backend Changes

The backend changes include:

- replacing InMemorySaver with SqliteSaver
- creating a SQLite connection
- saving checkpoints to chatbot.db
- retrieving existing threads from the database
- keeping conversations separated by thread ID

## Frontend Changes

The frontend changes include:

- importing the SQLite-backed chatbot backend
- loading existing thread IDs from the database
- initializing the sidebar with saved conversations
- continuing to support new chat and resume chat features

## retrieve_all_threads

The backend includes a helper function that retrieves all unique thread IDs from the database.

This is needed because the database stores multiple checkpoints for the same thread.

The helper function:

1. lists all checkpoints
2. extracts thread IDs
3. removes duplicate thread IDs
4. returns the unique thread IDs as a list

The frontend uses this list to populate the sidebar.

## Why Duplicates Exist

A single conversation can create multiple checkpoints.

For example:

START → chat_node → END

This can create multiple saved checkpoint rows.

So the same thread ID may appear many times in the database.

Only unique thread IDs should be shown in the sidebar.

## Streamlit Session State

Streamlit session state is still used for frontend state.

It stores:

- current active thread ID
- visible message history
- list of chat threads for the sidebar

SQLite stores backend conversation memory.

Streamlit session state manages what is shown in the current browser session.

## Example Behavior

User starts a chat:

User: Hi, my name is Saurabh  
Assistant: Hello Saurabh.

User closes the app.

User opens the app again.

The old thread still appears in the sidebar.

User clicks the old thread and asks:

User: What is my name?  
Assistant: Your name is Saurabh.

## Current Limitation

SQLite is good for local demos and small projects.

For production systems, a stronger database such as PostgreSQL should be used.

A production version should also include:

- user authentication
- user-specific thread ownership
- durable database deployment
- cleanup or retention strategy
- better chat titles instead of raw thread IDs

## Concepts Covered

This folder demonstrates:

- database-backed persistence
- SQLite checkpointer
- SqliteSaver
- chatbot.db storage
- durable chat history
- thread-based conversation storage
- retrieving unique thread IDs
- restoring old conversations after restart
- Streamlit frontend integration with database-backed LangGraph

## Folder Summary

This module upgrades the LangGraph chatbot from temporary in-memory memory to durable database-backed memory.

The main idea is:

Use SqliteSaver to save LangGraph checkpoints in chatbot.db.  
Use thread IDs to separate conversations.  
Load saved thread IDs into the Streamlit sidebar.  
Allow users to resume old chats even after restarting the app.