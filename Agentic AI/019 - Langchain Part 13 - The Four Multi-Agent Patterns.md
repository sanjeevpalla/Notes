# 🧩 MCP & Agentic LangChain, Part 7 — The Four Multi-Agent Patterns, End to End, & the Meridian AI Project Overview

*Series: MCP Deep Dive / Agentic AI with LangChain (Batch "Agent Tki") — Part 7*
*Instructor: Mayank*
*Source: 4 Oct class ("Langchain GCP Project")*

> **Note on scope:** despite the filename, this class is **not** primarily a GCP build. It is the deepest pass yet on LangChain's four documented multi-agent patterns — **Subagent, Handoff, Skills, and Router** — each with a live notebook walkthrough, followed by a ~50-minute *overview* (architecture slides and a codebase walkthrough, not a live deploy) of the course's first real portfolio project, **Meridian AI**, a procurement-audit document-QA system on FastAPI + LangChain + Gemini + GCP. Mayank is explicit that the actual GCP deployment "did not run" this session due to a cloud issue on his end, and is deferred to the next class — the same pattern seen in the previous ("3 Oct") session. This guide documents everything that *was* taught: the four patterns in full depth, the framework comparison (LangChain/LangGraph vs. CrewAI vs. AutoGen), and the Meridian AI architecture as presented.

---

## 📋 Table of Contents

1. [Framing: Why Multi-Agent, and When It Earns Its Complexity](#1-framing-why-multi-agent-and-when-it-earns-its-complexity)
2. [The Subagent Pattern](#2-the-subagent-pattern)
3. [The Handoff Pattern](#3-the-handoff-pattern)
4. [The Skills Pattern](#4-the-skills-pattern)
5. [The Router Pattern](#5-the-router-pattern)
6. [Comparing the Four Patterns](#6-comparing-the-four-patterns)
7. [Framework Comparison: LangChain/LangGraph vs CrewAI vs AutoGen](#7-framework-comparison-langchainlanggraph-vs-crewai-vs-autogen)
8. ["Jeff" Revisited: Router, Model-Selector, and Cost-Saving Classifier](#8-jeff-revisited-router-model-selector-and-cost-saving-classifier)
9. [MCP Tie-In: Tools Are Just Tools](#9-mcp-tie-in-tools-are-just-tools)
10. [Introducing the Meridian AI Project](#10-introducing-the-meridian-ai-project)
11. [Meridian AI: Architecture and Tech Stack](#11-meridian-ai-architecture-and-tech-stack)
12. [What's Built vs What's Deferred](#12-whats-built-vs-whats-deferred)
13. [Course Roadmap Going Forward](#13-course-roadmap-going-forward)
14. [Live Debugging Moments and What They Teach](#14-live-debugging-moments-and-what-they-teach)
15. [Glossary](#15-glossary)
16. [Revision Notes](#16-revision-notes)
17. [Cheat Sheet](#17-cheat-sheet)
18. [Interview Questions and Answers](#18-interview-questions-and-answers)
19. [Scenario-Based Questions](#19-scenario-based-questions)
20. [Hands-on Exercises](#20-hands-on-exercises)
21. [Practice Assignment](#21-practice-assignment)
22. [Additional Resources](#22-additional-resources)
23. [Final Revision Sheet](#23-final-revision-sheet)

---

## 1. Framing: Why Multi-Agent, and When It Earns Its Complexity

### 📖 Definition

LangChain v1's core identity: **Agent = Model + Harness**. A multi-agent system is simply *several agent objects wired together in one of a small number of well-understood shapes.* The "shape" you pick is the architectural decision; everything else (which model, which tools) is detail.

### 🔍 Why a Single Agent Breaks Down

A single agent with one giant prompt and 20+ tools starts failing for three concrete reasons:

1. **Tool confusion** — the model has to choose between a dozen similar-looking tools, and accuracy drops as the tool list grows.
2. **Context overload** — unrelated domains of knowledge (billing docs, refund policy, product specs) all sit in one context window, diluting relevance.
3. **Prompt overreach** — one prompt trying to cover every situation at once becomes vague and self-contradictory.

LangChain's documentation lists four conditions under which multi-agent complexity actually earns its cost:

| Condition | What It Means |
|---|---|
| **Context management** | Keep each agent's context window focused on one job |
| **Specialized knowledge** | Deep domain expertise without overwhelming a shared context |
| **Distributed development** | Different teams own and deploy different agents independently |
| **Parallelization** | Spawn specialized workers for subtasks and run them concurrently |

> ⚠ **Mayank's candid take:** *"I think that LangChain has made it implementation-wise a little bit more difficult... other frameworks do the handling themselves. But LangChain has made it a little bit more difficult. In LangGraph we get a lot more control."* This is a recurring theme: LangChain trades convenience for low-level control, and that trade-off is deliberate.

### 🎯 Key Takeaways

- Multi-agent is not "more agents = better." It's justified only when single-agent limits (tool confusion, context overload, prompt overreach) are actually being hit.
- LangChain documents exactly **four** named multi-agent patterns, plus a fifth escape hatch (custom LangGraph workflow) for anything that doesn't fit the other four.
- 💡 **Interview framing Mayank repeats almost verbatim:** *"Multi-agent is having more than one agent doesn't cut it off. Any sensible interviewer will next ask you: can you tell me, between the router and the handoff, what is the difference?"* — knowing the four patterns by name and by discriminator is itself an interview signal.

---

## 2. The Subagent Pattern

### 📖 Definition

> "A supervisor agent calls another agent as a tool."

A main/supervisor agent owns its own tools. One of those "tools" is actually a wrapper function that invokes a **separate, fully independent agent** — its own model, own tools, own context window — and returns that sub-agent's result as the tool's output.

### 🔍 How It Works

```text
User
  │
  ▼
Main / Supervisor Agent  (has its own tools + prompt)
  │
  ├─ tool call → handle_booking_question(query)
  │                     │
  │                     ▼
  │              Booking Specialist (full agent: own model, own tools, own prompt)
  │                     │
  │                     ▼
  │              returns result string
  │
  ▼
Supervisor incorporates result, replies to User
```

The user never talks to the sub-agent directly. Every response is mediated through the supervisor.

### 🪜 Key Properties (drilled repeatedly in class)

1. **Sub-agents are stateless.** No state flows in except what the supervisor's tool call passes.
2. **A sub-agent cannot call another sub-agent automatically.** LangChain has no native nested-sub-agent wiring — *"Subagent cannot call another subagent."* Two sibling sub-agents are not connected to each other unless you **deliberately** wire one as a tool of the other: *"it is totally on us."*
3. **The result always returns via the supervisor.** The end user gets the response from the agent they originally called — never directly from the sub-agent.
4. **The sub-agent rephrases, not relays, the incoming request.** Demonstrated live: the supervisor's tool call passed a user's raw question down, but the sub-agent's own trace showed it had reformulated the request in its own words before acting — *"Did we get the exact same messages? No, right? ... the second agent put the brain and just tried to understand what it wants."*
5. **The main agent can skip delegation entirely for trivial queries.** Asking the demo agent "Hi, how are you?" triggered no tool/sub-agent call at all — delegation is dynamic, not forced.
6. **Nested/multi-level supervisors are possible.** An agent-as-tool can itself be a supervisor with its own sub-agent-as-tool, to any depth — confirmed in response to a student question, with the caveat that *not every tool is a "supervisor"*; only a tool that specifically wraps an agent invocation counts.
7. **Models can differ per sub-agent.** A cheap model for simple collection tasks, a stronger model for classification or complex resolution — dynamic selection, never hard-coded to one rule.
8. **Distinct from Agent-to-Agent (A2A) protocol.** A2A is a **cross-framework** protocol (e.g., a LangChain agent talking to a CrewAI agent over a shared wire format). Using an agent as a tool *within the same framework* is a simpler, different concept: *"Agent to agent, when it happens, that is between different frameworks."*

> 🎯 **Core interview discriminator (memorize verbatim):** *"Supervisor is getting help from the agent. It is not handing over the task."* This single sentence is what separates Subagent from Handoff.

### 💻 Code Example — Cinema-Booking Supervisor (reconstructed from the live walkthrough)

```python
# Each specialist is a FULL independent agent — its own model, tools, prompt
booking_specialist = create_agent(
    model="claude-sonnet-4",
    tools=[check_seat_availability],
    prompt="Help the customer pick a showtime and confirm seat availability."
)

ticket_specialist = create_agent(
    model="claude-haiku-4",
    tools=[issue_ticket],
    prompt="Issue a ticket once seats are confirmed."
)

@tool
def handle_booking_question(query: str) -> str:
    """Delegate a booking-related question to the booking specialist agent."""
    print("Delegating booking question:", query)
    result = booking_specialist.invoke({"messages": [{"role": "user", "content": query}]})
    print("Booking specialist result:", result)
    return result

@tool
def handle_ticket_booking(query: str) -> str:
    """Delegate ticket issuance to the ticket specialist agent."""
    return ticket_specialist.invoke({"messages": [{"role": "user", "content": query}]})

supervisor_agent = create_agent(
    model="claude-sonnet-4",
    tools=[handle_booking_question, handle_ticket_booking],
    prompt=(
        "You are a cinema supervisor. Delegate booking questions appropriately. "
        "Always book 2 seats if available. Don't ask the user unnecessarily."
    )
)
```

> ⚠ **Live-debugging note:** after tightening the supervisor's prompt ("always book 2 seats if available"), the first re-run still asked the user for confirmation instead of auto-booking — a normal LLM non-determinism quirk, not a bug. A second run behaved as instructed. Mayank candidly called the "safer" first behavior *"actually a very, very good thing"* even though it didn't match the instruction that run.

### 🚀 Best Practices

- Keep each sub-agent's tool list narrow and domain-specific.
- Let the supervisor's prompt state delegation rules explicitly (e.g., "delegate booking questions to X"), but know the model can often infer this correctly from tool descriptions alone, with reduced reliability if you omit it.
- Use cheaper models for simple collection/classification sub-agents, reserving stronger models for synthesis or complex resolution.

### 🎯 Key Takeaways

- Subagent = **delegation with return address** — the supervisor always gets the answer back and relays it.
- No native sibling-to-sibling wiring; you must build that yourself if needed.
- Highest score on *distributed development* and *parallelization* among the four patterns (see Section 6); scores zero on *direct user interaction*, since the user never talks to a sub-agent directly.

---

## 3. The Handoff Pattern

### 📖 Definition

> "One agent's prompt and tools change based on state." Introduced by OpenAI's experimental **Swarm** project: *"the key idea is to let agents delegate tasks to other agents using a special tool call."*

### 🔍 The "Changing Clothes" Mechanism (Mayank's signature framing)

There is genuinely **one single agent object** underneath. A state variable (e.g. `current_step`) tracks which "role" is currently active. A **middleware** (`wrap_model_call`) intercepts every model call *before* it happens, reads the current step from state, and **overrides the system prompt and tool list** for that call, based on a `STEP_CONFIG` dict that maps each step to its own `{prompt, tools}`.

> 💡 **Memory trick:** think of a single actor (say, "the agent") with a middleware acting as the makeup artist. Depending on the current scene (state), the makeup artist turns the same actor into a different character — different costume (tools), different script (prompt) — but it's still one performer underneath.

Three analogies Mayank used, all pointing at the same mechanism:

| Analogy | Maps to |
|---|---|
| Single customer-service phone number, internally transferred | You experience one continuous call even though the "department" behind it changed |
| Same staff room, different "equipment" per subject (calculator for math, atlas for geography) | Same agent "slot," different prompt + tools per active subject |
| One actor ("Hrithik"), different costume per scene via a makeup artist | Middleware = makeup artist; state = which scene is active |

### 🎯 Key Distinguishing Property (vs. Subagent)

> *"The agent which takes up your request, or which your request is handed off to, it will always and always be the one who returns you back."* There is **no orchestrator sitting in between** — the user talks directly to "the agent" throughout, even though its prompt/tools/model are silently swapped out between turns.

This was debated at length in class via a Unix-process analogy raised by a student: a child process either (a) returns control to its parent (≈ Subagent) or (b) completes its job independently and exits without reporting back up (≈ Handoff). Mayank validated this as *directionally* correct, while being candid that LangChain's actual implementation is really "dynamically changing the agent's behavior based on selection of dynamic prompting and tools in the config," not a true independent-process handoff:

> ⚠ *"Could the implementation be better in LangChain? Yes... This is the way LangChain is suggesting that you use it... it can be better, but for the time being, this is how it is. But it is handoff. With the virtue of how handoff behaves, that is the case."*

Compared explicitly to:
- **OpenAI Swarm**: a triage agent transfers the full conversation directly to a target agent via a special "transfer" tool call.
- **AutoGen**: also has a native triage-agent + direct-transfer handoff concept, with no orchestrator — Mayank considers AutoGen's and Swarm's handoff implementations cleaner than LangChain's, while noting AutoGen's newer team API became unnecessarily complex compared to its older, simpler one.

### 🪜 Mechanics

- **State mutation by tools**: tools don't just fetch information — they can *write* into state (e.g., a `record_warranty_status` tool sets `warranty_status` and advances `current_step`).
- **Checkpointer required**: `InMemorySaver()` persists state across the multiple separate model calls that make up one logical "conversation," since each "step" really is a distinct call against a stateful graph/agent.
- **Optional state keys**: a `TypedDict` using `NotRequired` (from `typing_extensions`) marks keys that may not be set yet, defaulting sensibly (e.g., `current_step` defaults to the first step if absent).
- **Model can be swapped too**, not just prompt/tools — by overriding `request.model` inside the same `wrap_model_call` middleware.

> 🎯 **Explicit interview-relevant claim:** *"If you are creating anything related to customer care, handoff is the best design pattern to work it out."*

### 💻 Code Example — Customer Support Warranty Flow (reconstructed)

```python
class SupportState(AgentState):
    current_step: NotRequired[str]      # "warranty_collector" | "issue_classifier" | "resolution_specialist"
    warranty_status: NotRequired[str]
    issue_type: NotRequired[str]

@tool
def record_warranty_status(status: str, runtime: ToolRuntime) -> str:
    """Record customer warranty status and transition to issue classification."""
    runtime.state["warranty_status"] = status
    runtime.state["current_step"] = "issue_classifier"
    return f"Recorded warranty status: {status}"

@tool
def record_issue_type(issue_type: str, runtime: ToolRuntime) -> str:
    """Record the classified issue type and advance to resolution."""
    runtime.state["issue_type"] = issue_type
    runtime.state["current_step"] = "resolution_specialist"
    return f"Recorded issue type: {issue_type}"

@tool
def escalate_to_human(reason: str) -> str: ...

@tool
def provide_solution(solution: str) -> str: ...

STEP_CONFIG = {
    "warranty_collector": {
        "prompt": (
            "You are a customer support agent. Greet the customer warmly, ask if their "
            "device is under warranty, and use record_warranty_status to record their response. "
            "Be conversational and friendly. Don't ask multiple questions at once."
        ),
        "tools": [record_warranty_status],
    },
    "issue_classifier": {
        "prompt": (
            "Ask the customer to describe the issue, determine if it's a hardware problem, "
            "and use record_issue_type to record the classification."
        ),
        "tools": [record_issue_type],
    },
    "resolution_specialist": {
        "prompt": "You have warranty_status and issue_type. Resolve or escalate as appropriate.",
        "tools": [provide_solution, escalate_to_human],
    },
}

class StepMiddleware(AgentMiddleware):
    def wrap_model_call(self, request, handler):
        current_step = request.state.get("current_step", "warranty_collector")
        config = STEP_CONFIG[current_step]
        request.system_prompt = config["prompt"]
        request.tools = config["tools"]
        # request.model = ...   # optionally swap the model too
        return handler(request)

agent = create_agent(
    model="claude-sonnet-4",
    tools=[record_warranty_status, record_issue_type, escalate_to_human, provide_solution],
    middleware=[StepMiddleware()],
    checkpointer=InMemorySaver(),
    state_schema=SupportState,
)
```

### ❌ Common Mistakes

- Confusing Handoff's "single agent, swapped clothes" model with Subagent's "orchestrator + separate agents" model — they look similar at a glance but have opposite return-path semantics.
- Forgetting the checkpointer — without `InMemorySaver` (or an equivalent), state set by one tool call won't persist into the next model call.
- Assuming a 10+ tool list bloats the checkpointer — it doesn't; the checkpointer stores **state**, not tool definitions, and the middleware limits which tools are actually sent to the LLM per step.

### 🎯 Key Takeaways

- Handoff = **one agent, many costumes** — prompt, tools, and even model are swapped via middleware based on state, but the user always talks to "the same agent."
- Highest score on *direct user interaction* and *multi-hop* among the four patterns.
- LangChain's implementation is openly conceded by the instructor to be less elegant than OpenAI Swarm's or AutoGen's native handoff — but it's still functionally a handoff.

---

## 4. The Skills Pattern

### 📖 Definition

> "Specialized capabilities are packaged as invocable skills that augment agent behavior. Skills are primarily prompt-driven specialization that an agent can invoke on demand."

### ⚠ Instructor's Own Editorial Opinion

Mayank states twice, in near-identical words, that he disagrees with classifying Skills as a "multi-agent" pattern at all:

> *"Skill, I'm not sure ideally how, why they have included it in multi-agent. Honestly, it doesn't make sense, but yeah, it is where it is."*

He considers it the **simplest** of the four patterns — there's only ever one agent — and arguably mis-categorized by LangChain's own documentation.

### 🔍 How It Works — Two Mechanisms

**Mechanism 1 — tool-based lookup.** A `load_skill(name)` tool, given a skill name, returns the skill's full content (name/description/content), which gets appended into context for that turn.

> ❌ **Common mistake, demonstrated live on purpose:** when the exact skill name isn't surfaced anywhere in the prompt or tool description, the model **hallucinates a plausible-but-wrong name** (e.g., guessing "refund and cancellation policy" instead of the actual registered key) and the lookup fails. The exact skill name must be visible to the LLM — in the tool description or system prompt — or routing breaks.

**Mechanism 2 — middleware injection ("the Claude way").** A custom middleware collects every skill's `{name, description}` pair — **not** the full content, which stays out of context until needed — and injects that lightweight index into the system prompt on every call: *"these are the available skills; use `load_skill` to get full detail when needed."* This keeps token cost low: name+description runs roughly 20–30 tokens per skill, so *"even if you have 100 skills, 2,000 tokens are not very much."*

> 💡 Mayank's recollection (flagged by him as approximate, "last I read"): Claude's Sonnet/Opus models dynamically pull in skill content only when needed, while Haiku allegedly receives all names+descriptions upfront — presented as his own understanding, not independently verified in class.

### 🔍 Other Details

- An `allowed: bool` field on each skill lets middleware conditionally hide a skill from the model — the same idea as toggling a connector on or off.
- Skill ≈ "a prompt repo," as Mayank confirmed in response to a student's phrasing.
- **Explicitly distinguished from RAG**: a student asked how loading a refund-policy file via a skill differs from RAG. Mayank: *"RAG is totally different... let's wait till we do the RAG"* — deferred to a dedicated future lecture, not resolved in this session.
- A live Claude Desktop demo ("use the email-reply skill and reply to my manager about a holiday tomorrow") showed the same name+description-first, content-on-demand mechanism in a real product, reinforcing that the LangChain middleware pattern mirrors how Claude's own skill system works.

### 💻 Code Example — Refund Policy Skill (reconstructed)

```python
class Skill(BaseModel):
    name: str
    description: str
    content: str
    allowed: bool = True

SKILLS = {
    "refund_policy": Skill(
        name="refund_policy",
        description="Full refund if canceled 2+ hours before showtime, 50% credit within 2 hours.",
        content="... (full policy text, loaded only on demand) ...",
    )
}

@tool
def load_skill(name: str) -> str:
    """Load a specialized skill's full content. Available skills: refund_policy, ..."""
    return SKILLS[name].content

class SkillMiddleware(AgentMiddleware):
    def wrap_model_call(self, request, handler):
        skill_list = "\n".join(
            f"- {s.name}: {s.description}" for s in SKILLS.values() if s.allowed
        )
        request.system_prompt += (
            f"\nAvailable skills:\n{skill_list}\n"
            "Use the load_skill tool when you need detailed information about handling "
            "a specific type of request."
        )
        return handler(request)
```

### 🎯 Key Takeaways

- Skills is single-agent, prompt-driven specialization — not a true multi-agent topology, by the instructor's own admission.
- The name+description/content split is the whole trick: cheap index always visible, expensive content loaded on demand.
- Lowest score on *parallelization* among the four patterns (there's nothing to parallelize — it's one agent).

---

## 5. The Router Pattern

### 📖 Definition

> "A classification step fans out to specialized agents in parallel, then synthesizes." Used for "distinct knowledge verticals that can be queried independently and then combined."

### 🔍 The Key Discriminator

The router/classifier itself **has no agency** — it is not an agent and cannot answer anything on its own:

> *"It is not having any brain, it is not an agent. It is just a function which tells you which [agent] to call."*

This is the sharp contrast with Subagent: a subagent's **supervisor is a full agent** that could answer a trivial query itself without invoking any tool. A router's classifier can *only* route — it has no fallback conversational ability.

### 🪜 How It Works

```text
User Query
   │
   ▼
Classifier (plain function, LLM-based classifier, or a lightweight model like "Jeff")
   │
   ├────────────┬────────────┬────────────┐
   ▼            ▼            ▼            ▼
GitHub Agent  Notion Agent  Slack Agent   (run in parallel, one or many fan out)
   │            │            │
   └────────────┴────────────┘
                │
                ▼
          Synthesize step
                │
                ▼
          Final Answer → User
```

The user never sees the individual agents' separate outputs — only the synthesized final answer.

### 🔍 Built with LangGraph, Not Plain Agent Orchestration

The router pattern is explicitly built with **LangGraph** rather than a plain `create_agent` call, because *"LangGraph allows you to create a graph so that your entry point is not an agent"* — here the entry point is the `classify_query` **function**, not an agent object.

### ⚠ Pedagogical Point Hammered Hard

> *"Just defining [multiple agents] doesn't make them multi-agent."*

Merely instantiating three separate agent objects (GitHub, Notion, Slack) in your code is **not yet** multi-agent. It only becomes multi-agent once they're wired together into an actual workflow (graph, orchestrator, router, etc.). Mayank predicted — and used as a teaching device — that "80 to 85% of people" would wrongly answer "yes, it's already multi-agent" the moment multiple agent objects exist in code, even before any wiring happens.

### 💻 State Schema (reconstructed)

```python
class AgentInput(TypedDict):
    query: str

class AgentOutput(TypedDict):
    source: str      # "github" | "notion" | "slack"
    result: str

class Classification(TypedDict):
    source: str       # which knowledge vertical to query

class RouterState(TypedDict):
    query: str
    classifications: list[str]
    results: list[AgentOutput]
    final_answer: str

def classify_query(state: RouterState) -> Command:
    """Entry point: analyze the query and decide which knowledge base(s) to consult."""
    classification = classifier_llm.invoke(
        f"Analyze this query and determine which knowledge base to consult "
        f"(github, notion, slack): {state['query']}"
    )
    return Command(goto=classification.source, update={"classifications": [classification.source]})

# Each specialized agent (github_agent, notion_agent, slack_agent) has its own tool set,
# e.g. search_code / search_issues for GitHub, search_pages for Notion, search_messages for Slack.

def synthesize(state: RouterState) -> RouterState:
    """Combine all agent outputs into a single final answer."""
    combined = "\n".join(r["result"] for r in state["results"])
    state["final_answer"] = synthesizer_llm.invoke(f"Combine these findings:\n{combined}")
    return state
```

### 💡 "Jeff" as a Router

Confirmed explicitly in class: *"Jeff can be used as a router"* — a lightweight, non-generative classifier (first introduced in Part 6) is a valid, cheaper substitute for an LLM-based classifier at the routing step: *"I hope all of you agree that we can use Jeff here as well. It's an awesome approach of saving cost."* See Section 8 for the full picture of where "Jeff"-style classifiers fit across this course.

### 🎯 Key Takeaways

- Router = **classification, then parallel fan-out, then synthesis** — the classifier itself is "brainless" (no agency).
- Built on LangGraph because the entry point is a function, not an agent.
- Defining multiple agent objects is not multi-agent until they're wired into a workflow — this is a favorite interview trap.
- High score on *parallelization*, lower on *distributed development* than Subagent.

---

## 6. Comparing the Four Patterns

### 📊 Dimension Comparison (from LangChain's own docs, as presented in class)

| Pattern | Distributed Development | Parallelization | Multi-hop | Direct User Interaction |
|---|---|---|---|---|
| **Subagent** | ⭐⭐⭐ Highest | ⭐⭐⭐ Highest | Moderate | ❌ None (always via supervisor) |
| **Handoff** | Moderate | Low | ⭐⭐⭐ Highest | ⭐⭐⭐ Highest |
| **Skills** | Low (single agent) | ❌ Lowest | ⭐⭐⭐ High | Moderate |
| **Router** | Moderate | ⭐⭐⭐ High | Moderate | Low (synthesized, not direct) |

### 🔍 Call-Count Comparison ("Buy Coffee" Toy Scenario)

| Pattern | Call Pattern |
|---|---|
| **Subagent** | Request → main agent → sub-agent → tool execution → return = **4 calls** |
| **Handoff** | Fewer discrete LLM calls, but multiple internal **hops** — explicitly clarified: *"4 hops, not LLM calls"* |
| **Router** | Classification call + N parallel agent calls + synthesis call |

### ⚠ Fault-Handling Tradeoff

> *"If a subagent fails, the orchestrator will try to call it again."* A key reason to prefer multi-agent over monolithic design: isolated failure domains. But the inverse risk is also real — *"if a second agent fails, the caller will not have a clue"* unless failure handling is explicitly built in.

### 🪜 Choosing a Pattern — Decision Guidance

> **Custom LangGraph workflow is the escape hatch:** "anything which is not fitting in the above [four patterns] can be used there."

Mayank repeatedly urges students to treat LangChain's **official docs decision guide** as the definitive reference over any third-party explainer — including his own class — for choosing between patterns.

### 🎯 Key Takeaways

- No single "best" pattern — the decision depends on which dimension (distribution, parallelism, hop count, direct interaction) matters most for your use case.
- Subagent and Router both parallelize well; Handoff and Skills don't (by design — they're both fundamentally sequential/single-threaded).
- Handoff maximizes direct user interaction; Subagent minimizes it to zero.

---

## 7. Framework Comparison: LangChain/LangGraph vs CrewAI vs AutoGen

### 📊 Framework Feature Comparison

| Framework | Agent Definition | Orchestration Primitive | Control Level |
|---|---|---|---|
| **CrewAI** | `role` / `goal` / `backstory` (often YAML) | `Crew` + `Process.sequential` or `Process.hierarchical` | High abstraction, less insight into internals |
| **AutoGen** | Triage agent + native transfer/handoff | `Swarm`/team concept | Native handoff, but newer team API grew needlessly complex |
| **LangChain/LangGraph** | `create_agent()` + middleware | Graph nodes, `Command`, middleware | Low-level, maximal control |

### 🔍 Mayank's Core Principle

> *"Normally the more low-level control you are having in a framework, the better you are able to work."*

He draws an explicit analogy to **Go/Java vs. Python** for production systems: *"most production-grade applications... created in Golang now, or Java, because they give that level of low-level control."* More control translates to better observability, cost control, and execution control — which is why he favors LangChain/LangGraph despite its steeper implementation cost, over CrewAI's higher-level abstraction where *"you don't have much insight about how these things are happening."*

### ⚙ Dynamic, Cost-Optimized Model Selection

A custom function (e.g. `get_most_optimized_model()`) can be invoked inside middleware to pick between provider models (OpenAI, Claude, Gemini, etc.) at runtime based on cost, latency, or task complexity. Mayank is explicit that **there is no universal "best model" selection rule** — you build this logic yourself, optionally delegating the decision to a lightweight classifier like "Jeff" instead of a full LLM call.

### 🎯 Key Takeaways

- CrewAI optimizes for ease of use; LangChain/LangGraph optimizes for control.
- AutoGen has arguably the cleanest native handoff concept, but its evolving API has become harder to use over time.
- Dynamic model selection (per task, per cost budget) is a pattern you build, not something any framework hands you out of the box.

---

## 8. "Jeff" Revisited: Router, Model-Selector, and Cost-Saving Classifier

### 📖 Recap from Part 6

"Jeff" (phonetic rendering; the transcript never gives a confirmed spelling) was established as a **lightweight, non-generative classifier** used for fast routing/scoring decisions in place of a full LLM call — explicitly *not* a schema-validation library like Pydantic.

### 🔍 Where "Jeff" Shows Up in This Class

This session reuses the same concept across three distinct applications, confirming it's a general-purpose cost-saving substitute for an LLM wherever the decision is a **simple classification**, not open-ended generation:

1. **As a Router's classifier** — *"Jeff can be used as a router... it's an awesome approach of saving cost."*
2. **As a dynamic model-selector** — deciding which provider/model to call next, instead of hard-coding one rule.
3. **As a lightweight decision model in a non-chat context** — raised by a student building a maze/treasure-collection game agent for a hackathon; Mayank's advice: for a simple 4-direction (up/down/left/right) decision problem, **use a "Jeff"-style lightweight model instead of an LLM**, purely for cost reasons.

> 🎯 **Interview-relevant takeaway:** the question "why not just use an LLM for every decision?" has a concrete answer — cost and latency. A classifier like "Jeff" handles narrow, low-ambiguity decisions far more cheaply than a full generative call, and should be the default choice whenever the decision space is small and well-defined (routing, model selection, simple game-state decisions).

### 🎯 Key Takeaways

- "Jeff" is not one specific product — it's shorthand in this course for *"a lightweight, non-generative classifier used wherever a full LLM call would be overkill."*
- Appears consistently across routing, model selection, and simple decision-making contexts.
- Its exact spelling is still unconfirmed in the transcript — treated here as a recurring, now well-understood concept rather than a mystery to re-solve each time.

---

## 9. MCP Tie-In: Tools Are Just Tools

### 🔍 The Core Point

> *"Since MCP tools are also tools, the multi-agent concept which involves tools is also applicable to MCP. In MCP, we will just be adding tools to our agent via MCP rather than defining it ourselves."*

None of the four multi-agent patterns change when the tools come from an MCP server instead of being defined locally — a sub-agent, a handoff step, a skill, or a router's specialist agent can all be given MCP-sourced tools exactly as they'd be given locally-defined `@tool`-decorated functions.

### 💡 Instructor's Personal Practice

Asked when to actually reach for MCP in real work, Mayank shared his own habit: he uses MCP mainly for **proof-of-concept** work; once he knows exactly which tool he needs, he prefers calling that API directly rather than going through MCP — *"otherwise, I can just use an API there only, right?"*

### 🎯 Key Takeaways

- MCP is a tool-sourcing mechanism, not a new multi-agent pattern — it slots into the four existing patterns unchanged.
- For production code where the tool need is already known and fixed, direct API calls can be simpler and more efficient than going through MCP's extra indirection.

---

## 10. Introducing the Meridian AI Project

### 📖 What It Is

**Meridian AI** — explicitly positioned as *"the first project... which you can add into your CV,"* in contrast to the toy cinema-booking ("Sinibot"/"CineBot") examples used purely for teaching, which Mayank explicitly tells students **not** to put on their CV: *"Please don't add it in your CV if you have learned from me. That was just for experimentation and understanding."*

A **multi-agent procurement audit** system — a document question-answering application built with FastAPI, LangChain, Gemini, and Google Cloud.

### 🔍 Problem Framing

A company's procurement approval process has a human bottleneck across four sign-off roles:

```text
Purchase Request → Risk & Compliance → Tax & Treasury → Finance Control → CFO
```

Each step is a bottleneck, and the rules governing approval (payment terms, approval limits, tax rules) are "buried" in long PDFs — most of the company's relevant data is unstructured.

### 🪜 Two Core Features

1. **"Ask your document"** — upload any document, ask questions about it in plain English (RAG-based QA).
2. **"Audit a purchase"** — paste a purchase request; **three AI specialist agents** check risk, tax, and accounting; a **CFO-step** agent synthesizes a decision memo. This is explicitly multi-agent/multi-step: *"Meridian AI reads a complete document and uses AI specialists to check a purchase request, then writes a clear memo with a decision."*

### 📊 Project Stats (as stated by instructor)

| Metric | Value |
|---|---|
| Development time | ~2–3 weeks |
| Self-assessed difficulty | 8 out of 10 |
| CV-worthiness | Yes — explicitly the first "real" project in the course |

### 🎯 Key Takeaways

- Meridian AI replaces the slow middle steps of a human sign-off chain (risk, tax, finance control) with specialist AI agents, keeping a human (CFO) as the final decision-maker reading an AI-written memo.
- This is the course's first explicit "put this on your resume" deliverable — distinct from every teaching toy used so far.

---

## 11. Meridian AI: Architecture and Tech Stack

### 🏗 Architecture Diagram

```text
┌─────────────┐      ┌──────────────┐      ┌─────────────────┐
│   React     │ ───▶ │  GCP Cloud   │ ───▶ │    FastAPI       │
│  Frontend   │      │  Run (API    │      │   Backend        │
│ (horizontal │      │  gateway,    │      │ (routing +       │
│  scaling)   │      │  auto-scale) │      │  middleware,      │
└─────────────┘      └──────────────┘      │  LRU cache,      │
                                             │  health/status)  │
                                             └────────┬─────────┘
                                                       │
                              ┌────────────────────────┼─────────────────────────┐
                              ▼                        ▼                         ▼
                     ┌────────────────┐      ┌──────────────────┐      ┌──────────────────┐
                     │  Agent System   │      │   RAG / Vector    │      │   Gemini LLM      │
                     │ (control, risk, │◀────▶│   DB (document     │◀────▶│  (center of all   │
                     │  tax agents)    │      │   embeddings)      │      │   reasoning)       │
                     └────────────────┘      └──────────────────┘      └──────────────────┘
                              │
                              ▼
                     ┌─────────────────────────────────────────────┐
                     │  Observability: Cloud Trace / Monitoring /   │
                     │  Logging · Secret Manager · GCS (file store) │
                     └─────────────────────────────────────────────┘
```

### 📊 Tech Stack Table

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | React | User-facing UI, horizontally scalable |
| API Gateway | GCP Cloud Run | Auto-scales up/down with load |
| Backend | FastAPI | Routing, middleware, LRU cache, health/status endpoints |
| Orchestration | LangChain + LangGraph | Multi-agent coordination |
| LLM | Gemini | *"We will be using Gemini only here... because it's best with Google Cloud"* |
| Retrieval | RAG + Vector DB | Document embeddings for "ask your document" |
| Storage | Google Cloud Storage (GCS buckets) | Uploaded document storage |
| Secrets | Secret Manager | Credential storage |
| Observability | Cloud Trace, Cloud Monitoring, Cloud Logging | Tracing, metrics, logs |
| CI/CD | GitHub Actions | Deployment pipeline (`.github/workflows/`) |

### 🔍 Codebase Walkthrough (described, not typed live)

The repo was shown on screen (not typed character-by-character), including:
- `backend/` — FastAPI app, `@lru_cache`, health/status endpoint, Pydantic schemas.
- Agent prompt files (`control_agent_prompt`, `risk_agent_prompt`, etc.), each paired with its own tool module.
- A tool using the **Frankfurter API** for live currency exchange rates (reused from an earlier class's FX demo).
- `Dockerfile` + `.dockerignore` + `requirements.txt`.
- `.github/workflows/deploy...` — CI/CD pipeline definition.
- `README.md` with architecture/tech explanations.

> ⚠ Because none of this code was read verbatim into the transcript (only pointed at on screen), it is **described here, not reproduced as exact code** — treat the table and bullet list above as the architecture-level truth, not a line-for-line source reconstruction.

### 🪜 Intended Development Workflow (as stated, not yet executed live)

> *"Test the code without any cloud account, package it in a Docker container, deploy it to Google Cloud, watch it, and switch it off safely."*

The intended flow is local-first development → Dockerize → deploy to Cloud Run → observe via the monitoring stack → shut down safely to control cost. This workflow was **described**, not demonstrated end-to-end, in this session.

### 🎯 Key Takeaways

- Meridian AI is a full-stack, multi-agent, cloud-native application — the most architecturally complete example in the course so far.
- Gemini is the chosen LLM specifically because of native GCP integration, not a universal claim that Gemini is "the best" model.
- The local-first → Docker → Cloud Run → observe → shutdown workflow is the recommended cost-safe deployment discipline for any cloud project, not specific to Meridian AI.

---

## 12. What's Built vs What's Deferred

### ✅ What Was Actually Covered This Session

- Full architecture and tech-stack explanation (slides + repo walkthrough).
- Problem framing and the two core features ("ask your document," "audit a purchase").
- A brief, non-technical GCP free-trial/billing walkthrough (90-day trial, ~$300 free credit, a refundable ~₹1000 UPI pre-authorization that Mayank admits he's never figured out how to reclaim).

### ❌ What Is Explicitly Deferred

| Deferred Item | Instructor's Stated Reason / Plan |
|---|---|
| Actually running/deploying on GCP | *"I was facing some issue with my Google Cloud... by the next class I will have it running"* |
| Eval/fact-checking layer for LLM output | Deferred to a future project — current version relies on RAG grounding only, no formal eval |
| RAG fundamentals | Promised, not delivered this session — deferred to a dedicated RAG lecture |
| MCP integration into this specific project | *"MCP is not there... I will add it, don't worry. Just tell me which MCP and I will add it"* |
| Vertex AI / Agent Engine coverage | Deferred to a **future project using Google's ADK** instead of LangChain; also flagged as having unpredictable, high pricing |
| Repo documentation/diagrams | Promised "today or tomorrow," not yet present |
| Prerequisites list (API keys, cloud access) | Promised same day, not yet present |
| Placeholder company name ("Aldermore") | To be changed to a finance-company name, not yet done |
| A2A protocol | Explicitly named as a remaining course topic, not yet taught |
| Standalone system design content | Will never be taught as a separate topic — absorbed implicitly through project architecture decisions |

> ⚠ **Pattern recognition across the series:** this is the **second consecutive class** (after the "3 Oct" session) whose filename promised GCP work that didn't materialize live. Treat any class titled "GCP Project" as provisional until the guide for that specific class confirms actual deployment occurred.

### 🎯 Key Takeaways

- The architecture is fully specified; the deployment is not yet executed.
- RAG, eval, A2A, and Vertex AI/ADK are all named-but-not-yet-taught topics — useful to track across the series as "owed" content.

---

## 13. Course Roadmap Going Forward

### 🪜 Stated Plan

- **Next class**: Meridian AI actually running on GCP cloud (promised).
- **GCP overall**: 2–3 projects planned (riding the 90-day/$300 trial).
- **AWS**: at least 2 projects planned.
- **Azure**: at least 1 project planned (1-month trial).
- **Timeline**: more projects through October; "interview ready" expected after October; November/December covers other frameworks quickly plus framework-specific projects.
- **Second project** (after Meridian AI): introduces **Redis**, described as "much more in depth."
- **AI system-design interview prep**: will be published free on YouTube rather than folded into paid course content — explicit reasoning: doing it properly inside the course would push the schedule "till next year... June, July."
- **Forward-Deployed-Engineer-style content**: real client-project pricing/challenges from Mayank's own agency work, planned for December/January.
- **Lighter practice notebooks**: promised same-day addition to GitHub, in response to a student request for less pre-written, more hands-on material.

### 🎯 Key Takeaways

- The course's project cadence is roughly one major cloud project per month across GCP/AWS/Azure, each layering in new infrastructure concepts (Redis next).
- System design is taught *through* project decisions, never as a standalone module — a deliberate pedagogical choice stated explicitly more than once.

---

## 14. Live Debugging Moments and What They Teach

| Moment | What Happened | Lesson |
|---|---|---|
| API key expiration mid-demo | Had to regenerate the key and restart the Jupyter kernel | Standard operational friction — budget time for it in live demos |
| Supervisor didn't auto-book on first run | First run asked for confirmation instead of auto-booking per the updated prompt; second run worked | LLM agentic behavior is non-deterministic run to run — don't assume a single failed run means a bug |
| Skill-name hallucination | Model guessed a plausible-but-wrong skill name when the real name wasn't surfaced in context | Always make exact identifiers (skill names, tool names) visible to the model — don't rely on inference alone |
| Mermaid diagrams not rendering in a student's VS Code | Resolved by reinstalling / updating VS Code, since Mermaid preview is now built into its native Markdown preview | A tooling/environment issue, not a conceptual one — worth knowing as a quick fix if you hit it yourself |
| GCP billing refund confusion | Instructor openly admitted not knowing how to reclaim the refundable deposit across multiple accounts | Candid acknowledgment that cloud billing UX has real rough edges even for experienced users |

### 🎯 Key Takeaways

- Live demos surfaced genuine non-determinism (the booking re-run) and genuine tooling issues (Mermaid/VS Code) — both are normal, not signs of a broken approach.
- The skill-name hallucination was *deliberately induced* to teach a point — a good technique to remember for your own debugging: strip away an identifier and see if the model guesses wrong, to confirm your injection/prompt actually matters.

---

## 15. Glossary

| Term | Definition | Why It Matters |
|---|---|---|
| Agent = Model + Harness | LangChain v1's definition of what an agent fundamentally is | The baseline unit every multi-agent pattern composes |
| Subagent pattern | Supervisor agent invokes a separate, independent agent as a tool | Delegation with a guaranteed return path through the supervisor |
| Handoff pattern | One underlying agent whose prompt/tools/model change based on state | User talks directly to "the agent" throughout — no visible orchestrator |
| Skills pattern | Prompt-driven specialization loaded on demand (name+description always visible, content loaded lazily) | Cheapest pattern; arguably not true multi-agent |
| Router pattern | A non-agentic classifier fans a query out to parallel specialist agents, then synthesizes one answer | Classifier has zero agency — purely a routing function |
| A2A protocol | Cross-framework agent-to-agent communication protocol | Distinct from same-framework agent-as-tool delegation |
| "Jeff" | Course shorthand for a lightweight, non-generative classifier used for cheap routing/decision-making | Appears across routing, model selection, and simple decision tasks |
| `InMemorySaver` | LangGraph checkpointer that persists state across multiple model calls | Required for Handoff's state to survive between steps |
| `NotRequired` | `typing_extensions` marker for optional `TypedDict` keys | Used for state keys that may not be set yet (e.g. `current_step`) |
| Meridian AI | The course's first CV-worthy portfolio project: a multi-agent procurement-audit document-QA system | Marks the transition from teaching toys to a real deliverable |
| Cloud Run | GCP's auto-scaling container-hosting service | Used as the API gateway layer for Meridian AI |
| Vertex AI Agent Engine | GCP's managed agent-hosting platform | Deferred to a future project using Google's ADK; flagged as pricing-unpredictable |

---

## 16. Revision Notes

### ⏱ One-Minute Revision

- Four multi-agent patterns: **Subagent** (supervisor + tool-wrapped agents, result always returns via supervisor), **Handoff** (one agent, swapped prompt/tools/model via middleware based on state, user always talks directly to "the agent"), **Skills** (single agent, name+description always visible, content loaded on demand — arguably not true multi-agent), **Router** (brainless classifier fans out to parallel specialists, then synthesizes).
- Core discriminator: Subagent = "getting help, not handing over"; Handoff = "handing over, no intermediary."
- Defining multiple agent objects ≠ multi-agent until they're wired into a workflow.
- LangChain/LangGraph trades convenience for low-level control (vs. CrewAI's higher abstraction, vs. AutoGen's cleaner-but-evolving handoff API).
- "Jeff" = lightweight classifier, reused for routing, model selection, and simple decisions — cost-saving substitute for an LLM call.
- MCP tools slot into all four patterns unchanged — MCP is a tool source, not a new pattern.
- Meridian AI = the course's first CV-worthy project (procurement audit, FastAPI + LangChain + Gemini + GCP); architecture fully specified, but live GCP deployment deferred (again) to the next class.

---

## 17. Cheat Sheet

```text
SUBAGENT    → supervisor.tools includes a wrapper that calls another full agent;
              result ALWAYS returns via supervisor; sub-agent CANNOT call another
              sub-agent automatically; sub-agents are stateless.

HANDOFF     → ONE agent object; middleware (wrap_model_call) swaps
              system_prompt / tools / model based on state["current_step"];
              requires a checkpointer (InMemorySaver); user always talks
              directly to "the agent," no intermediary.

SKILLS      → single agent; name+description always in system prompt (cheap);
              full content loaded via load_skill(name) tool only when needed
              (expensive, lazy); exact names must be visible or model hallucinates one.

ROUTER      → classify_query() is a plain FUNCTION (or lightweight classifier,
              e.g. "Jeff"), not an agent; fans out to N specialist agents in
              parallel; a synthesize() step merges results into ONE final answer;
              built with LangGraph because entry point isn't an agent.

DISCRIMINATOR ONE-LINERS:
  Subagent → "Supervisor is getting help. It is not handing over the task."
  Handoff  → "Whoever takes the request is who returns you back. No intermediary."
  Router   → "The classifier has no brain. It just tells you which agent to call."
  Skills   → "Just instructions. Arguably not even multi-agent."

WHEN TO REACH FOR A LIGHTWEIGHT CLASSIFIER ("Jeff") INSTEAD OF AN LLM:
  - Routing among a small, fixed set of known categories
  - Dynamic model/provider selection
  - Simple, low-ambiguity decisions (e.g., 4-direction game moves)
```

---

## 18. Interview Questions and Answers

### 🟢 Beginner

**Q1.** **Question:** What does "Agent = Model + Harness" mean in LangChain v1?
**Answer:** An agent is a model wrapped with a harness — the tools, prompt, and execution loop that let the model act, not just respond.
**Explanation:** This framing separates "the brain" (model) from "the body/environment" (harness), and multi-agent systems are built by composing multiple such agent objects.
**Why Interviewers Ask This:** Tests whether you understand the foundational unit before discussing multi-agent composition.
**Possible Follow-up:** How does a multi-agent system differ from just one agent with more tools?

**Q2.** **Question:** Name the four multi-agent patterns covered in this class.
**Answer:** Subagent, Handoff, Skills, Router.
**Explanation:** Each solves a different composition problem — delegation, state-based role switching, on-demand specialization, and parallel classification-then-synthesis, respectively.
**Why Interviewers Ask This:** Baseline vocabulary check before deeper discriminator questions.
**Possible Follow-up:** Which pattern maximizes direct user interaction?

**Q3.** **Question:** In the Subagent pattern, who does the end user ultimately receive a response from?
**Answer:** Always the supervisor/main agent — never directly from the sub-agent.
**Explanation:** The sub-agent's result is returned as a tool's output to the supervisor, which then replies to the user.
**Why Interviewers Ask This:** Core discriminator between Subagent and Handoff.
**Possible Follow-up:** How does this differ in the Handoff pattern?

**Q4.** **Question:** Can a sub-agent call another sub-agent automatically in LangChain?
**Answer:** No — this must be deliberately wired by the developer; there's no automatic nested sub-agent support.
**Explanation:** Two sibling sub-agents are not connected to each other by default.
**Why Interviewers Ask This:** Checks whether you understand the pattern's actual limits versus assumed capabilities.
**Possible Follow-up:** How would you deliberately wire one sub-agent to call another?

**Q5.** **Question:** What single sentence distinguishes Subagent from Handoff?
**Answer:** "Supervisor is getting help from the agent. It is not handing over the task."
**Explanation:** Subagent retains a return path through the supervisor; Handoff transfers the task so the handling agent responds directly.
**Why Interviewers Ask This:** A favorite "sounds similar, actually different" trap question.
**Possible Follow-up:** Give a real-world analogy for each.

**Q6.** **Question:** In the Handoff pattern, how many actual agent *objects* exist underneath?
**Answer:** One.
**Explanation:** A single agent's prompt, tools, and even model are swapped via middleware based on state — it only *looks* like multiple agents to the user.
**Why Interviewers Ask This:** Tests understanding of the "changing clothes" mechanism, which is non-obvious at first glance.
**Possible Follow-up:** What persists this state across multiple calls?

**Q7.** **Question:** What LangChain component is required for the Handoff pattern's state to persist between steps?
**Answer:** A checkpointer, e.g. `InMemorySaver()`.
**Explanation:** Each "step" is a separate model call against a stateful graph/agent; without a checkpointer, state written by one tool call wouldn't be visible to the next.
**Why Interviewers Ask This:** Practical implementation detail that's easy to omit and easy to be asked about.
**Possible Follow-up:** What happens if you forget the checkpointer?

**Q8.** **Question:** In the Skills pattern, what two pieces of information are always visible to the model, and what is loaded only on demand?
**Answer:** Name and description are always visible (cheap); full content is loaded on demand via a tool like `load_skill`.
**Explanation:** This keeps token cost low even with many skills, since only the needed skill's full content enters context.
**Why Interviewers Ask This:** Tests understanding of the core cost-control mechanism behind the pattern.
**Possible Follow-up:** What happens if the exact skill name isn't surfaced to the model?

**Q9.** **Question:** What happened in the live demo when the exact skill name wasn't given to the model?
**Answer:** The model hallucinated a plausible-but-wrong skill name, and the lookup failed.
**Explanation:** This was deliberately induced to show that exact identifiers must be explicitly surfaced, not inferred.
**Why Interviewers Ask This:** Practical lesson about prompt/tool design that generalizes beyond this one pattern.
**Possible Follow-up:** How would you harden a skill-lookup system against this failure mode?

**Q10.** **Question:** In the Router pattern, is the classifier itself an agent?
**Answer:** No — it has no agency and cannot answer anything on its own; it only decides which specialist agent(s) to call.
**Explanation:** This is the sharp discriminator versus Subagent's supervisor, which *is* a full agent capable of answering trivial queries itself.
**Why Interviewers Ask This:** Tests precise understanding of what "agent" means in this context.
**Possible Follow-up:** Could you replace the classifier with something even simpler than an LLM?

### 🟡 Intermediate

**Q11.** **Question:** Why is defining three separate agent objects in your code not automatically "multi-agent"?
**Answer:** Because multi-agent requires those agents to be wired together into an actual workflow (graph, orchestrator, router). Simply instantiating agent objects with no connecting logic is not yet a multi-agent system.
**Explanation:** This is explicitly called out as a common wrong answer ("80–85% of people" would say yes incorrectly).
**Why Interviewers Ask This:** Separates candidates who understand composition from those who just know vocabulary.
**Possible Follow-up:** What's the minimum wiring needed to make it genuinely multi-agent?

**Q12.** **Question:** Compare LangChain/LangGraph's control level to CrewAI's. Which gives more insight into execution, and why might that matter in production?
**Answer:** LangChain/LangGraph gives lower-level control (via middleware, explicit state, explicit graph nodes); CrewAI abstracts more away (role/goal/backstory, Process.sequential/hierarchical). Lower-level control gives better observability, cost tracking, and execution control — important for production debugging and cost management.
**Explanation:** Mirrors the instructor's Go/Java vs. Python analogy for production-grade control.
**Why Interviewers Ask This:** Tests whether you can reason about framework tradeoffs beyond "which is easier to use."
**Possible Follow-up:** What's the tradeoff CrewAI makes in exchange for that abstraction?

**Q13.** **Question:** Why would you use a lightweight, non-generative classifier (like "Jeff") instead of an LLM for a router's classification step?
**Answer:** Cost and latency — when the decision space is small and well-defined (e.g., 3–4 known categories), a cheap classifier handles it reliably without the cost of a full generative call.
**Explanation:** This generalizes across routing, model selection, and simple decision tasks (e.g., a game agent choosing a direction).
**Why Interviewers Ask This:** Tests cost-awareness in agentic system design, a common real-world concern.
**Possible Follow-up:** When would you *not* want to use a lightweight classifier for this?

**Q14.** **Question:** In the Handoff pattern, how would you make the model itself change based on state, not just the prompt and tools?
**Answer:** Override `request.model` inside the same `wrap_model_call` middleware that already overrides `system_prompt` and `tools`.
**Explanation:** The `ModelRequest` object exposed to middleware carries the model field too, so it's the same mechanism extended.
**Why Interviewers Ask This:** Tests whether you understand middleware's full surface area, not just the commonly-shown prompt/tools swap.
**Possible Follow-up:** Why might you want a cheaper model for an early step and a stronger model for a later step?

**Q15.** **Question:** What's the difference between the Subagent pattern's agent-as-tool delegation and the Agent-to-Agent (A2A) protocol?
**Answer:** Agent-as-tool is intra-framework (e.g., a LangChain agent invoking another LangChain agent as a tool); A2A is a cross-framework protocol (e.g., a LangChain agent talking to a CrewAI agent).
**Explanation:** Both involve one agent "using" another, but at different levels of standardization and interoperability.
**Why Interviewers Ask This:** A frequently confused pair of concepts; precise distinction is a strong signal.
**Possible Follow-up:** In what scenario would you actually need A2A rather than simple agent-as-tool?

### 🔴 Advanced

**Q16.** **Question:** A student argued that Handoff is like a Unix child process either returning to its parent (Subagent) or completing independently without reporting back (Handoff). Is this analogy accurate, and what's the key caveat the instructor raised?
**Answer:** Directionally correct, but LangChain's actual Handoff implementation isn't a truly independent "process" — it's really dynamic prompt/tool/model selection on a single underlying agent via middleware and state, not a fully separate execution context that "dies" independently.
**Explanation:** The analogy captures the *return-path* behavior correctly but overstates how architecturally separate the "handed-off-to" agent actually is in LangChain's implementation.
**Why Interviewers Ask This:** Tests whether you can evaluate an analogy critically rather than accepting it at face value — a sign of deep rather than surface understanding.
**Possible Follow-up:** How do OpenAI Swarm's and AutoGen's handoff implementations differ from LangChain's in this respect?

**Q17.** **Question:** Design a multi-agent system for a customer support use case that needs both (a) a stateful conversation that feels continuous to the user, and (b) the ability to escalate to a specialized, independently-deployed fraud-investigation team that doesn't report back into the main conversation. Which pattern(s) would you combine, and why?
**Answer:** Use **Handoff** for the continuous-feeling, state-driven conversation (warranty collection → issue classification → resolution), and layer in a **Subagent**-style call specifically for the fraud escalation step, since that work is independently owned/deployed and its result should return through the main conversational agent rather than the user talking to the fraud team directly — or, if the fraud team genuinely never reports back and takes over entirely, that specific step behaves more like its own Handoff transition.
**Explanation:** This tests whether you can reason about mixing patterns rather than treating them as mutually exclusive — real systems often combine Handoff for the user-facing flow with Subagent-style delegation for backend specialist work.
**Why Interviewers Ask This:** Senior-level system design question — correctness depends on reasoning about *return paths* and *ownership*, not just naming a pattern.
**Possible Follow-up:** How would you handle the case where the fraud team's investigation takes days, not seconds?

**Q18.** **Question:** The instructor stated that LangChain's own documentation categorizes Skills as a "multi-agent" pattern, while personally disagreeing with that categorization. Construct the strongest argument for *each* side.
**Answer:** **For calling it multi-agent:** Skills introduce *behavioral specialization* comparable to having a separate "specialist" even though it's not a separate agent object — functionally, the agent "becomes" a specialist once a skill is loaded, which mirrors the outcome (if not the mechanism) of Handoff. **Against:** there is only ever one agent object, one model, one context window, and no delegation, state-driven role switching, or parallel fan-out — the defining structural features of the other three patterns are all absent, so grouping it under "multi-agent" conflates "specialized behavior" with "multiple agents," which are different things.
**Explanation:** This tests the ability to argue both sides of a definitional disagreement the instructor himself raised, rather than just repeating his stated opinion.
**Why Interviewers Ask This:** Evaluates critical reasoning about taxonomy and whether you can distinguish "looks similar in outcome" from "is structurally the same."
**Possible Follow-up:** Where would you draw the line between "specialization" and "multi-agent" in your own system design vocabulary?

---

## 19. Scenario-Based Questions

> **Scenario 1:** Your production multi-agent customer support system, built with the Handoff pattern, starts occasionally losing track of `warranty_status` between the warranty-collection step and the issue-classification step — customers are being re-asked a question they already answered.

**Structured Answer:**
1. **Initial investigation:** Check whether the checkpointer (`InMemorySaver` or a persistent equivalent) is actually configured and attached to the agent, and whether the same `thread_id`/session identifier is being reused across calls.
2. **Metrics/logs to check:** Log the full state dict at the start and end of every `wrap_model_call` invocation; check for state being reset or a new thread_id being generated per request instead of being reused.
3. **Possible causes:** `InMemorySaver` is process-local and doesn't survive a server restart or a request landing on a different instance (if horizontally scaled); a bug in the tool that's supposed to write `warranty_status` into state; a missing/incorrect `state_schema`.
4. **Debugging approach:** Reproduce locally with verbose state logging; check if the issue correlates with server restarts or multi-instance deployment.
5. **Resolution:** If it's a horizontal-scaling issue, move from `InMemorySaver` to a distributed, persistent checkpointer backend; if it's a tool bug, fix the state-write logic.
6. **Prevention:** Use a persistent checkpointer in any production deployment that isn't guaranteed single-instance; add state-integrity assertions/logging as a standard practice.

> **Scenario 2:** Leadership wants to know whether to use MCP or direct API integration for a new internal tool your agent needs to call, in a product company context (not a POC).

**Structured Answer:**
1. **Initial investigation:** Clarify whether the exact tool/API need is already fully known and stable, or still being explored.
2. **Metrics/logs to check:** N/A — this is an architecture decision, not a live debugging scenario; the "metric" here is development velocity vs. long-term maintenance cost.
3. **Possible causes/considerations:** MCP adds a useful abstraction layer when the tool surface is still evolving or when you want to swap underlying services without changing agent code; it adds indirection and overhead when the tool need is already fixed and well understood.
4. **Debugging/decision approach:** Following the instructor's own stated practice — use MCP for POC/exploration; once the exact tool is known and stable, prefer a direct API call for simplicity and performance.
5. **Resolution:** Recommend MCP only if the tool surface is genuinely expected to evolve or be shared across multiple agents/teams; otherwise recommend direct integration.
6. **Prevention:** Document this decision explicitly so future engineers don't assume MCP is always the "more sophisticated, therefore always better" choice — it's a tradeoff, not a strict upgrade.

---

## 20. Hands-on Exercises

### 🧪 Easy

1. Build a two-agent **Subagent** system: a main agent that answers general questions directly, delegating only "weather" questions to a weather-specialist sub-agent with its own dedicated tool. Confirm via print statements that trivial queries skip the sub-agent entirely.
2. Implement a minimal **Skills** middleware that injects two skills' name+description into the system prompt, and verify (by removing one skill's description) that the model can no longer correctly route to it.

### 🧪 Medium

3. Extend the warranty-collection **Handoff** example with a fourth step ("satisfaction_survey") that only becomes reachable after `resolution_specialist` completes, and confirm state persists correctly across all four steps using `InMemorySaver`.
4. Build a minimal **Router** with a plain Python function (not an LLM) as the classifier, routing between two mock specialist agents based on a keyword check, then add a synthesis step that merges both outputs when a query matches both specialists.

### 🧪 Advanced

5. Combine **Handoff** and **Subagent**: build a customer-support agent using Handoff for the main conversational flow, where one of the steps' tools delegates to a separate, independently-modeled fraud-investigation sub-agent whose result is incorporated back into the Handoff conversation.
6. Implement a simple "Jeff"-style lightweight classifier (a small non-LLM model or rule-based function) and use it as the Router pattern's classification step instead of an LLM call; measure and compare latency/cost against an LLM-based classifier on the same test queries.

---

## 21. Practice Assignment

### 🏗 Project: Multi-Channel Support Triage System

**Objective:** Build a system that ingests a support request from an unknown channel (email/chat/ticket, simulated as plain text with a `channel` field) and resolves it using at least two of the four multi-agent patterns together.

**Requirements:**
- A **Router** step classifies the request into one of three categories: billing, technical, account-access.
- Each category is handled by a **Handoff**-style stateful conversation (collect info → classify sub-issue → resolve), reusing the `STEP_CONFIG`/middleware mechanism from Section 3.
- The technical category's resolution step delegates to a **Subagent** for any request requiring a "diagnostic run" (simulated tool), with the result returned through the main conversational agent.
- Include a lightweight non-LLM classifier (a "Jeff"-style function) as an alternative, swappable classification backend for the Router step.

**Architecture:** Use LangGraph for the Router's entry point and overall graph; use `create_agent` + custom middleware for each category's Handoff flow; use `@tool`-wrapped agent invocation for the Subagent diagnostic step.

**Expected Functionality:** A request like `{"channel": "email", "text": "My laptop keeps crashing, can you run a diagnostic?"}` should be classified as technical, enter the Handoff flow, and (if diagnostics are requested) delegate to the diagnostic sub-agent before returning a final resolution.

**Suggested Implementation:** Start with the Router's state schema and classifier function first (test it standalone), then build each category's Handoff flow independently, then wire the Subagent delegation into the technical category's resolution step last.

**Expected Output:** A final synthesized response per request, with full state logged at each step for debugging.

**Challenges:** Keeping the Handoff middleware's `STEP_CONFIG` DRY across three categories without duplicating boilerplate; ensuring the lightweight classifier and the LLM classifier are truly swappable (same interface).

**Bonus Improvements:** Add a fourth category reachable only via escalation from any of the other three (modeling a real support hierarchy); add basic cost/latency logging comparing the LLM classifier vs. the lightweight one across 50 test requests.

---

## 22. Additional Resources

- LangChain official documentation — multi-agent patterns (Subagent, Handoff, Skills, Router) and the pattern-selection decision guide (treat as the authoritative source over any third-party explainer, per the instructor's own repeated recommendation).
- LangGraph documentation — `Command`, graph nodes, checkpointers (`InMemorySaver` and persistent alternatives).
- OpenAI Swarm (archived experimental project) — origin of the "handoff" pattern name and mechanism.
- CrewAI documentation — `role`/`goal`/`backstory`, `Process.sequential` vs `Process.hierarchical`, for framework comparison.
- AutoGen documentation — triage-agent/team concepts, for framework comparison.
- `typing_extensions` documentation — `NotRequired` and other `TypedDict` modifiers.
- Frankfurter API (frankfurter.app) — the free currency-exchange-rate API referenced from Meridian AI's tooling.

---

## 23. Final Revision Sheet

### ⭐ Core Concepts
- Four multi-agent patterns: Subagent, Handoff, Skills, Router — plus custom LangGraph workflow as the escape hatch.
- Subagent: supervisor + tool-wrapped independent agents; result always returns via supervisor.
- Handoff: one agent, middleware-swapped prompt/tools/model by state; user always talks directly to "the agent."
- Skills: single agent, name+description always visible, content loaded on demand; arguably mis-categorized as "multi-agent."
- Router: brainless classifier fans out to parallel specialists, then synthesizes one answer.

### ⭐ Important Definitions
- "Agent = Model + Harness."
- A2A protocol = cross-framework agent communication, distinct from same-framework agent-as-tool.
- "Jeff" = lightweight, non-generative classifier for cheap routing/decision tasks.

### ⭐ Important Commands/Code
- `wrap_model_call` middleware overriding `system_prompt`, `tools`, `model`.
- `InMemorySaver()` checkpointer for state persistence across Handoff steps.
- `NotRequired` for optional `TypedDict` state keys.
- `load_skill(name)` tool + skill-index-in-system-prompt middleware.
- `classify_query()` as a LangGraph entry-point function (not an agent) for Router.

### ⭐ Architecture/Process
- Meridian AI: React → Cloud Run → FastAPI → (Agent system + RAG/VectorDB + Gemini) → GCS/Secret Manager/observability stack; CI/CD via GitHub Actions.
- Recommended deployment discipline: local-first → Docker → Cloud Run → observe → shut down safely.

### ⭐ Best Practices
- Match pattern to need: Subagent for distributed/parallel work with no direct user interaction; Handoff for continuous customer-facing conversations; Router for parallel classification-and-synthesis; Skills for cheap on-demand specialization within one agent.
- Use a lightweight classifier ("Jeff") instead of an LLM wherever the decision space is small and well-defined.
- Prefer low-level-control frameworks (LangChain/LangGraph) for production systems needing strong observability and cost control.

### ⭐ Common Mistakes
- Assuming multiple agent objects in code = multi-agent (it doesn't, until wired into a workflow).
- Omitting exact identifiers (skill names) from context and expecting the model to infer them correctly.
- Forgetting a checkpointer in Handoff, breaking state persistence.

### ⭐ Interview Points
- Know all four patterns by name AND by their one-line discriminator.
- Be ready to critically evaluate analogies (e.g., the Unix child-process framing for Handoff) rather than accept them uncritically.
- Be ready to justify framework choice (LangChain/LangGraph vs. CrewAI vs. AutoGen) in terms of control vs. abstraction tradeoffs.

### ⭐ Things to Remember
- This class's GCP work is **architecture-only** — no live deployment occurred, deferred again to the next class, mirroring the prior session's pattern.
- RAG, formal eval, A2A, and Vertex AI/ADK remain "owed" topics across this course as of this class.