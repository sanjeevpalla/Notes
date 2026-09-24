# AI System Scoping Document

**Time:** 2–3 hours  
**Format:** Written document (~2 pages)

---

## The Task

Choose one of the three business briefs below. Produce a structured AI system scoping document as if you are the AI Architect assigned to this initiative. Do not read the model answer before completing your own.

---

## Scenario Options

### Scenario A — LogiCo: Supply Chain Visibility
LogiCo is a mid-size logistics company that manages freight across road, rail, and sea. Their operations team manually monitors shipment status across carrier systems, updating customers by email when delays are detected. Leadership wants "AI to give customers real-time visibility into their shipments and proactively notify them of issues." The team has access to historical shipment data, carrier API feeds, and a customer communication platform.

### Scenario B — MediaStream: Content Recommendations
MediaStream is a streaming platform with 2 million subscribers and a catalogue of 15,000 titles. Their current recommendation engine is rule-based and was built in 2018. Engagement metrics have been flat for two years. Leadership wants "AI-powered recommendations that keep subscribers watching." The engineering team has access to three years of viewing history and a modest ML infrastructure.

### Scenario C — GovDoc: Document Processing
A public sector agency processes 8,000 incoming correspondence items per week — letters, forms, and emails from citizens. Each item must be read, classified by type, routed to the appropriate team, and acknowledged within five working days. The process is entirely manual. Leadership wants "AI to accelerate document processing and reduce manual handling."

---

## Deliverables

For your chosen scenario, complete all six sections below.

---

### Section 1 — Problem Statement
*1–2 paragraphs. What is the underlying business problem? What outcome does the organisation need to achieve? Be specific about the metric that matters.*

[Your answer here]

---

### Section 2 — AI Approach Category
*1 paragraph. Which AI approach category applies — RAG, fine-tuned classifier, generative pipeline, agent, classical ML, or a combination? State your choice and give a one-sentence rationale.*

[Your answer here]

---

### Section 3 — System Boundary
*What is the AI system responsible for? What is it explicitly NOT responsible for? What are the upstream inputs and downstream outputs?*

**In scope:**

[Your answer here]

**Out of scope:**

[Your answer here]

**Upstream dependencies (what feeds the AI system):**

[Your answer here]

**Downstream consumers (what the AI system feeds):**

[Your answer here]

---

### Section 4 — Success Metrics
*2–3 metrics. Each must be specific, measurable, and connected to the business outcome — not a model performance metric.*

| Metric | Definition | Target |
|---|---|---|
| | | |
| | | |
| | | |

---

### Section 5 — Key Unknowns
*3 unknowns that must be resolved before design can proceed. Explain why each blocks the design.*

1. **Unknown:** [state it]  
   **Why it blocks design:** [explain]

2. **Unknown:** [state it]  
   **Why it blocks design:** [explain]

3. **Unknown:** [state it]  
   **Why it blocks design:** [explain]

---

### Section 6 — Stakeholder Communication Summary
*1 paragraph written for a non-technical executive. No jargon, no acronyms, no architecture. Just: what is this system, why are we building it, and how will we know it worked.*

[Your answer here]

---

## Self-Assessment Rubric

Check each criterion against your completed document.

| Criterion | What good looks like | ✓ / ✗ |
|---|---|---|
| Problem statement specificity | Names a measurable business outcome, not a technology preference | |
| Approach category correctness | The chosen category matches the task type (retrieval vs. reasoning vs. classification) | |
| System boundary clarity | In-scope and out-of-scope are unambiguous; a developer could use this to decide whether a new feature request is in scope | |
| Success metric quality | Each metric is measurable, connected to the business, and has a target | |
| Unknown quality | Each unknown is a genuine blocker, not a wishlist item | |
| Executive summary | A non-technical reader could explain this system to a colleague after reading it | |

**Overall quality signal:** If you presented this document in a design review, would stakeholders leave the room with a shared understanding of what is being built, what it is not, and how success will be measured? If yes, the scoping is working.
