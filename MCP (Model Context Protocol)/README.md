# 🔌 MCP (Model Context Protocol)

> Notes from a live, cohort-based Agentic AI course's MCP deep-dive series (instructor: **Mayank Aggarwal**), covering why MCP exists, how to build with it, and how to host and productionize it.

## 📑 Contents

| # | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|---|---|:-:|
| 1 | 🧠 [Why MCP Exists & Core Architecture](<01. Why MCP Exists & Core Architecture.md>) | LLMs as black boxes · function calling's scaling limits · MCP's official definition · host-client-server architecture | ✅ |
| 2 | 🔗 [MCP Client, Primitives, Lifecycle & JSON-RPC](<02. MCP Client, Primitives, Lifecycle & JSON-RPC.md>) | Client's role · the three primitives (tools, resources, prompts) · init/operation/shutdown lifecycle · why JSON-RPC over plain REST | ✅ |
| 3 | 🔍 [Capability Negotiation & Operation](<03. Capability Negotiation & Operation.md>) | Capability negotiation · elicitation · discovery & calling (operation phase) · privacy/information leakage & middleware guardrails · a full JSON-RPC trace · a no-code AI-newsletter project via Claude Desktop connectors | ✅ |
| 4 | 🔌 [Transport Layer, Shutdown Phase & Building Real Servers](<04. Transport Layer, Shutdown Phase & Building Real Servers.md>) | STDIO vs. streamable HTTP transport · shutdown phase (client- and server-initiated) · legacy SDK vs. FastMCP · MCP Inspector · resources & prompts · connecting to Claude Desktop | ✅ |
| 5 | 🏗 [Real Integrations, a Complete Real-World Project & System Design](<05. Real Integrations, a Complete Real-World Project & System Design.md>) | Connecting servers via connectors & config files · third-party community servers · a real FastAPI "time tracker" + MCP server built from scratch · Docker, sampling, async & scaling Q&A | ✅ |
| 6 | ☁️ [Hosting, Productionizing & Clients](<06. Hosting, Productionizing & Clients.md>) | HTTP transport & live debugging · JSON-RPC handshake via curl · deploying on Prefect Horizon & Vercel (paid-tier auth wall + workaround) · DB reinitialization flaw · building an MCP client from scratch · "AI never calls the tool directly" · tool granularity, guardrails & multi-tenancy | ✅ |

## 🔗 Reference

- [MCP Lifecycle Visualizer](https://modelcontextprotocol-lifecycle.netlify.app/) — interactive walkthrough of the MCP initialization/operation/shutdown lifecycle.

---


