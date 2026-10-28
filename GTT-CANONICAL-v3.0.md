# GTT — Canonical v3.0

Status: CURRENT
Canonical status: RELEASED (ratified 2026-10-28)
Generation: GTT 3.0
Supersedes: GTT 2.1 (`GTT-CANONICAL-v2.1.md`)
Implementation status: see [`CONFORMANCE.md`](CONFORMANCE.md)
Terminology: see [`GLOSSARY.md`](GLOSSARY.md)
Versioning: see [`VERSIONING.md`](VERSIONING.md)
License: Apache-2.0 (this repository); reference implementations carry their own license

---

## How to read this document

GTT 3.0 defines the **target contract** of the methodology: what GTT is and what any conforming implementation must do.

It does not claim that every capability is already shipped. Whether a given release of the GTT suite implements a capability is stated in one place only, [`CONFORMANCE.md`](CONFORMANCE.md), as `implemented`, `partial` or `planned`. Nothing may be presented to users as available unless `CONFORMANCE.md` marks it `implemented` for that release (§34).

Normative words: **must** / **must not** are requirements; **should** is a strong default that may be departed from with a recorded reason; **may** is optional.

The document has six parts:

- **I. Foundation:** what GTT is and why.
- **II. Model:** the concepts.
- **III. Lifecycle:** what happens, in order.
- **IV. Capabilities:** the governance mechanisms.
- **V. Integration:** ADEs, agents, CI and other methods.
- **VI. Governance of the canon:** roles, terminology, versions, conformance.

---

# Part I — Foundation

## 1. What GTT Is

**GTT — Governance Through Thinking** is a methodology for AI-assisted software development.

It establishes the context, evidence, architectural intent, decision boundaries and controlled change mechanisms that let AI agents and ADEs (AI Development Environments) do real work on a system without becoming the authority over it.

The name states the method:

- **Governance**: the human keeps authority over what the system is supposed to be.
- **Through Thinking**: that authority is exercised on explicit, evidence-grounded reasoning (THINK, §15), not on prompts and not on the code the agent happened to produce.

The central distinction is:

> **Agent reasoning ≠ Change authorization.**

GTT is not an agent framework, an ADE or a security product (§5). It is the governance layer that sits above them and stays valid whichever agent, model or ADE is used.

## 2. The Problem GTT Solves

AI agents generate implementation faster than humans can review it. Without a governed reference:

- the code becomes the de facto specification;
- architectural intent erodes one reasonable-looking change at a time;
- decisions are made implicitly inside prompts and conversations, with no trace;
- every agent session starts from a different, partial picture of the system;
- switching agent or ADE means losing the project's "memory".

GTT gives the project a single governed source of truth that the human ratifies. Agents reason over it, propose changes to it, and implement freely inside it. Only the human changes it.

## 3. Authority Model

The operating model is:

> **Human decides → GTT governs → Agent / ADE executes within the boundary.**

For any change to governed state, including protected artifacts:

> **Agent proposes → Human decides → GTT applies.**

- The **human** is the only decision authority over governed state.
- **GTT** holds the governed state, enforces the boundary deterministically, and applies a decision exactly as decided.
- The **agent** reasons, proposes and implements.
- The **ADE** is the execution environment.

No configuration, autonomy profile, ADE or agent can transfer decision authority away from the human.

## 4. Principles

### 4.1 Context before reasoning
AI reasoning must operate over an explicit, governed context.

### 4.2 Evidence before assumption
Authoritative evidence must be distinguished from gaps, conflicts, proposals and assumptions.

### 4.3 Reasoning does not create authority
An agent's reasoning does not make a proposed change valid. A proposal is never a decision and is never written as one.

### 4.4 Human decision boundary
Architectural and governance decisions remain with the human. Agents prepare; only a human decides, ratifies and freezes.

### 4.5 Provenance
Important conclusions and proposals must be traceable to their evidence, or explicitly marked as a gap, a conflict or a proposal.

### 4.6 Deterministic governance
Wherever a governance rule can be computed, a deterministic tool computes it. A fact that can be computed is never established by an agent's judgment alone, and no agent supervises another agent as governance.

### 4.7 ADE and agent independence
GTT must not depend on any particular agent, model, vendor or ADE.

### 4.8 Governance before execution
An operation that would change governed state must pass the applicable GTT boundary before it becomes an accepted change.

### 4.9 Context efficiency
GTT should provide the minimum sufficient governed context for an operation, never at the cost of omitting an applicable constraint, decision, protection or required evidence.

### 4.10 Derived session continuity
Session continuity is derived from the project's actual state, not from agent-authored memory. Session state is never authority.

### 4.11 Low-friction governance
> **The developer decides what only the developer can decide. GTT does everything else.**

Before interrupting the developer, GTT must check whether the answer can be determined from governed context, the active autonomy profile, project policy, project state or deterministic rules. If it can, GTT acts, validates and reports. A human is asked only for a genuine decision or authorization.

Friction is reduced by automation, never by weakening governance. Ordinary work (implementation, tests, refactors, commits, Stories) is never gated on a confirmation.

### 4.12 Honest verification
Every governance claim states how it is verified: **verified** (a deterministic check), **partial**, or **declared-only** (recorded, checked by human audit). GTT must never present a declared rule as a verified one.

### 4.13 Cooperative integration
GTT adds itself to a project; it does not take the project over. It must coexist with instructions, tools and conventions the project already has (§28).

## 5. What GTT Is Not

GTT does not become:

- an AI agent framework or a replacement for an ADE;
- an operating-system, Git, CI/CD or production authorization system;
- an autonomous architectural decision-maker;
- a replacement for human governance;
- a mandatory dependency on any vendor (Claude Code, Kiro, Codex, Copilot, Cursor, Antigravity or any other);
- a requirement that all project context be loaded into every agent session;
- a replacement for an ADE's native memory, rules or instruction mechanisms;
- a gate on ordinary development work.

GTT may integrate with all of these without depending on them.

---

# Part II — Model

## 6. The Two Planes

GTT divides a project into two planes with different rules.

| | **Governance plane** | **Work plane** |
|---|---|---|
| Answers | What the system is supposed to be | How it is being built |
| Holds | Intent, architecture, constraints, decisions (ADRs), Epics (goal and scope), development method, protected artifacts, observed boundaries, the freeze | Code, tests, refactors, commits, Stories, day-to-day tasks |
| Who changes it | Only the human, through the change flow (§18) | The agent and the developer, freely |
| GTT's role | Holds and enforces the boundary | Observes (§17), never gates |

> Governance before and above autonomous work; observation during work.

An observation in the work plane that is not BLOCKING never stops the work.

## 7. Governed Context

**Governed Context** is the controlled body of information that defines the system: vision, principles, architecture, stack, constraints, glossary, the design of each Epic, ADRs, approved decisions and the declared sources.

- It is the **source of truth** for the project. Generated code is an artifact of the governed context, never a replacement for it.
- Working preferences and conversation content must not silently override it (§26).

### 7.1 Execution Context
The **Execution Context** is the minimum relevant projection of Governed Context that GTT selects for a specific operation: a task, an artifact or a decision. It is not a new authority layer. Reducing context must not remove anything required to enforce a boundary.

### 7.2 Context efficiency
The goal is *minimum sufficient governed context, not maximum available context*. Efficiency reduces token cost, duplication, conflicting copies and accidental authority created by unrelated material. GTT should provide a retrieval index so an agent can locate the authoritative section for a concept instead of loading whole documents.

## 8. Evidence Boundary

GTT separates seven stages:

```text
authorized sources → grounding → evidence dossier → reasoning → proposal → human decision → governed artifact
```

- **Sources** are declared and versioned. A selected source is not an authority; authority and precedence are declared, never inferred.
- **Grounding** retrieves and exposes evidence. It does not decide.
- The **Evidence Dossier** consolidates the relevant evidence for reasoning. It is a governed representation of evidence, not an authority.
- **Reasoning** operates over the dossier and must not use raw sources to bypass the boundary.
- A **conflict** between sources is exposed, never silently resolved.

### 8.1 Provenance tags

| Canonical tag | Meaning | Accepted alias |
|---|---|---|
| `[SOURCE: file:line]` | Statement backed by a declared source | `[FUENTE: …]` |
| `[GAP]` | Required information is missing | `[VACÍO]`, `[VACIO]` |
| `[CONFLICT]` | Sources disagree; not resolved | `[CONFLICTO]` |
| `[PROPOSAL]` | Agent proposal awaiting a human decision | `[PROPUESTA]` |

Rules:

- An unmarked statement during elicitation is not authoritative evidence.
- `[PROPOSAL]` must never appear inside ratified governed context.
- An unresolved `[CONFLICT]` refuses freeze.
- A blocking `[GAP]` refuses freeze. A non-blocking gap is recorded as **OPEN**. An OPEN item is scoped and visible, and it authorizes nothing.

The aliases are accepted for compatibility with GTT 2.1 projects. New artifacts should use the canonical tags.

## 9. Architectural Intent

GTT preserves the intended architecture through:

- governed context;
- **Architecture Decision Records (ADRs)**;
- the development method (§20);
- declared dependency boundaries;
- protected artifacts.

Rules:

- ADRs should follow a widely used format (MADR-compatible: status, context, decision, consequences) so standard ADR tooling can read them.
- An ADR records a decision the human has already approved. Writing an ADR is not a way to make a decision.
- Architectural drift must be surfaced (§17), never silently accepted.

## 10. Work Breakdown

| Unit | Plane | Governed? | Change path |
|---|---|---|---|
| **Epic** (goal, scope, design) | Governance | Yes. Each Epic's design is approved by the human and frozen with the context | Change flow (§18) |
| **Story** (task inside an Epic) | Work | No | Created, changed and closed freely by agent or developer |

The backlog must stay reconciled with the defined Epics. A Story never requires a proposal or an approval.

## 11. Human Decision Boundary

GTT separates *reasoning* from *decision authority*.

- A human may approve, reject, modify or ask for more analysis of any proposal.
- Automated enforcement may decide whether an operation is permitted under an already-defined policy. It must not create a new architectural decision.
- When an agent "helps choose" (for example, a development method), it proposes with its reasoning. Without a human answer the item stays **OPEN**. **No value is ever applied silently by default.**

---

# Part III — Lifecycle

## 12. Lifecycle Overview

```text
 SETUP ──► ELICITATION ──► THINK ──► FREEZE ──► BUILD ◄──► OBSERVE
   │        (sources,       (design,   (human      (work plane,  (drift,
   │         questionnaire,  Epics,     ratifies)   free)         levels)
   │         assessment)     method)       ▲                        │
   │                                       │                        ▼
   │                                  RE-FREEZE ◄── CHANGE FLOW ◄───┘
   │                                                (request → proposal →
   │                                                 human decision → promote)
   └──► DESIGN RESET (§19): explicit return to a pre-freeze state
```

The steps are restartable and resumable. A conforming implementation must be able to tell, from project state alone, which step the project is in and what the next action is (§24).

## 13. Setup

Setup installs GTT into a project and records four human choices. None of them is defaulted silently.

1. **Participating ADEs and one Primary ADE.**
   - Detecting an ADE is not participation; the human chooses which ADEs participate.
   - The Primary ADE is only the one GTT hands off to. It has no governance authority.
2. **Language** of the governed artifacts and of the user-facing interaction. It is asked first, so it applies to everything that follows.
3. **Autonomy profile** (§21): how much GTT does without asking.
4. **Development method** (§20): how the solution is organized. The human may answer "help me choose".

Setup must follow cooperative integration (§28). It must never overwrite or take ownership of an existing instruction file such as `AGENTS.md`.

Setup ends with a **handoff**: an explicit statement of the next action and the artifact it concerns.

## 14. Elicitation and Design Assessment

Elicitation produces the material THINK needs to propose a governed design.

### 14.1 Sources
- The human declares one or more sources (design documents, requirements, specifications).
- Each source is recorded with its type and a content hash, and versioned. A changed source is a new version, never a silent edit.
- Several design documents may be declared together. They are either consolidated into one brief or kept as separate declared sources with explicit precedence. Conflicts between them are tagged `[CONFLICT]`.

### 14.2 Design Assessment
When a design exists, it is assessed in writing before use. A poor design is strengthened through proposals, never silently rewritten. The **minimum floor** of a governable design is:

- problem and scope;
- architectural style;
- technology stack;
- datastore.

### 14.3 Initial Design Questionnaire
When no design exists, GTT provides a guided questionnaire.

- Questions are asked one at a time.
- Before asking, the agent fills every answer it can support with evidence, tagged.
- It asks only about real gaps and decisions.
- Every minimum-floor line must be **MET**, meaning stated or confirmed by the human.

The questionnaire has an observable state: `absent`, `pending`, `in-progress` or `complete`.

### 14.4 Readiness and THINK Depth
- Readiness is `NOT_READY`, `READY` or `READY_WITH_OPEN_ITEMS`. A readiness above `NOT_READY` requires every minimum-floor line MET.
- THINK Depth (`QUICK`, `STANDARD`, `DEEP`) sets how far THINK explores. It never lowers the minimum floor.

### 14.5 Proactive start and resumption
- When the project is waiting on elicitation, the Primary ADE must be told so at session start, through the deterministic next action (§24). It must not depend on the user knowing a file path or writing a special prompt.
- Elicitation must resume across sessions without asking again what is already resolved.
- Each ADE's adapter (§27) is responsible for delivering the next action at session start, by the most reliable mechanism that ADE offers:
  - a session-start hook;
  - an MCP resource or prompt;
  - an always-on rule, as a last resort.

## 15. THINK

**THINK** is the reasoning phase in which an agent turns evidence into a proposed governed design.

THINK:

- reads the evidence dossier, not raw sources;
- proposes the governed context: vision, principles, architecture, stack with declared dependency boundaries, constraints and glossary;
- cuts the solution into **Epics** according to the development method, and drafts one design per Epic using the method's template;
- marks everything it cannot ground as `[GAP]`, `[CONFLICT]` or `[PROPOSAL]`.

THINK output is a proposal. The human reviews it in two confirmations:

- **Brief confirmation:** the consolidated source brief is the right input.
- **Context confirmation:** the drafted governed context and Epic designs are accepted.

THINK is also used inside the change flow (§18), for impact analysis of a change request.

## 16. Freeze

A **freeze** is the human ratification of a governed state.

- Only a human freezes. Freezing must be refused when any of these holds:
  - placeholders remain;
  - a required artifact is missing;
  - provenance is malformed;
  - a `[CONFLICT]` or a blocking gap is unresolved;
  - a source hash does not match;
  - an Epic design is unapproved;
  - the development method is undeclared or inconsistent;
  - declared boundaries do not parse;
  - on a re-freeze, a BLOCKING observation is open.
- A freeze records a digest of the governed state, the source commit and its history.
- **There is no unfreeze.** A frozen state changes only through the change flow (§18), which ends in a new freeze.
- Elicitation scaffolding that no longer applies is removed at the first freeze.

## 17. Build and Observation

After freeze, development proceeds in the work plane without approvals.

GTT **observes** the project against the frozen state and its declared boundaries, deterministically, and graduates each deviation:

| Level | Meaning | Effect |
|---|---|---|
| `NOTICE` | Informational deviation | Recorded; not announced; work continues |
| `WARNING` | Meaningful drift, blocks nothing | Announced once, recorded; work continues |
| `GOVERNANCE` | A boundary of the design was crossed | Announced once, recorded; a human decision is needed before the next freeze (and before promotion where the profile says so); work continues |
| `BLOCKING` | A state the governed design explicitly prohibits | The affected operation stops until fixed; everything else continues |

Observations go to an idempotent **governance backlog**. The human may accept, reject or defer each one. Only a boundary ratified as BLOCKING, or an observation the human rejected, stops anything.

The judgment counterpart of observation is an **audit**: a human-or-agent review of whether the governed context still matches the system, and of what no declared boundary covers.

## 18. Change Flow

Every change to governed state follows one path:

```text
Change request (human, in plain words)
      ↓
Impact analysis + THINK (agent)
      ↓
Proposal (agent) ──► staged as a promotion set
      ↓
Human decision: approve / reject / modify / withdraw
      ↓
Promote (GTT applies the approved set atomically, with backup)
      ↓            └──► Undo (restores the backup of the last promotion)
New freeze
```

Rules:

- The change request is written by the human. The agent processes it; it does not invent one.
- A **proposal** states the intended change explicitly enough that the human can decide without reading the agent's reasoning. It lives in the Proposal Store (§31), never in the governed context itself.
- A **promotion set** is the complete list of governed artifacts a proposal changes. It is applied all-or-nothing.
- Promotion is a deterministic GTT operation triggered by the human. An agent must not apply a promotion by writing governed files directly, nor by hand-writing an application script.
- Resolving an OPEN item, changing an Epic, changing the development method, lowering the autonomy profile, and changing a protected artifact are all changes to governed state, and follow this flow.

## 19. Design Reset

A project must be able to start its design over **explicitly**:

- **Before the first freeze**, a design reset returns the project to a clean elicitation state. Sources may be replaced, added or removed, and the questionnaire restarted. A snapshot is taken first, so the reset can be undone.
- **After a freeze**, a reset of governed state is a change request (§18) with impact analysis, never a shortcut around the freeze.
- **Adding or replacing design documents** at any time records new source versions and triggers a new design assessment. After a freeze, it enters through the change flow.
- A reset must never silently restore what the human asked to discard.

---

# Part IV — Capabilities

## 20. Method Governance

The **development method** shapes how THINK designs and how the solution is cut into Epics. It is declared at setup, ratified at freeze, and verified afterwards.

```text
declare (setup / THINK)  →  ratify (freeze)  →  verify (check / CI / audit)
```

### 20.1 Method Packs
A **Method Pack** is the single source of a method's definition. Implementations, CLIs and websites read it; they must not keep parallel texts. Each pack declares:

- **Identity:** id, name, kind (`architecture-style` or `decomposition-strategy`), the setup question it answers, and whether it is recommended.
- **Relations:** whether it requires an ADR, and which methods it is compatible and incompatible with.
- **Verification level:** `verified`, `partial` or `declared-only`.
- **Content sections:**
  - what it is;
  - when it fits;
  - **what it rules out** (a method that rules nothing out has no place in the catalog);
  - its effect on Epics, Epic design, architecture and dependency rules;
  - its Epic design template;
  - its verification rules.

Methods may be combined (for example, DDD to define Epics and Vertical Slice to cut them). Incompatibilities are declared in the pack and warned about at selection.

### 20.2 Method Lock
The ratified method choice is the **Method Lock**, recorded in the governed architecture.

- An architectural style requires an ADR.
- A decomposition strategy is a light control and needs no ADR.
- After freeze, changing the method is a change request with impact analysis. It never reopens the questionnaire.

### 20.3 Method Conformance
Conformance rules are **architecture fitness functions**: executable checks that the system still respects the ratified method (for example, "a slice does not depend on another slice").

- Rules are written in a neutral form.
- GTT must not implement its own code analyzers. It uses each ecosystem's native tool through an adapter (for example, dependency-cruiser, import-linter, ArchUnit, NetArchTest, deptrac, Go package analysis).
- The result of a check is one of `pass`, `partial`, `conflict`, `declared-only` (no adapter for this stack), `not-declared`, `open` or `cannot-determine`.
- A method violation is a **reported conflict**, never a silent correction of code or context.
- Rules that cannot be computed are listed for audit, as declared-only.

## 21. Autonomy Profiles

An **autonomy profile** sets how much GTT does without asking. It never changes *who decides*: governed decisions always belong to the human, in every profile.

| Profile | For | GTT does without asking |
|---|---|---|
| **Light** | One developer, governed prototype | Every deterministic operation and every safe relevant change; asks only for governed decisions and destructive operations |
| **Medium** | Individual project, small team | Every deterministic operation; relevant changes are confirmed first |
| **Hard** | Production, critical systems | Derived state only (index, validation); ids and relevant changes are proposed and confirmed |
| **Team** | Collaborative governance | Hard gates plus CI, actor traceability and the team's declared working agreements |

- After freeze, moving to a stricter profile is allowed. Moving to a less strict one is a governed change (§18).
- GTT 2.1 implementations called these **Method Plans**. That name is deprecated because it was easily confused with the development method.

## 22. GTTGuard

GTTGuard protects the **authority to change** specific files, classes or methods.

> GTTGuard does not hide code from agents. It protects the authority to change it.

- **Declaration:** the developer declares protection in the source, with the language's native syntax: `@GTTGuard` in Java or Python, `[GTTGuard]` in C#, or a comment marker in languages without annotations.
- **Normalization:** GTT detects declarations and maintains a normalized **Protection Registry**. For each protected artifact it records what is protected, why, under which policy, and where the protection came from. The developer never edits the registry by hand.
- **Policy:** the canonical policy is `HUMAN_APPROVAL`. The agent may read, analyze and propose changes; the change is applied only through the change flow (§18) after explicit human approval.
- **Scope:** GTTGuard is a methodology-level mechanism. It does not replace filesystem, Git, branch-protection, CI/CD or production controls.

## 23. Enforcement

Enforcement applies already-defined rules to operations an agent or ADE requests (read, create, write, modify, delete, execute, propose, freeze, change governed state). Each request is evaluated against:

- the actor;
- the operation;
- the artifact;
- the governed state;
- the protection policy;
- the applicable constraints.

The result is **allow**, or **deny with a governance response**: a pointer to the proposal path.

- The decision logic must be shared by all adapters, so every ADE gets the same answer for the same request.
- Each ADE integration declares its **enforcement level**:
  - `realtime-verified`: a hook that blocks, tested inside that ADE;
  - `realtime-unverified`: a hook exists but has not been tested in the ADE;
  - `ci-gate`: enforced only when the change reaches CI;
  - `instruction-only`.
- Enforcement is not filesystem security.

## 24. Validation, Status and Next Action

- **Validation** is one entry point that runs every deterministic governance check and returns structured results:
  - required artifacts and placeholders;
  - provenance;
  - source integrity;
  - Epic designs;
  - Method Lock;
  - dependency boundaries;
  - backlog reconciliation;
  - protection registry;
  - ADE adapter consistency;
  - contracts;
  - the retrieval index.

  It answers "is the governance structure valid?", never "is the architecture right?". That remains a human question.
- **Status** is derived from actual project state, never from an agent's narrative:
  - lifecycle step;
  - freeze;
  - pending change request;
  - proposals;
  - OPEN items;
  - observations;
  - validation;
  - protected artifacts.

  Status is observational and creates no authority.
- **Next action** is a deterministic one-line statement of what the project is waiting on, and from whom. It is computed for every lifecycle state, including a freshly installed project. It is what every adapter delivers at session start.

## 25. Session Continuity

Session continuity is generated by GTT from project state, not maintained by hand or by agent memory. Its inputs are:

- Git history;
- the last freeze;
- proposals;
- the change request;
- OPEN items;
- observations;
- validation status;
- the next action.

Rules:

- Session state is an operational handoff. It is never evidence, a decision or a constraint, and it must not enter grounding just because it exists.
- Agent-authored notes in session state are non-authoritative unless ratified through the change flow.
- Continuity is portable across ADEs. It complements native ADE memory and does not replace it.

## 26. Working Preferences and Team Agreements

- **Working preferences** (personal, local) describe how someone likes to work.
  - They are not authority and not evidence.
  - They have a bounded size because they are loaded repeatedly.
  - An agent must not create or elevate its own preferences.
- **Team working agreements** (shared, under the Team profile) declare team policy for what GTT may do without asking. Where the team has declared nothing, propose-and-confirm applies.
- A preference or agreement that contradicts governed context is invalid for that context. A rule that deserves authority enters the change flow.

---

# Part V — Integration

## 27. Control Plane and Adapter Boundary

The **GTT Control Plane** is the logical set of governance services:

- context selection and retrieval;
- evidence handling;
- validation;
- protection;
- enforcement;
- proposals and promotion;
- freeze;
- observation;
- session continuity;
- status and next action.

It is a logical concept, not a required network service.

ADEs connect through **adapters** that translate ADE-specific mechanisms into the GTT contract:

- hooks;
- rules;
- skills;
- steering;
- workflows;
- MCP.

Rules:

- An adapter must not redefine GTT, and GTT must not require an agent to reproduce GTT logic in its prompt.
- Each adapter declares:
  - its enforcement level (§23);
  - how it delivers the next action at session start (§14.5).
- **MCP (Model Context Protocol)** is the preferred vendor-neutral adapter. A conforming implementation should expose status, next action, validation, method check, retrieval and proposal staging as MCP tools and resources, and the elicitation flow as an MCP prompt, so that any MCP-capable ADE gets the same governance without a bespoke overlay.
- **CI is an adapter too.** A CI gate runs validation and conformance on every change request to the repository (for example, a pull request) and reports governance results. It is the enforcement floor for ADEs without real-time hooks.

## 28. Cooperative Instruction Files

Projects already contain agent instruction files (`AGENTS.md`, `CLAUDE.md`, editor rule files) and tool directories shared with other tools.

- GTT must **never** overwrite, take ownership of, or refuse to install because of such a file.
- GTT writes its instructions as a **delimited, owned block** inside the existing file (for example, between `<!-- gtt:begin -->` and `<!-- gtt:end -->` markers). It updates and removes only that block.
- In shared directories, GTT owns only the files it created and declares them.
- GTT's instruction block should stay short. It states the invariants and points to the governed detail, instead of duplicating it in every ADE.

## 29. Relationship to Other Methods

GTT is a governance layer; it composes with development methods rather than replacing them.

- **Spec-Driven Development (SDD):** specs produced by SDD tooling are declared sources (§14.1) or Epic designs (§15). GTT adds what SDD leaves open: who may change a ratified spec, provenance of each statement, a freeze, and a governed change flow. GTT is the governance layer over SDD.
- **Evolutionary architecture:** Method Conformance (§20.3) implements architecture fitness functions.
- **ADRs:** GTT uses ADRs as the record of decisions (§9).
- **C4 / arc42:** the governed architecture may be structured with these views. GTT does not mandate them.
- **Agile delivery:** Stories, iterations and boards live in the work plane and are not governed (§10).

## 30. CLI Role

A GTT CLI is an implementation vehicle for the methodology. It provides deterministic operations, including:

- install and update;
- setup choices;
- sources and design reset;
- status and next action;
- validation and conformance;
- freeze;
- change flow;
- adapters;
- export and clean.

Rules:

- The CLI must not redefine GTT. Method texts and rules come from the Method Packs and the governed contract, not from CLI strings.
- The CLI should be usable both interactively and non-interactively (CI). In non-interactive mode, every missing human decision is an explicit error that names the flag to pass, never a silent default.
- User-facing output must avoid internal terminology (§32).

---

# Part VI — Governance of the Canon

## 31. Artifact Roles

The canon defines **roles**, not paths. A conforming implementation maps each role to a location and publishes the map. The reference map is Annex A.

| Role | Holds |
|---|---|
| **Governed Context** | Vision, principles, architecture (with Method Lock), stack (with dependency boundaries), constraints, glossary, declared sources |
| **Epic Designs** | One design per Epic |
| **Decision Records** | ADRs |
| **Backlog** | Epics and Stories |
| **Change Request** | The human's current change request |
| **Proposal Store** | Proposals and staged promotion sets; withdrawn proposals |
| **Freeze Record** | Digest, commit and history of each freeze |
| **Governance Backlog** | Observations and their human decisions |
| **Protection Registry** | Normalized GTTGuard declarations |
| **Source Store** | Versioned, hashed declared sources and the consolidated brief |
| **Method Catalog** | Method Packs |
| **Setup Record** | Participating ADEs, Primary ADE, autonomy profile, development method, language |
| **Session State** | Derived session handoff |
| **Retrieval Index** | Artifact and section index |
| **Working Agreements / Preferences** | Team agreements (shared), personal preferences (local) |
| **Instruction Plane** | The agent contract and the ADE adapters |

The Governed Context, Epic Designs, Decision Records, Change Request, Freeze Record and Instruction Plane are protected from autonomous modification in an installed project.

## 32. Terminology

Canonical terms are defined in [`GLOSSARY.md`](GLOSSARY.md). It separates:

- **user terms**, which a developer needs to use GTT and which user-facing text may use freely;
- **internal terms**, which belong in contracts and implementation, not in prompts, errors or the website.

Terminology changes only through governance (§35). Implementations, documentation and the website must use the glossary's terms and must not keep parallel definitions.

## 33. Versioning

GTT has exactly two public version numbers:

- the **methodology generation** (this document: 3.0);
- the **suite version** (the reference implementation: Bootstrap, CLI and website released together under one number).

Contract and layout versions are internal and must not appear in user-facing text. Each suite release states which generation it implements. See [`VERSIONING.md`](VERSIONING.md).

## 34. Canon and Implementation

- The canon defines **what** GTT is. Implementations demonstrate it.
- A capability is canonical because it is defined here, not because it appears in code, a website, a manual, a prompt or a branch.
- For every capability defined here, [`CONFORMANCE.md`](CONFORMANCE.md) must record its implementation status in the current suite release: `implemented`, `partial` or `planned`. It must also give the evidence (operation, script or command, and test) or the identified gap.
- User-facing material (website, README, CLI help) may present a capability as available only when `CONFORMANCE.md` marks it `implemented`. Otherwise it must be labelled as planned or partial.
- If any GTT artifact contradicts this canon, the contradiction must be surfaced and resolved, not silently ignored. This applies in both directions:
  - an implemented feature missing from the canon is a governance gap;
  - a canonical capability with no implementation path is an implementation gap.

## 35. Change Governance of the Canon

- Changes to this document are themselves governed: proposed, decided by the canon's human maintainers, recorded in [`CHANGELOG.md`](CHANGELOG.md), and released as a new generation or revision.
- One generation is current at a time. [`GTT-CANONICAL.md`](GTT-CANONICAL.md) names it.
- A superseded generation remains readable at its original path, marked SUPERSEDED, so existing references do not break. It is no longer an authority.

## 36. Summary

```text
                          HUMAN
                   decides · ratifies · freezes
                            │
             change request │ ▲ proposal
                            ▼ │
   ┌──────────────────────────────────────────────────┐
   │                 GTT CONTROL PLANE                │
   │  Governed Context · Evidence · Method Lock       │
   │  Freeze · Change Flow · GTTGuard · Enforcement   │
   │  Validation · Observation · Next Action          │
   └──────────────────────────────────────────────────┘
          │ adapters (hooks · rules · MCP · CI)
          ▼
   ┌─────────────────┐        work plane: free
   │   ADE / AGENT   │ ───►  code · tests · Stories
   └─────────────────┘        observed, never gated
```

> **Agent reasoning ≠ Change authorization.**
>
> **Human decides → GTT governs → Agent/ADE executes within the boundary.**
>
> **Agent proposes → Human decides → GTT applies.**

The purpose is not to make the agent less capable. It is to let the agent's capability operate inside an explicit, traceable, efficient, deterministic and human-governed boundary, with as little friction for the developer as that boundary allows.

---

## Annex A — Reference Layout (GTT Bootstrap, suite ≥ 1.7)

Informative. Maps the roles of §31 to the reference implementation. Another implementation may choose different paths if it publishes its own map.

| Role | Reference path |
|---|---|
| Governed Context | `gtt-domain/context/` (`solution-vision.md`, `principles.md`, `architecture.md`, `stack.md`, `constraints.md`, `glossary.md`, `sources.md`) |
| Epic Designs | `gtt-domain/context/design/` |
| Decision Records | `gtt-domain/adr/` |
| Backlog | `gtt-domain/backlog.md` |
| Change Request | `gtt-domain/change-request.md` |
| Proposal Store | `gtt-domain/proposals/` (`staged/`, `withdrawn/`) |
| Freeze Record | `gtt-domain/.frozen` |
| Governance Backlog | `gtt-domain/governance-backlog.json` |
| Protection Registry | `.gtt/protection/registry.yaml` |
| Source Store | `docs/sources/<ID>/v<N>/`, `.gtt/selected-sources.json`, `SOURCE-BRIEF.*` at the project root |
| Method Catalog | `.gtt/methods/` |
| Setup Record | `.gtt/ade.json`, `.gtt/methodology.json` (autonomy profile), `.gtt/development-method.json` |
| Session State | `gtt-domain/session.md` |
| Retrieval Index | `.gtt/index/` |
| Working Agreements / Preferences | `gtt-domain/working-agreements.md` / `.gtt/local/preferences.md` |
| Instruction Plane | `AGENTS.md`, `.gtt/docs/agents/`, ADE overlays (`.claude/`, `.kiro/`, `.github/instructions/`, `.cursor/rules/`, `.agents/`, `.openhands/`) |

GTT 2.1 named `gtt/proposals/` and `gtt/protection/registry.yaml`. Those paths were never used by the reference implementation and are withdrawn.

## Annex B — Changes from GTT 2.1

See [`CHANGELOG.md`](CHANGELOG.md).
