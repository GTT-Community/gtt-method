# GTT Glossary

Canonical terms for **GTT 3.0**. This file is the single source: the website, the Bootstrap and the CLI use these definitions and must not keep parallel ones (Canon §32).

- **User terms** are what a developer needs to use GTT. User-facing text (website, README, CLI prompts and errors) should use only these.
- **Internal terms** belong in contracts, agent instructions and implementation.

## User terms

| Term | Definition | Canon |
|---|---|---|
| **GTT** | *Governance Through Thinking*. A methodology for AI-assisted software development in which the human keeps authority over what the system is supposed to be. | §1 |
| **ADE** | AI Development Environment: the tool an agent works in (Claude Code, Kiro, Copilot, Cursor, Antigravity, Codex, OpenHands…). | §1, §27 |
| **Governed Context** | The ratified source of truth of the project: vision, principles, architecture, stack, constraints, glossary, sources. Only the human changes it. | §7 |
| **Epic** | A governed unit of the solution, with a goal, a scope and an approved design. Stories inside it are not governed. | §10 |
| **THINK** | The reasoning phase in which an agent turns evidence into a proposed design, and analyzes the impact of a change request. | §15 |
| **Freeze** | The human ratification of the governed state. There is no unfreeze; changes go through the change flow. | §16 |
| **Change request** | What the human wants changed in governed state, written in plain words. It starts the change flow. | §18 |
| **Proposal** | A change prepared by an agent, explicit enough for the human to decide. Never a decision. | §18 |
| **Development method** | How the solution is organized and cut into Epics (for example, Vertical Slice + Walking Skeleton). Chosen by the human, recorded at freeze, checked afterwards. | §20 |
| **Autonomy profile** | How much GTT does without asking: Light, Medium, Hard or Team. Never changes who decides. *Formerly "Method Plan".* | §21 |
| **GTTGuard** | A marker that protects the authority to change a file, class or method. Changes then need human approval. | §22 |
| **Observation** | A deviation GTT detects during development, graded NOTICE, WARNING, GOVERNANCE or BLOCKING. Only BLOCKING stops anything. | §17 |

## Internal terms

| Term | Definition | Canon |
|---|---|---|
| Governance plane / Work plane | What the system should be (governed) vs. how it is being built (free, observed). | §6 |
| Execution Context | The minimum relevant projection of Governed Context for one operation. | §7.1 |
| Evidence Boundary | The separation of sources, grounding, dossier, reasoning, proposal, decision and governed artifact. | §8 |
| Evidence Dossier | The consolidated evidence THINK reasons over. Not an authority. | §8 |
| Provenance tags | `[SOURCE]`, `[GAP]`, `[CONFLICT]`, `[PROPOSAL]` (aliases `[FUENTE]`, `[VACÍO]`, `[CONFLICTO]`, `[PROPUESTA]`). | §8.1 |
| OPEN | A recorded, non-blocking gap. It authorizes nothing. | §8.1, §11 |
| Architectural Intent | The intended architecture, preserved through context, ADRs, method and boundaries. | §9 |
| ADR | Architecture Decision Record of a decision the human already approved. | §9 |
| Story | A work-plane task inside an Epic. Not governed. | §10 |
| Primary ADE | The participating ADE GTT hands off to. It has no governance authority. | §13 |
| Design Assessment | The written evaluation of an existing design against the minimum floor. | §14.2 |
| Minimum floor | Problem/scope, architectural style, stack, datastore. All must be confirmed by the human. | §14.2 |
| Initial Design Questionnaire | Guided elicitation when no design exists. | §14.3 |
| Readiness | `NOT_READY`, `READY`, `READY_WITH_OPEN_ITEMS`. | §14.4 |
| THINK Depth | `QUICK`, `STANDARD`, `DEEP`: how far THINK explores. | §14.4 |
| Next action | The deterministic statement of what the project waits on, and from whom. | §24 |
| Promotion set | The complete set of governed artifacts a proposal changes. Applied all-or-nothing. | §18 |
| Promote / Undo | Apply an approved promotion set atomically / restore its backup. | §18 |
| Design reset | An explicit return to a clean elicitation state before the first freeze. | §19 |
| Method Pack | The single-source definition of a development method. | §20.1 |
| Method Lock | The ratified development method recorded in the governed architecture. | §20.2 |
| Method Conformance | Executable fitness-function checks of the Method Lock. | §20.3 |
| Verification level | `verified`, `partial`, `declared-only`. | §4.12, §20.1 |
| Protection Registry | The normalized, GTT-maintained list of GTTGuard declarations. | §22 |
| Enforcement level | `realtime-verified`, `realtime-unverified`, `ci-gate`, `instruction-only`. | §23 |
| Governance backlog | The idempotent record of observations and their human decisions. | §17 |
| Control Plane | The logical set of GTT governance services. | §27 |
| Adapter | The translation from an ADE's mechanisms (hooks, rules, MCP, CI) to the GTT contract. | §27 |
| Instruction Plane | The agent contract and ADE adapters. Protected in an installed project. | §31 |
| Session state | A derived session handoff. Never authority. | §25 |
| Working preferences / agreements | Personal (local) / team (shared) policy. Never authority over governed context. | §26 |

## Deprecated terms

| Deprecated | Use instead | Why |
|---|---|---|
| Method Plan | Autonomy profile | Confused with development method / Method Pack |
| L0 / L1 | Governed Context / Epic Designs | Internal shorthand that leaks into user text |
| Confirmation A / B | Brief confirmation / Context confirmation | Self-describing names |
| `gtt/proposals/`, `gtt/protection/` | See Canon Annex A | Paths never used by the reference implementation |
| Governed Thought Teamwork, Governance Throw Think | Governance Through Thinking | One expansion only |
| CDAD | GTT | Former name of the methodology |
