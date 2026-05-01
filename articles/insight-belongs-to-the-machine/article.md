## Introduction

A few years into the agentic AI cycle, one question has come to separate demonstrations from production systems. It is usually asked from the back of the room — by risk, sometimes by legal, occasionally by a regulator who happens to be in attendance. *What happens at minute eight of step four when the wire fails?* The slide on screen has just shown the agent reading documents, summarizing the case, drafting the response, and filing the artifacts. The label reads *Autonomous*. The question is not adversarial. It is the discipline that experienced operators in regulated industries have learned to apply, because the difference between *the agent works* and *the system can be defended* is exactly the discrepancy accountability has to reconcile.

That is the question this document is written to answer.

The short answer is in the title. Insight — messy, unstructured, judgment-rich — belongs to the model. Decisions — the kind with side effects, regulatory weight, and money attached — belong to deterministic systems with humans accountable on top. The long answer is the rest of the document.

The structure is a progression. Part I lays out the philosophy: a three-force balance between agentic autonomy, probabilistic reasoning, and deterministic logic, and the architectural decisions that evolve out of that balance. Part II compares cognition and coordination across the dimensions that matter in production — including a cost formula a reader can plug their own numbers into rather than a price tag I cannot defend. Part III is a thirteen-pattern library organized in three tiers. Part IV walks through five use cases chosen because they show *peak synergy* between agentic and deterministic capabilities — the cases where neither tool alone could plausibly do the work. Part V is a detailed walkthrough of a high-complexity mortgage underwriting process, including the funding saga that runs when the wire is rejected at the last millisecond. Part VI describes the ecosystem-level governance required to scale any of this beyond a pilot, including how the protocol layer (MCP for tools, A2A for agent-to-agent communication) has changed the architectural conversation in 2026.

Each section is written to stand on its own. The flow is also designed to work end-to-end for a reader who wants the full argument.

A note on what this is, and what it is not. This is not an argument against agentic AI. It is an argument for putting agents where their probabilistic strengths earn their keep, and putting workflows where deterministic accountability is the only acceptable design. The two are complementary. They become dangerous only when confused.

My thesis, stated plainly is: lead with the workflow; let the agent serve it. Use cognition where ambiguity is the work. Use coordination where certainty is the contract. Confuse the two and you have built a system that is impressive in a demo, unsafe in production, and unrecognizable to a regulator.

---

# Part I — The Philosophy

A note before the philosophy. In my work helping enterprises put agentic systems into production, the question that defines the architecture tends to arrive too late in the engagement. The team has built a working agent. The agent is genuinely impressive. Someone — usually the architect, occasionally the head of compliance — asks how the agent will be governed once it is operating against real data, real customers, and real money. That question is the moment the philosophy below becomes useful. The brain has been built. The governance is what the philosophy is for.

What follows is the philosophy I keep coming back to in those scenarios.

---

## 1. The Three-Force Balance

Every high-functioning agentic system is a composition of three forces in constant tension.

The first is **Agentic Autonomy** — the model's ability to decide the next best action based on context. It is what makes an agent feel useful instead of mechanical. It is also what makes an agent feel unpredictable instead of trustworthy.

The second is **Probabilistic Reasoning** — the model's capacity to produce outputs based on confidence and patterns rather than rigid certainty. This force has two distinct expressions in enterprise systems, and conflating them is a category error worth naming early. *Statistical prediction* — classical machine learning models that emit a propensity, a class, or a score from structured features — is one expression. *Generative cognition* — large language models that emit narrative, synthesis, and tool-calling reasoning from unstructured input — is the other. They share probabilistic foundations and they share nothing else. They have different governance models, different update cadences, different failure modes, and different proper places in the architecture. Section 7 treats this distinction in detail.

The third is **Deterministic Logic** — the immutable rules, policies, and workflows that behave identically every single time. Determinism is unfashionable. It is also what regulators trust, what ledgers require, and what makes a system insurable.

The discipline of agentic architecture is the discipline of placing each of these forces where it belongs. Autonomy at the boundary of ambiguity. Probability at the moment of synthesis. Determinism at the moment of consequence.

A system that lets autonomy decide where the money moves is a system that has confused force with function. A system that lets determinism decide what a tax return means is a system that will fail the first time someone uploads a JPEG instead of a PDF. A system that lets a generative model do the work of statistical prediction — scoring credit risk, scoring fraud likelihood, scoring next-best-action — is a system that has chosen the wrong probabilism for the job.

The composition matters more than any one of the three.

---

## 2. The Functional Matrix

To choose the right tool, the requirement has to be classified by what it actually is. Most architectural mistakes happen because a probabilistic problem gets a deterministic solution, or a deterministic problem gets handed to a probabilistic system that does not know it is supposed to be exact.

| Category | Component Type | Primary Function | Governance Model | Proper Tooling |
| -------------- | --------------------------- | ------------------------------------------------------ | ---------------------------------------------------------------- | -------------------------------- |
| Generative | Probabilistic (unstructured) | Ideation, drafting, synthesis, tool-calling reasoning | HITL gates, design-time / runtime split, guardrails, prompt versioning | Foundation Models / LLMs |
| Predictive | Probabilistic (structured) | Pattern matching, confidence scoring, propensity, risk | Model risk management, lineage, drift monitoring, controlled retraining | Classical ML / Predictive Models |
| Adaptive | Probabilistic (learning) | Real-time optimization from interaction outcomes | Bounded learning rates, controlled exploration, drift detection within a designed envelope | Adaptive ML engines |
| Deterministic | Rules-Based | Policy enforcement, sequencing, math | Rule versioning, change control, audit | Workflow Engines |
| Agentic | Autonomous | Orchestrating tools and specialist handoffs | Tool catalogs, capability bounds, traceability | Agent Frameworks / Control Plane |
| Human | Accountable | Validation, ethical oversight, exception handling | Audit logging, sign-off authority, separation of duties | Human-in-the-Loop (HITL) Gates |

The matrix is not a menu. It is a triage. Every requirement in an enterprise AI program needs to be sorted into one of these categories before tooling is selected. The temptation to skip this step — to pick the tool first and decide what the requirement is later — is the single most common reason enterprise AI projects produce systems that work in narrow demos and collapse in production.

Two notes on reading the matrix.

First, *generative* and *predictive* are both probabilistic, but they are not interchangeable. Asking a generative model to produce a propensity score is asking it to do a job a credit-risk model does better, faster, and more cheaply. Asking a predictive model to draft a narrative is asking it to do a job an LLM does better. Section 7 develops this distinction; the matrix names it.

Second, *adaptive* is treated as a distinct row because its governance model is materially different from offline-trained predictive models. An adaptive model updates continuously in production. The discipline that makes that safe — bounded learning rates, controlled exploration budgets, drift detection within a designed envelope — is not the same discipline that governs an offline-trained credit risk score. Treating them as the same is a common source of architectural confusion.

---

## 3. The Five Pillars of Agentic Architecture

The matrix above sorts requirements. The five pillars that follow describe the layers of a working system. Each pillar separates *thinking* from *doing*, and each one has a clear tooling answer.

| Layer | Functional Nature | Primary Utility | Tooling Strategy |
| ----------------------- | ----------------- | ---------------------------------------------------------- | ------------------------------------ |
| Cognitive Synthesis | Generative | Reasoning, drafting, complex summarization | Frontier LLMs |
| Statistical Prediction | Predictive | Risk scoring, intent classification, pattern matching | Specialized ML Models |
| Semantic Grounding | Retrieval | Grounding outputs in facts to prevent hallucination (RAG) | Vector Stores / Deterministic Search |
| Deterministic Execution | Workflow | Sequencing, retries, gating, state management | Workflow Orchestrators |
| Control Plane | Governance | Tool catalogs, HITL thresholds, audit trails | Governance Layer |

The mistake to avoid is treating the layers as interchangeable. Cognitive synthesis is not statistical prediction. Semantic grounding is not deterministic execution. The control plane is not a logging service.

When a team conflates layers, the consequences are predictable. Statistical prediction collapsed into cognitive synthesis becomes a chatbot trying to estimate risk by feel. Deterministic execution outsourced to cognitive synthesis becomes an LLM trying to manage state across a thirty-day mortgage process. The control plane treated as an afterthought becomes the place where governance debt accumulates until the first audit forces a rebuild.

The pillars are not architectural decoration. They are the difference between a system that scales and a system that becomes unfixable.

---

## 4. Design-Time vs. Runtime: The Most Important Split

If you take one architectural decision away from this document, take this one.

Use AI autonomy at **design-time**. Keep AI autonomy out of **runtime**.

Design-time is where the workflow is generated, the rules are drafted, the edge cases are simulated, and the policy is reviewed. This is where probabilistic creativity earns its keep. A design-time agent can describe a mortgage underwriting process in plain English, generate the workflow scaffolding, and let a domain expert — a Chief Loan Officer who knows what a self-employed borrower with fluctuating 1099 income looks like — review and refine the result. Pega Blueprint is the cleanest example of this in the market today: a domain expert narrates the process, an AI drafts a deterministic workflow on enterprise rails, and the resulting artifact is the thing that runs in production. The model has been allowed to think and refine iteratively, but the agreement of outcome and the ensuing certainty of execution is gated and once built the model does not get to change the workflow any longer. The creativity is vetted and locked in before a regulator ever sees the output.

Runtime is where the vetted workflow executes against real customers, real money, and real regulators. Runtime should be boring. Runtime should be predictable. Runtime should be the place where the engine just runs the rules that were agreed to. Nothing improvises live.

The catastrophic mistake is letting the model improvise the workflow live. A pattern that is safe during design — let the model decide the next step — is dangerous in production. A bank cannot afford to find out that the model decided, this morning, that the sanctions check could be skipped because the customer's name *looked clean*.

Coordination is the sequence, the routing, and the state management that holds a process together. Lead with the workflow. Let the agent serve it. The result is faster, cheaper, and — what matters most for a regulated environment — fully auditable.

This is what is meant by *augmentation of judgment without outsourcing accountability*.

---

## 5. Things to Watch Out For: The Philosophy Layer

There are three failure modes at this layer. Each one is recoverable when caught early and very expensive when caught late.

The first is **the Three-Force Imbalance**. A program that overweights any one of agentic autonomy, probabilistic reasoning, or deterministic logic produces a recognizable pathology. Too much autonomy: the system feels brilliant in conversation and reckless in production. Too much probability without grounding: the outputs are confident and wrong. Too much determinism without intelligence: the system rejects every customer whose paperwork is not in the standard format.

The second is **the Matrix-Skip**. A team that picks the tool before classifying the requirement will end up with a foundation model trying to do math, a workflow engine trying to read a JPEG, or a vector store standing in for a system of record. The matrix is cheap. Skipping it is not.

The third — and most damaging — is **the Runtime-Improvisation Trap**. This is the architecture in which the model is permitted to rewrite its lending policy live in production because *the agent thought the customer was a special case*. The trap is seductive precisely because it looks like agility. In practice, it is the place where governance debt compounds into governance disaster. The discipline of holding the line at the design-time / runtime boundary is what prevents this drift, and it is the discipline regulated firms have learned to enforce as a non-negotiable contract.

The defense against all three is a single architectural commitment: treat the design-time / runtime split as a contract, not a guideline.

---

# Part II — Cognition vs. Coordination

The central thesis of this document is that **cognition is the talent and coordination is the manager**. You do not hire a poet to manage a global supply chain, and you do not ask a Large Language Model to manage a bank's ledger state.

Part I established the philosophy. Part II makes the comparison concrete.

---

## 6. The Comparison That Settles the Argument

The table below describes the same six characteristics from two perspectives: where the agent earns its keep, where the workflow earns its keep, and which one wins the *doing* lane in a regulated environment.

| Characteristic | Agentic Strength (Cognition) | Workflow Strength (Coordination) | Why Workflow Wins the "Doing" Lane |
| ---------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| SLA Management | Poor. Agents have no native concept of a ticking clock or escalation. | Native. Timers, urgency levels, automated escalations are core features. | Workflows ensure work is done on time; agents only ensure it is thought about. |
| Audit & Lineage | Probabilistic. An audit trail of token probabilities is hard for a regulator to parse. | Deterministic. Visual, state-based maps show exactly who did what and when. | A regulator wants a flowchart of logic, not a transcript of a chatbot's reasoning. |
| Rollback / Undo | Unreliable. Agents can *try* to undo, but often lack the state tracking to be precise. | Absolute. Sagas with compensating actions reverse a specific chain of events. | In banking, a failed transaction must be reversed with mathematical certainty. |
| Complex Routing | High Context. Excellent at classifying intent from messy human input. | High Logic. Handles a thousand branch conditions without losing the thread. | Use the agent to classify the *why*; use the workflow to execute the *where*. |
| RBAC & Security | Fragmented. Security is often prompt-based or bolted onto the API. | Unified. Centralized access control is baked into the engine. | Workflows enforce who can touch data; agents only decide if they should. |
| Interoperability | Brittle. Agents struggle with legacy API nuances or strict formatting. | Robust. Built-in connectors for REST, SOAP, and Mainframe with error handling. | Workflows bridge the gap between New AI and Legacy Reality. |

A regulator reading the right column should recognize each entry as a deal-breaker. None of them are negotiable in a system that handles money, healthcare, or entitlements. None of them are areas where probabilistic reasoning produces an acceptable answer.

Cognition is what agents are for. Coordination is what workflows are for. Confusing the two is the single most common architectural mistake in enterprise AI today.

---

## 7. Three Probabilisms, Three Governance Models

The cognition-versus-coordination split is the load-bearing distinction. There is a second distinction, sitting inside *cognition*, that is equally load-bearing once a program moves past the first prototype.

The probabilistic side of an enterprise AI architecture has three distinct expressions. Treating them as one is the next most common architectural mistake after confusing cognition with coordination.

**Generative cognition** is the work LLMs do. Reading a fifty-page medical record. Drafting a SAR narrative an examiner can defend. Reviewing a contract against playbook and surfacing a non-standard indemnity clause. The input is unstructured. The output is narrative or tool-calling reasoning. The governance model is HITL gates, the design-time / runtime split, guardrails on input and output, evaluator-optimizer reflection where it earns its place, and prompt versioning. The update cadence is on prompt and policy revisions. *Cleanest commercial articulation: the Predictable AI doctrine — creative reasoning at design-time, governed execution at runtime — operationalized through Pega Blueprint at design-time and the family of runtime agents (Design, Conversation, Automation, Knowledge, Coach) at runtime.*

**Statistical prediction** is the work classical machine learning does. Scoring a credit risk. Scoring a fraud likelihood. Predicting an SLA breach. Predicting churn. The input is structured features. The output is a propensity, a class, or a score. The governance model is model risk management — SR 11-7 in U.S. banking, equivalent regimes elsewhere — combined with lineage tracking, drift monitoring, challenger models, and a controlled retraining cadence on the order of weeks or months rather than continuously. The update cadence is governed and discrete. *Cleanest commercial articulation: Pega Predictive Analytics, with models authored and managed in Prediction Studio, embedded into case workflows through Process AI to drive intelligent routing, SLA prediction, and case-level fraud scoring.*

**Adaptive learning** is the work online-learning models do. Recommending a next-best-action. Updating a propensity score from the customer's response in the last interaction. Optimizing an offer mix in real time. The input is a stream of interaction outcomes. The output is a continuously updated prediction. The governance model is the most distinctive of the three: the model itself updates in production, which means the discipline is in *bounding the envelope* of what can update. Bounded learning rates. Controlled exploration budgets. Drift detection against a defined operating range. Designed-in fallback paths when drift exceeds tolerance. The update cadence is continuous, but the envelope is design-time. *Cleanest commercial articulation: Adaptive Decision Manager inside Customer Decision Hub, where adaptive models drive next-best-action while operating inside a governed strategy framework.*

The three probabilisms are not interchangeable. The most common architectural mistake at this layer is asking a generative model to do statistical prediction — asking an LLM to score credit risk by reading the application as text — when a predictive model trained on structured features will be more accurate, faster, cheaper, and far easier to defend in front of a model risk committee. The reverse mistake is asking a predictive model to do generative work, which is rarer because predictive models cannot draft narrative and the limitation is obvious.

The summary table:

| Probabilism | Input | Output | Governance | Update Cadence |
| ---- | ---- | ---- | ---- | ---- |
| Generative | Unstructured (text, image, document) | Narrative, synthesis, tool-calling | HITL, design-time / runtime split, guardrails, prompt versioning | On prompt or policy revision |
| Predictive | Structured features | Propensity, class, score | Model risk management, lineage, drift monitoring, controlled retraining | Discrete, governed (weeks to months) |
| Adaptive | Stream of interaction outcomes | Continuously updated prediction | Bounded learning rate, controlled exploration, drift envelope, designed fallback | Continuous, within a design-time envelope |

Three probabilisms. Three governance models. Each with a proper place in the architecture. The discipline is in not letting one of them do the work of another.

This distinction also resolves an ambiguity that surfaces in the Pattern 11 (Design-Time / Runtime) discussion later in the document. Pattern 11 says the workflow is not rewritten at runtime. It does *not* say that no model can update at runtime. An adaptive model that updates continuously inside a designed envelope is operating exactly as intended; it is not a violation of Pattern 11. The pattern protects the *process*, not the model. Models can learn at runtime. The orchestration around the model cannot.

---

## 8. Economics: A Formula, Not a Number

The case for the workflow is not only a safety case. It is also an economic one. The honest way to make that case is not with a price tag — vendor pricing varies by deal, by region, by deployment model, and by negotiation — but with a structure that lets a buyer plug in their own numbers and arrive at a defensible relative cost.

What follows is that structure.

> **Sidebar: The Total Cost of Ownership Ratio**
>
> Total cost of ownership for an architecture, over a defined evaluation period, can be modeled as the sum of seven components:
>
> $$\text{TCO} = L + (V \times C_t) + (V \times H) + D + Q + M + A$$
>
> Where:
>
> - **L** = Licensing and platform fees over the evaluation period
> - **V** = Volume of transactions over the evaluation period
> - $C_t$ = Per-transaction cognitive cost (token usage at provider rates; zero for pure-workflow steps)
> - **H** = Per-transaction human handling time, converted to dollars at fully loaded labor cost
> - **D** = Development effort to reach production (engineer-hours × loaded rate)
> - **Q** = Testing and validation effort (including security, compliance, and adversarial testing)
> - **M** = Maintenance effort over the evaluation period (including model drift remediation, prompt revisions, and connector updates)
> - **A** = Audit effort over the evaluation period (including log review, regulatory inquiry response, and external audit support)
>
> The relative cost of two architectures, then, is:
>
> $$R = \frac{\text{TCO}_{\text{agentic}}}{\text{TCO}_{\text{workflow}}}$$
>
> A few practical notes on applying the formula.
>
> **Volume changes everything.** At low volume, agentic systems are cheaper because $L_\text{workflow}$ dominates and fixed development cost is amortized over few transactions. At high volume, the per-transaction terms ($V \times C_t$ and $V \times H$) dominate, and the picture inverts. The crossover point is the number a buyer most needs to compute and most often refuses to.
>
> **Cognitive cost is not just tokens.** Agentic patterns multiply token consumption. Reflection (Pattern 6 in this document) and parallel fan-out (Pattern 7) can multiply per-transaction token cost several times over. The honest input for $C_t$ is the realistic pattern budget, not the cost of a single LLM call.
>
> **Audit effort is the term most often understated.** A workflow with a visual lineage produces an audit response in hours. A pure-agentic system without that lineage can produce one in weeks — assuming the relevant logs were retained. In regulated environments, A is frequently the term that flips R.
>
> **Maintenance is also asymmetric.** Workflow logic, expressed as versioned rules and decision tables, is updated by changing the rule. Agentic logic, expressed as prompts, is updated by retesting the prompt against every prior failure mode. The first scales. The second compounds.
>
> The formula is not a calculator. It is a discipline. The goal is to make the conversation about *which terms dominate at which volume*, rather than about a vendor price tag that no one in the room can defend.

A few directional observations from applying the formula across enterprise engagements:

**Speed is not separate from cost.** Workflow execution measured in milliseconds becomes a per-transaction H term close to zero for routing and state management. Agent execution measured in seconds — sometimes tens of seconds, more in reflection loops — becomes an H term that scales with volume. In a customer-facing process, the latency itself produces customer abandonment, which is its own cost line.

**Prototype economics are not production economics.** A consistent pattern across the field is the prototype that costs almost nothing to run on ten cases and then crosses into a production volume where the agentic terms in the formula dominate. The cost line can shift from *cheap* to *expensive* in a matter of weeks. The discipline that has emerged is to model production economics during the prototype, at the volume where the V-multiplied terms behave very differently than they do at demo scale. That modeling exercise is cheap when done early and effectively impossible to retrofit once the program is mid-flight.

**Authoring cost is changing.** The historical complaint that workflow platforms were *hard* belonged to an era when every business process had to be translated into rules by a Lead System Architect. Modern design-time AI tools — Pega Blueprint is the clearest example — let domain experts narrate a process in plain English while the system enforces enterprise standards on the resulting design. The D term for workflow has compressed materially. The reputation has not yet caught up.

The economics flip the moment a program moves from prototype to production. The formula is how a buyer sees the flip coming.

---

## 9. The Authoring Environment and the Protocol Layer

The authoring environment is the most underrated factor in enterprise AI. Most architectural conversations focus on what the system can *do*. The harder question is what the system *cannot* be made to do, by accident, in production.

| Feature | Agentic Frameworks (Pro-Code) | Enterprise Workflow (Low-Code/Blueprint) | Governance Implication |
| ---------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| User Persona | Specialized developers. Requires Python, state machines, and prompt engineering. | Domain experts. Business analysts and product owners drive design via natural language. | Democratization: workflow moves design from the IT bottleneck to the people who own the risk. |
| Security Posture | Opt-in / bolted on. Developers must remember to code RBAC, PII masking, and audit logging. | Forced / baked in. Security protocols are mandatory components of the authoring rails. | Liability: workflow makes it impossible to deploy a process that lacks an audit trail or access control. |
| Maintenance | Brittle. Logic is buried in code; updates require a full development lifecycle. | Visual. Logic is represented as versioned rules and stages that are instantly readable. | Agility: business leaders can read a workflow; they cannot read a thousand-line graph definition. |
| Logic Validation | Probabilistic testing. You hope the agent follows the prompt; testing is non-deterministic. | Deterministic simulation. You can prove the logic works for every defined scenario. | Compliance: regulators prefer systems where *If A then B* is a rule, not a suggestion. |

In a high-stakes environment, **flexibility is a vulnerability**. Pro-code agentic frameworks are flexible by default and secure only by discipline. Enterprise workflow platforms are secure by default and flexible only by intent. The difference matters most on the days when discipline lapses — which is to say, every day, in any organization above a certain size.

There are three reasons this matters in practice.

The first is the **skill ceiling**. Design-time AI tools flip the old reputation that workflow platforms are hard. A Chief Loan Officer narrates a process; the system ensures the resulting application adheres to enterprise standards. The architect's job becomes governance, not translation.

The second is the **forced security rail**. Agentic orchestration frameworks are security-blind by default. Enterprise workflow authoring environments mandate identity and RBAC at every step, encryption and PII handling at the platform level, and audit lineage that cannot be turned off by a developer looking for a shortcut. The audit trail is not a feature you remember to enable. It is the environment.

The third is **the protocol layer**, which is what changed most in the last twelve months. Two open standards now sit underneath every serious agentic system, and both matter for enterprise governance.

**Model Context Protocol (MCP)**, originally introduced by Anthropic in November 2024 and donated to the Linux Foundation's Agentic AI Foundation in December 2025, is now the de facto standard for how agents connect to tools and data. OpenAI adopted it in March 2025, Google DeepMind in April 2025, Microsoft and AWS later that year. By early 2026, MCP had passed ninety-seven million monthly SDK downloads and ten thousand active public servers. For an enterprise architect, the practical consequence is that the *governed tool catalog* described later in Pattern 4 is no longer a thing each platform implements separately — it is a protocol-level concern with a shared specification, vendor-neutral governance, and an emerging set of enterprise auth and audit primitives.

**Agent2Agent Protocol (A2A)**, introduced by Google in April 2025 and now governed by the Linux Foundation alongside MCP, plays the complementary role: it standardizes how independent agents discover and communicate with each other across frameworks and across organizational boundaries. By early 2026, A2A v1.0 was running in production at over a hundred and fifty organizations including Microsoft, AWS, Salesforce, SAP, ServiceNow, and S&P Global. Signed Agent Cards — cryptographic verification that an agent is what it claims to be — became table-stakes for inter-agent trust.

The two protocols solve different problems. MCP is how an agent calls a tool. A2A is how an agent calls another agent. Together, they push the integration layer below the framework, which means the choice of framework matters less than it did a year ago, and the choice of *governance environment* matters more.

The protocol layer is also showing up in enterprise workflow platforms, not just in pure-agentic frameworks. Pega Agentic Process Fabric, launched in Q3 2025 as part of Infinity '25, supports both MCP and A2A natively — making it one of the first enterprise workflow platforms to ship the protocol layer rather than wrap it. The architectural significance is that an enterprise can now orchestrate agents across MCP-compliant tools and A2A-compliant peers without leaving the governed workflow environment, which is the move that makes the protocol layer enterprise-deployable rather than experiment-deployable.

The implication for enterprise architecture is straightforward. The protocol layer is converging. The governance layer is not. A bank that builds on MCP-compliant tools can swap underlying models more easily than it could in 2024. The bank cannot, however, swap out the question of who approved the tool, who logged the call, and who is accountable when the tool moves money. That question is still answered by the workflow engine and the control plane — and it is still the question regulators ask first.

---

## 10. The Pressure Curve: Why Frameworks Converge

This is my framing, not an industry-recognized taxonomy. I find it useful because it explains why frameworks that started in different places have been racing toward similar architectures.

Every agentic framework, in the end, has to answer four questions in roughly the same order. Not because of passing trends, but because the failures force it. The questions are:

1. **How do agents take steps?** (Sequencing.)
2. **How do they hand off to each other?** (Orchestration.)
3. **How do they remember state across failures, restarts, and human reviews?** (Persistence.)
4. **How are they governed in production — security, audit, SLA, regulatory reporting?** (Accountability.)

A framework can ship without answering question two. It cannot scale without answering question three. It cannot enter a regulated environment without answering question four. Most frameworks have moved through these in order because the failures of one stage are the requirements of the next.

A look at the actual 2026 state of the art tells the story.

**LangGraph** has shipped first-class checkpointing, persistent state, and human-in-the-loop primitives. Production users include LinkedIn, Uber, and Klarna, with documented patterns for PostgreSQL-backed durable execution and time-travel debugging. The framework is no longer chasing question three. It is shipping it.

**OpenAI Agents SDK**, the production successor to the experimental Swarm released in March 2025, added native tracing, guardrails, sandboxing, sub-agents, and a long-horizon harness in its April 2026 update. Question two and question three are both shipped primitives.

**CrewAI** is mature, role-based, with a hundred and fifty-plus enterprise customers. The role-and-process framing answers question two well; persistence and governance are still maturing.

**Anthropic's Claude Agent SDK** (renamed from Claude Code SDK in late 2025) ships safety policies as first-class architectural concerns, including constitutional principles enforced at the model level rather than as bolt-on post-processing. This is a partial answer to question four built into the framework rather than left to the operator.

**AWS Strands Agents** integrates deeply with Bedrock, AgentCore, and the AWS identity and audit primitives. Question four is offloaded to AWS-native enterprise infrastructure.

**Google ADK** ships hierarchical agent trees with native A2A support. Question two is pushed to a protocol-level concern.

The honest 2026 comparison, then, is not that frameworks are failing to do what workflows do. It is that frameworks have largely caught up on questions one through three, and most of the remaining gap is at question four.

| Capability | Modern Agentic Frameworks (LangGraph, Agents SDK, CrewAI, Claude Agent SDK, Strands, ADK) | Enterprise Workflow (e.g., Pega) | What This Means |
| -------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Sequencing & State | Mature. Checkpointing, persistence, time-travel, fault recovery now table-stakes. | Mature. Native. | Parity. |
| Multi-Agent Orchestration | Mature. Handoffs, sub-agents, role-based crews shipping in production. | Mature, with explicit case-folder semantics. | Parity at the *what*; difference at the *who is accountable for the state*. |
| Human-in-the-Loop | Mature. First-class interrupt and resume primitives in major frameworks. | Mature, with built-in role and authority routing. | Parity at the technical level; workflow leads on RBAC integration. |
| SLA & Escalation | Manual. Custom code required to handle deadlines and tiered escalation. | Native. Out-of-the-box configuration. | Workflow leads. |
| Audit Lineage for Regulators | Developer-centric (e.g., LangSmith traces). Improving but framework-specific. | Business-centric. Visual lineage suitable for compliance officers. | Workflow leads. |
| Saga / Compensating Actions | Experimental. Being added to frameworks; rarely a first-class primitive. | Native. Mature support for rollback logic and state recovery. | Workflow leads. |
| RBAC & Identity Federation | Implicit. Usually managed at the API or application layer. | Explicit. Enterprise-grade security baked into every rule. | Workflow leads. |
| Cost Predictability at Scale | Variable. Token-heavy patterns multiply bills. | Predictable. Platform fees plus low per-transaction cost. | Workflow leads. |

The conclusion to draw is not that frameworks are bad. It is that the boundary has become clearer: frameworks are the right answer for *cognitive labor* — the messy, ambiguous work of reading, summarizing, classifying, and synthesizing — and workflows remain the right answer for *the system of record*, the place where the bank decides that a loan is officially approved, that funds have moved, and that the audit trail is locked.

Never ask a framework to be the final word on state. That is still the workflow's job.

---

## 11. Things to Watch Out For: The Comparison Layer

Three traps are worth flagging at this layer.

The first is the **demo economics fallacy**. An agentic prototype that runs ten transactions in a sandbox costs almost nothing. The same architecture, scaled to the volume of a real bank — millions of cases per year, with reflection and parallel fan-out applied to each — produces a token bill that arrives like a surprise tax assessment. The economics that matter are the production economics. The TCO formula in §8 exists to make the surprise visible while it is still a planning question, not a board question.

The second is the **frameworks have caught up dismissal**. It is true that LangGraph, the OpenAI Agents SDK, and the rest have shipped checkpointing, persistence, and HITL. It is also true that they have not closed the gap on SLA management, regulator-grade audit lineage, or saga-based rollback. The catch-up is real on questions one through three of the pressure curve. It is incomplete on question four. Treating *partial parity* as *full parity* is how programs end up with a system that handles the cognitive lift well and fails the audit anyway.

The third is the **flexibility-as-virtue assumption**. In an unregulated context, flexibility is a virtue. In a regulated context, it is a liability. The bank that wants its agent to *decide what to do based on context* at runtime is asking for a system that cannot be defended in a regulatory hearing. The right framing is the inverse: in regulated environments, the system should be *incapable of doing the wrong thing, by design*.

---

# Part III — The Pattern Library

A note before the patterns. The thirteen below are not theory. They are the patterns I see succeed in regulated environments and the patterns I see fail. Some of them are trivial to explain and hard to apply consistently — Pattern 11 (design-time / runtime split) is the clearest example. Others are easy to apply and hard to design well — Pattern 12 (saga with compensating actions) is the case study in Part V. The library exists so that an architecture conversation can move past *should we use AI here?* to *which pattern, in which lane, with what compensating action?*

Each pattern is named, located in its functional lane (cognition, coordination, or hybrid), and accompanied by a strategic insight that explains where it fits and how it fails.

The patterns are organized in three tiers. Tier 1 is the foundational five — patterns every production build should include. Tier 2 is the quality five — patterns that solve for correctness, robustness, and token economics. Tier 3 is the enterprise three — patterns that separate a demo from a production system a Chief Risk Officer will sign off on.

---

## 12. Tier 1: The Foundational Five

**1. Prompt Chaining (Decomposition).** A sequence of LLM calls where the output of one feeds the next, separated by deterministic checks. Functional lane: cognitive synthesis. Decomposition is the primary antidote to hallucination. By breaking a complex task into verifiable sub-steps, you move from one big guess to a series of small, verifiable thoughts. The tradeoff is latency: a five-step chain can multiply total response time by the number of round-trips.

**2. Routing (Classification → Specialist).** A classifier reads the input and routes it to a specific specialist path. Functional lane: hybrid (cognitive classification, deterministic routing). Every all-purpose prompt eventually fails. Routing lets you use small, fast models for triage and specialized logic for execution, optimizing both accuracy and token cost. The strategic move is to keep the classifier cheap and the specialist focused.

**3. Retrieval-Augmented Generation (RAG).** Grounding model generation in retrieved, factual data rather than internal weights. Functional lane: semantic grounding. In banking, any answer the model cannot cite is an answer the bank cannot defend. RAG provides the footnote architecture required for regulatory transparency. The pattern fails when retrieval quality is poor or when the model is allowed to wander outside the retrieved context.

**4. Tool-Using Agent.** The agent chooses which governed API or database query to invoke based on intent. Functional lane: cognitive agency. This is the bridge from *talking* to *doing*. The critical detail: the model does not write the tool. It picks the verb from a governed catalog with fixed permissions. With MCP now established as the protocol layer for tool-to-agent connection, the catalog itself is increasingly a vendor-neutral concern; what remains a per-enterprise decision is *which* MCP servers are exposed, *what* permissions they carry, and *how* their invocations are logged for audit. An agent that can browse the open internet is an agent that can leak data, hallucinate sources, or invoke the wrong API. An MCP-compliant catalog with enterprise authentication and full audit is the modern expression of this pattern.

**5. Human-in-the-Loop (HITL) Gate.** A mandatory pause in the workflow requiring human approval before proceeding. Functional lane: accountable coordination. HITL is not a feature. It is an architectural safeguard. The maturity trajectory should run *In → On → Over the loop* as confidence grows: the human is *in* the loop early, *on* the loop as the system matures, and *over* the loop only when the failure modes are well understood and reversible.

---

## 13. Tier 2: The Quality Five

**6. Evaluator-Optimizer (Reflection).** One model generates a candidate, another critiques it against a rubric, the first revises. Functional lane: cognitive quality control. Automated self-correction significantly improves output quality. It is also the most expensive pattern in the library — a budget multiplier rather than a budget addition. Use it where the cost of a wrong answer dwarfs the cost of an extra inference. Avoid it where a deterministic check would do the same job for a fraction of the price.

**7. Parallel Fan-out with Aggregation.** Running the same input through multiple paths simultaneously and comparing results. Functional lane: diagnostic safety. Disagreement is a signal. When high-stakes risk scores differ across models, the system should not average them. It should trip a HITL gate. The pattern is most powerful when the paths are genuinely independent — different models, different prompts, using several distinct and organized collections of data or documents to search for information.

**8. Orchestrator-Workers (the Hybrid Core).** A coordinator decomposes a task and assigns it to specialists. Functional lane: stateful coordination. This is the anchor pattern of the entire framework. The orchestrator should be the workflow. The workers should be the agents. This ensures the audit trail is a deterministic map, not a chat transcript. Inverting the roles — letting an agent orchestrate workflows — is the single fastest way to produce a system that cannot be reconstructed under regulatory questioning.

**9. Delegation / Handoff (A2A).** Lateral transfer of a case from one specialist agent to another. Functional lane: accountable handoff. Context is what lives or dies at the boundary. The Agent2Agent protocol, now governed by the Linux Foundation and running in production at organizations including Microsoft, AWS, Salesforce, SAP, ServiceNow, and S&P Global, has made the technical handoff itself a solved problem — Signed Agent Cards establish trust between agents from different vendors and frameworks. What is *not* solved by A2A, and what remains a workflow concern, is the *case folder*: the durable, versioned record of why the case is in this state, who has touched it, and what compensating actions are pending. A2A passes the message; the workflow passes the accountability.

**10. Guardrail Wrap.** A deterministic policy layer sitting in front of and behind every model call. Functional lane: deterministic safety. Every guardrail trip is a logged event. Rising trip rates are the earliest signal of model drift or a sophisticated prompt injection attack. Treat the guardrail layer as a sensor as well as a filter.

---

## 14. Tier 3: The Enterprise Three

**11. Design-Time Generation → Runtime Execution.** Using AI to generate and review workflows at design-time but executing them deterministically at runtime. Functional lane: sovereign governance. This is the most critical split for banks. It ensures all creative reasoning is vetted before it touches real money. Runtime execution becomes boring and predictable, which is exactly what runtime execution should be. The cleanest commercial articulation of this pattern is the Predictable AI doctrine — creative reasoning at design time, governed execution at runtime — operationalized through Pega Blueprint at design-time and a family of runtime agents (Design, Conversation, Automation, Knowledge, and Coach) at runtime. A domain expert narrates the process in natural language, AI drafts the workflow on enterprise rails, and the resulting artifact — not the model — runs in production.

A precise note on the pattern's scope. *Pattern 11 protects the workflow, not the model.* A model that updates at runtime — an adaptive next-best-action model, a Process AI prediction that retrains on case outcomes — is not a violation of Pattern 11. It is the third probabilism described in Section 7, operating exactly as intended. The pattern protects the *process*, not the model. Models can learn at runtime. The orchestration around the model cannot.

**12. Saga with Compensating Actions.** Declaring an undo action for every forward action with a side effect. Functional lane: deterministic rollback. Reversibility is the default. Sagas ensure that if a complex agentic process fails at step seven, the bank's ledger is automatically and perfectly offset back to step one. The pattern is treated in detail in Part V; for now, it is sufficient to note that sagas are the architectural mechanism that turns *undo* from a luxury into a guarantee.

**13. Creative Sandbox with Deterministic Exit.** Allowing agents high latitude for exploration in a sandbox, with a rigid gate for production exit. Functional lane: controlled innovation. Probability earns its keep in the sandbox. Determinism earns its keep at the exit. Confusing the two leads to unreviewable production outputs.

---

## 15. Things to Watch Out For: The Pattern Layer

There are three patterns that, badly applied, become anti-patterns.

**Pattern 1 (Prompt Chaining)** stacks latency. Every additional step in the chain is a round-trip to the model. A chain that solves a hard problem is worth the latency. A chain that exists because no one wrote a deterministic check is just a slow guess. Audit your chains for steps that could be replaced by a rule.

**Pattern 6 (Evaluator-Optimizer)** is where token bills go to die. Reflection is genuinely powerful. It is also genuinely expensive. The discipline that pays off is to reserve frontier models for cognitive synthesis and use smaller, faster models for routing, classification, and triage. Reflection layered on a triage step is the single most common source of runaway token costs in production agentic systems.

**Pattern 11 (Design-Time / Runtime)** is the pattern most often skipped because it feels obvious. The lapse is rarely deliberate. It usually happens when a team builds a *smart agent* that can adapt its workflow based on context, and slowly the adaptation surface grows until the workflow is being rewritten live in production. Watch for the symptom: when a team starts asking, *what did the agent decide to do this morning?*, the design-time / runtime split has already been compromised.

The strategy checklist for any working session is short: *Which pattern handles the final decision?* (HITL.) *How do we undo a failed step?* (Saga.) *Are we orchestrating with a prompt or a map?* (Orchestrator-Workers.) *Is the model improvising the process live?* (If yes, fix it. Pattern 11.)

---

# Part IV — Use Cases

The pattern library above is the vocabulary. The use cases below show how the vocabulary is used. Each one was chosen because it shows *peak synergy* between agentic and deterministic capabilities — the kind of work where neither tool alone could plausibly do the job. The cognitive lift is genuinely hard, and the coordination demands are genuinely strict. Where regulation applies, it is named explicitly. Where it does not, it is not forced.

For each use case, the optimal pattern is described first, followed by the failure modes of an agent-only or workflow-only design.

---

## 16. Mortgage Underwriting (Financial Services)

**The optimal pattern.** All three probabilisms cooperate. An agent reads messy bank statements, tax returns, and self-employment income narratives — *generative cognition*. A predictive model trained on historical loan performance scores credit risk and likelihood of default from structured features — *statistical prediction*. The workflow manages the regulatory clock under TILA-RESPA Integrated Disclosure (TRID) timing rules, enforces ECOA fair lending requirements at every decision point, prepares the loan-level data for HMDA reporting, runs the deterministic debt-to-income and loan-to-value calculations, and routes to a Senior Underwriter when ratios cross thresholds — *coordination*. The cleanest implementation has the agent reading the documents, a predictive model authored in Prediction Studio scoring the credit risk, and the workflow orchestrating the whole sequence with the regulatory clock and audit lineage built in.

| Architecture | Pros | Cons |
| ------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| Agentic Only | Highly flexible. Handles unusual income types like gig workers easily. | Brittle and expensive. High latency, hallucinated income totals, no clear audit trail for regulators. ECOA disparate-impact analysis becomes a forensic exercise rather than a structured report. |
| Workflow Only | Secure and fast. Absolute adherence to lending rules and federal regulations. | Rigid. Fails when documents are slightly blurry or income is not in a standard box. Manual data entry inflates the H term in the cost formula. |

The credibility test for any mortgage AI program is the same. An auditor walks in and asks, *Why was this loan denied?* The hybrid pattern produces a flowchart with the calculated DTI, the policy that was applied, and the timestamp of every decision. The agentic-only pattern produces a chat transcript. Only one of those is a defense under ECOA.

This use case is treated as the deep-dive walkthrough in Part V, including the funding saga that runs when the closing wire is rejected.

---

## 17. Complex Claims Adjudication (Insurance)

**The optimal pattern.** An agent analyzes photos of vehicle or property damage, summarizes police reports, and flags fraud signals across unstructured notes — *generative cognition*. An adaptive fraud model — updating continuously as new fraud patterns surface from SIU investigations — produces a real-time fraud-likelihood score on the structured claim features, escalating high-score claims for closer review — *adaptive learning, governed inside a designed envelope*. A workflow checks policy coverage limits and deductibles against the contract, enforces the timelines required by state Unfair Claims Settlement Practices Acts (which typically mandate acknowledgment of a claim within fifteen to thirty days and a coverage decision within thirty to sixty days, depending on jurisdiction), reserves capital against the claim, and executes the payout via a saga with compensating actions if the bank transfer fails — *coordination*.

| Architecture | Pros | Cons |
| ------------- | ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Agentic Only | Excellent at detecting fraud patterns across unstructured notes. | Unaccountable. May approve a claim exceeding policy limits because it *felt right* in context. Misses NAIC-mandated turnaround timestamps. |
| Workflow Only | Reliable. Never pays out more than the policy allows. | Frustrating. Requires the customer to fill out endless structured forms; cannot read a police report or a photo of a fender. |

The agent's gift is reading a hand-written police report and noticing the inconsistency between the driver's account and the officer's diagram. The workflow's gift is knowing, with mathematical certainty, that the policy limit is twenty-five thousand dollars, the claim is for thirty-one, and the state in which this claim sits requires an acknowledgment within fifteen days. Both gifts are necessary. Neither is sufficient.

---

## 18. Prior Authorization (Healthcare)

**The optimal pattern.** An agent synthesizes fifty-plus pages of medical history to identify medical necessity for a procedure against payer guidelines — *generative cognition*. A predictive model scores expected length-of-stay and readmission risk from structured clinical features, informing the medical necessity determination with a quantitative signal the LLM cannot produce — *statistical prediction*. A workflow manages the HIPAA-compliant data flows, enforces the CMS Interoperability and Prior Authorization Final Rule (CMS-0057-F) turnaround mandates that took effect January 1, 2026 — seventy-two hours for expedited (urgent) requests and seven calendar days for standard requests, applicable to Medicare Advantage organizations, Medicaid and CHIP programs, and Qualified Health Plan issuers on the federally-facilitated exchanges — and triggers a HITL gate for nurse review when the model's confidence score falls below a defined threshold — *coordination*.

| Architecture | Pros | Cons |
| ------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Agentic Only | Summarizes patient history in seconds, saving hours of clinical review. | Dangerous. LLMs can miss critical *negative* constraints, like a drug allergy buried in notes. No native compliance with the seventy-two-hour expedited mandate or the FHIR Prior Authorization API requirements (the latter due January 1, 2027). |
| Workflow Only | Compliant. Perfect tracking of privacy consents and regulatory timelines. | Slow. Requires clinicians to manually re-enter data from faxes into structured fields. The H term in the cost formula dominates; clinical hours are the most expensive. |

The negative-constraint failure is the one that haunts every healthcare AI program. A model that summarizes a patient as a *good candidate for the procedure* while missing a single buried sentence noting a fatal allergy is a model that has done the cognitive work brilliantly and the coordination work catastrophically. CMS-0057-F is also unusually clean as a requirement: the regulation gives the architect a deterministic clock (seventy-two hours, seven days), a deterministic API contract (FHIR), and a deterministic public reporting requirement (annual metrics published by March 31). These are not specifications a probabilistic agent can satisfy on its own. They are the rails the cognitive layer runs on.

---

## 19. AML/KYC Investigation (Banking)

**The optimal pattern.** An adaptive alert prioritization model — updating from investigator dispositions on prior alerts — produces a real-time priority score on each alert from the bank's transaction monitoring system, focusing investigator attention where the precision is highest — *adaptive learning*. An agent triages the prioritized alerts, performs cognitive analysis across counterparty relationships and unstructured news sources, drafts the narrative for the Suspicious Activity Report (SAR), and synthesizes evidence for the investigator — *generative cognition*. A workflow enforces the Bank Secrecy Act timing requirements (a SAR must be filed within thirty calendar days of initial detection, extendable to sixty days only if no suspect has been identified), runs deterministic OFAC sanctions screening, manages the case lifecycle including continuing-activity reviews, files the SAR through FinCEN's BSA E-Filing System with the required structure, and maintains the audit lineage required by examiners — *coordination*.

| Architecture | Pros | Cons |
| ------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Agentic Only | Excellent at synthesizing weak signals across noisy data. Drafts SAR narratives at speed. | No native concept of the thirty-day filing clock. No deterministic OFAC match. Cannot be the system of record for a regulated filing. The *zombie alert* problem — alerts that loop in agentic review while the clock runs out — is exactly the failure pattern BSA examiners look for. |
| Workflow Only | Compliant. Files on time, every time. Clean OFAC screening. | High false-positive rate produces analyst burnout. Cannot read free-text counterparty news or unstructured KYC documentation. The investigator's H term dominates. |

The synergy is unusually clean here. The cognitive work — reading news, synthesizing counterparty risk, drafting a SAR narrative that an investigator can defend — is exactly the kind of work LLMs are good at. The deterministic work — the thirty-day clock, the OFAC list, the BSA E-Filing system, the audit trail an examiner reviews — is exactly the kind of work that punishes probabilistic reasoning. An AML program that treats these as a single problem is a program that misses one of the deadlines. An AML program that treats them as the cognitive layer feeding the coordination layer is a program that gets through an examination.

---

## 20. Contract Lifecycle Management

**The optimal pattern.** An agent reviews third-party contracts against the corporate playbook, extracts obligations and deviations, drafts redline suggestions, and surfaces risk language a reviewer would otherwise miss — *generative cognition*. A predictive vendor-risk model scores each counterparty from structured features — financial health, geographic concentration, operational dependency, prior incident history — producing a quantitative risk signal that the obligation review can be prioritized against — *statistical prediction*. A workflow routes contracts through approval hierarchies, enforces signature authority, manages renewal and breach-notification dates, integrates with the contract repository, and maintains the audit trail required by internal controls and (in regulated industries) by third-party risk management frameworks such as the OCC's guidance on third-party relationships in banking or the EU's Digital Operational Resilience Act (DORA) for financial entities — *coordination*.

| Architecture | Pros | Cons |
| ------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Agentic Only | Excellent at extracting obligations and surfacing non-standard language across thousands of contracts in hours rather than weeks. | No durable case state. Cannot enforce signature authority. Renewal dates and breach windows go unmanaged. Vendor-risk reporting becomes ad hoc. |
| Workflow Only | Routing, approvals, and renewal management are deterministic and reliable. | Cannot read the contract. Cannot identify a deviation from the playbook. Cannot draft a redline. The legal-team H term dominates. |

This use case sits at the lower-regulation end of the slate, but it is the case where the temptation to under-architect is strongest. The cognitive lift is so visible — the agent's ability to find a buried indemnity clause, surface a non-standard liability cap, or flag deviation from playbook is genuinely impressive — that the workflow can feel like a secondary concern. The lesson from organizations a year or two into a contract-AI program is consistent: without durable case state, signature authority, and renewal management, the program loses track of which version was signed, when the auto-renewal triggers, and which counterparty has a force-majeure provision worth invoking. The synergy is real. So is the discipline required to place each capability in the right lane.

---

## The Summary Test

When reviewing any proposed AI feature, sort it into one of two buckets.

If it is *cognitive* — reading, summarizing, ideating, classifying intent — use an agent. If it is *coordination* — routing, timing, auditing, spending money, enforcing security — use a workflow. The buckets are not aesthetic preferences. They are architectural commitments.

Most agentic AI failures trace back to a single mistake: asking an agent to do the coordination work that workflows do better. By combining the brain of the agent with the rails of the workflow, the result is a system that is not just smart, but *insurable* and *auditable*.

---

# Part V — Deep Dive: The Mortgage Underwriting Walkthrough

A note before the walkthrough. The first time I watched a saga fire in production — a real one, not a tabletop — was a wire that bounced at 4:47 PM Eastern on a Friday. The closing attorney was on the phone. The applicant was an hour away from being a homeowner. The underwriter had stepped out. The system did not need any of those people to make the next move. It walked the rollback in eleven seconds, posted the offsetting ledger entries, released the capital hold, and presented a screen to the on-call investigator that said, in plain English, what had happened and what the choices were. That moment is what made me believe the architecture in this document. What follows is a walkthrough of the architecture that produces that moment.

The use case is high-complexity mortgage underwriting. The choice is deliberate. Mortgage underwriting involves massive document volumes, strict regulatory timelines under TRID, ECOA fair lending requirements at every decision point, the need for absolute financial accuracy, and — at the end — the irreversible act of moving hundreds of thousands of dollars between institutions.

Three scenarios are described. The first is the optimal hybrid. The second is the agent-only failure mode. The third is the workflow-only friction. The fourth section describes what happens when the wire fails — the funding saga that is the architectural mechanism for *undo* in regulated environments.

---

## 21. Scenario A: The Optimal Hybrid

In this version, the principle holds: cognition belongs to the agent, coordination belongs to the workflow, and statistical prediction sits between them — scoring what the agent reads and feeding what the workflow decides.

| Step | Primary Tool | Pattern & Rationale |
| ---------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Application Intake | Workflow | Activation. The workflow engine receives the payload, assigns a unique Case ID, and enforces RBAC to ensure only authorized users can view the data. The TRID disclosure clock starts here. |
| 2. Document Triage | Agent | Routing (Pattern 2). An agent classifies messy uploads — PDFs, JPEGs of paystubs — to identify what is a tax return vs. a bank statement. |
| 3. Data Extraction | Agent | RAG (Pattern 3). The agent extracts specific line items (Gross Income, Deductions) and provides citations back to the source document for auditor review. |
| 4. Calculation & Logic | Workflow | Deterministic Logic. Debt-to-Income (DTI) and Loan-to-Value (LTV) ratios are calculated by a math engine. This is too high-stakes for a probabilistic LLM. |
| 5. Policy Validation | Workflow | Guardrail Wrap (Pattern 10). The system checks the calculated ratios against current federal and bank-specific lending policies, ECOA fair lending requirements, and HMDA data capture rules. |
| 6. Predictive Risk Scoring | Predictive Model | Statistical Prediction. A credit-risk model — trained offline on historical loan performance, governed under model risk management, lineage-tracked, drift-monitored — scores probability of default from the structured features the workflow has computed. The score is a quantitative input the agent and the human will both see, not a decision in itself. *Cleanest commercial articulation: a model authored in Prediction Studio, embedded into the case workflow through Process AI.* |
| 7. SLA Breach Prediction | Predictive Model | Adaptive / Process AI. A second model continuously predicts whether the case will hit the TRID disclosure deadline given current queue depth, document complexity, and underwriter workload. If breach probability exceeds threshold, the workflow proactively escalates rather than waiting for the deadline to slip. The model retrains on outcomes from prior cases. |
| 8. Risk Synthesis | Agent | Evaluator-Optimizer (Pattern 6). An agent identifies discrepancies — *Income on tax return doesn't match bank deposits* — and drafts a narrative for the underwriter. The narrative explicitly references the credit-risk score from Step 6 and the rationale for any divergence between the model's score and the agent's qualitative read. |
| 9. Final Decision | Human | HITL Gate (Pattern 5). The workflow pauses. A human reviews the agent's synthesis, the workflow's math, and the predictive model's score, then provides the final Approve or Deny signature. ECOA adverse-action timing starts here if denied. |
| 10. Funding/Closing | Workflow | Saga (Pattern 12). The workflow triggers the wire transfer. If the external bank rejects the wire, the workflow automatically releases the hold on the funds. |

The pattern is now visible across three layers, not two. The agent reads, classifies, and drafts. The predictive layer scores risk and predicts SLA breach — quantitative signals from structured features that the agent cannot produce and the workflow alone cannot infer. The workflow calculates, validates, orchestrates, and executes. The human signs.

What makes this design defensible is not that it includes AI. It is that it includes the *right kind* of AI in each step. Generative cognition where ambiguity is the work — reading messy documents, drafting underwriter narratives. Statistical prediction where calibration is the work — credit risk, SLA breach probability. Determinism where consequence is the work — calculating ratios, enforcing policy, executing the wire, capturing HMDA data points, producing the ECOA-compliant adverse action notice. Each probabilism in its proper governance regime, each deterministic step with a deterministic guarantee.

---

## 22. Scenario B: Agent-Only and the Probabilistic Coordination Risk

In this version, an LLM-based agent — using a modern framework with the checkpointing and HITL primitives that have shipped over the past eighteen months — is asked to manage the entire process. Step 1 (Intake), Step 2 (Document Triage), and Step 3 (Data Extraction) perform reasonably, with the caveat that token costs are high because the agent is reasoning about every document at every stage. The framework will save state. It will resume after a failure. It will pause for human review.

The breakdown begins at Step 4.

**Math and Policy.** Agents are probabilistic. A model asked to compute a debt-to-income ratio may round it. A model asked to check a policy may hallucinate an exception because the prompt was slightly ambiguous. Modern frameworks have not changed this. They have made the surrounding state machine more reliable; they have not made the model deterministic. Neither failure is detectable without a separate deterministic check — at which point the deterministic check is doing the work that should have been done deterministically in the first place.

**Statistical Calibration.** The agent-only design has no calibrated credit-risk score. Asking a generative model to produce a probability of default by reading the application as text produces a number that *sounds* confident, but it is not a number that has been trained on historical loan performance, validated against challenger models, or governed under a model risk management framework. The first time the bank's MRM committee asks for the score's lineage, the program discovers it does not have an answer that survives the meeting. The same holds for SLA breach prediction: the framework can detect that a deadline has passed; it cannot, in the absence of a predictive model trained on prior cases, predict that a breach is likely four hours from now.

**SLA and Decision.** Frameworks have begun shipping timer primitives. They are not yet equivalent to native workflow SLA management, particularly around tiered escalation, urgency-based routing, and the automatic adverse-action timing required under ECOA. The TRID three-day disclosure window is not a primitive any framework ships natively. It is a regulatory clock that has to be coded against.

**HMDA Reporting.** The Home Mortgage Disclosure Act requires loan-level data submission with specific field-by-field accuracy. A probabilistic system that *mostly* captures the right data points is a system that fails the annual submission.

**Funding.** This is where the architecture fails most visibly. If the funding call fails, an agent's checkpoint may save the *fact* of the failure but cannot, on its own, walk a deterministic compensating action sequence in the exact reverse of the forward path. The case becomes a *zombie*: money is recorded as sent but never arrived. The bank has a ledger entry that does not match a bank account. The customer has an attorney calling them. The auditor has a question that no one in the room can answer.

The agent-only design produces three structural failure modes: unpredictable latency from multi-agent reflection loops, an audit black box where the trail is a sequence of state snapshots optimized for debugging rather than a flowchart of logic optimized for examination, and an accountability gap where there is no central control plane to point to and say *this is the rule that was followed*.

The first one is annoying. The second is bad. The third is what ends programs.

---

## 23. Scenario C: Workflow-Only and the Rigid Automation Friction

In this version, a traditional deterministic engine is used without any AI or agentic capabilities — and, in the most stripped-down version, without statistical models either.

Step 1 (Intake), Step 4 (Calculation), Step 5 (Policy Validation), Step 9 (Final Decision), and Step 10 (Funding/Closing) execute well. Intake is efficient and secure. Calculations are perfect. SLAs are tracked. ECOA adverse-action notices are generated correctly. HMDA data is captured cleanly. Funding is precise.

The breakdown is at Step 2, Step 3, Step 6, Step 7, and Step 8.

**Triage (Step 2) and Extraction (Step 3).** The system cannot read unstructured documents. The customer or a bank employee has to manually type every line item from their tax returns into a web form. Straight-Through Processing collapses. Every case requires heavy human lifting. A high-quality JPEG of a paystub is a failed case.

**Predictive Risk Scoring (Step 6) and SLA Breach Prediction (Step 7).** Without predictive ML, the underwriter has the calculated DTI and LTV but no calibrated probability of default and no forward-looking SLA breach prediction. Decisions are made on policy thresholds and human judgment alone. The bank's risk team cannot point to a model-driven score in the case file. The operations team cannot pre-empt SLA breaches; they can only react to them after the deadline has slipped. Both gaps are survivable in a low-volume program. Neither is acceptable at the volume of a regulated lender that has to defend its risk discipline to examiners.

**Risk Synthesis (Step 8).** A human has to manually compare the tax returns to the bank records to find discrepancies. The system can only flag what is in the structured database. The agent's gift — noticing that the income on the tax return does not match the bank deposits — is unavailable.

**Development Speed.** Any change to how a document is *read* requires a developer to write new code rather than just updating an agent's prompt. The system is correct, complete, and slow to evolve.

The workflow-only architecture is the safest of the three. It is also the most expensive in human labor (the H term in the cost formula dominates) and the most punishing for the customer.

---

## 24. The Mortgage Funding Saga: When the Wire Fails

In a high-stakes financial services environment, a saga is the architectural mechanism that ensures *undo* is not a luxury but a deterministic guarantee. While an agent might initiate a transaction based on a cognitive insight, the workflow engine manages the *compensating actions* — the sequence of reversals required to walk a process backward when something goes wrong.

Regulators do not allow AI to make irreversible decisions. Sagas make reversibility the default and highlight only the *irreversible-by-exception* cases for human oversight.

What follows is a complete walkthrough of one saga: a mortgage funding event that fails at the last millisecond.

### The Forward Path

The mortgage has been approved. The HITL gate at step seven of Scenario A has fired. A human underwriter has signed off. The system is now executing the closing.

**Step 1 — Asset Reservation (Workflow).** The workflow places a *hard hold* on the bank's capital for the loan amount. This is a deterministic act. A specific four-hundred-thousand-dollar block of bank capital is reserved against the case ID. It cannot be reserved twice. It cannot be released by anyone outside the workflow's authority. The hold is recorded. The audit trail exists.

**Step 2 — Ledger Entry (Workflow).** A deterministic entry is made in the General Ledger as *Pending Disbursement*. The bank's books now reflect the imminent outflow. This entry exists for two reasons. First, the bank's accounting must remain consistent with the operational state. Second, if the wire fails, the entry has to be offset — not deleted — to maintain a perfect audit trail.

**Step 3 — Closing Disclosure Generation (Agent).** An agent synthesizes the final loan terms into a citizen-readable Closing Disclosure (CD). This is the place where cognitive synthesis earns its keep. The terms are technical. The disclosure must be plain English and TRID-compliant. The agent reads the rate, the fees, the schedule, and produces a document that an applicant can actually understand. The workflow validates the generated disclosure against a deterministic checklist before it goes out — every required TRID field present, every required disclosure mentioned, no hallucinated clauses.

**Step 4 — Wire Transmission (Agent via Tool).** The agent uses Pattern 4 — Tool-Using Agent — to invoke the Federal Reserve Wire (Fedwire) API through an MCP-governed tool catalog. The agent does not write the API call. It picks the verb from a governed catalog. The catalog has fixed permissions, fixed retry logic, and fixed rate limits. The agent's only job is to issue the call with the correct case context. The workflow holds the state.

The wire is sent. The system waits.

### The Failure Event

The receiving bank rejects the wire.

The rejection comes back as an HTTP 403 with a payload indicating an Anti-Money Laundering flag. Somewhere in the receiving institution's last-millisecond compliance scan, the destination account triggered a watchlist match. The wire is refused. The funds remain at the originating bank. The closing attorney is on the phone with the title company. The applicant is two hours away from being a homeowner. The workflow now has to do something that no agent can be trusted to do.

It has to walk the process backward, in exact reverse order, with mathematical precision, while leaving a perfect audit trail.

### The Compensating Path

| Rollback Step | Tool | Compensating Action and Rationale |
| ------------------------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Cancel Wire Request | Workflow | Issues a Void command to the Fedwire API. Ensures no duplicate funds are sent if the error was transient and a retry happens to succeed. |
| 2. Void Ledger Entry | Workflow | Creates an offsetting Reversal entry in the General Ledger. The original entry is preserved. The reversal is recorded. The audit trail shows both. The books balance. |
| 3. Release Asset Hold | Workflow | Removes the hold on the bank's capital, making it available for other loans. Prevents *ghost capital* from being locked in a failed case. |
| 4. Failure Investigation | Agent | Analyzes the raw API error code (HTTP 403, AML Flag). Provides a cognitive synthesis to explain *why* the wire failed, in language an underwriter can act on. |
| 5. Escalation Gate | Human | HITL Gate (Pattern 5) fires. A human decides whether this is *Fix and Retry* — perhaps the AML flag is a false positive on a common name — or *Permanent Denial*. ECOA adverse-action timing applies if denied. |

Each step is deterministic. Each step is recorded. The order is the exact reverse of the forward path. There is no point at which the system *tries* to undo anything. The undo is the design.

### Why Both Tools Are Required

| Tool | Role in the Saga | Why It Cannot Do the Other Job |
| ----------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Agent (Cognition) | Understands the reason for the rejection and drafts the explanation for the customer. | If an agent manages the rollback, it might forget to release the ledger entry if it gets distracted by a new prompt. |
| Workflow (Coordination) | Manages the state and ensures every forward action has an equal and opposite reaction. | A workflow cannot understand a cryptic AML rejection note; it only knows the wire failed. |

The agent is the right tool for *interpreting the failure*. The workflow is the right tool for *executing the rollback*. Inverting these roles is the architectural mistake that turns a recoverable failure into a regulatory event.

What makes this saga work is that every compensating action is declared *at design-time*. The bank does not invent the rollback when the wire fails. The bank declared the rollback months earlier, when the workflow was designed. The runtime is deterministic because the design was honest about what could go wrong.

This is what *Accountability Infrastructure* actually means. It is not a slogan. It is the architectural commitment that, regardless of which step fails, the bank's books remain accurate and reversible.

---

## 25. Things to Watch Out For: The Saga Layer

There are four traps in saga design that consistently produce production incidents.

The first is **the missing compensating action**. The temptation, when designing a saga, is to invest fully in the forward path and treat the rollback as a problem to revisit once the happy path is working. The rollback rarely gets revisited. The discipline that prevents this is non-negotiable: every forward action with a side effect must have a compensating action declared at design-time. *Every*. The test is articulability — if the rollback cannot be stated in plain language at design-time, the forward step is not yet finished.

The second is **the *delete versus offset* confusion**. When a ledger entry needs to be reversed, the wrong instinct is to delete it. The right action is to offset it with a reversal entry. The original record is preserved. The reversal is recorded alongside it. The audit trail is intact. A regulator can see that a wire was attempted, that it failed, and that it was reversed — three facts, three entries, no missing data. Deletion destroys the audit trail. Offset preserves it.

The third is **letting the agent manage rollback**. An agent reflecting on a failed wire might decide to retry the call, or to log the error, or to summarize the situation for the underwriter. None of those are the rollback. The rollback is the deterministic execution of compensating actions in exact reverse order. Asking an agent to manage it is asking for the agent to forget step three.

The fourth is **the *zombie case***. When a workflow does not have a complete saga, a failed transaction can become a state that is neither forward nor reversed. Money is recorded as sent. The receiving account does not show it. The case sits in limbo. The architecture that prevents zombies is the architecture that *guarantees* every forward step is either completed successfully or compensated to a clean prior state. There is no *limbo* in a well-designed saga.

The strategic principle is the one that opened Part I and recurs through every section of this document. The agent provides the *brain*: handling the messy, unstructured, creative work. The workflow provides the *rails*: handling the legal, financial, and procedural certainty. A saga is the rails doing what the rails are for.

---

# Part VI — The Ecosystem

The architecture above describes how a single agentic system should be designed. Scaling that architecture across an enterprise — across thousands of cases, hundreds of agents, and dozens of business units — requires an *Accountability Infrastructure* that treats AI not as a collection of standalone bots, but as a governed workforce.

For a bank, the challenge is ensuring that *Agent Sprawl* does not become *Governance Debt* that shuts down the program during its first regulatory audit. The framework below establishes an ecosystem view for the intake, management, and governance of agentic and workflow assets, and notes where the protocol layer (MCP, A2A) has changed what an enterprise has to build itself versus what is now solved at the standards layer.

---

## 26. The Intake Gate: A Triage Matrix

To prevent the misuse of tools — *a hammer for a screw* — every business request should pass through a triage that evaluates two dimensions: *ambiguity* (how messy is the data?) and *statefulness* (how many rules and steps are involved?).

| Dimension | Low Ambiguity (Structured Data) | High Ambiguity (Unstructured Data) |
| ------------------------------ | ------------------------------- | ---------------------------------- |
| Low Statefulness (Single Step) | Standard API / Script | Specialist Agent (Pattern 2/3) |
| High Statefulness (Multi-Step) | Deterministic Workflow | The Optimal Hybrid (Pattern 11) |

The strategic guidance is short and uncomfortable. If the request involves *moving money, legal liability, or external regulatory reporting*, the workflow owns the state — even if an agent performs the cognitive tasks within those steps.

The triage is the front door of the ecosystem. Skipping it is how an organization ends up with three separate *smart classification agents* doing the work that one routing rule could have done in milliseconds for a fraction of the cost.

---

## 27. Managing the Population: The Control Plane

To solve agent sprawl, the bank must implement a centralized control plane. This is not a repository. It is the system of record for the agentic workforce. Note a centralized control plane can and likely should consist of network of federated control planes that account for all agentic capabilities, but allows management grouped by capability categories.

A working control plane has three components.

The first is a **specialist roster**. Every agent must be registered with a clear scope of work and an authority limit. *Authority limit* is the operative phrase. An agent that can read documents is not the same kind of asset as an agent that can move money. The roster makes the difference visible and enforceable.

The second is a **versioned rubric library**. The rubrics used for Evaluator-Optimizer (Pattern 6) cycles must be corporate assets. *Good* must be defined consistently across the bank. A rubric that varies by team is a rubric that produces inconsistent risk decisions. A rubric that lives in a single developer's laptop is a rubric that disappears when the developer changes jobs.

The third is a **governed tool and agent catalog**. The tool catalog component is now substantially a protocol-level concern: MCP gives the enterprise a vendor-neutral specification for how agents discover and invoke tools, and the Agentic AI Foundation maintains the standard. What remains an enterprise responsibility is *which* MCP servers are exposed, *which* permissions and rate limits apply, *which* OAuth or enterprise-IdP flows are required, and *how* invocations are logged for examination. Similarly, A2A gives the enterprise a vendor-neutral specification for how agents talk to each other; what remains an enterprise responsibility is *which* agents are trusted, *what* Signed Agent Cards are accepted, and *what* case-folder discipline applies to inter-agent handoffs. Protocols have closed the integration surface. Governance has not migrated. It still lives at the control plane.

The control plane is the bank's defense against scale becoming chaos.

---

## 28. The Quality and Safety Mesh

Quality management at the ecosystem level moves from *prompt tuning* to *systemic monitoring*.

A **global guardrail service** (Pattern 10) sits in front of all model interactions. *Input guardrails* block prompt injection and PII leaks before they reach the model. *Output guardrails* flag hallucinations and policy violations before they reach the customer. The trip rate of the guardrail service is itself a signal. Rising trip rates are early warning of model drift, prompt injection campaigns, or upstream data quality problems.

**Diagnostic disagreement** (Pattern 7) is used at high-stakes decision points. Three models score a risk. They disagree. The system does not average them. It trips a HITL gate. Disagreement is a signal that the case is hard. Hard cases deserve human attention.

The **token tax audit** is the ongoing review of cost-to-value ratios. The TCO formula in §8 is the discipline. If an agentic chain costs orders of magnitude more than a deterministic rule and produces only marginally better accuracy, the chain is a candidate for *agent-offloading* — moving the work back to the workflow. The point is not that agents are too expensive. The point is that agents must be priced honestly against the deterministic alternatives. Without that audit, the bank is paying *cognitive tax* on routing decisions that a rule could have made for fractions of a cent.

The mesh is the difference between *operating* the system and *running* it.

---

## 29. Governance: The Design-Time / Runtime Split at Scale

Pattern 11 was introduced earlier as the most critical architectural split for a single system. At the ecosystem level, it becomes the governance constitution.

**Design-time sovereignty** is where AI is allowed to be creative. Use it to generate workflows, draft code, simulate edge cases, and propose policy changes. All creative work happens here, under the review of bank architects and risk officers.

**Runtime determinism** is where AI is allowed to be useful but not autonomous. The workflow engine executes the *vetted artifact*. The model is not allowed to rewrite the bank's lending policy or routing logic live in production. *Vetted* is the operative word. Anything that touches a real customer has been reviewed by the people accountable for it.

The **saga registry** is the ecosystem-level artifact that makes reversibility a corporate asset. Every business unit must maintain a registry of compensating actions for every side-effectful agentic action it owns. If an autonomous system initiates an insurance payout, the registry must show exactly how that action is reversed if the process aborts. The registry is what an auditor reads when they ask, *How do you undo a failed step?*

---

## 30. The Five Ecosystem Mandates

The full ecosystem framework reduces to five mandates. They are listed not as a checklist but as a set of architectural commitments that, taken together, define what *production-grade* means for agentic AI.

**Never delegate coordination to an agent.** Coordination is a millisecond-speed, deterministic task for workflows. Asking an agent to do it is paying expensive tokens for cheap rule execution.

**Mandate forced security.** Use authoring environments where RBAC and audit lineage are impossible to skip. The security posture is the environment, not a feature added later by a careful developer.

**Audit the thinking, execute the rule.** Keep a transcript of agent reasoning for compliance, but use a deterministic engine for the final decision. The two are complementary. The transcript explains the reasoning. The rule provides the defense.

**Manage agents as specialists, not generalists.** Avoid *God-mode* agents. Build small, focused agents and use Orchestrator-Workers or Delegation patterns to coordinate them. The specialist roster is the organizational reflection of this principle.

**Reversibility is the default.** If a process cannot be undone via a saga, it requires a synchronous human approval before execution. The saga is the architectural mechanism that turns *undo* into a guarantee. The HITL gate is the safety net for the cases the saga cannot cover.

---

## 31. Things to Watch Out For: The Ecosystem Layer

Three failures are common when programs scale past their first three production agents.

The first is **agent sprawl**. Without a control plane, agents proliferate. Every team builds its own classification agent, its own document reader, its own summarizer. The result is eleven agents doing the work of one specialist roster, with no shared rubric, no shared tool catalog, and no shared audit trail. The protocol layer helps — MCP-compliant tools are at least discoverable across teams — but discoverability is not governance. The wisdom that has emerged from organizations further down this path is to build the control plane *before* the third agent goes to production. After the third, the control plane becomes a retrofit. After the tenth, it becomes a rewrite. The cost curve is steep enough that early investment pays itself back many times over.

The second is **governance debt**. Agentic systems prototype quickly. They productionize slowly. The gap between *it works in the demo* and *it survives the audit* is filled with security work, audit logging, RBAC integration, and saga design. The pattern across programs that have measured this carefully is consistent: governance work deferred to *after* launch tends to come due all at once, often during the first regulatory examination. Programs that have absorbed this lesson treat governance as part of the build, not as a follow-on phase.

The third is **the *we will get to determinism later* trap**. The temptation is real and entirely understandable: the agentic prototype demonstrates value early, the pilot succeeds, and the deterministic guardrails sit on the backlog while momentum carries the system into production. The lesson the field has learned, often from programs that operated for months before a first incident exposed the gap, is that the cost of retrofitting determinism is dramatically higher than the cost of designing it in from the first commitment. Programs that have been through this cycle now treat determinism as a precondition for the pilot, not a phase that follows it.

---

# Conclusion

The argument of this document can be reduced to a single sentence. Lead with the workflow; let the agent serve it.

That sentence contains a philosophy, a comparison, a pattern library, five use cases, a detailed financial services walkthrough, a governance framework, and a cost formula. Each layer is built on the same architectural commitment: cognition is for agents, coordination is for workflows, and the most expensive enterprise AI mistake is confusing the two.

The case is not that agents are dangerous. Agents are extraordinary. They read messy documents, summarize fifty-page medical histories, draft underwriter narratives, draft SAR narratives a BSA examiner can defend, and surface contract risk language a reviewer would otherwise miss. The cognitive layer they unlock has no precedent in enterprise software. To exclude them from the architecture is to leave that capability on the table.

The case is also not that workflows are old-fashioned. Workflow engines have been the backbone of regulated enterprises for two decades for a reason. They handle SLAs in milliseconds, route a thousand-branch decision without losing the thread, enforce RBAC at every step, and produce an audit trail a regulator can read. Modern workflow platforms let domain experts narrate a process in plain English while the system enforces enterprise standards on the resulting design — and the same platforms have already absorbed predictive and adaptive machine learning as governed first-class citizens, which is the infrastructure most agentic-only programs are still building. The historical complaint that workflow platforms were *hard* belongs to the architectural era they outgrew.

The case is that the three layers are complementary. The agent provides the brain. Statistical AI sits between them — scoring what the agent reads and feeding what the workflow decides. The workflow provides the rails. Together, they produce systems that are *smart enough to understand the customer, calibrated enough to score the risk, and disciplined enough to pass an audit*. Any of the three alone produces a system that fails one of those tests.

The architectural discipline is to know which is which.

I started this document with a wire that bounced at 4:47 PM Eastern on a Friday, an attorney on the phone, and an applicant an hour from being a homeowner. The reason that story has resonated with me is that nobody in the situation had to be a hero. The system did not need a phone call to a developer. It did not need a manual ledger correction. It did not need a regulator to be told, weeks later, what had happened. It did the right thing, in the right order, in eleven seconds, and presented the next decision to the right person on a screen. That is what an agent and a workflow look like when they have been put in their proper lanes.

Insight belongs to the machine. Decisions belong to the human. The workflow is what makes that arrangement enforceable on the worst Friday afternoon of the year.

---

# Appendix: Products and Trademarks Referenced

This document references commercial products and open standards to illustrate architectural patterns. Inclusion of any product name is for architectural illustration and does not constitute endorsement. Product names are the trademarks of their respective owners.

## A note on the document's vantage point

The author's professional work is concentrated in the Pega ecosystem, which is why the Pega portfolio is the most extensively referenced commercial example in this document. The architectural patterns described, however, apply across the workflow and BPM platform category. Other enterprise platforms have introduced relevant agentic, predictive, adaptive, and workflow capabilities — see *Other Workflow and BPM Platforms* below. Where Pega is named in the prose, it is because it is the platform the author works in most closely, not because it is the only platform with the relevant capability.

## Pegasystems products referenced

The following Pegasystems Inc. trademarks are referenced in this document: Pega®, Pega Infinity™, Pega Blueprint™, Pega Predictable AI™, Pega Predictable AI Agents™ (including Design Agents, Conversation Agents, Automation Agents, Knowledge Agents, and Coach Agents), Pega GenAI™, Pega GenAI Knowledge Buddy™, Pega Customer Decision Hub™, Adaptive Decision Manager™, Pega Process AI™, Pega Agentic Process Fabric™, Prediction Studio™, and Pega Coach™. Pega is a registered trademark of Pegasystems Inc. Additional Pegasystems trademarks may apply.

## Other workflow and BPM platforms

Several enterprise workflow, business process management, and AI platforms have introduced capabilities relevant to the architectural patterns discussed in this document. The list below is not exhaustive and is intended as a neutral acknowledgment of the broader landscape: Appian, Camunda, IBM (including Watsonx Orchestrate and Cloud Pak for Business Automation), Microsoft (including Power Automate and Copilot Studio), Salesforce (including Agentforce and Flow), and ServiceNow (including Now Assist and AI Agents). Each of these platforms has introduced AI-augmented workflow capabilities in 2025-2026 that intersect with the patterns in this document. Their respective product names are the trademarks of their respective owners.

## AI frameworks and standards referenced

The following frameworks, SDKs, and protocols are referenced in this document and are the trademarks of their respective owners: LangGraph (LangChain, Inc.), OpenAI Agents SDK (OpenAI), Claude Agent SDK (Anthropic), CrewAI (CrewAI Inc.), AWS Strands Agents (Amazon Web Services), Google Agent Development Kit / ADK (Google).

The Model Context Protocol (MCP) is governed by the Linux Foundation's Agentic AI Foundation. The Agent2Agent Protocol (A2A) is an open protocol with multi-vendor governance under the Linux Foundation.

## Regulatory and standards references

References to specific regulations and standards in this document — including but not limited to TILA-RESPA Integrated Disclosure (TRID), Equal Credit Opportunity Act (ECOA), Home Mortgage Disclosure Act (HMDA), Bank Secrecy Act (BSA), FinCEN reporting requirements, Office of Foreign Assets Control (OFAC) sanctions screening, NAIC Unfair Claims Settlement Practices Acts, CMS Interoperability and Prior Authorization Final Rule (CMS-0057-F), FHIR, OCC third-party risk management guidance, and the EU Digital Operational Resilience Act (DORA) — are intended for architectural illustration. Specific regulatory requirements, effective dates, thresholds, and applicability vary by jurisdiction, institution type, and program. Readers should consult primary regulatory sources and qualified legal counsel for any application of these regulations to their own programs.
