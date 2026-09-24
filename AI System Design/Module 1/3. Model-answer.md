# Assessment — Model Answer

All three scenarios are worked below. Read only the one matching the scenario you chose before comparing it against your own answer — the rubric criteria are scenario-independent, but each worked example is written to be self-contained.

---

## Scenario A — LogiCo: Supply Chain Visibility

### Section 1 — Problem Statement

LogiCo's operations team currently monitors shipment status manually across separate carrier systems for road, rail, and sea freight, emailing customers when a delay is spotted. This is reactive: the team only notices a problem after it has already happened, and customers hear about it after they've already noticed the shipment is late or after they've called to ask. The underlying business problem is not "customers lack a tracking page" — it's that LogiCo has no way to anticipate a delay before it occurs, which is what actually drives the support contact volume and the customer trust problem leadership is reacting to.

The outcome the organisation needs is a reduction in the number of shipments that reach a customer-visible delay state without prior warning, and a corresponding reduction in "where is my shipment" inbound contacts — not simply a dashboard that displays the same reactive information faster.

### Section 2 — AI Approach Category

This request bundles two problems of very different character, and separating them is the first architectural decision. "Real-time visibility" — showing customers current shipment status — is a data integration and engineering problem: aggregating carrier API feeds into one customer-facing view. It requires no AI. "Proactively notify them of issues" is the genuine AI problem, and it splits into two parts: a discriminative task (predicting, from tracking events and historical transit patterns, that a shipment is at elevated risk of missing its delivery window before the delay is confirmed by the carrier) and a small generative task (drafting the plain-language customer notification once a risk is flagged). The prediction component is a classification/time-series problem best served by a classical ML or gradient-boosted model trained on historical shipment data — not a generative pipeline, since the output is a risk score, not text. The notification-drafting component is generative but low-stakes and template-adjacent.

### Section 3 — System Boundary

**In scope:**
- Ingesting carrier tracking events (already normalised by the upstream integration layer) and computing a delay-risk score per shipment
- Classifying the likely delay reason category (customs, carrier capacity, weather, last-mile) where the data supports it
- Drafting the proactive customer notification text once a shipment crosses the risk threshold
- Flagging low-confidence or ambiguous risk predictions for an operations analyst to review before a notification is sent

**Out of scope:**
- Building the real-time tracking dashboard itself — this is a data engineering integration project, not an AI system, and should not be scoped or budgeted as one
- Carrier API connectivity and event normalisation (an upstream prerequisite this system depends on, not part of it)
- Actually delivering the notification (the existing customer communication platform owns delivery channel, timing, and opt-in preferences)

**Upstream dependencies:**
- Normalised carrier tracking event feed (road, rail, sea) with consistent timestamp and status semantics across carriers
- Three-plus years of historical shipment data with actual delivery outcomes, for training the risk model

**Downstream consumers:**
- The customer communication platform, which receives the drafted notification and risk category and handles send logic
- The operations dashboard, which surfaces at-risk shipments flagged for analyst review

### Section 4 — Success Metrics

| Metric | Definition | Target |
|---|---|---|
| Early-warning precision | Of shipments flagged at-risk, % that go on to actually miss the delivery window | ≥ 70% within 90 days |
| Notification lead time | Average time between a risk flag and the eventual delay event, for true positives | ≥ 24 hours advance notice |
| Reactive "where is my shipment" contact volume | Inbound support contacts about shipment status, per 1,000 shipments | 20% reduction vs. pre-launch baseline within 6 months |

*Note: precision matters more than recall here in the early months — a system that cries wolf on low-risk shipments erodes the trust the whole initiative depends on. It is acceptable to miss some genuine delays initially in exchange for keeping false-positive notifications rare; recall is expanded as the model and thresholds are tuned.*

### Section 5 — Key Unknowns

1. **Unknown:** Whether tracking event granularity and timestamp semantics are actually consistent across road, rail, and sea carriers.
   **Why it blocks design:** A risk model trained on inconsistently normalised events across modes will learn carrier-specific artefacts rather than genuine delay signal. If normalisation is incomplete, the upstream data integration project (out of scope for this system but a hard dependency) must be finished and validated first.

2. **Unknown:** What "delay" means operationally — deviation from the carrier's own ETA, from the customer-facing promised delivery date, or from the historical median transit time for that lane.
   **Why it blocks design:** This is the label the entire model is trained to predict. Without an agreed, single definition, the training data cannot be constructed, and different stakeholders (ops vs. customer-facing teams) may implicitly assume different definitions until this surfaces as a dispute in review.

3. **Unknown:** Customer appetite for proactive notifications — do customers want more contact when a shipment may be at risk, or does that read as noise?
   **Why it blocks design:** If notification thresholds are miscalibrated, this initiative could increase inbound contact volume (customers replying to ask what a "risk" notification means) rather than reduce it. This needs input from customer experience/legal before the notification threshold and copy are finalised, not after launch.

### Section 6 — Stakeholder Communication Summary

We are building a system that looks at a shipment's tracking history in progress and predicts, ahead of time, whether it's at risk of arriving late — instead of waiting for a delay to already have happened before telling the customer. When a shipment looks at risk, the system drafts a plain-language update explaining the situation, which our existing customer messaging platform sends out. We expect this to mean customers hear about problems from us before they notice or call in, and that fewer of them need to contact support to ask where their shipment is. We will know it is working when a large majority of our "at risk" flags turn out to be real, when customers get meaningfully early warning rather than a same-day notice, and when shipment-status support contacts drop. Before we build, we need to confirm our carrier tracking data is consistent enough to predict on, agree on what "delayed" means as a measurable definition, and check with customer experience that proactive alerts will be read as helpful rather than as noise.

---

## Scenario B — MediaStream: Content Recommendations

### Section 1 — Problem Statement

MediaStream's engagement metrics have been flat for two years despite a 15,000-title catalogue, and the current recommendation engine is a rule-based system unchanged since 2018. The business risk behind "flat engagement" is subscriber churn: subscribers who don't find enough to watch are subscribers who cancel. The underlying outcome the organisation needs is not "AI-powered recommendations" as an end in itself — it is subscribers watching more of the catalogue they're already paying for, in a way that measurably improves retention. Before committing engineering effort, it also needs to be established whether this is actually a recommendation-quality problem, or whether subscribers are already being shown relevant content and simply not finding the catalogue compelling — a diagnostic question a recommendation-quality audit should answer before the project is scoped as a rebuild.

### Section 2 — AI Approach Category

This is fundamentally a discriminative ranking task, not a generative one: given a subscriber's viewing history, rank the catalogue by predicted relevance. Three years of viewing history (watches, completions, skips as implicit feedback) supports a modern recommender architecture — collaborative filtering or a two-stage candidate-generation-plus-ranking model — which is the correct category here. A common mistake is reaching for a generative LLM (e.g., "have an LLM read viewing history and suggest titles") for a problem that is a large-scale ranking task with a fixed, well-defined output space (the catalogue); an LLM is neither the efficient nor the well-suited tool for ranking 15,000 titles against 2 million subscribers in real time. A generative layer may be justified later as an additive feature (natural-language explanations for recommendations, or conversational search) but that is separate scope from the core recommendation engine.

### Section 3 — System Boundary

**In scope:**
- Candidate generation and ranking model producing a personalised, ordered list of titles per subscriber for home-screen placement
- Cold-start handling for new subscribers (limited history) and new/low-interaction titles
- Ingestion of viewing events (watch, completion %, skip, re-watch) as the model's feedback signal

**Out of scope:**
- Content acquisition and licensing decisions
- Editorially curated rows (e.g., "New This Month") that are intentionally not personalised
- Any conversational search or natural-language discovery feature — a separate initiative if pursued, not part of the core ranking system
- UI/UX design of how recommendation rows are laid out on screen

**Upstream dependencies:**
- Three years of viewing history at sufficient per-subscriber density to support collaborative signals (to be confirmed — see unknowns)
- Title metadata catalogue (genre, cast, description) for content-based features and cold-start fallback

**Downstream consumers:**
- The home-screen rendering service, which requests a ranked list per subscriber session
- The personalisation API consumed by client apps across platforms

### Section 4 — Success Metrics

| Metric | Definition | Target |
|---|---|---|
| Watch-time per subscriber per week | Average hours watched, tracked against pre-launch baseline | +10% within 90 days of full rollout |
| Catalogue utilisation | % of the 15,000-title catalogue receiving meaningful watch time (vs. the current concentration on a small popular subset) | Measurable increase in long-tail engagement, tracked monthly |
| 28-day churn rate (A/B) | Subscribers on the new recommendation model vs. control group, churn within 28 days | Statistically significant reduction vs. control |

*Note: watch-time is a leading indicator; churn is the metric that actually matters and is proposed as an A/B test specifically because "engagement went up" and "engagement went up because of recommendations, and it reduces churn" are different claims — only the controlled comparison distinguishes them.*

### Section 5 — Key Unknowns

1. **Unknown:** Whether three years of viewing history is dense enough per subscriber to support collaborative filtering, or whether the catalogue/subscriber base is sparse enough that cold start dominates.
   **Why it blocks design:** If most subscribers have thin interaction histories, a pure collaborative-filtering approach will underperform and content-based features (genre, metadata similarity) need to carry more of the weight from the start — a materially different model architecture.

2. **Unknown:** What "modest ML infrastructure" can actually support in production at 2 million subscribers — real-time inference latency and throughput budget.
   **Why it blocks design:** This determines whether a full two-stage (candidate generation + ranking) architecture is feasible immediately or whether a simpler single-stage model is required at launch, with the two-stage architecture introduced once infrastructure is scaled.

3. **Unknown:** Whether flat engagement is actually a recommendation problem, versus a content-supply or catalogue-breadth problem.
   **Why it blocks design:** If subscribers are already being shown relevant recommendations and still not returning, rebuilding the recommendation engine will not move the metric that matters, and the investment should be redirected. This requires a diagnostic pass on current click-through vs. completion data before the project is fully scoped.

### Section 6 — Stakeholder Communication Summary

We are replacing a recommendation system built in 2018 with one that learns from how subscribers actually watch — what they finish, what they skip, what they come back to — and uses that to personalise what each subscriber sees first. The goal is for subscribers to find more of what they're already paying for in our catalogue, which we expect to translate into more watching and fewer cancellations. We will test this properly: a portion of subscribers will see the new recommendations while a control group keeps the current system, so we can confirm any improvement is actually caused by the new approach before rolling it out fully. Before we build, we need to check that our viewing history data is rich enough to personalise well, confirm what our infrastructure can support at our current scale, and rule out that the real problem is something recommendations can't fix.

---

## Scenario C — GovDoc: Document Processing

### Section 1 — Problem Statement

The agency processes 8,000 correspondence items per week through a fully manual workflow. Current average handling time from receipt to classification and routing is 2.3 days, against a five-day SLA. The manual process is the binding constraint on throughput — the team cannot absorb volume spikes, seasonal peaks, or headcount reductions without SLA breaches.

The business problem is not speed per se. It is that manual classification accuracy is 91% (based on a recent internal audit), which means approximately 720 misrouted items per week requiring correction — adding handling time and creating citizen experience failures. The underlying outcome the organisation needs is: higher classification accuracy at current volume, with sufficient throughput headroom to absorb a 30% increase in correspondence volume without proportional headcount growth.

### Section 2 — AI Approach Category

This is a discriminative classification task with a well-defined output space (the routing categories) and substantial labeled training data (historical items with known correct classifications). A fine-tuned text classification model is the appropriate approach — not a generative pipeline, which would introduce unnecessary output variability, and not RAG, which addresses knowledge retrieval rather than classification. Classical ML is worth evaluating as a baseline given the structured nature of the task.

### Section 3 — System Boundary

**In scope:**
- Receiving correspondence items (text extraction from PDF, email, and scanned documents)
- Classifying each item into one of the defined routing categories
- Producing a routing decision with a confidence score
- Flagging low-confidence items for human review
- Logging every classification decision with the input, output, and confidence score

**Out of scope:**
- Drafting responses to correspondence (a separate generative task not in this initiative)
- Managing the routing workflow itself (the existing case management system handles routing; the AI delivers a structured classification to it)
- Citizen identity verification or data matching against other government systems

**Upstream dependencies:**
- Document ingestion pipeline: OCR for scanned items, email parsing, PDF text extraction — must deliver clean text before classification
- Routing category taxonomy: the complete, stable list of routing categories maintained by the operations team

**Downstream consumers:**
- The case management system receives the classification and confidence score via API and executes the routing
- The human review queue receives flagged low-confidence items

### Section 4 — Success Metrics

| Metric | Definition | Target |
|---|---|---|
| Straight-through processing rate | % of items classified and routed without human intervention | ≥ 80% within 90 days of launch |
| Classification accuracy on reviewed items | % of human-reviewed items where the AI classification was correct | ≥ 96% (vs. 91% current baseline) |
| Average handling time (classification stage) | Time from item receipt to routing decision | ≤ 4 hours (vs. 2.3 days current) |

*Note: the 80% straight-through target is deliberately conservative. If accuracy is 96% and the confidence threshold is set to route only high-confidence items autonomously, the initial rate will be lower but quality will be high. The target is reviewed at 90 days and adjusted based on observed confidence distribution.*

### Section 5 — Key Unknowns

1. **Unknown:** Volume and quality of labeled historical data available for training.
   **Why it blocks design:** The fine-tuning strategy depends entirely on having sufficient high-quality labeled examples per category. If the category distribution is highly imbalanced (many items in common categories, few in rare ones), the classifier will underperform on rare categories — which may be exactly where misrouting has the highest consequence. Until we can audit the training data, we cannot confirm that fine-tuning is viable without a data remediation effort.

2. **Unknown:** The stability and completeness of the routing category taxonomy.
   **Why it blocks design:** If the taxonomy is subject to ongoing changes (new categories added, categories merged, definitions revised), the classifier requires retraining each time. An unstable taxonomy turns a one-time ML investment into ongoing maintenance. We need to understand the taxonomy change cadence before committing to a fine-tuned approach versus a more adaptable architecture.

3. **Unknown:** The regulatory position on AI-assisted classification of citizen correspondence.
   **Why it blocks design:** Public sector AI deployments may require a human decision-maker to remain formally accountable for routing decisions, even with AI assistance. The human review design and the confidence threshold at which autonomous routing is permitted may be determined by policy rather than by quality metrics. Legal and compliance sign-off is required before architecture decisions are finalised.

### Section 6 — Stakeholder Communication Summary

We are proposing an AI system that reads incoming correspondence and decides which team should handle it — the same judgment currently made by staff reviewing each item manually. The system will handle straightforward items automatically and route ambiguous items to a human reviewer for a final decision. We expect this to significantly reduce the time citizens wait for their correspondence to reach the right team, while improving routing accuracy compared to the current manual process. We will know it is working when the proportion of items handled without manual intervention reaches our target, and when the rate of misrouted items — currently 9% — falls below 4%. Before we design the system, we need to confirm the availability of historical training data, agree on the routing category taxonomy, and obtain sign-off from legal on the AI-assisted classification approach.

---

## Rubric Commentary

**Problem statement:** Good answers name a metric the business already tracks and identify the specific failure mode behind the vague leadership ask — bad answers describe the technology instead. LogiCo's ask conflates a data-engineering problem (visibility) with an AI problem (prediction); GovDoc's ask hides a quality problem behind a speed complaint; MediaStream's ask assumes the cause (bad recommendations) without confirming it. Naming that gap explicitly is what separates a strong answer from a restatement of the brief.

**Approach category:** This is the section where scope inflation is easiest to spot. GovDoc is fine-tuned classification — proposing RAG or a full generative pipeline over-engineers a fixed-output-space task. MediaStream is a ranking/recommendation task — proposing an LLM to "read history and suggest titles" ignores that ranking 15,000 items for 2 million users is a scale and latency problem an LLM is poorly suited to. LogiCo is the subtlest: part of the ask ("visibility") isn't an AI problem at all, and a common mistake is scoping the whole request as one AI system instead of separating the AI component (delay prediction) from the plain data-integration component.

**System boundary:** The out-of-scope list is as important as the in-scope list in every scenario. Drafting responses (GovDoc), owning the tracking dashboard (LogiCo), and building a conversational search feature (MediaStream) are all common scope-creep targets that surface in the first stakeholder meeting. Good scoping documents make these boundaries explicit before that meeting happens.

**Success metrics:** In all three scenarios, a common mistake is proposing a model-quality metric (accuracy, precision on a held-out test set) as the primary success measure instead of an operational or business metric. Model-quality metrics belong in the evaluation architecture (Module 6); the scoping document's success metrics should be measurable from the business system itself — throughput, contact volume, watch-time, churn.

**Unknowns:** Strong unknowns are structural blockers, not wishlist items — something that would change the entire technical approach if answered differently, not a nice-to-have detail. The regulatory unknown in GovDoc and the "is this actually a recommendation problem" unknown in MediaStream are the ones architects most often skip, because they require asking whether the initiative's premise is correct at all, not just how to build it.

**Executive summary:** Should read as if written to someone who doesn't know what an ML model is. The test: would a non-technical stakeholder who read this be able to explain, to a colleague, what is being built, what it isn't, and how success will be known?
