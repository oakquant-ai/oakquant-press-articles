*A complete architectural reference for OakQuant: the bounded-agent platform behind the Financial DNA Protocol, the Cambium coherence engine, and the seven services that together turn identity-as-static into identity-as-relationship. Written for systems engineers, architects, and the next person to extend the platform.*

## Overture: Why an Architecture Paper Now

OakQuant began as a question. If financial advice is going to be reshaped by language models — and it is, whether we like it or not — what would a responsible architecture look like? Not a chatbot bolted onto a robo-advisor. Not a closed black box trained on consumer data. Not yet another agent framework that gives a model latitude to call tools and hopes for the best.

The answer that emerged is the one we now run in production: a platform composed of seven services, each owning a clear responsibility, with a deterministic spine standing between the model and every decision it touches. The model has genuine reasoning latitude inside narrow questions. It never decides what question to ask. It never decides whether its answer counts. Those decisions belong to code, configuration, and evaluation — all three auditable.

This paper is the architectural reference for that platform. The intent is twofold. First, to document for the team and future contributors how OakQuant is actually built, so that ten years from now someone reading this can pick up the system without re-discovering the load-bearing decisions. Second, to publish — frankly — what works, so that other teams building bounded-agent platforms can borrow the parts that fit and improve on the parts that don't.

The structure is deliberate. Part I introduces the thesis and the cast of services. Part II walks the architectural patterns that shape every service. Part III takes each service in turn — its responsibility, its substrate, its seams. Part IV explains the three configuration substrates that hold the system together. Part V is the data flow: identity capture to weekly insight, with every hop traced. Part VI documents the audit boundary and the bounded-agent contract. Part VII covers the evaluation and regression CI that decides which manifest can run in production. Part VIII addresses cross-domain extension and what the next plug-in looks like. Part IX is the trade-off log — what we chose, what we rejected, and why. Part X looks at what is still open. Part XI gives the security and access-control model.

A note on style. This is an architecture paper, not a marketing piece. Where a decision is contestable, I say so. Where the spec drifted from the implementation, I correct the spec and document the drift. Where a substrate is partial, I document the gap. The platform is real software; treating it otherwise serves nobody.

-----

# Part I — Platform Thesis and Cast

## 1. The Bounded-Agent Thesis

A bounded-agent system addresses three failure modes that have, individually and together, defined the last three years of agentic AI in production. First, models decide things the system should never have let them decide. Second, models are asked questions outside their competence. Third, the system has no way of knowing whether the model is wrong.

OakQuant's response is the three-part shorthand that every contributor on the team can recite:

> The deterministic spine decides what to ask. The model only answers. The eval framework decides whether the answer counted.

Each clause is operative. The deterministic spine is real code in `cambium/core/spine.py`. The model is genuinely unbounded inside the narrow question it is asked — it can cite any dimension in the permitted set, frame the justification any way it likes, propose any adjustment magnitude up to a clipped ceiling. The eval framework is the only path to changing what the model is asked: a candidate manifest passes through regression CI against hand-labeled golden examples; PROMOTE, HOLD, or BLOCK is the gate; only PROMOTE survives.

The thesis has three architectural consequences that show up everywhere in the platform:

$$
\text{Authority} = (\text{Spine}, \text{Model}, \text{Eval}) \to \text{Decision}
$$

The model is one input. It is never the decider. This is the load-bearing commitment that lets the platform pass institutional audit, that lets us publish provenance certificates, and that lets us swap models without re-writing prompts. Every other design choice in this paper falls out of it.

## 2. The Seven Services

OakQuant is composed of seven services plus two content systems. Each service owns a clear responsibility. Each service has a substrate it cannot replace and a seam it can.

| Service | Role | Substrate | Seam |
|---|---|---|---|
| **Sky** | React 19 frontend with A2UI rendering | Vite, Redux Toolkit, RTK Query, Tailwind v4 | Talks only to Canopy over HTTPS with bearer tokens |
| **Canopy** | FastAPI integration layer / API gateway | FastAPI 0.119, Authlib OAuth 2.0, httpx | Single integration point for Sky; proxies to Grove/Acorn/Cambium |
| **Acorn** | Multi-agent reasoning service | FastAPI, FastMCP, LangChain/LangGraph | Hosts MCP tools; runs the Vanguard/Oracle/Catalyst stack |
| **Grove** | Celery workflow orchestration | Celery 5.4, FastAPI, Redis, python-statemachine | Owns workflow registry, task action registry, HITL gates, schedulers |
| **Cambium** | Bounded-agent behavioral coherence engine | Anthropic SDK + timber-common consumer | Library + MCP server; runs the three bounded call sites |
| **Timber** | Shared persistence + encryption + vector library | SQLAlchemy 2, pgvector, FastEmbed, Fernet | PyPI library consumed by Grove, Canopy, Acorn, Cambium |
| **Ranger** | Control plane: config, secrets, audit, tenancy | FastAPI, SQLAlchemy 2, Redis, PostgreSQL RLS | Holds versioned typed config; emits change events; backs HITL |
| OakQuant Press | Publishing engine | Next.js 15 + FastAPI | Renders this paper, among others |
| OakQuant Press Articles | Content repo | Markdown + YAML frontmatter | Where articles live |

The first seven are the platform. The last two are how the platform documents and publishes itself. They are not part of the runtime but they are part of the architecture; the existence of this paper is itself a load-bearing piece of the program.

> **Sidebar: Why seven services and not three or thirty?**
> The split is functional, not arbitrary. Each service owns a distinct authority: Sky owns rendering, Canopy owns the gateway, Acorn owns agentic reasoning, Grove owns orchestration, Cambium owns bounded-agent coherence, Timber owns persistence, Ranger owns control-plane state. Combining two would mix authorities; splitting one would dilute responsibility. Three years of iteration converged on this set, with `farm` (a candidate eighth service for the data lake) deferred until the platform's data volumes demand it.

## 3. Cast Map

A picture is worth the words it saves. The relationship between the services is best read as authority flow:

![OakQuant cast map](figures/cast-map.svg "Cast map — seven services arranged by authority direction.")

The arrows are not API calls — they are *authority*. Sky asks Canopy; Canopy asks Acorn, Grove, or Cambium; Grove orchestrates Cambium; Acorn delegates MCP tool calls to Cambium; everything reads from Timber and Ranger. Cambium is the only service that talks to the model. Ranger is the only service that holds the canonical config rows. Timber is the only library every service consumes.

Inverting any of these arrows breaks an invariant. Sky talking to Grove directly bypasses authentication. Grove talking to a model directly violates the bounded-agent thesis. Ranger reading from another service hides the configuration source-of-truth.

-----

# Part II — Architectural Patterns

The platform encodes a small number of repeated patterns. They show up across every service, and once you see them, the structure of the codebase becomes legible.

## 4. The Thirteen Agentic Patterns and Where They Live

Cambium and Grove between them implement thirteen patterns I have described in earlier writing as the canonical set for agentic-AI systems. Cambium owns the patterns that govern what happens *inside* a bounded call. Grove owns the patterns that govern what happens *between* calls and across time. A few cross both.

| # | Pattern | Owner | Where it lives |
|---|---|---|---|
| 1 | Prompt Chaining | Cambium | `cambium/core/spine.py::Spine.process_event` — classifier → reasoner → critique → synthesizer |
| 2 | Routing | Grove | Workflow-level domain dispatch via Celery routing keys |
| 3 | RAG | Both | Grove owns the vector store; Cambium consumes retrieval via the service abstraction |
| 4 | Tool Use | Both | Grove owns tool execution; Cambium calls tools via context-enrichment, never via the model |
| 5 | Evaluator-Optimizer | Cambium | Constitutional self-critique on the reasoner; multi-judge consensus on the synthesizer |
| 6 | Parallel Fan-Out | Grove | Celery `group` for concurrent classification across events |
| 7 | Orchestrator-Workers | Grove | Grove is the orchestrator; Cambium plugs in as a worker via `GroveAdapter` |
| 8 | HITL Gate | Grove | Pending-review states; the four Cambium HITL gates (manifest promotion, active-learning label, drift review, user-feedback review) |
| 9 | Delegation / Handoff | Grove | Cross-domain handoff events orchestrated by Grove; MCP server interface for cross-system delegation |
| 10 | Guardrail Wrap | Cambium | Cambium *is* the canonical implementation — declarative rules, content policy, referential integrity |
| 11 | Design-time → Runtime | Both | Manifest content-hashed at design time; promoted to runtime by regression CI; Grove workflows compiled from DB-stored Pydantic-validated definitions |
| 12 | Saga | Grove | Compensations registry (`grove/domains/workflow/compensations.py`); Cambium exposes idempotent inverses |
| 13 | Creative Sandbox with Deterministic Exit | Cambium | The reasoner is the sandbox; the spine is the exit |

The split is not arbitrary. *Cambium owns patterns whose discipline is about what the model can do inside one call. Grove owns patterns whose discipline is about what happens across calls.* Trying to do pattern 10 (Guardrail Wrap) inside a Grove workflow would scatter validation across YAML; trying to do pattern 6 (Parallel Fan-Out) inside Cambium would mix orchestration with reasoning. The two services together cover the surface; their seam is the `GroveAdapter` plus the typed events module.

## 5. The Design-time → Runtime Pattern (in detail)

Pattern 11 is the meta-pattern that both layers implement. At each altitude, the shape is the same: a design-time artifact passes through an eval-time gate, becomes a runtime artifact, and is verifiable after the fact.

In Cambium:

$$
\text{Manifest}_{\text{design}} \xrightarrow{\text{regression CI}} \text{Manifest}_{\text{runtime}} \xrightarrow{\text{insight + provenance cert}} \text{Verifiable}
$$

In Grove:

$$
\text{WorkflowDef}_{\text{design}} \xrightarrow{\text{schema validation}} \text{WorkflowDef}_{\text{runtime}} \xrightarrow{\text{session run + trace}} \text{Verifiable}
$$

The Manifest is content-hashed: identical content produces identical hash, independent of creation timestamp. The hash flows into every artifact (trace rows, certificates, evaluation reports). A WorkflowDefinition is Pydantic-validated against a strict schema with hard invariants — initial-state existence, transition reachability, absorbing-state presence, template-scope validity for `${scope.path}` references. The validation runs at upsert time; production never sees an invalid workflow.

> **Sidebar: Content-hashing for non-cryptographers**
> A manifest's content hash is just SHA-256 over the JSON-canonical form of its fields, with timestamps excluded. The point is not security — it is *identity*. Two manifests with identical content are the same regardless of when they were created. A trace row pointing at a manifest hash unambiguously identifies which version produced it. Provenance certificates inherit this identity for free.

## 6. The A2UI Protocol

OakQuant uses a typed, surface-independent UI protocol called A2UI (Agent-to-UI). Instead of producing rendered HTML or chat strings, Cambium and Acorn emit structured directive documents that any surface can render. The directive carries a `kind` discriminator and a typed `spec` payload.

Three things make A2UI worth the abstraction. First, the audit boundary stays at the document, not at the rendered string. A provenance certificate references an A2UI document. When a user reports "this insight landed wrong," we know exactly what they saw, because we can re-render the same document. Second, surfaces (Sky, email, mobile, voice, Slack) evolve independently — each implements its own renderer; Cambium and Acorn keep emitting the same typed document. Third, the protocol is small enough to be canonical: there are no surface-specific opt-outs, no fallback "raw HTML" escape hatches, no rendered-string passthrough.

A2UI is contract-shared across three repositories in lockstep. The Pydantic source of truth lives in `cambium/a2ui/components.py`. Acorn's `models/a2ui.py` mirrors it server-side. Sky's `src/components/a2ui/types.ts` provides the TypeScript twins; `src/components/a2ui/blocks/` holds the React components; `CambiumInsightRenderer.tsx` dispatches on block `type`. The procedure for updating the contract is encoded as a developer skill (`cambium-update-a2ui-contract`) that mechanically walks a contributor through the three-repo change.

| Block type | Role |
|---|---|
| `text` (with role: headline / subhead / body / caption / meta) | Prose at a specific role within the document |
| `dimension_badges` | A row of dimension chips with weight + polarity |
| `event_references` | A list of cited events with directional signs |
| `suggested_actions` | Reflect / act / ask prompts |
| `feedback` | Surface-native feedback affordance |
| `provenance` | Manifest-hash chip plus a verify affordance pointing at the certificate |

The directive `kind` is either `render_workflow_screen` (Acorn's original — used for Grove HITL screens) or `render_cambium_insight` (added with the Cambium rollout). Adding a third kind is a contract change requiring the skill procedure.

## 7. The Service Abstraction

Every Cambium configurable parameter — domain config, eval thresholds, feature flags, model overrides, redaction policy, manifest, identity, trace storage — flows through a single `CambiumService` Protocol with four implementations:

| Implementation | Use | Substrate |
|---|---|---|
| `InMemoryCambiumService` | Tests, demos | In-process dicts |
| `YamlCambiumService` | Local dev | File-system YAML configs |
| `TimberCambiumService` | Production | Timber-common repositories + rules + keystore + vectors |
| Ranger-driven service | Operator | Reads from Ranger's `ConfigStore`, subscribes to pubsub for invalidation |

The pattern is Pattern 7 (Orchestrator-Workers) applied to configuration: the spine is the orchestrator; service implementations are workers. The spine never reads environment variables, never reads files, never knows whether the configuration came from a YAML on disk or a Postgres row or a Redis cache. This is what lets the same Cambium core run in tests, in development, in production, and in Ranger's operator console without code change. Environment variable `CAMBIUM_SERVICE_IMPL` picks the implementation at boot.

## 8. Lockstep Schemas

The platform has *three* canonical schema substrates. Each lives in exactly one place and the others reference it. Drift is treated as a bug.

| Substrate | Canonical location | Mirrors |
|---|---|---|
| **Workflow definition** | `timber/common/schemas/workflow/` (Pydantic v2) | Stored in DB column `grove_workflow_configs.config_content`; consumed by Grove via `WorkflowRegistry` |
| **A2UI directive** | `cambium/a2ui/components.py` (Pydantic v2) | Mirrored in `acorn/models/a2ui.py`; mirrored in `sky/src/components/a2ui/types.ts` |
| **Cambium types** | `cambium/core/types.py` (Pydantic v2) | Mirrored as Pydantic validators in `ranger/config/validators/cambium_*.py`; consumed by every service via library import |

Notice the pattern: the canonical location is a schema package; the substrates that consume it inherit the shape verbatim. There are no XML schemas, no JSON Schema files, no protobufs. Pydantic v2 is the schema language. Sky's TypeScript twins are hand-maintained but the developer skill enforces lockstep updates.

-----

# Part III — The Services in Detail

This part takes each service in turn. The goal is for an engineer joining the team to read one of these sections and understand what the service is, where to find the load-bearing code, and what the seams to other services look like.

## 9. Sky — React Frontend

Sky is the user-facing surface. React 19, TypeScript 5.9, Vite 7, Tailwind v4. Redux Toolkit with RTK Query is the only state library; each Grove v3 domain has its own RTK slice (`agent-api`, `execution-api`, `workforce-api`, `intel-api`, `analytics-api`, `realtime-api`, etc.) registered in `src/store/store.ts`.

Sky talks only to Canopy. The base URL is `VITE_CANOPY_API_URL` (default `http://localhost:5010`). Every RTK Query slice attaches `Authorization: Bearer <token>` via `tokenService`. OAuth 2.0 authorization code flow runs against Canopy's `/api/oauth/*` endpoints; the token lives in `tokenService`; the React `AuthProvider` gates `ProtectedRoute` and `PublicRoute`.

The A2UI renderer lives at `src/components/a2ui/`. `A2UIRenderer.tsx` is the top-level dispatcher — it switches on `directive.kind` and falls back gracefully on unknown kinds. Two kinds today: `render_workflow_screen` (Acorn's Grove-form rendering) and `render_cambium_insight` (added with Cambium). Six block renderers under `src/components/a2ui/blocks/` cover the Cambium spec; each component is a standalone React 19 functional component using `GlassPanel` / `GlassButton` design primitives from `src/components/glass/`.

The chat surface is `src/components/chat/AgentChat.tsx`. Each `ChatMessage` may carry an `a2ui?: A2UIDirective` rendered inline below the message; `handleDirectiveResult` appends a new assistant turn when the submit returns `nextDirective`. This is how the platform supports multi-step interactive flows without committing to a specific UI choreography in the model.

## 10. Canopy — FastAPI Integration Layer

Canopy is the gateway. FastAPI 0.119 on port 5010. The README phrasing — *"Unified API Gateway, Single integration point for all backend capabilities"* — is the architectural intent.

Canopy is FastAPI, not Flask. (Early Cambium drafts misnamed it; corrected in this paper.) The structural shape: `app.py` registers routers (`health`, `dashboard`, `oauth`, `oauth_admin_api`, `api_endpoints`, `grove_router` (84KB!), `acorn_router`, `docs_router`, `user_admin`, `user_api`, `external_router`, `grove_ws_router`, `cambium_router`). Service-to-service calls go through `clients/`: `clients/acorn_client.py` (httpx + `X-Acorn-API-Key`), `clients/grove_client.py` (split legacy v2 + v3), `clients/ranger_client.py`, `clients/cambium_client.py`, `clients/external_client.py`.

Auth is OAuth 2.0 (Authlib + Timber's `oauth_service`). The server endpoints live in `routers/oauth.py`. Canopy is the IdP for the platform; Ranger verifies tokens via `pyjwt[crypto]` against Canopy's signing key. User context flows downstream via `X-User-Id` / `X-User-Email` / `X-User-Name` headers, not in the JWT.

A deliberate non-choice: Canopy is not an MCP client. Every cross-service call uses httpx with a service-API-key header. MCP is Acorn's territory (Acorn runs FastMCP and is the LLM tool consumer). When Sky sends a chat turn that needs Cambium tools, the path is Sky → Canopy → Acorn → Cambium MCP tool. Canopy never invokes MCP directly. This choice keeps Canopy thin, predictable, and free of agent state.

## 11. Acorn — Multi-Agent Reasoning Service

Acorn is the agentic-reasoning service. FastAPI + FastMCP on port 5020. Python 3.13. The agents are:

- **Vanguard** (`agents/vanguard_orchestrator.py`) — top-level dispatcher. Replaced an earlier LangGraph state machine because the four-node graph always ended at Oracle anyway.
- **Oracle** (`agents/oracle_agent.py`) — financial analysis with eleven typed tools (advice, portfolio holdings, market outlook, etc.). The Oracle directly consumes Timber's vector service (`from common.services.vector.search import vector_search_service`) — library pattern, no HTTP.
- **Catalyst** — synthesis / formatting. (Removed entirely as a post-LangGraph simplification; the system prompt for Oracle now covers the formatting role at lower token cost.)

The tool registry lives in `tools/` with the `@tool` decorator. `register_all_tools(mcp)` walks the registry and publishes each tool to FastMCP behind layered RBAC → rate-limit → cache → audit wrappers. With Cambium, four new tools landed: `cambium_get_coherence`, `cambium_get_weekly_insight`, `cambium_verify_certificate`, `cambium_list_domains`. Each calls Cambium via `clients/cambium_client.py`.

Acorn also owns post-turn telemetry. `services/signal_collector/` captures one `RequestSignal` per turn — `query_embedding` (1024-dim FastEmbed BAAI/bge-base-en-v1.5), tools used, outcome, response length — and writes them to Valkey with 90-day TTL. PII redaction lives in `services/signal_collector/pii_filter.py` (`hash_user_id`, `scrub_query`, `redact_args`). Cambium reuses these helpers when forwarding identity hashes.

The A2UI Pydantic models live in `models/a2ui.py`. As of the Cambium drop, the directive `kind` accepts both `render_workflow_screen` and `render_cambium_insight`. The Acorn copy is contract-mirrored from `cambium/a2ui/components.py`; the developer skill `cambium-update-a2ui-contract` keeps the three repos in lockstep.

## 12. Grove — Celery Workflow Orchestration

Grove is the workflow engine. Celery 5.4 + FastAPI + Redis + Postgres. The v3 architecture is DB-config-driven with YAML fallback — workflow definitions live in `grove_workflow_configs.config_content` and are validated against `WorkflowDefinition` (a Pydantic schema in timber-common's `common/schemas/workflow/`). The legacy v1/v2 layer in `workflows/engine/` is being phased out.

The key extension surfaces are:

- **Workflow registry**: `grove/domains/workflow/registry.py` — tenant-aware lookup; resolution order DB → file.
- **Task action registry**: `grove/domains/tasks/actions/__init__.py::_register_builtin_actions()` — register custom action handlers via `register_action_handler(name, BaseAction)`.
- **Celery task registration**: `register_*_celery_tasks(celery_app)` from each domain's `celery_tasks.py`. Cambium added `register_cambium_celery_tasks` to expose `cambium.process_event` and `cambium.synthesize_window` as Celery tasks.
- **Typed events**: `grove/domains/events/cambium.py` (new with Cambium) — eight typed Pydantic event classes that Grove subscribes to via `subscribe_cambium_events` on Redis `cambium.*` channels.
- **Saga compensations**: `grove/domains/workflow/compensations.py` (new with Cambium) — `register_compensation(name, fn)` for idempotent inverse operations.
- **HITL gates**: `grove/domains/tasks/actions/human_tasks/cambium_gates.py` (new with Cambium) — four gate actions (manifest promotion, active-learning label, drift review, user feedback review) that pause workflows via the existing `HumanTaskRequired` mechanism.

The state machine is config-driven. `WorkflowDefinition` carries a state map, each state's `transitions` evaluated procedurally inside `WorkflowExecutor`. Transition types are `ALWAYS`, `CONDITIONAL`, `ON_SUCCESS`, `ON_ERROR`, `ON_EVENT`. Conditional transitions evaluate a string expression in a sandboxed `eval()` with `{'__builtins__': {}}` — limited but safe. State types are `NORMAL`, `TERMINAL`, `ERROR`, `WAITING`. Workflow validation enforces at least one absorbing state; production never sees an unreachable workflow.

The Financial DNA Protocol — 16 screens of behavioral-finance assessment — lives entirely in Grove as `config/workflows/financial_dna_evaluation_workflow.yaml`. Each screen is a state with a `human_task` UI definition; the workflow runs 8-12 minutes in the user's session; the captured `FinancialDNASession` is the raw output. Cambium's `cambium/domains/finance/identity.py::from_grove_session` reduces the session payload into the `IdentityProfile` that Cambium consumes. This split is load-bearing: identity *capture* is workflow (Grove); identity *consumption* is bounded reasoning (Cambium).

## 13. Cambium — Bounded-Agent Coherence Engine

Cambium is the youngest service and the most architecturally distilled. PyPI package `cambium`; Python 3.13; consumes timber-common, anthropic, pydantic, PyYAML. The whole library fits in roughly five thousand lines of code; the discipline is in the architecture, not the volume.

The component map:

```
cambium/
├── core/                # Domain-agnostic spine, types, infrastructure
│   ├── types.py             # IdentityProfile, BehavioralEvent, ...
│   ├── domain.py            # DomainConfig, DomainRegistry, BaselineScorer
│   ├── spine.py             # Spine — the orchestrator
│   ├── redaction.py         # PII redaction with reversible mapping
│   ├── trace.py             # Append-only trace recorder
│   ├── calibration.py       # Brier, ECE, reliability bins
│   ├── provenance.py        # Signed certificates
│   ├── mcp_server.py        # MCP tool interface (six tools)
│   └── otel.py              # OpenTelemetry hooks (no-op fallback)
│
├── prompts/             # Versioned prompt templates (v1)
├── call_sites/          # The three bounded LLM call sites
├── claude/              # Anthropic SDK wrapper + critique + consensus
├── coherence/           # Scorer + deterministic explainer
│
├── evals/               # Eval framework
│   ├── golden.py            # GoldenSet, GoldenExample
│   ├── metrics.py           # F1, IoU, MAE, direction agreement
│   ├── runner.py            # EvalRunner — per-call-site eval
│   ├── active_learning.py   # Five selection strategies
│   ├── drift.py             # PSI, KL divergence
│   └── judges/              # Deterministic, LLM, consensus
│
├── regression/          # CI gate (PROMOTE / HOLD / BLOCK)
├── ab/                  # Shadow A/B harness + paired stats
│
├── timber/              # Timber integration (rules, factories, service_impl)
├── grove/               # Grove integration (adapter, events, handoffs, compensations)
├── a2ui/                # Surface-independent UI components
│
├── service.py           # CambiumService Protocol + four implementations
│
└── domains/
    ├── finance/             # Plugin #1 (10 dims, 13 categories, 4 narrative tags)
    └── _template/           # Copy-and-edit starter
```

The three call sites — classifier, reasoner, synthesizer — are the operative bounded surfaces. Each is a thin function that receives input, calls the model once, and returns a structured dict. The spine wraps each call: redact → call → validate → clip → rule-apply → trace. The model is never asked an unbounded question. Tool use is the spine's responsibility, never the call site's; the spine fetches context via the `CambiumService` and hands it to the prompt.

Three architectural commitments deserve emphasis. First, the reasoner's adjustment is bounded to ±0.3 of the deterministic baseline; the spine clips after the call. The model can propose larger; the system rejects larger. Second, the optional constitutional critique can only *weaken* an adjustment toward zero, never strengthen it. The invariant is enforced both by the prompt and by post-hoc clamping. Third, the synthesizer's output is judged by an LLM rubric before delivery; multi-judge consensus is available with disagreement routing to active learning.

## 14. Timber — Shared Library

Timber is the persistence and infrastructure library that every service consumes. PyPI package `timber-common`, version 0.5.0 at the time of writing. The import path is `common.*` (not `timber_common.*` — a frequent source of confusion).

The substrate is broad: model factory (YAML → SQLAlchemy 2 declarative class), field-level Fernet encryption, GDPR retention helpers, vector search via pgvector + FastEmbed, OAuth 2.0 service, LLM service, Redis cache layer, declarative rules engine (added with the Cambium drop), encrypted keystore with HMAC signing (added with the Cambium drop), generic pub/sub event bus (added with the Cambium drop).

The model factory is the canonical example of config-driven design. A YAML file under `data/models/*.yaml` declares one or more models — each with `mixins:`, `columns:`, `relationships:`, optional `encryption:` / `gdpr:` / `cache_strategy:` blocks. `model_factory.create_model_from_config_file(path)` builds the SQLAlchemy class, registers it with `model_registry`, attaches mixin behavior, wires field-level encryption events on Base. The MIXIN_REGISTRY at `common/models/mixins.py` exposes `TimestampMixin`, `SoftDeleteMixin`, `EncryptedFieldMixin`, `GDPRComplianceMixin`, `SearchableMixin`, `CacheableMixin`, `AuditMixin`. Extension via `register_mixin(name, cls)` — no factory change needed.

Cambium ships its own YAML configs under `cambium/timber_configs/*.yaml` for every Cambium-specific table: identity profile, behavioral event, classified event, coherence score, insight, manifest, trace record, golden example, provenance certificate, domain config. Each config matches Timber's actual `models:`-list format (the original Cambium spec used a flatter `fields:` shape that didn't match Timber's factory; this paper documents the corrected shape).

> **Sidebar: Why `common` and not `timber_common`?**
> Historical accident, mostly. The repository was named `timber` for the platform's "harvested useful material" metaphor; the Python package inside it is `common` because the modules were originally intended to be shared across multiple services in the same repo. Renaming the package would break every consumer; renaming the repo would require a coordinated migration. We documented the discrepancy, updated every consumer (including Cambium), and moved on.

## 15. Ranger — Control Plane

Ranger is the platform's control plane. FastAPI on port 5100. Owns the typed config store, the secrets vault, the audit log with hash-chained tamper evidence, the multi-tenancy primitives (JWT decoder, tenant context, Postgres RLS policies), and the future React admin UI.

The config substrate is the canonical example of a Pattern 11 (Design-time → Runtime) implementation at the control-plane altitude. Config rows live in `ranger_config` keyed by `(tenant_id, service, environment, namespace, key)`. Every mutation creates a new `version`, flips the previous row's `is_current=False`, and writes an audit row. Soft-delete via `deleted_at`. Tenant isolation is double-defended: app-layer filter on `tenant_id` plus Postgres RLS policies reading `current_setting('app.current_tenant_id')`. The session variable is set by `ranger/tenancy/context.py::set_tenant_context()` via `SELECT set_config(...)` at the start of each request.

Pydantic-backed `ConfigValidator` subclasses register against namespace strings in `DEFAULT_VALIDATORS`. Cambium added three:

| Namespace | Validator | Payload |
|---|---|---|
| `cambium_manifest` | `CambiumManifestValidator` | Mirrors `Manifest`; enforces SHA-256 hex hashes; ceiling bounds in (0, 1] |
| `cambium_constitution` | `CambiumConstitutionValidator` | Slug ID, semver version, content text, tone hints, banned tokens, effective_from |
| `cambium_threshold` | `CambiumThresholdValidator` | Mirrors `EvalThresholds`; all metrics bounded in (0, 1] |

For high-volume Cambium tables that don't fit the versioned-config shape — traces, certificates, review queue, gate decisions — Ranger adds dedicated SQLAlchemy models in `ranger/db/models/cambium.py` with RLS policies in `ranger/db/sql/cambium_rls_policies.sql`. The provenance-certificate and gate-decision tables use the hash-chain primitives (`ranger/audit/chain.py::compute_entry_hash`, `verify_chain`) so chain integrity can be verified for any audit-sensitive row.

The React admin UI is "Phase 3.2" — planned, not built. The HTTP API surface is ready: read paths for manifests, traces, queues; a `POST .../decide` for gate decisions; a `GET .../verify` for provenance certificates. When the UI lands it inherits the contract.

-----

# Part IV — The Three Configuration Substrates

This part is the one most teams underestimate. *How a platform configures itself* is half the architecture. OakQuant has three configuration substrates, each owning a clear range of authority.

## 16. The Three Substrates

| Substrate | Owner | Authority |
|---|---|---|
| **Postgres (Ranger control plane)** | Ranger | Manifests, constitutions, thresholds, model registry, secrets, audit log, HITL queues, gate decisions, feature flags, prompts |
| **Grove workflow YAMLs + DB rows** | Grove | Workflow definitions, task definitions, decision rules, scheduler beats, UI screens, routing rules |
| **Timber YAML model factory + service code** | Timber | Database table schemas, encryption fields, GDPR retention policy, vector index dimensions, mixin behavior |

The split is functional. Anything that *changes the runtime behavior of the bounded-agent system* belongs in Ranger (Pattern 11 — versioned, audited, tenant-scoped, pubsub-invalidated). Anything that *defines a workflow's structure* belongs in Grove (Pattern 11 again — Pydantic-validated, DB-stored, schema-evolved). Anything that *defines a persistence shape* belongs in Timber's YAML factory (declarative at deploy time, code-generated at boot).

Operationally, when a team member wants to change behavior, the first question is: which substrate owns this? Adding a new prompt rubric is a Ranger config change. Adding a new workflow state is a Grove definition change. Adding a new encrypted column is a Timber YAML change. The three substrates have different review burdens, different deployment paths, and different audit trails — and that is deliberate, because they have different blast radii.

## 17. Substrate 1: Ranger's ConfigStore

The pattern of any Ranger-stored manifest editor:

```python
class FooValidator(ConfigValidator):
    NAMESPACE = "foo_namespace"

    def validate(self, value: dict) -> FooEntry:
        return FooEntry.model_validate(value)  # Pydantic raises on error
```

Registration: add to `DEFAULT_VALIDATORS` in `ranger/config/validators/__init__.py`. From that point, every CRUD operation against the namespace is versioned, audited, pubsub-published. Cambium's manifest registry, constitution editor, threshold editor — all use this pattern. The Phase-3.2 React UI will inherit history, diff, audit-log replay, and tenancy enforcement for free.

The pubsub side is Redis, channel name `oak.config.changed.<service>`. Consumers register via the `oak_config` client library shipped from Timber; it builds a `RedisPubSubSubscriber` and dispatches change events to handlers. Cache invalidation is the canonical use case.

## 18. Substrate 2: Grove's Workflow Definitions

The pattern of a Grove workflow:

```yaml
workflow:
  name: financial_dna_evaluation
  version: "3.0.0"
  category: behavioral_assessment

states:
  foundation_phase:
    type: initial
    tasks:
      - name: s1_source_code
        task_name: financial_dna.screen.s1_source_code
        timeout: 120
    transitions:
      - on: task_success
        to: cognitive_style_phase
```

A workflow is a YAML or JSON document conforming to `common.schemas.workflow.WorkflowDefinition`. The schema enforces hard invariants: state names match `[a-zA-Z_][\w]*`, every transition target is a defined state, at least one state is absorbing (`terminal` / `final` / `error`), template variables reference standard scopes. Production never sees an invalid workflow because validation runs at upsert.

The DB row lives in `grove_workflow_configs.config_content` (JSON). The legacy YAML files under `config/workflows/*.yaml` are the fallback source-of-truth; new workflows go through DB. The `WorkflowRegistry` resolves DB-first, file-second.

## 19. Substrate 3: Timber's YAML Model Factory

The pattern of a Timber-generated model:

```yaml
models:
  - name: CambiumTraceRecord
    table_name: cambium_trace_record
    mixins:
      - TimestampMixin
      - EncryptedFieldMixin
      - AuditMixin
    encryption:
      fields: [prompt_redacted, response_raw, structured_output]
    columns:
      - name: trace_id
        type: String(36)
        primary_key: true
        default: uuid4
      # ...
```

At boot, `initialize_timber()` walks the YAML files under `data/models/` and `cambium/timber_configs/`, builds each model via `model_factory.create_model_from_config_file`, wires the mixin behavior, and registers the resulting class with `model_registry`. The `EncryptedFieldMixin` attaches SQLAlchemy `before_insert` / `before_update` / `load` listeners that encrypt and decrypt the fields named in `model._config['encryption']['fields']`. The `GDPRComplianceMixin` exposes `export_data` / `delete_data` methods reading `gdpr.user_field` and `gdpr.export_fields`.

There is no Alembic. Schema is recomputed at boot from the YAML files; production deployments run `initialize_timber()` against the existing database with `create_all` semantics (only missing tables are created). Migrations beyond column additions require manual SQL — a deliberate trade-off documented in CONTRIBUTING.md. The cost is real; the benefit is that the YAML *is* the schema, not an artifact derived from a different schema.

## 20. The Substrate Matrix

A reference for "which substrate owns my change":

| Change | Substrate | Review |
|---|---|---|
| New prompt template, new judge rubric, new constitution | Ranger | Manifest content-hash flip + regression CI |
| New workflow state or transition | Grove | Pydantic validation + Blueprint review |
| New encrypted column or new table | Timber YAML | Schema review + manual migration if non-additive |
| New mixin behavior | Timber Python code | PR review with consumer impact analysis |
| New domain dimension or category | Cambium domain YAML | Regression CI against the domain's golden set |
| New A2UI block type | Three repos (Cambium, Acorn, Sky) | Lockstep skill procedure |
| New service-level config flag | Ranger (default), Cambium service Protocol if call-site visible | Manifest CI if affects model output |

-----

# Part V — Data Flow: From Identity Capture to Weekly Insight

This part is the operative walkthrough. A user signs up; eight weeks later they receive a weekly coherence insight in their Sky timeline. What happened in between?

## 21. Step One: Identity Capture (Grove → Cambium)

The user is invited to take the Financial DNA Protocol. Sky launches the FDNA workflow via Canopy → Grove's `/api/grove/api/v3/execution/start` endpoint with workflow name `financial_dna_evaluation`. Grove instantiates a new session bound to the Pydantic-validated workflow definition; the state machine enters `foundation_phase`; the first task `s1_source_code` raises `HumanTaskRequired` with the screen's UI definition; Grove publishes a Redis event on `workflow:{session_id}:events` and returns `WorkflowExecutionStatus.PAUSED`.

Sky renders the directive (`render_workflow_screen` kind, with the screen's form fields). The user fills in the screen. Sky POSTs the form payload through Canopy to Grove's `resume_session` endpoint. Grove's `WorkflowExecutor.resume(...)` records the response, transitions to the next state, raises `HumanTaskRequired` for the next screen. Sixteen screens later the workflow completes with a `FinancialDNASession` row in `financial_dna_session` table.

A Grove post-completion task invokes Cambium via `cambium.domains.finance.identity.from_grove_session(session.payload)`. This reduces the screen-by-screen responses into a Cambium `IdentityProfile`. The profile is persisted via `service.put_identity(...)`. In production this routes through `TimberCambiumService` → the timber-common repository for `CambiumIdentityProfile` → field-level Fernet encryption on `dimensions` and `domain_data`.

## 22. Step Two: Event Stream (Plaid → Grove → Cambium)

Concurrently the user has linked their bank account through Plaid. Grove's scheduler (`config/scheduler/beat_schedule.yaml`) runs a daily fetch task that pulls new transactions; each transaction is normalized into a Cambium `BehavioralEvent` via `cambium.domains.finance.events.from_plaid_transaction(...)`. The event is enqueued into a Grove Celery task `cambium.process_event` (registered by `register_cambium_celery_tasks`). The task body invokes `GroveAdapter.process_event(identity_payload, event_payload)` from `cambium/grove/adapter.py`.

Inside the adapter, the spine runs end-to-end:

1. The redactor produces a pseudonymized form of the event description; the user-scoped redaction mapping is fetched from Timber's repository (encrypted at rest).
2. The classifier prompt is built using `cambium/prompts/classifier.py::build_classifier_prompt` with the redacted text, the domain's permitted categories, and the domain voice.
3. The Anthropic SDK is invoked at the classifier's model id (default `claude-haiku-4-5-20251001`). The structured output is parsed and validated against the permitted category set. One repair retry if invalid; second failure labels `unclassified`.
4. The deterministic baseline is computed via `cambium.domains.finance.baseline.FinanceBaseline.score(identity, event, classified.semantic_category)`. The formula multiplies category-dimension weights by `(identity_dimension - 0.5) * 2` (remapping [0,1] to [-1,+1]), pulls the result toward the category's valence, and applies a magnitude modifier.
5. The reasoner prompt is built with all of the above plus the baseline value and the ±ceiling. The model returns `{reasoner_adjustment, dimensions_cited, justification, confidence}`. The optional critique pass weakens the adjustment if needed.
6. The rules package applies: adjustment ceiling clip, dimension citation filter, catastrophizing token filter. Each rule violation is recorded but the repair (if available) is applied so the call completes.
7. The final `CoherenceScore` is constructed: `score = clip(baseline + adjustment, -1, +1)`.
8. A `TraceRecord` is appended for the classifier call and for the reasoner call.

If the reasoner's confidence falls below 0.6, the adapter publishes a `LowConfidenceClassification` event on Redis channel `cambium.low_confidence_classification`. Grove's `subscribe_cambium_events` subscriber dispatches it to the active-learning queue handler, which writes a row in `cambium_review_queue` with `priority=normal`.

## 23. Step Three: Weekly Synthesis (Grove → Cambium → Sky)

Every Sunday a Grove beat task fans out a per-user synthesis pipeline. For each user, the task gathers the past seven days of `CoherenceScore` rows and the underlying `BehavioralEvent` rows, then invokes `GroveAdapter.synthesize_window(...)`. The spine:

1. Builds the synthesizer prompt referencing the in-window scores and events.
2. Calls the synthesizer model (default `claude-opus-4-7`). The structured output is the insight body, headline, tone, referenced event ids, referenced dimensions, suggested actions.
3. Runs synthesizer rules: dimension citations filtered to permitted set, event references filtered to in-window ids, catastrophizing filter.
4. Issues a signed `ProvenanceCertificate` over the insight + manifest hash + golden-set version. The signing key is held in Timber's `EncryptedKeystore`.
5. The renderer in `cambium/a2ui/renderers.py::render_insight` composes an `A2UIDocument` with text blocks, dimension badges, event references, suggested actions, a feedback block, and a provenance block.
6. The adapter publishes `WeeklyInsightReady` on Redis channel `cambium.weekly_insight_ready` carrying the directive id and certificate id.

Grove's delivery subscriber routes the event: it sends a push notification, an email, and writes a Sky timeline entry. The Sky entry references the `A2UIDocument`; when the user opens the timeline, Sky's `CambiumInsightRenderer.tsx` dispatches each block to the appropriate sub-component; `GlassPanel` glassmorphism wraps the layout; the user can tap "Verify" on the provenance block to fetch the certificate from Cambium through Canopy.

A user feedback affordance sits on the insight. If the user taps "didn't land," Sky POSTs `/api/cambium/feedback` through Canopy; the feedback emits a `UserFeedbackReceived` event; Grove routes it to active learning.

## 24. The Trace

Every call site writes one trace row. The trace is append-only — redactions are *new rows* with `redaction_marker: true`, never edits-in-place. The trace carries: trace id, timestamp, user id hash, domain, call site, manifest hash, prompt hash, redacted prompt, raw response, structured output, latency, token counts, model id, rule violations, critique applied flag.

Two consumers read traces. Audit reads with cryptographic confidence: given a provenance certificate, follow its `payload_canonical_hash` to verify the insight matches what was produced; follow its `manifest_hash` back to the active manifest at issue time; follow the trace rows to see every model call that contributed. Eval reads for active learning and drift: a daily job scans yesterday's traces, applies the five selection strategies (low confidence, judge disagreement, direction flip, novel pattern, user negative signal), and enqueues candidates for human labeling.

The 18-month regulatory retention period is declared in the YAML factory (`retention.duration_months: 18, policy: regulatory_compliance` — though the current factory doesn't run scheduled retention jobs; that gap is acknowledged in the trade-off log).

-----

# Part VI — The Audit Boundary

OakQuant's audit story is the consequence of one architectural commitment: *the model is one input to the decision, never the decider.* This part documents what falls inside and outside the audit boundary, and why.

## 25. What Is Inside the Audit Boundary

| Artifact | Where |
|---|---|
| Identity profile | `cambium_identity_profile` row + encryption + GDPR export |
| Behavioral event | `cambium_behavioral_event` row |
| Trace record | `cambium_trace_record` row (append-only, 18-month retention) |
| Coherence score | `cambium_coherence_score` row |
| Insight | `cambium_insight` row (user-controlled retention) |
| Provenance certificate | `cambium_provenance_certificate` row + signature + hash chain |
| Manifest | `cambium_manifest` row + content-hash + Ranger version history |
| A2UI document | Re-renderable from `cambium_insight` + manifest |
| Rule violations | `trace_record.rule_violations` JSON |
| Feedback signal | `cambium_review_queue` row |
| Workflow run | Grove session row + workflow definition version |
| Config change | Ranger `ranger_config` row + audit log + pubsub event |
| Gate decision | `cambium_gate_decision` row + hash-chained audit |

The pattern: every value-bearing artifact lives in a typed row with a clear retention policy and a clear consumer. There are no "intermediate" stored strings; nothing exists *between* the model's output and the trace row. If a value affects user-facing behavior, there is a row.

## 26. What Is Outside the Audit Boundary

| Artifact | Why outside |
|---|---|
| Surface-rendered string | A2UI document is the audit boundary; the rendered string is a function of the document and the surface |
| Cached model response | Caching is a performance optimization; the canonical answer is in the trace |
| Embedding vector for similarity search | Re-computable from the source content; not load-bearing |
| Application log | Logs are operational, not audit-grade; rotated faster than retention |
| Service-internal queue position | Routing detail; not value-bearing |
| User session token | Authentication, not behavioral |
| Static asset (cover image, icon) | Cosmetic, not interpretive |

Drawing the line precisely matters. If everything is audit, audit means nothing. The criterion is *interpretive value*: does this artifact directly inform what the user reads or experiences? Then it is inside the boundary. Otherwise, it is outside.

## 27. Provenance Certificates

A provenance certificate proves that a specific insight came from a specific manifest with a specific golden-set version. The signature is over a canonical encoding of the insight bound to the manifest hash and golden-set version. The signing key lives in Timber's `EncryptedKeystore`; `key_id` is referenced in the certificate so rotated keys can still verify old certificates.

Anyone with the public key can verify offline. The verifier (`cambium.core.provenance.verify_certificate`) reconstructs the canonical encoding, compares the SHA-256, and verifies the signature. This is useful in three settings:

1. **Regulatory audit.** A regulator presents an insight and asks for proof of origin. The certificate suffices.
2. **User-shared insight.** A user forwards an insight to a financial advisor. The advisor can verify the certificate without depending on the user.
3. **Cross-system delivery.** An insight delivered via email or voice carries the certificate; the receiving system can verify before acting on it.

The certificate format is content-hashed and immutable. A new manifest produces a new certificate format only if the manifest content hash changes (which forces the regression CI). Old certificates remain verifiable indefinitely.

## 28. The Bounded-Agent Contract

The contract every Cambium call site honors:

> The model receives a redacted prompt. The model returns structured output. The model never reads from disk, never writes to disk, never reads from the database, never calls a tool, never reads from a vector store. The spine handles all of that; the model gets enriched context, not handles.

This is the load-bearing operational commitment. Patterns 3 (RAG) and 4 (Tool Use) are *workflow*, not *agent decision*. The spine fetches the few-shot examples; the spine queries the vector store; the spine decides whether the merchant lookup is worth including. The model receives the result and reasons over it. The model has no authority to invoke external state.

The benefit shows up at audit time: every model call is reproducible given the manifest, the prompt, the structured input. Drift testing on a fixed canary set is meaningful because the input stays constant. Pattern 12 (Saga) compensations work because the spine controls the boundary between model output and system state.

The cost is real: the platform cannot, today, do anything that requires the model to *decide* what to look up. It cannot do agentic web search, agentic file editing, agentic tool selection. That is intentional. Those patterns belong in a different platform with a different audit story.

-----

# Part VII — Evaluation and the Regression CI

The evaluation story is what makes the bounded-agent thesis operative. Without it, the spine is just defensive coding; with it, manifest promotion has a gate.

## 29. The Golden Set

A golden set is a versioned JSONL of hand-labeled examples per (domain, call site). Each example carries an id, the input payload, the expected output, a labeler attribution, a rationale, a version-added tag, an adversarial flag, optional tags, optional embedding. Two reviewers minimum per example. Removal is tombstoning (`tags: ["deprecated"]`), not deletion. The set grows monotonically.

The v1 finance golden seed ships 60 examples (20 classifier, 20 reasoner, 20 synthesizer) under `golden_seeds/finance_*_v1.jsonl`. Each row is one line. The cover ratio is roughly 60% mainstream, 30% mode-boundary, 10% adversarial. Authoring discipline is encoded in the `cambium-author-golden-set` developer skill.

The set version flows into the manifest content hash. Changing the set changes the manifest. Changing the manifest runs the regression CI.

## 30. The Three Judges

Three judge types operate at different points in the eval pipeline:

| Judge | Where | What |
|---|---|---|
| **Deterministic** | Classifier + reasoner | Pure comparison of predicted vs expected; produces F1, accuracy, MAE, direction agreement, dimension IoU |
| **LLM** | Synthesizer | Scores the insight against a six-dimension rubric (evidence, fidelity, tone, specificity, actionability, respect); structured output in [0,1] per dimension plus an overall mean |
| **Consensus** | Synthesizer optional | Two LLM judges score the same insight; disagreement beyond tolerance routes to active learning; consensus is the minimum of the two |

The deterministic judges are pure Python (`cambium/evals/judges/deterministic.py`). The LLM judge is one Anthropic call per insight (`cambium/evals/judges/llm_judge.py`). The consensus judge runs two LLM judges with different model ids (e.g. Sonnet and Opus, or two Sonnet variants), tolerance defaulting to 0.2 (`cambium/claude/consensus.py`).

Consensus surfaces uncertainty rather than averaging it away. When the two judges disagree, the case becomes an active-learning candidate. The consensus score for promotion purposes is the minimum — a deliberate conservatism that means a single skeptical judge can block promotion of an insight format.

## 31. Calibration Tracking

Every Cambium output carries a confidence in [0, 1]. Calibration is tracked via Brier score, expected calibration error (ECE), and reliability bins. The formula for ECE:

$$
\text{ECE} = \sum_{b=1}^{B} \frac{|B_b|}{N} \, \left| \text{acc}(B_b) - \text{conf}(B_b) \right|
$$

where $B_b$ is the $b$-th confidence bin, $\text{acc}(B_b)$ is the accuracy of predictions in that bin, and $\text{conf}(B_b)$ is the mean confidence in that bin. Cambium computes ECE with ten bins.

A poorly-calibrated manifest cannot promote even if its accuracy is high. The regression CI applies an absolute ceiling on ECE (default 0.10) and refuses promotion above it. The intuition is simple: a model that says "80% confident" should be right 80% of the time. A well-tuned classifier might score 95% accuracy on average but disagree wildly within each confidence band; that pattern would damage downstream consumers (active learning, drift detection) that depend on confidence as a signal.

## 32. Drift Detection

Two metrics run on a fixed daily canary set: Population Stability Index (PSI) over the coherence score distribution, KL divergence over the classifier's category distribution.

$$
\text{PSI} = \sum_{i=1}^{B} (a_i - e_i) \, \ln\frac{a_i}{e_i}
$$

where $a_i$ is the actual fraction in bin $i$ and $e_i$ is the expected fraction. Standard interpretation: PSI below 0.1 is no significant change; below 0.25 is minor; above 0.25 is significant — emit a `ManifestDriftAlert` event with severity `alert`.

Drift can come from three sources: provider-side model update (anthropic ships a new version), prompt change applied without regression CI (it shouldn't happen, but the canary catches it if it does), distributional shift in input data (the user base changes). The alert is the same; the disposition is different. Grove's drift-review HITL gate routes the alert to a human reviewer who decides.

## 33. The Regression CI Gate

The CI is the *only* authority that activates a new manifest in production. `cambium.regression.evaluate_gates` takes a candidate manifest, the baseline manifest, the metrics from each run, the absolute thresholds, and the regression tolerance. It returns `PROMOTE`, `HOLD`, or `BLOCK` with reasons.

BLOCK fires if any absolute floor is missed (classifier F1 below floor, reasoner MAE above ceiling, synthesizer judge below floor, ECE above ceiling). HOLD fires if a metric regresses by more than the tolerance (default 3%) from baseline. PROMOTE only if every floor is met and no metric regressed beyond tolerance.

A `ManifestPromotionPending` event accompanies a PROMOTE decision; Ranger holds the actual manifest activation behind a human gate. The CI says "this candidate is *eligible* to promote"; an authorized human says "promote this candidate now." Two locks on the same door.

## 34. Active Learning

Five selection strategies feed the labeling queue:

1. **Low confidence** — classifier confidence below threshold (default 0.5).
2. **Judge disagreement** — consensus disagreement above tolerance.
3. **Direction flip** — reasoner pushed the baseline across zero.
4. **Novel pattern** — the example's embedding sits far from existing golden examples (Mahalanobis distance, or simple cosine to nearest neighbor).
5. **User negative signal** — feedback marked the insight as "didn't land" or worse.

Selected candidates land in `cambium_review_queue` via the Grove HITL pattern. Reviewers (humans) label them through Ranger's Phase-3.2 UI. Labeled examples join the next golden-set version. The set grows monotonically; the next manifest runs against the larger set; the regression CI tightens over time.

This loop is the production analog of fine-tuning. Cambium does no model fine-tuning; the IP is in the golden sets and the rubrics, not in the weights. The loop ensures the golden set continuously reflects the real distribution of production inputs.

-----

# Part VIII — Cross-Domain Extension

OakQuant's value compounds across domains. The platform's first domain — personal finance — is real, but the architecture is generic. Adding a second domain is configuration plus four small Python files plus a golden set, not a forked codebase.

## 35. The Domain Plug-in Pattern

A domain provides five artifacts:

1. **`config.yaml`** under `cambium/domains/<slug>/config.yaml` — dimensions, categories, narrative tags, ceiling, prompt voice, dotted paths to baseline/identity/events modules.
2. **`identity.py`** — reducer from the domain's capture instrument into an `IdentityProfile`.
3. **`events.py`** — adapter(s) from the domain's signal source(s) into `BehavioralEvent`.
4. **`baseline.py`** — deterministic scorer implementing the `BaselineScorer` Protocol.
5. **`golden_seeds/<slug>_*_v1.jsonl`** — at minimum 20 hand-labeled examples spread across the three call sites.

The platform-level invariants are non-negotiable: three call sites, structured outputs, content-hashed manifest, signed provenance, append-only trace. The domain *cannot* customize these — they are how a new domain inherits Cambium's audit guarantees. The domain *can* customize the dimensions, the categories, the narrative tags, the baseline weights, the prompt voice, the redaction extras, the eval thresholds, the catastrophizing tokens.

A new domain registers at boot via `service.domain_registry().register_from_yaml(Path("cambium/domains/<slug>/config.yaml"))`. The registry imports the baseline class dynamically. The spine is unchanged.

## 36. Candidate Domains

The architecture supports any domain where there is a measurable identity instrument, a measurable behavioral signal, and a user-perceived value in reflective coherence:

| Domain | Identity instrument | Behavioral signal | Example dimensions |
|---|---|---|---|
| Finance (plugin #1) | Financial DNA Protocol | Plaid transactions | future_orientation, risk_tolerance, family_orientation, … |
| Health | Lifestyle assessment + biometric baseline | Wearable, food log, sleep | energy_management, recovery_priority, … |
| Career | Work-values + skills inventory | Calendar + Slack + GitHub | focus_priority, collaboration, … |
| Sustainability | Environmental values | Carbon-impact transactions + travel | consumption_minimalism, locality, … |
| Learning | Goals + curiosity profile | Content consumption + reading log | depth, breadth, … |
| Relational | Values + relationship aspirations | Communication patterns | reciprocity, presence, … |

Build order is a function of four factors: signal-source quality (Plaid for finance is ideal — high volume, low noise, broadly available), instrument readiness (existing validated psychometrics save months), user-value clarity (where weekly coherence has highest perceived value), regulatory acceptability (avoid domains with extreme regulatory burden until the platform matures). Finance scores high on all four. Health and career are the strongest candidates for plugin #2.

## 37. Cross-Domain Handoffs

Some signals matter across domains. A finance-domain pattern of "escape spending" correlates with career-domain burnout markers. A health-domain decline in sleep correlates with finance-domain impulse spending. The platform handles these via the cross-domain handoff pattern (Grove-owned).

The source domain's pipeline detects the cross-domain signal and emits a `CrossDomainHandoffRequested` event with a `HandoffContext` payload. Grove's cross-domain handoff workflow (`config/workflows/cambium_cross_domain_handoff.yaml`) validates the context, invokes the target domain's spine via `GroveAdapter.process_event` with the supplemental context, and routes the result back into the user's combined view.

The architectural commitment: domain code stays isolated. Cambium's finance domain never imports career-domain symbols. Cross-domain reasoning is workflow-level orchestration in Grove, not direct coupling. This keeps each domain inspectable, replaceable, and independently versioned.

-----

# Part IX — Trade-offs and Decision Log

This part is the one I most wish other platforms published. Architecture is choices; choices are visible only when they are written down.

## 38. What We Chose, What We Rejected, Why

**Bounded-agent over autonomous agent.** Rejected: agentic tool use inside call sites. Cost: the platform cannot do anything that requires the model to decide what to look up. Benefit: every model call is reproducible; provenance certificates are meaningful; institutional audit is achievable.

**Three call sites, not two or four.** Rejected: collapsing classifier and reasoner; rejected: adding a fourth synthesis step. Cost: some domains might benefit from a richer call-site mix. Benefit: minimum to express the coherence task; more invites Pattern 1 (chaining) becoming Pattern 7 (orchestrator-workers), which belongs in Grove. The `cambium-add-call-site` skill exists for the rare case the architecture demands a fourth.

**±0.3 reasoner ceiling.** Rejected: larger ceilings. Cost: in 5–10% of cases the reasoner would want to adjust more. Benefit: empirical — larger ceilings collapse the baseline's anchor effect; the model's signal becomes dominant where the baseline's structural fit should dominate. Domains can override but should justify; the manifest content hash changes either way.

**Append-only trace.** Rejected: edit-in-place. Cost: storage volume grows monotonically. Benefit: audit-grade requires append-only with explicit redaction markers; read-write traces invite silent edit.

**Content-hashed manifest.** Rejected: numeric version strings only. Cost: hash strings are less human-readable. Benefit: manifest identity is structural; two manifests with identical content are the same regardless of creation time; the regression CI compares hashes, not version numbers.

**Service-layer abstraction with four implementations.** Rejected: separate test/dev/prod codepaths. Cost: every implementation must honor the full Protocol. Benefit: the same Cambium core runs in tests, dev, production, and Ranger without code change; new substrates plug in without touching the spine.

**Domain plug-ins ship YAML + four Python files + golden seed.** Rejected: code-only domain definitions; rejected: pure-YAML domains. Cost: maintainers need to write Python for the baseline scorer. Benefit: lowest plausible bar without sacrificing inspectability of the baseline formula. A pure-YAML baseline would force a DSL; a pure-Python config would lose the editable surface.

**Golden sets in JSONL with two-reviewer discipline.** Rejected: database-only golden sets; rejected: single-reviewer labels. Cost: JSONL files don't enforce schema at write time. Benefit: plain text, diffable, reviewable; database-only golden sets become unreviewable assets; single-reviewer labels become idiosyncratic.

**Apache 2.0 for the library, CC-BY 4.0 for the golden seeds.** Rejected: same license for both. Cost: contributors must respect both. Benefit: the library should be permissively reusable as platform infrastructure; the golden sets are author-attributable research artifacts.

**Pydantic v2 as the universal schema language.** Rejected: JSON Schema, protobuf, XML Schema, OpenAPI. Cost: TypeScript twins are hand-maintained. Benefit: one schema language across Python (every service), one source of truth, easy validation, easy serialization, generates JSON Schema on demand for documentation purposes.

**Postgres + pgvector + Redis as the runtime substrate.** Rejected: a separate vector database (Pinecone, Weaviate, Qdrant — though clients exist as Timber extras), separate event bus (Kafka). Cost: pgvector lacks some advanced ANN features. Benefit: one operational story; tenant isolation by RLS, transaction safety, single backup.

**No Alembic; schema rebuilt from YAML at boot.** Rejected: Alembic migrations. Cost: non-additive schema changes require manual SQL. Benefit: the YAML *is* the schema; no second source of truth; deployments are simpler. Reconsider when the production schema crosses a complexity threshold.

**Canopy is httpx, not MCP.** Rejected: MCP everywhere. Cost: agent-driven tool calls route through Acorn instead of directly to Cambium. Benefit: Canopy stays thin and predictable; one gateway, one auth model; MCP complexity is bounded to Acorn where it actually adds value.

## 39. Known Gaps

| Gap | Severity | Mitigation |
|---|---|---|
| Timber lacks scheduled retention jobs | Medium | YAML retention blocks are advisory until a scheduler exists; Cambium's docs note 18-month trace retention but enforcement is operational |
| Vector pgvector ANN not yet wired | Medium | `vector_search_service.search` does Python-side cosine; pgvector `ORDER BY embedding <-> :q` queries TBD |
| Ranger UI does not exist | Medium | HTTP API is complete; React UI is Phase 3.2 |
| Sky parser `KNOWN_KINDS` does not include `render_cambium_insight` | Low | Renderer handles the kind; parser warns but renders correctly |
| Grove's `RegistryDomain` enum mixes string and auto() ints with the new `CAMBIUM_WORKER` entry | Low | Functional but stylistically inconsistent; clean up in a follow-up PR |
| Acorn's `app.py` wiring for Cambium not auto-applied | Low | Documented in `architecture/CAMBIUM_INTEGRATION.md`; manual two-line edit |
| No scheduled drift run | Medium | Daily canary set + PSI/KL is implemented as library code; the cron-scheduled invocation lives in Grove and needs wiring |
| `TimberCambiumService` falls back to in-memory if timber-common is partial | Medium | Documented; reconsider once timber-common's rules / keystore / events modules ship in a tagged release |

These are not blockers for v1 — they are the honest punchlist. Each is tracked and has an owner.

-----

# Part X — Open Questions

A platform is never finished. The architecture has stabilized; the open questions point at where the next year's work will land.

## 40. What Still Wants Better Answers

**How aggressive should active learning be?** Today the five strategies enqueue uniformly. As the golden set grows, some strategies may produce more valuable labels than others. Should the queue weight strategies, or should reviewers triage? Operationally unresolved.

**Should the constitution be tenant-scoped?** Today the constitution is platform-level. Some enterprise customers will want their own tone or banned-token set. Tenant-scoped constitutions would require manifest-per-tenant which would require regression CI per tenant. The economics are not yet clear.

**How does cross-domain reasoning scale?** Two domains can hand off via Grove without performance concern. Six domains with arbitrary handoffs invites cyclic flows. The handoff workflow needs a depth limit and a circuit breaker; both are easy to add but the failure modes aren't well-characterized.

**Where should the eval-time canary set come from?** Today the canary set is hand-curated. As the platform serves more users, real-traffic-derived canaries (with PII redaction) might be more representative. The trade-off is reproducibility — a hand-curated set never changes between runs.

**Can Cambium drive Grove (instead of Grove driving Cambium)?** Today Cambium is a worker that Grove orchestrates. For event-driven domains (real-time fraud, immediate alerts) it might make sense to invert the relationship. The MCP server interface gives Cambium read access to anything; whether Cambium should ever *initiate* a Grove workflow is unclear.

**Is the regression tolerance the right shape?** Today the CI uses a 3% absolute tolerance per metric. A geometric tolerance (per-metric relative to its variance) might be more principled. Empirically it would matter only on tight margins.

These are not stop-the-world questions. They are the kind of questions a mature platform should be ready to entertain after the first year of production use.

-----

## 41. The Composition

A bounded-agent platform is many architectural choices, but two stand out as load-bearing in retrospect.

The first is the *separation of authority*. The model has authority over the answer to the bounded question; nothing else. The spine has authority over what is asked. The eval framework has authority over which spine-and-prompt combination can run. The service abstraction has authority over what context the spine has. Ranger has authority over what the active manifest is, gated through HITL. Grove has authority over when things happen. No service can usurp another's authority; that is the whole architecture.

The second is the *content-hashed manifest as the unit of versioning*. Everything that matters — prompts, models, rubrics, rules, golden set, constitution, ceiling — flows into one hash. Two manifests with identical content are the same. A trace row points at a hash; a certificate points at a hash; a regression CI run points at a hash. The hash is the platform's name for "this version of the system." Without it, every other audit story would have to be rebuilt at every artifact.

Everything else in this paper is downstream of those two choices.

## 42. Closing

Cambium and the rest of the platform were built to defend a thesis: that the meaningful question for agentic AI is not what the tools can do, but who governs the work. The answer in OakQuant is *the spine governs the model; the eval framework governs the spine; the platform's HITL gates govern promotion of new spines; humans govern the gates.* Authority is layered, each layer auditable, no layer optional.

The platform is a year and a half of decisions. Some were obvious in retrospect; some were not. The ones that mattered most are the ones that limited what the system could do — the ±0.3 ceiling, the append-only trace, the bounded call site, the typed event hierarchy, the content-hashed manifest. *What a platform forbids defines what it can be trusted to do.* That is not a slogan; it is the architecture.

If you are building a bounded-agent platform of your own, the parts that travel well are the seven-service split, the three configuration substrates, the service abstraction, the manifest pattern, the regression CI, the typed-event hierarchy, and the lockstep schema discipline. The parts that are OakQuant-specific are the Financial DNA Protocol, the Plaid signal source, the finance domain weights. Take the first set; supply your own second set; you will have a different platform with similar architectural commitments. That is the point of writing this paper.

The platform's name is OakQuant — *oak* for the tree, *quant* for the analytical surface. The naming convention extends across services: Sky is the canopy you look up at, Canopy is the protective layer over the forest floor, Grove is where the workers grow, Acorn is where reasoning begins, Cambium is the thin living layer where growth actually happens, Timber is the harvested useful material, Ranger is the manager of the whole forest. The metaphor has held for two years and never been confusing in code review. That, too, is part of the architecture.

-----

# Part XI - Security and Access Control

## 43. The Confidentiality Problem

The platform publishes openly under most circumstances. The architectural reference, the agentic-patterns mapping, the cambium investor brief - all are public. But the same platform also produces material that cannot be public: pre-release book chapters being reviewed by named beta readers, internal architecture deep-dives that reference unpublished IP, investor briefings, partner-specific case studies.

The press needs to serve both without bifurcating the codebase. A reader landing on `press.oakquant.ai/articles/<some-slug>` should be able to read it if they have access and be rejected indistinguishably from an article that doesn't exist if they don't. The architecture should fail closed: a bug in the rendering pipeline must never accidentally expose a private article, and the default for any unknown visibility value is private.

The model has three layers, deployed in three phases.

## 44. The Visibility and Access Model

Each article and book carries two fields in its `meta.yaml`:

- `visibility`: one of `public`, `unlisted`, or `private`. Absence means `public` (backward compatibility for the corpus that pre-dates this work). An unrecognised value (e.g. `visibility: foo`) is treated as `private` (fail closed on typos).
- `access`: a list of identifiers permitted to view when visibility is `private`. The list resolves to a set of canonical user_ids via the identity model below.

The access list accepts multiple identifier kinds. Phase 1 supports GitHub usernames directly. Phase 2 adds email and Timber user_id. Phase 3 adds GitHub team membership and email domain matching. Each form resolves to the same canonical user_id space, so re-keying an article when a new identity provider lands is unnecessary.

A separate canonical identity model holds the access boundary. The platform's source of identity is a Timber `users` row identified by UUID `user_id`. External identity providers (GitHub OAuth, magic-link email, future Google/Microsoft SSO) attach to this canonical identity via a `linked_accounts` table. A user has exactly one `user_id` regardless of how many providers they use; access lists key on canonical id, with the resolver mapping provider-handle form (`pumulo`) to canonical form (`user_id: 4e8c...`).

## 45. Phase 1: GitHub OAuth as the Gate

The v1 implementation is GitHub-only. Every invitee must have a GitHub account; that account's login is matched against the `access:` list of the requested article.

The implementation is small. Press FastAPI already runs a GitHub OAuth flow for authoring (a `current_session` dependency, in-memory session dict, signed cookie). Extending it to readers requires no new infrastructure: the same session model gates the new visibility check, the same `sess.login` value is matched against the access list.

Four endpoints become access-aware: article get, article list (filtered to the requester's permitted set), asset directory listing, and asset binary stream. All four return 404 on denied access rather than 403, so the existence of a private article is not leaked through a probe attack. Cache-Control becomes `private` on every protected asset response so intermediaries do not retain the body. An in-memory append-only access log records every grant and deny.

The frontend has one structural addition: server components in Next.js's App Router fetch articles from the press backend via an internal Docker-network URL, which today does not forward the user's cookie. Phase 1 adds the cookie forwarding so SSR can render private content for the requester. The article render page's `generateMetadata` also gains a fail-closed path: when an article isn't visible to the requester, the rendered metadata is a generic "Sign in to view" with `robots: noindex,nofollow` so link previews don't leak titles or cover images.

The audit trail is real from day one. Every read of a private artifact writes an entry with `(user_login, artifact_slug, visibility, outcome, timestamp)`. The schema is mirrored in a `PressAccessLog` table in Timber that the press will migrate to in Phase 2, but the field shape is stable now.

## 46. Phase 2 and Beyond: Magic-Link, Account Linking, Watermarking

Phase 2 widens the IdP set. Adding Timber's existing `oauth_service` as a second sign-in option lets non-GitHub readers register via passwordless magic-link email. The magic-link path issues a short-lived signed token (15 minutes, single-use) and confirms email ownership on click. Readers who already have a GitHub identity can keep using it; new readers can sign up by email. Both paths land at the same canonical `user_id` in Timber.

Phase 3 adds account linking. A reader who started with GitHub can add an email; a reader who started with email can connect their GitHub. The `linked_accounts` table holds the attachment; either provider's sign-in resolves to the same canonical identity. Access lists do not need re-keying when a new provider is added.

For books, an additional measure: per-response watermarking. PDF and EPUB downloads stamp the requester's display name, timestamp, and an audit-log row UUID on every page before streaming. The watermark is content-stream embedded for IP-sensitive titles (durable across redistribution) and overlay-annotated for everything else (cheaper, strippable but adequate for low-sensitivity content). No download is cached; every response is rendered for one requester.

The full spec, including the schema for the `linked_accounts` table, the magic-link token lifecycle, rate limits, the GDPR right-to-be-forgotten flow, and the failure modes for upstream-unavailable scenarios, lives in `docs/SECURITY_DESIGN.md` in the oakquant-press repository. The spec is the source of truth; code that touches authentication, authorization, or content storage on press.oakquant.ai references it or updates it.

The summary architectural commitment: identity is the gate, not knowledge. A shared URL grants nothing. Access is auditable. Re-identification is reversible only with cryptographic key access. Fail-closed beats fail-open at every step.

-----

## References

1. *Building Effective Agents.* Anthropic. December 2024. The foundational read for patterns 1–7.
2. *Thirteen Patterns for Building Agentic AI You Can Actually Trust.* Pumulo Sikaneta. 2025. The long-form discussion of each pattern referenced in §4.
3. *Insight Belongs to the Machine. Decisions Belong to the Human.* Pumulo Sikaneta. OakQuant Press, May 2026. The companion essay on bounded-agent architecture in regulated industries.
4. *The Analyst Consensus on Agentic AI Architecture.* Pumulo Sikaneta. OakQuant Press, May 2026. Market-side companion piece.
5. *Klontz Money Scripts: A New Framework for Behavioral Financial Therapy.* B. Klontz et al. 2008. The behavioral-finance source of Cambium's narrative tags for the finance domain.
6. *Behavioral Life-Cycle Hypothesis.* H. Shefrin and R. Thaler. 1988. One of the seven academic frameworks grounding the Financial DNA Protocol.
7. *Prospect Theory: An Analysis of Decision under Risk.* D. Kahneman and A. Tversky. 1979. The basis for several Financial DNA Protocol screens.
8. *Goal-Setting Theory: Building a Practically Useful Theory of Goal Setting and Task Motivation.* E. A. Locke and G. P. Latham. 2002.
9. *Self-Determination Theory.* E. L. Deci and R. M. Ryan. 2000.
10. *Social Cognitive Theory: An Agentic Perspective.* A. Bandura. 2001.
11. *Financial Socialization Theory.* J. F. Gudmunson and S. Danes. 2011.
12. *Population Stability Index: A Practical Guide.* Various. 2010s industry standard for distribution-shift detection.
13. *Calibration of Probability Estimates: Combining Brier Score and Expected Calibration Error.* Various. ICML community standard.
14. *Model Context Protocol.* Anthropic. 2024. The standard powering Acorn's tool surface.

-----

*This paper was authored as part of the Cambium project drop in May 2026. The source code referenced — `cambium`, `grove`, `timber`, `ranger`, `sky`, `canopy`, `acorn` — is at the corresponding `oakquant-ai/<service>` repositories. Corrections welcome at the public press email.*
