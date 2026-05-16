# 07 - Streamlit UI for LangGraph Chatbot

This folder contains a simple web UI for a LangGraph chatbot using Streamlit.

The goal of this module is to take the LangGraph chatbot built earlier and make it accessible through a browser-based chat interface.

## Files

### langgraph_backend.py

This file contains the LangGraph chatbot backend.

It includes:

- chatbot state
- message-based state field
- chat node
- LLM call
- graph creation
- START and END edges
- checkpointer
- compiled chatbot object

Workflow:

START → chat_node → END

The backend handles the chatbot logic and conversation memory.

### streamlit_frontend.py

This file contains the Streamlit frontend.

It includes:

- chat message display
- chat input box
- Streamlit session state
- user message handling
- LangGraph chatbot invocation
- assistant response display

The frontend is responsible for the user interface.

## How the App Works

The user types a message in the Streamlit chat input box.

The frontend converts the user message into a HumanMessage and sends it to the LangGraph backend.

The LangGraph chatbot processes the message using the LLM and returns the updated state.

The frontend extracts the latest AI message from the returned state and displays it in the UI.

Flow:

User → Streamlit UI → LangGraph chatbot → LLM → LangGraph chatbot → Streamlit UI

## Streamlit Components Used

### st.chat_message

Used to display user and assistant messages in chat format.

Roles used:

- user
- assistant

### st.chat_input

Used to create the input box where the user types a message.

### st.session_state

Used to store UI message history across Streamlit reruns.

This prevents old messages from disappearing when the user sends a new message.

## LangGraph Concepts Used

The backend uses the same LangGraph chatbot pattern from the previous module.

Concepts used:

- StateGraph
- message-based state
- chat node
- add_messages reducer
- checkpointer
- thread_id
- invoke

## Why session_state is Needed

Streamlit reruns the script from top to bottom whenever the user sends a new message.

If a normal Python list is used to store messages, it gets reset on every rerun.

To avoid this, the frontend stores UI messages in st.session_state.

This keeps the chat history visible in the browser.

## Why thread_id is Needed

The LangGraph backend uses a checkpointer for memory.

When invoking the chatbot, the frontend passes a config with thread_id.

The thread_id tells LangGraph which conversation state to continue.

If the same thread_id is used, the chatbot remembers previous messages.

If a new thread_id is used, a new conversation starts.

## Important Difference

st.session_state and LangGraph persistence are different.

st.session_state is used for the frontend UI history.

LangGraph persistence is used for backend chatbot memory.

Both are needed in this app.

## Example Conversation

User: Hello  
Assistant: Hello, how can I help you?

User: My name is Saurabh  
Assistant: Nice to meet you, Saurabh.

User: What is my name?  
Assistant: Your name is Saurabh.

## How to Run

Install dependencies:

pip install streamlit langgraph langchain langchain-openai python-dotenv

Run the Streamlit app:

streamlit run streamlit_frontend.py

## Folder Summary

This folder demonstrates how to connect a LangGraph chatbot backend with a Streamlit frontend.

The main idea is:

Streamlit handles the UI.  
LangGraph handles the chatbot workflow and memory.

This is the foundation for building more practical chatbot applications with RAG, tools, streaming, persistence, and human-in-the-loop features.