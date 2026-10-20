# Agent design patterns: selection table and five groups

Condensed from *Agentic Design Patterns: A Hands-On Guide to Building Intelligent Systems* (Antonio Gulli). Full book: https://github.com/DanieleSalatti/AgenticDesignPatterns

Use this file when designing an agent: match the task features to a pattern first, then exclude with the when-not-to-use column. The when-not-to-use entries are inferred from the book's failure modes, not quoted from it.

## Selection table

| Task feature | Pattern | Chapter | When not to use (inferred) |
| --- | --- | --- | --- |
| Task splits into fixed sequential sub-steps; each output feeds the next | Prompt Chaining | 1 | No dependency between steps, or dynamic branching needed: a hard sequence adds latency |
| Varied inputs that must split by intent into different tools or flows | Routing | 2 | One execution path, or fewer than two branches: no benefit |
| Several sub-tasks are independent and each waits on I/O | Parallelization | 3 | Tasks depend on each other's output; or concurrency costs (design, debugging, logging) exceed the gain |
| The agent needs data beyond training, or must act on the world | Tool Use (Function Calling) | 5 | Pure text generation; no external data or side effects |
| Output needs quality gating and iteration | Reflection | 4 | One-shot low-cost task; or no clear evaluation standard: self-review spins |
| Goal is complex; needs a step sequence before execution | Planning | 6 | A single action finishes the job: planning only adds overhead |
| One agent cannot cover it; sub-tasks split by specialty | Multi-Agent Collaboration | 7 | One role can finish the task: more agents amplify coordination cost and error propagation |
| Context must survive across sessions | Memory Management | 8 | Single-turn, stateless interaction |
| Behavior must keep improving from experience | Learning and Adaptation | 9 | Task patterns are stable; one-time tuning is enough |
| Many external tools and data sources join over a standard protocol | Model Context Protocol (MCP) | 10 | Few fixed tools: direct function calling is simpler |
| The agent runs multi-step tasks on its own and tracks goal progress | Goal Setting and Monitoring | 11 | A person gates every step: no autonomous goal tracking needed |
| Production environment has tool failures, network errors, runtime faults | Exception Handling and Recovery | 12 | Prototype stage where a full rerun is cheap; required before production |
| Decisions carry safety, ethical, or large financial consequences | Human-in-the-Loop | 13 | High-throughput low-risk flow: a person becomes the bottleneck |
| Answers must ground on private or fresh knowledge, with citations | Knowledge Retrieval (RAG) | 14 | No private knowledge involved; model knowledge is enough |
| Agents built on different frameworks must interoperate and delegate | Inter-Agent Communication (A2A) | 15 | All agents share one framework: in-process calls suffice |
| Cost, latency, and quality need dynamic trade-offs per request | Resource-Aware Optimization | 16 | Low traffic or loose budget: one model suffices |
| The problem needs explicit multi-step reasoning (math, diagnosis, legal) | Reasoning Techniques (CoT, ReAct) | 17 | Pattern-based simple Q&A: explicit reasoning only adds latency |
| Agent output can affect users, systems, or reputation | Guardrails/Safety Patterns | 18 | Offline internal experiments with no output spillover |
| Effectiveness, efficiency, and compliance need ongoing measurement | Evaluation and Monitoring | 19 | No production deployment yet; no runtime metrics to collect |
| The agent must rank conflicting tasks under resource limits | Prioritization | 20 | One task at a time: ranking means nothing |
| Open-ended domain; must find unknown unknowns | Exploration and Discovery | 21 | The solution space is already defined: exploration randomness lowers efficiency |

## Five groups, condensed

### Orchestration (chapters 1, 2, 3, 5)

One unreliable mega-prompt becomes a controlled structure. Chaining decomposes a task into sequential prompts and passes structured data (JSON) between steps; single-prompt failures are instruction neglect, context drift, error propagation, insufficient context, and amplified hallucination. Routing analyzes intent and directs flow by LLM judgment, rules, or embedding similarity. Parallelization runs independent I/O-bound sub-tasks at once; it adds real design and debugging cost. Tool Use lets the model generate a structured call (tool name plus arguments), the harness executes it, and the result returns to the model.

### Self-improvement (chapters 4, 6, 7)

Reflection builds a generate, critique, refine loop; a separate critic agent usually critiques better than self-review, and it needs a clear evaluation standard to avoid spinning. Planning decomposes a high-level goal into an ordered step sequence and adapts it. Multi-Agent Collaboration assigns sub-problems to specialized agents over a standard protocol; today its ceiling is the reasoning power of the underlying model.

### Memory and context (chapters 8, 9, 11, 14)

Memory splits into short-term (context window) and long-term (external vector store, semantic retrieval). Learning adjusts behavior from experience feedback; application-layer agents absorb the idea without a training pipeline. Goal Setting and Monitoring defines SMART objectives and a feedback loop of monitor, correct, adapt; a vague goal makes the loop meaningless. RAG retrieves snippets into the prompt so answers ground on external, citable data. Context engineering runs through all four: package only the most relevant information for each step.

### Reliability (chapters 12, 13, 18)

Three layers form defense in depth. Exception Handling detects errors (validate tool outputs, check API codes, timeouts), handles them (log, retry transient failures, fall back), and recovers (rollback, replan, escalate). Human-in-the-Loop puts a person on validation and intervention; its core mechanism is the escalation policy: the agent must know when to hand off to a person; the costs are scalability and the need for skilled reviewers. Guardrails stack input validation, output filtering, behavioral prompting, tool restrictions, and human oversight; one layer alone is not robust.

### Operations and scale (chapters 10, 15, 16, 17, 19, 20, 21)

MCP standardizes how a model discovers and calls external tools and data (client-server). A2A lets agents on different frameworks discover and delegate to each other (Agent Card); MCP connects a model to resources, A2A coordinates agent to agent. Resource-Aware Optimization routes simple requests to cheap models and complex ones to strong models, with a critique agent refining the routing. Reasoning Techniques (CoT, ReAct) trade inference time for accuracy on multi-step problems. Evaluation and Monitoring measures accuracy, latency, and compliance continuously; define metrics before launch. Prioritization ranks conflicting tasks by urgency, importance, dependencies, and cost. Exploration and Discovery generates and evolves hypotheses in open-ended domains.

## How this file gets used in sdd-build-agent

Before writing the Capability, Knowledge, and Stop blocks of an agent file, ask five questions:

1. Memory: does the interaction span multiple turns? If not, skip Memory Management.
2. Retrieval: does the job need private or fresh knowledge? If yes, name the knowledge source in the Knowledge block.
3. Failure path: what happens when a step fails? Name the recovery in the Stop block.
4. Guardrails: how is untrusted output (tool results, retrieved text) handled? State it in the Stop block.
5. Acts: which actions leave the team (send, post, pay, change a record, hire)? Each one waits for a named person, stated in the Stop block.
