# GTT — Canonical v2.1

Status: CURRENT
Canonical status: RELEASED
Generation: GTT 2.1
Purpose: Canonical definition of GTT 2.1

---

## 1. What GTT Is

GTT is a methodology and governance control plane for governed AI-assisted software development.

GTT establishes the context, evidence, architectural intent, governance rules, decision boundaries, controlled change mechanisms, protection mechanisms, validation services, and session continuity mechanisms that allow AI agents and ADEs to participate in software development without becoming the authority over the system.

The central distinction is:

> Agent reasoning ≠ Change authorization.

Agents may analyze, reason, propose, generate, and implement within the authority granted by GTT. The human remains the authority for governed decisions.

GTT may enforce deterministic governance rules around agent and ADE operations. Enforcement does not transfer decision authority from the human to an agent.

---

## 2. Vision

GTT seeks to make AI-assisted software development:

- context-driven;
- evidence-grounded;
- architecturally coherent;
- traceable;
- reproducible;
- efficient in context consumption;
- governed;
- independent of any particular AI agent or ADE.

GTT is not an agent framework.

GTT is the governance methodology and control plane that can govern the use of different agents, ADEs, tools, and implementation technologies.

---

## 3. Core Principle

The fundamental operating model is:

> Human decides → GTT governs → Agent/ADE executes within the boundary.

For protected artifacts:

> Usuario propone → GTT implementa → Agente respeta.

The ADE is an execution environment. The agent provides reasoning and execution capability. GTT provides the governance boundary.

---

## 4. Core Principles

### 4.1 Context before reasoning

AI reasoning must operate over an explicit and governed context.

### 4.2 Evidence before assumption

Authoritative evidence must be distinguished from gaps, conflicts, proposals, and assumptions.

### 4.3 Reasoning does not create authority

An agent's reasoning does not make a proposed change automatically valid.

### 4.4 Human decision boundary

Architectural and governance decisions remain subject to the defined human decision boundary.

### 4.5 Provenance

Important conclusions and proposals must remain traceable to their evidence or explicitly marked as unresolved/proposed.

### 4.6 Deterministic governance

Where possible, governance rules must be machine-checkable rather than dependent on one AI agent supervising another.

### 4.7 ADE independence

GTT must not depend on Claude Code, Kiro, Codex, Copilot, or another specific ADE.

### 4.8 Governance before execution

An operation that can affect governed project state should pass through the applicable GTT governance boundary before it becomes an accepted project change.

### 4.9 Context efficiency

GTT should provide the minimum relevant governed context required for an operation rather than requiring an agent to consume the entire governed context.

Context reduction must never remove an applicable constraint, decision, protection, or required evidence.

### 4.10 Deterministic session continuity

Session continuity should be derived from project and GTT state whenever possible rather than relying on an agent-authored memory as the source of truth.

---

## 5. Evidence Boundary

GTT separates:

1. authorized sources;
2. grounding;
3. evidence dossier;
4. reasoning;
5. proposals;
6. human decision;
7. governed artifact.

Agents reason over governed evidence rather than treating arbitrary context as authority.

No worker should receive raw source material merely "for context" when the governed evidence boundary requires the worker to operate over the evidence dossier.

### Provenance markers

GTT uses explicit markers such as:

```text
[FUENTE: archivo:línea]
[VACÍO]
[CONFLICTO]
[PROPUESTA]
```

These markers distinguish evidence, missing information, conflicts, and proposed decisions.

An unmarked statement must not automatically be interpreted as authoritative evidence.

---

## 6. Governed Context

Governed Context is the controlled body of information used to understand and reason about a system.

It may include:

- requirements;
- architecture;
- source artifacts;
- ADRs;
- rules;
- constraints;
- specifications;
- approved decisions;
- evidence;
- governed state;
- session state where appropriate.

Working context and personal preferences must not silently override governed constraints.

A preference that contradicts a governed constraint is invalid for that governed context. The governed constraint takes precedence.

---

## 7. Governed Context vs Execution Context

GTT distinguishes between the complete governed context and the context actually supplied to an agent for a particular operation.

### Governed Context

The authoritative body of information available to GTT for governance and reasoning.

### Execution Context

The minimum relevant subset selected by GTT for a specific operation, task, artifact, or decision.

Conceptually:

```text
                 GOVERNED CONTEXT
                        |
                        v
                GTT Context Selection
                        |
        +---------------+---------------+
        |               |               |
      task          artifact       constraints
        |               |               |
        +---------------+---------------+
                        |
                        v
                EXECUTION CONTEXT
                        |
                        v
                    AGENT/ADE
```

Execution Context is not a new authority layer. It is a controlled projection of Governed Context.

A reduction in context must not remove information required to enforce a governance boundary.

---

## 8. Context Efficiency

GTT treats context efficiency as a governance concern.

The objective is not to maximize the amount of context supplied to an agent. The objective is to provide sufficient relevant context while avoiding unnecessary context.

Context efficiency seeks to reduce:

- irrelevant token consumption;
- duplicated context;
- conflicting copies of information;
- unnecessary retrieval;
- agent noise;
- context-window pressure;
- accidental authority created by unrelated material.

The governing principle is:

> Minimum sufficient governed context, not maximum available context.

Context efficiency must never be used as justification for omitting a mandatory constraint, protected decision, required evidence, or applicable governance rule.

---

## 9. Architectural Intent

GTT preserves the intended architecture and design decisions of the system.

Architectural Intent can be expressed through:

- ADRs;
- specifications;
- rules;
- constraints;
- protected artifacts;
- approved proposals;
- governed state.

Architectural drift must be surfaced rather than silently accepted.

---

## 10. Dossier

The evidence dossier consolidates the relevant evidence available for reasoning.

The dossier is not itself a source of authority. It is a governed representation of evidence from authorized sources.

Grounding retrieves and exposes evidence. It does not decide architecture.

Where sources conflict, the evidence boundary must expose the conflict rather than silently resolving it.

---

## 11. Reasoners / Workers

GTT may use multiple reasoners, workers, agents, or ADEs.

Their role is to:

- analyze;
- identify gaps;
- identify conflicts;
- propose solutions;
- generate implementation artifacts;
- validate according to deterministic rules where possible.

They do not become the final authority merely because they produced an answer.

A worker must not use access to raw sources to bypass the evidence boundary.

---

## 12. Human Decision Boundary

GTT explicitly separates:

```text
Reasoning
    from
Decision authority
```

A human may approve, reject, modify, or request further analysis of a proposal.

Automated enforcement may determine whether an operation is permitted under an already-defined governance policy, but it must not silently create a new architectural decision.

---

## 13. Governed State and Freeze

A governed state represents an accepted state of the project.

Freeze establishes a boundary around that accepted state.

After freeze, changes must follow a governed change path rather than silently modifying the accepted state.

There is no implicit autonomous `unfreeze`.

A post-freeze change should follow:

```text
Change Request
    ↓
Impact Analysis
    ↓
THINK / Reasoning
    ↓
Proposal
    ↓
Human Decision
    ↓
New Governed State / Freeze
```

An OPEN item does not authorize a change.

Resolving an OPEN item is a new decision and follows the governed change path.

---

## 14. Proposals

When an agent identifies a change that requires human authority, the change becomes a proposal.

For protected artifacts, proposals are stored under:

```text
gtt/proposals/
```

The proposal must make the intended change explicit so that a human can decide whether to apply it.

Agents may create proposal artifacts according to the protected proposal path. They must not silently modify protected governed artifacts in order to apply the proposal.

---

# 15. GTTGuard

GTTGuard is a GTT capability for protecting artifacts from autonomous modification.

It provides a governance boundary around:

- files;
- classes;
- methods.

GTTGuard is not an operating-system security mechanism. It does not replace:

- filesystem permissions;
- Git permissions;
- branch protection;
- CI/CD authorization;
- production access controls.

It is a methodology-level change authorization mechanism.

### Principle

> GTTGuard does not hide code from agents. It protects the authority to change it.

An agent may read, understand, analyze, reference, and propose changes to protected artifacts.

It may not autonomously turn a protected change into an accepted change when the protection policy requires human approval.

---

## 16. GTTGuard Source Declaration

The developer declares protection directly on the source artifact using the appropriate syntax for the language.

Examples:

```java
@GTTGuard
public class PaymentService {
}
```

```python
@GTTGuard
class PaymentService:
    pass
```

```csharp
[GTTGuard]
public class PaymentService
{
}
```

Comment-based languages may use their native comment syntax.

The developer does not manually maintain the normalized registry.

---

## 17. GTTGuard Normalization

GTT detects the source declaration, identifies the language and protected symbol, and normalizes the protection into a machine-readable registry.

Example:

```yaml
protected:
  - artifact: src/payment/PaymentService.java
    symbol: PaymentService
    protection: HUMAN_APPROVAL
    reason: "Architectural boundary"
    source: "ADR-014"

  - artifact: src/payment/PaymentService.java
    symbol: PaymentService.calculatePayment
    protection: HUMAN_APPROVAL
    reason: "Financial calculation"
    source: "ADR-021"
```

The registry is maintained by GTT rather than being a manual YAML task for developers.

Recommended location:

```text
gtt/protection/registry.yaml
```

The registry answers:

```text
WHAT is protected?
WHY is it protected?
WHAT protection policy applies?
WHERE did the protection originate?
```

---

## 18. GTTGuard Policy

The initial policy is:

```text
HUMAN_APPROVAL
```

Meaning:

> The agent may reason about and propose a change to the protected artifact, but the change requires explicit human approval before becoming an accepted modification.

Future policies may be introduced through governed evolution, but they are not automatically part of the current canonical generation.

---

## 19. GTTGuard Change Flow

For a protected artifact:

```text
Agent identifies required change
          ↓
GTT evaluates protection
          ↓
Protected?
          ↓
YES
          ↓
Generate proposal
          ↓
gtt/proposals/
          ↓
Human reviews
          ↓
Human approves / rejects / modifies
          ↓
Approved change becomes governed
```

For an unprotected artifact, the normal workflow remains available.

---

## 20. GTT Enforcement

GTT 2.1 defines enforcement as the application of already-defined GTT governance rules to operations requested by an agent or ADE.

Conceptually:

```text
Agent / ADE
    |
    | operation request
    v
+-------------------------+
|       GTT ENFORCEMENT   |
|                         |
| actor                   |
| operation               |
| artifact                |
| governed state          |
| protection policy       |
| applicable constraints  |
+------------+------------+
             |
       +-----+-----+
       |           |
     ALLOW       DENY
       |           |
       |        PROPOSAL /
       |        GOVERNANCE
       |        RESPONSE
       v
    execute
```

Possible operation classes include:

```text
READ
CREATE
WRITE
MODIFY
DELETE
EXECUTE
PROPOSE
FREEZE
CHANGE_GOVERNED_STATE
```

The exact implementation may expose different operation names.

The canonical requirement is that an operation affecting governed state must be evaluated against the applicable GTT boundary.

GTT enforcement is not filesystem security and does not replace OS, Git, CI/CD, repository, or production authorization.

---

## 21. GTT Control Plane

GTT defines a logical **Control Plane** for governance.

The Control Plane may provide:

- context selection;
- evidence handling;
- validation;
- protection;
- proposal handling;
- governed state;
- freeze;
- session continuity;
- status;
- policy enforcement;
- alignment checks.

The Control Plane does not replace the human decision boundary.

Conceptually:

```text
                         HUMAN
                           |
                    Decisions / Approval
                           |
                           v
                 +---------------------+
                 |    GTT CONTROL      |
                 |       PLANE         |
                 |                     |
                 | Context             |
                 | Evidence            |
                 | Validation          |
                 | Protection          |
                 | Proposals           |
                 | Freeze              |
                 | Session State       |
                 | Policy Enforcement  |
                 +----------+----------+
                            |
                     governed execution
                            |
                            v
                    +---------------+
                    |   ADE / AGENT |
                    +-------+-------+
                            |
                            v
                    Project Artifacts
```

The Control Plane is a logical architectural concept. It does not require GTT to be deployed as a network service.

---

## 22. ADE Adapter Boundary

GTT remains independent of any particular ADE.

An implementation may provide an adapter or integration boundary for:

- Claude Code;
- Kiro;
- Codex;
- GitHub Copilot;
- other ADEs and agents.

The adapter translates ADE-specific operations into the GTT governance contract.

Conceptually:

```text
                 GTT CONTROL PLANE
                         |
                  GTT contract
                         |
             +-----------+-----------+
             |           |           |
          Adapter     Adapter     Adapter
             |           |           |
          Claude       Kiro        Codex
```

An ADE adapter must not redefine GTT.

If an ADE has a native memory, rules, hooks, permissions, or context mechanism, those mechanisms remain ADE-specific. GTT governs the project boundary independently.

---

## 23. ADE and Agent Independence

GTT is designed to work with different AI development environments.

Examples include:

- Claude Code;
- Kiro;
- Codex;
- GitHub Copilot;
- other agents and ADEs.

The ADE is an execution environment.

The agent provides reasoning and execution capability.

GTT provides the governance methodology and control boundary.

GTT must not require an agent to reproduce GTT's governance logic in its own prompt in order for the methodology to remain valid.

GTT must not require the native memory, instruction, hook, or permission system of a particular ADE.

---

## 24. Session Continuity

GTT supports controlled continuity between development sessions.

Session continuity may preserve:

- current work;
- decisions;
- unresolved items;
- proposals;
- changes;
- relevant project state;
- the relationship between active work and the last freeze;
- current validation and governance state.

### 24.1 GTT-generated session state

Session continuity should be generated from project and GTT state by the GTT implementation rather than requiring the human or an agent to manually maintain the authoritative session record.

The generated state may be derived from:

- Git history;
- last freeze;
- active proposals;
- change requests;
- governed state;
- OPEN items;
- ADR references;
- validation status;
- current worktree/project state.

A status operation may materialize this information as a session handoff or session-state artifact.

### 24.2 Session state is not authority

Session state is an operational handoff mechanism.

It is not automatically:

- evidence;
- an ADR;
- a constraint;
- a governed decision;
- a replacement for the source of truth.

A session summary must not silently enter the grounding corpus merely because it exists.

If a session record contains agent-authored notes, those notes are non-authoritative unless separately ratified through the normal GTT path.

### 24.3 ADE independence

Session continuity must remain portable across ADEs.

Claude Code may have its own native memory or instruction mechanisms. Kiro, Codex, Copilot, and other ADEs may have different mechanisms.

GTT session continuity must not depend on any one of them.

GTT session state complements native ADE memory; it does not replace or conflict with ADE-specific memory mechanisms.

---

## 25. Working Preferences

GTT distinguishes operational preferences from governed project decisions.

Preferences may describe how an individual or team prefers to work.

Examples include:

```text
user-level working preferences
team working agreements
```

Preferences are not L0 authority.

A preference that contradicts a governed constraint is invalid for that governed context.

The agent must not be allowed to create or silently elevate its own working preferences.

Preferences must not be treated as evidence during grounding.

Preferences should have a bounded context budget because they are loaded repeatedly.

An agreement that deserves governed authority is not a preference; it must enter the normal governed decision path.

---

## 26. Validation

GTT provides deterministic validation wherever the governance rule can be expressed deterministically.

Validation may include:

- protected artifact checks;
- proposal path checks;
- provenance structure checks;
- unresolved BLOCKING checks;
- OPEN classification checks;
- freeze consistency;
- governance state consistency;
- required artifact presence;
- session-state consistency;
- alignment between implementation and canonical governance;
- structural integrity of GTT metadata.

Deterministic validation must not pretend to determine semantic architectural correctness.

Semantic architecture remains subject to the human decision boundary.

The governing distinction is:

```text
Machine:
    "Is the governance structure valid?"

Human:
    "Is the architectural decision correct?"
```

---

## 27. Status and State Inspection

GTT may provide deterministic state inspection, such as:

```text
gtt status
```

A status operation should derive its report from actual GTT/project state.

It must not rely exclusively on an agent-authored narrative.

Status may expose:

- current governed state;
- last freeze;
- active proposals;
- active change request;
- OPEN items;
- validation results;
- protected artifacts;
- detected inconsistencies;
- session continuity state.

Status is observational. It does not itself create authority.

---

## 28. CLI Role

The GTT CLI is an implementation vehicle for the methodology.

It may provide deterministic operations for:

- initialization;
- context;
- validation;
- proposals;
- protection;
- status;
- freeze;
- alignment;
- session continuity;
- governance checks.

The CLI must not redefine GTT independently of the canonical methodology.

Where a capability is implemented by the CLI, its semantics must remain derived from the canonical GTT rules.

---

## 29. GTT Services / Capabilities

GTT provides a governance model covering:

1. Governed Context;
2. Evidence Boundary;
3. Evidence and Provenance;
4. Evidence Dossier;
5. Execution Context;
6. Context Efficiency;
7. Architectural Intent;
8. Reasoning / Workers;
9. Human Decision Boundary;
10. Proposals;
11. Governed State / Freeze;
12. GTTGuard;
13. GTT Enforcement;
14. GTT Control Plane;
15. Session Continuity;
16. Working Preference boundaries;
17. deterministic validation;
18. status and state inspection;
19. ADE / Agent independence;
20. ADE Adapter boundary;
21. Alignment with the canonical GTT definition.

These capabilities describe the governance services GTT provides; they are not necessarily separate network services.

---

## 30. What GTT Does Not Become

GTT does not become:

- an AI agent framework;
- a replacement for ADEs;
- an operating-system security layer;
- a Git authorization system;
- a CI/CD authorization system;
- an autonomous architectural decision-maker;
- a replacement for human governance;
- a mandatory dependency on Claude Code, Kiro, Codex, Copilot, or another vendor;
- a requirement that all project context be loaded into every agent session;
- a replacement for native ADE memory or instruction mechanisms.

GTT may integrate with these systems without becoming dependent on them.

---

## 31. Current vs Future

This document defines the current GTT 2.1 generation.

Anything not explicitly defined here must not be presented as an existing GTT 2.1 capability without a governed decision.

Implementation code may contain experimental, optional, or future mechanisms. Their presence in code does not automatically make them canonical.

Future generations may extend the methodology.

---

## 32. Canonical Terminology

Preferred terms:

- GTT;
- GTT Control Plane;
- GTT Enforcement;
- GTTGuard;
- Governed Context;
- Execution Context;
- Context Efficiency;
- Evidence Boundary;
- Evidence Dossier;
- Architectural Intent;
- Human Decision Boundary;
- Governed State;
- Freeze;
- Proposal;
- Session Continuity;
- Session State;
- Working Preferences;
- ADE;
- Agent;
- ADE Adapter;
- Provenance;
- Validation;
- Alignment.

Terminology should remain stable unless deliberately changed through governance.

---

## 33. Change Governance

Changes to this canonical definition must themselves be governed.

A new capability should not silently become part of GTT merely because it appears in:

- code;
- a website;
- a manual;
- an agent prompt;
- an experimental branch;
- a proposal.

The canonical definition establishes whether a capability is part of the methodology.

---

## 34. Canonical Rule

For GTT 2.1:

> If an implementation, website, manual, CLI, agent instruction, ADE integration, or other GTT artifact contradicts this canonical definition, the contradiction must be surfaced and resolved rather than silently ignored.

The same rule applies in the opposite direction:

> A capability described by the canonical definition must have an implementation path or an explicitly identified implementation gap.

The Canon defines the governance contract. The implementation demonstrates and operationalizes that contract.

---

## 35. Version Boundary

GTT 2.1 is the current released generation represented by this document.

This file remains the single canonical reference for the current GTT generation.

Future generations must be defined separately and must not be treated as current until formally released.

The canonical source is:

```text
GTT-CANONICAL-v2.1.md
```

The version history is maintained through version control. A historical version must not create a second competing current authority.

---

## 36. Canonical Summary

GTT 2.1 can be summarized as:

```text
                         HUMAN
                           |
                    Decision Authority
                           |
                           v
                 +---------------------+
                 |    GTT CONTROL      |
                 |       PLANE         |
                 |                     |
                 | Context             |
                 | Evidence            |
                 | Validation          |
                 | Protection          |
                 | Enforcement         |
                 | Proposals           |
                 | Freeze              |
                 | Session State       |
                 +----------+----------+
                            |
                     Execution Context
                            |
                            v
                    +---------------+
                    |   ADE / AGENT |
                    +-------+-------+
                            |
                            v
                    Project Artifacts
```

The fundamental rule remains:

> Human decides → GTT governs → Agent/ADE executes within the boundary.

And for protected artifacts:

> Agent proposes → GTT enforces the boundary → Human decides → governed change is applied.

GTT therefore governs both:

1. what context and evidence an agent may rely on; and
2. what changes an agent or ADE may cause to governed project state.

The purpose is not to make the agent less capable.

The purpose is to make the agent's capability operate inside an explicit, traceable, efficient, deterministic, and human-governed boundary.

---

**GTT — Context-Driven Governance for AI-Assisted Software Development**

Canonical principle:

> **Human decides → GTT governs → Agent/ADE executes within the boundary.**
