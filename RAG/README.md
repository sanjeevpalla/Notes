# 🔍 RAG (Retrieval-Augmented Generation)

> Notes spanning RAG fundamentals, a production-grade end-to-end build, and a real HR Policy assistant project — from first principles to guardrails, gateways, and evaluation.

## 📑 Core Concepts

| # | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|---|---|:-:|
| 1 | 🔰 [Fundamentals of RAG](<01. Fundenmtals of RAG.md>) | Two-pipeline RAG architecture (ingestion & retrieval) · embeddings & vector similarity · vector DB types · limits of traditional RAG | ✅ |
| 2 | 🌲 [Vectorless RAG](<02. Vectorless RAG.md>) | Reasoning over document structure with PageIndex · eliminating chunking, embeddings & vector DBs | ✅ |

## 🏗️ Production Grade RAG

End-to-end build of an enterprise RAG system (Kubernetes docs chatbot) — see the [dedicated README](<Production Grade RAG/README.md>) for a full concept overview.

| # | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|---|---|:-:|
| 1 | 🔐 [Production-Grade Advanced RAG — Part 1](<Production Grade RAG/Production-Grade Advanced RAG Part1.md>) | Security & architecture framing · vector DB selection · data ingestion pipeline (parsing → chunking → embeddings → Qdrant) · retrieval/reranking intro | ✅ |
| 2 | 🛡️ [Production-Grade Advanced RAG — Part 2](<Production Grade RAG/Production-Grade Advanced RAG Part2.md>) | Guardrails (Nemo Guardrails/Colang) · LLM gateway (Portkey) · evaluation framing | ✅ |

## 🧑‍💼 HR Policy RAG Assistant (Project)

A real, incrementally-built RAG project — from a from-scratch prototype to a fully modular, traced, guarded, cloud-backed, and evaluated system.

| # | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|---|---|:-:|
| 1 | 🧱 [HR Policy RAG Assistant from Scratch](<HR Policy RAG Assistant/01. HR Policy RAG Assistant from Scratch.md>) | Building the initial RAG assistant end to end | ✅ |
| 2 | 🧩 [Modular Implementation of RAG](<HR Policy RAG Assistant/02. Modular Implementation of RAG.md>) | Refactoring into a modular, maintainable RAG codebase | ✅ |
| 3 | 📊 [Adding LangSmith Tracing & Logging](<HR Policy RAG Assistant/03. Adding LangSmith Tracing & Logging to RAG.md>) | Observability with LangSmith tracing & logging | ✅ |
| 4 | 🛡️ [Adding Guardrails to RAG](<HR Policy RAG Assistant/04. Adding Guardrails to RAG.md>) | Guardrails for safe, reliable RAG responses | ✅ |
| 5 | 🗄️ [Migrating to Qdrant Cloud Vector DB](<HR Policy RAG Assistant/05. Migrating to Qdrant Cloud Vector DB in RAG.md>) | Moving from a local vector store to Qdrant Cloud | ✅ |
| 6 | 🚪 [Adding LLM Gateway to RAG](<HR Policy RAG Assistant/06. Adding LLM Gateway to RAG.md>) | Routing LLM calls through an LLM gateway | ✅ |
| 7 | 📈 [RAG Evaluation with LangSmith](<HR Policy RAG Assistant/07. RAG Evaluation with LangSmith.md>) | Evaluating RAG quality using LangSmith | ✅ |

---


