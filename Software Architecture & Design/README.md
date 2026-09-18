# 🏗 Software Architecture & Design

> Notes from the **Software Architecture** course (instructor: **Michael**), covering the full discipline from formal definitions through requirements, quality attributes, building blocks, databases, architectural patterns, and stream/big-data processing — capped off with two complete end-to-end system designs.

## 📑 Contents

| # | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|---|---|:-:|
| 1 | 🔰 [Introduction, Definition & Place in the SDLC](<01. Introduction, Definition & Place in the SDLC.md>) | Formal definition of software architecture · levels of abstraction · why structure determines purpose · business stakes · place in the SDLC (Design → Implementation → Testing → Deployment) | ✅ |
| 2 | 📋 [System Requirements & Architectural Drivers](<02. System Requirements & Architectural Drivers.md>) | Why large-scale requirements gathering is hard · Functional Requirements · Quality Attributes (non-functional) · System Constraints · Architectural Drivers | ✅ |
| 3 | 📊 [Quality Attributes Deep Dive](<03. Quality Attributes Deep Dive.md>) | Performance (latency & throughput) · Scalability (vertical/horizontal/team) · Availability & the "nines" · Fault Tolerance · SLA, SLO & SLI | ✅ |
| 4 | 🔌 [API Design, RPC, and REST APIs](<04. API Design, RPC, and REST APIs.md>) | API categories · six design best practices · RPC fundamentals · REST resources, statelessness, cacheability & HATEOAS · step-by-step REST design walkthrough | ✅ |
| 5 | 🧱 [Architectural Building Blocks](<05. Architectural Building Blocks.md>) | Load Balancers (DNS, hardware/software, GSLB) · Message Brokers & pub/sub · API Gateway (composition, best practices, anti-patterns) · Content Delivery Networks (Pull vs. Push) | ✅ |
| 6 | 🗄️ [Data Storage at Global Scale](<06. Data Storage at Global Scale.md>) | Relational databases & ACID · NoSQL categories (key-value, document, graph) · indexing, replication & sharding · CAP Theorem · distributed file systems & object storage | ✅ |
| 7 | 🏛️ [Software Architecture Patterns & Styles](<07. Software Architecture Patterns & Styles.md>) | Multilayer/Three-Tier Architecture · Microservices (motivation, benefits, best practices) · Event-Driven Architecture · Event Sourcing & CQRS | ✅ |
| 8 | 🌊 [Strategies for Processing Infinite Streams of Events](<08. Strategies for Processing Infinite Streams of Events.md>) | Tumbling, Hopping, Sliding & Session windows · Event Time vs. Arrival Time vs. Processing Time · handling late/out-of-order events via grace periods & watermarking | ✅ |
| 9 | 📈 [Big Data Architecture Patterns](<09. Big Data Architecture Patterns.md>) | The Three Vs (Volume, Variety, Velocity) · Batch vs. Real-Time Processing · Lambda Architecture (Batch, Speed & Serving Layers) · AdTech worked example | ✅ |
| 10 | 🎯 [Software Architecture & System Design Practice](<10. Software Architecture & System Design Practice.md>) | Capstone: the system design process end-to-end · full worked designs for a scalable discussion forum and an e-commerce marketplace | ✅ |

---
