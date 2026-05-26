# 09 - Resume Chat Threads in LangGraph Chatbot

This folder contains a Streamlit + LangGraph chatbot with a resume chat feature.

The goal of this module is to support multiple chat conversations and allow the user to switch between them from the sidebar.

## Files

### langgraph_backend.py

This file contains the LangGraph chatbot backend.

It includes:

- chatbot state
- message-based state
- chat node
- LLM call
- checkpointer
- compiled chatbot graph

The backend workflow is:

START → chat_node → END

The backend already supports memory through LangGraph persistence and thread IDs.

### streamlit_frontend_threading.py

This file contains the Streamlit frontend with multiple chat thread support.

It includes:

- sidebar UI
- New Chat button
- dynamic thread ID generation
- active thread management
- chat thread list
- resume chat functionality
- message history loading
- Streamlit session state
- LangGraph chatbot streaming

## Problem Solved

Before this module, the chatbot supported only one active conversation.

This module adds the ability to:

- start a new chat
- keep old chat threads
- display previous conversations in the sidebar
- click a previous conversation
- resume that conversation with its old memory

## Main UI Layout

The app has two main sections:

Main chat area:

- displays the current conversation
- accepts user input
- streams assistant response

Sidebar:

- shows the app title
- has a New Chat button
- lists previous conversations

## How New Chat Works

When the user clicks New Chat:

1. A new thread ID is generated
2. The new thread ID becomes the active thread
3. The UI message history is cleared
4. The new thread is added to the sidebar list

This creates a fresh conversation.

## How Resume Chat Works

When the user clicks an old thread from the sidebar:

1. The selected thread ID becomes the active thread
2. The app calls `chatbot.get_state()` using that thread ID
3. LangGraph returns the saved conversation state
4. Messages are extracted from the state
5. Messages are converted into Streamlit UI format
6. The old conversation is displayed again

## Thread ID

Each conversation has its own thread ID.

Example:

- thread_id_1 → first conversation
- thread_id_2 → second conversation
- thread_id_3 → third conversation

LangGraph uses the thread ID to store and retrieve the correct conversation state.

## Streamlit Session State

The frontend uses `st.session_state` to store:

- current active thread ID
- UI message history
- list of all chat threads

This is needed because Streamlit reruns the script whenever the user interacts with the app.

## Message Format Conversion

LangGraph stores messages as LangChain message objects such as:

- HumanMessage
- AIMessage

Streamlit displays messages using dictionaries like:

- role: user
- role: assistant
- content: message text

So when an old conversation is loaded, messages are converted from LangChain format to Streamlit UI format.

## Current Limitation

This version uses an in-memory checkpointer.

That means conversations are available only while the app session is running.

If the server or app restarts, old conversations may be lost.

The next improvement is to connect the LangGraph backend to a database-backed checkpointer.

## Possible Improvement

Currently, the sidebar can show raw thread IDs.

A better version would show generated chat titles instead.

Example:

Instead of:

`a43f9e2c-...`

Show:

`Factorial Python code`

This can be done by generating a title from the first user message and storing it with the thread ID.

## Concepts Covered

This folder demonstrates:

- multiple chat threads
- resume chat functionality
- dynamic thread ID generation
- UUID usage
- sidebar conversation list
- active thread switching
- LangGraph `get_state()`
- Streamlit session state
- message history conversion
- thread-based chatbot memory

## Folder Summary

This module upgrades the Streamlit LangGraph chatbot with multiple conversation support.

The main idea is:

Each chat conversation gets its own thread ID.  
LangGraph stores memory under that thread ID.  
Streamlit keeps track of available threads in the sidebar.  
Clicking a thread loads that conversation back into the UI.

This makes the chatbot behave more like a real ChatGPT-style application with multiple resumable conversations.