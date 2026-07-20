# 13 - MCP Client in LangGraph

This folder contains LangGraph examples showing how to connect a chatbot to MCP servers.

MCP stands for Model Context Protocol. It is a standardized way to connect LLM applications with external tools, APIs, databases, and services.

The goal of this module is to replace or extend manually written tools with MCP-based tool integration.

## Files

### chatbot_async.py

This file shows the async version of the existing LangGraph tool-calling chatbot.

It converts the earlier synchronous chatbot workflow into asynchronous execution.

Main changes:

- `def` node changed to `async def`
- `invoke()` changed to `ainvoke()`
- LLM call changed to `ainvoke()`
- graph execution runs inside `asyncio.run()`

This file is used as a bridge before adding MCP because the MCP client library works asynchronously.

### chatbot_mcp.py

This file shows a simple LangGraph MCP client example.

It connects to a local MCP math server and fetches tools such as:

- add
- subtract
- multiply
- divide
- power
- modulus

The MCP tools are fetched using `MultiServerMCPClient` and then bound to the LLM.

### langgraph_mcp_backend.py

This file contains the chatbot backend with MCP support.

It includes:

- LangGraph chatbot state
- async chat node
- normal LangChain tools
- MCP client configuration
- local MCP server connection
- remote MCP server connection
- fetched MCP tools
- merged tools list
- LLM with tools
- ToolNode
- tools_condition
- SQLite persistence
- compiled LangGraph chatbot

### streamlit_frontend_mcp.py

This file contains the Streamlit frontend for the MCP-enabled chatbot.

It includes:

- chat UI
- sidebar chat threads
- resume chat functionality
- async streaming
- MCP-enabled backend integration
- tool status display

## Why MCP is Used

Before MCP, tools were written directly inside the chatbot backend.

This works for small examples, but it becomes difficult to maintain when many tools and many chatbots are involved.

Example:

If a chatbot directly calls GitHub APIs and GitHub changes its API, every chatbot using that custom tool may need code changes.

MCP solves this by moving the tool logic to an MCP server.

The chatbot only connects to the MCP server and uses the tools exposed by that server.

## Tools vs MCP

Normal tools:

- tool code lives inside chatbot project
- good for small utilities
- can become hard to maintain at scale
- every app may duplicate integration logic

MCP tools:

- tool code lives inside MCP server
- chatbot acts as MCP client
- tools are discovered from the server
- same MCP server can be reused by many apps
- better for external systems and enterprise integrations

## MCP Client and Server

In this folder:

- LangGraph chatbot acts as the MCP client
- math server acts as a local MCP server
- expense tracker acts as a remote MCP server

The client connects to servers, fetches available tools, and passes those tools to the LLM.

## Local MCP Server

The local math MCP server is connected using `stdio` transport.

This is used when the MCP server runs on the same machine.

The server exposes tools such as:

- add
- subtract
- multiply
- divide
- power
- modulus

## Remote MCP Server

The remote expense tracker MCP server is connected using HTTP transport.

It exposes tools such as:

- add expense
- list expenses
- summarize expenses

## MultiServerMCPClient

`MultiServerMCPClient` is used to connect to one or more MCP servers.

It loads tools from all configured MCP servers.

The fetched tools are then bound to the LLM.

## Workflow

The LangGraph workflow is similar to the earlier tool-calling workflow:

START → chat_node → tools or END  
tools → chat_node

The chat node decides whether a tool is needed.

If a tool is needed, execution goes to ToolNode.

ToolNode executes the selected normal tool or MCP tool.

The result goes back to the chat node.

The LLM then generates the final response or calls another tool.

## Async Requirement

The MCP client integration works asynchronously.

Because of this, the chatbot workflow is converted to async.

Important async changes:

- custom LangGraph nodes use `async def`
- LLM calls use `ainvoke`
- graph execution uses `ainvoke` or `astream`
- frontend uses async streaming

## Combining Normal Tools and MCP Tools

This module also shows that normal tools and MCP tools can be used together.

Example:

Normal tools:

- stock price tool
- search tool

MCP tools:

- math tools
- expense tracking tools

All tools are merged and bound to the LLM.

## Production Note

Streamlit is useful for demos, but it is not ideal for production MCP agents because MCP and LangGraph async workflows work better with an async backend.

A better production architecture would be:

Frontend: React or Next.js  
Backend: FastAPI  
Agent workflow: LangGraph  
External integrations: MCP servers  
Persistence: PostgreSQL or another durable database  
Observability: LangSmith

## Concepts Covered

This folder demonstrates:

- MCP basics
- tools vs MCP
- MCP client-server architecture
- async LangGraph execution
- MultiServerMCPClient
- local MCP server connection
- remote MCP server connection
- stdio transport
- HTTP transport
- fetching MCP tools
- binding MCP tools to LLM
- combining normal tools and MCP tools
- integrating MCP with existing LangGraph chatbot

## Folder Summary

This module upgrades the chatbot from manually managed tools to MCP-based tool integration.

The main idea is:

LangGraph controls the agent workflow.  
MCP servers expose external tools.  
The LangGraph chatbot connects as an MCP client.  
Tools are discovered from MCP servers and bound to the LLM.  
Tool execution still follows the LangGraph ToolNode loop.

This makes the chatbot more modular, reusable, and easier to maintain as more external systems are added.