# AI System Design

Study notes for a seven-module AI Architect course: how to design, build, govern, measure, and run AI systems in production.

Each module has three files:

1. **Study guide**: the full notes. They cover the lecture, the lesson resources, and the case studies, and include a glossary, a cheat sheet, interview questions, and a final revision sheet.
2. **Worksheet**: a hands-on assignment that applies what the module teaches.
3. **Model answer**: a worked solution to the worksheet. Finish your own attempt before you read it.

---

## 🚀 Start Here

| File | What it is |
|------|------------|
| [AI System Design - The Complete Guide](1.%20AI%20System%20Design%20-%20The%20Complete%20Guide.md) | All seven modules combined into one reference. It includes a step-by-step design playbook, a case study library, and a catalogue of anti-patterns. |
| [Capstone Worksheet](2.%20Worksheet.md) | The final project (15–20 hours). Pick one of three scenarios (HealthBridge, LexAI, or RetailMind) and write a full AI architecture proposal that uses all seven modules. |
| [Capstone Model Answer](3.%20Model-answer.md) | Worked answers for all three capstone scenarios, plus notes on how the rubric scores them. |

---

## 📚 Modules

| # | Module | Key Topics | Worksheet | Model Answer |
|---|--------|------------|-----------|--------------|
| 1 | [The AI Architect Role and the Modern AI Landscape](Module%201/1.%20The%20AI%20Architect%20Role%20and%20the%20Modern%20AI%20Landscape.md) | Why a model is not a system, the capability/cost/control framework, checking whether AI is feasible, classifying problems, scoping documents | [AI System Scoping Document](Module%201/2.%20Worksheet.md) | [Answer](Module%201/3.%20Model-answer.md) |
| 2 | [Designing AI System Architecture](Module%202/1.%20Designing%20AI%20System%20Architecture.md) | Six core architecture patterns, the RAG stack and how it fails, vector databases and indexes, how agents are built, MCP and A2A, agent memory | [RAG Architecture Lab](Module%202/2.%20Worksheet.md) | [Answer](Module%202/3.%20Model-answer.md) |
| 3 | [Model Selection Strategy - Build vs Buy vs Fine-Tune](Module%203/1.%20Model%20Selection%20Strategy%20-%20Build%20vs%20Buy%20vs%20Fine-Tune.md) | The five-dimension scorecard, the limits of prompt engineering, when fine-tuning pays off, deployment options, hybrid routing, vendor lock-in | [Model Selection Decision Memo](Module%203/2.%20Worksheet.md) | [Answer](Module%203/3.%20Model-answer.md) |
| 4 | [Enterprise Integration and System Design](Module%204/1.%20Enterprise%20Integration%20and%20System%20Design.md) | The AI gateway pattern, fitting probabilistic outputs into deterministic systems, non-functional requirements, data pipelines, LiteLLM | [Integration Architecture Lab](Module%204/2.%20Worksheet.md) | [Answer](Module%204/3.%20Model-answer.md) |
| 5 | [Governance, Responsible AI, and Security](Module%205/1.%20Governance%2C%20Responsible%20AI%2C%20and%20Security.md) | OWASP Top 10 for agentic applications, red-teaming, guardrails, observability, regulation, audit-grade infrastructure | [Governance Architecture Review](Module%205/2.%20Worksheet.md) | [Answer](Module%205/3.%20Model-answer.md) |
| 6 | [Measuring What Matters](Module%206/1.%20Measuring%20What%20Matters.md) | The three-layer measurement framework, offline and online evaluation, LLM-as-judge, business metrics, behaviour drift, Langfuse and RAGAS | [Evaluation Architecture Design](Module%206/2.%20Worksheet.md) | [Answer](Module%206/3.%20Model-answer.md) |
| 7 | [Scalability, Cost Optimisation, and Production Operations](Module%207/1.%20Scalability%2C%20Cost%20Optimisation%2C%20and%20Production%20Operations.md) | Scaling needs specific to AI, cost architecture, LLMOps, production readiness checklist, AI incident response | [Production Architecture Design](Module%207/2.%20Worksheet.md) | [Answer](Module%207/3.%20Model-answer.md) |

---

## 🗺 Suggested Learning Path

1. **Learn**: Work through Modules 1 to 7 in order, since each one builds on the one before. For each module, read the study guide, do the worksheet, and then compare your work with the model answer.
2. **Put it together**: Read the [Complete Guide](1.%20AI%20System%20Design%20-%20The%20Complete%20Guide.md) to see how the seven modules fit into one design playbook.
3. **Apply**: Do the [Capstone](2.%20Worksheet.md) without looking at the model answer.
4. **Revise**: Before an interview or design review, go through the Cheat Sheet, Interview Q&A, and Final Revision Sheet in each study guide.

---

## 📁 Folder Structure

```
AI System Design/
├── README.md
├── 1. AI System Design - The Complete Guide.md
├── 2. Worksheet.md              # Capstone
├── 3. Model-answer.md           # Capstone model answer
├── Module 1/  … Module 7/
│   ├── 1. <Module Title>.md     # Study guide
│   ├── 2. Worksheet.md
│   └── 3. Model-answer.md
```
