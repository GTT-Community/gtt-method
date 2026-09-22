# GTT — Canonical v2.1

**Status:** CURRENT  
**Canonical status:** RELEASED  
**Generation:** GTT 2.1  
**Purpose:** Canonical definition of GTT 2.1

---

## 1. What GTT Is

GTT is a methodology for governed AI-assisted software development.

GTT establishes the context, evidence, architectural intent, governance rules, decision boundaries, and controlled change mechanisms that allow AI agents and ADEs to participate in software development without becoming the authority over the system.

The central distinction is:

> **Agent reasoning ≠ Change authorization.**

Agents may analyze, reason, propose, generate, and implement within the authority granted by GTT. The human remains the authority for governed decisions.

---

## 2. Vision

GTT seeks to make AI-assisted software development:

- context-driven;
- evidence-grounded;
- architecturally coherent;
- traceable;
- reproducible;
- governed;
- independent of any particular AI agent or ADE.

GTT is not an agent framework. It is a governance methodology that can govern the use of different agents, ADEs, tools, and implementation technologies.

---

## 3. Core Principle

The fundamental operating model is:

> **Human decides → GTT governs → Agent executes within the boundary.**

For protected artifacts:

> **Usuario propone → GTT implementa → Agente respeta.**

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

### Provenance markers

GTT uses explicit markers such as:

```text
[FUENTE: archivo:línea]
[VACÍO]
[CONFLICTO]
[PROPUESTA]
```

These markers distinguish evidence, missing information, conflicts, and proposed decisions.

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
- session state where appropriate.

Working context and personal preferences must not silently override governed constraints.

---

## 7. Architectural Intent

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

## 8. Dossier

The evidence dossier consolidates the relevant evidence available for reasoning.

The dossier is not itself a source of authority. It is a governed representation of evidence from authorized sources.

---

## 9. Reasoners / Workers

GTT may use multiple reasoners, workers, agents, or ADEs.

Their role is to:

- analyze;
- identify gaps;
- identify conflicts;
- propose solutions;
- generate implementation artifacts;
- validate according to deterministic rules where possible.

They do not become the final authority merely because they produced an answer.

---

## 10. Human Decision Boundary

GTT explicitly separates:

**Reasoning**

from

**Decision authority.**

A human may approve, reject, modify, or request further analysis of a proposal.

---

## 11. Governed State and Freeze

A governed state represents an accepted state of the project.

Freeze establishes a boundary around that accepted state.

After freeze, changes must follow a governed change path rather than silently modifying the accepted state.

There is no implicit autonomous "unfreeze".

A post-freeze change should follow:

```text
Change Request
→ Impact Analysis
→ THINK / Reasoning
→ Proposal
→ Human Decision
→ New Governed State / Freeze
```

---

## 12. Proposals

When an agent identifies a change that requires human authority, the change becomes a proposal.

For protected artifacts, proposals are stored under:

```text
gtt/proposals/
```

The proposal must make the intended change explicit so that a human can decide whether to apply it.

---

# 13. GTTGuard

GTTGuard is a GTT 2.1 capability for protecting artifacts from autonomous modification.

It provides a governance boundary around:

- files;
- classes;
- methods.

GTTGuard is **not** an operating-system security mechanism. It does not replace:

- filesystem permissions;
- Git permissions;
- branch protection;
- CI/CD authorization;
- production access controls.

It is a methodology-level change authorization mechanism.

### Principle

> **GTTGuard does not hide code from agents. It protects the authority to change it.**

An agent may read, understand, analyze, reference, and propose changes to protected artifacts.

It may not autonomously turn a protected change into an accepted change when the protection policy requires human approval.

---

## 14. GTTGuard Source Declaration

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

## 15. GTTGuard Normalization

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

---

## 16. GTTGuard Policy

The initial GTT 2.1 policy is:

```text
HUMAN_APPROVAL
```

Future policies may be introduced through governed evolution, but they are not automatically part of GTT 2.1.

---

## 17. GTTGuard Change Flow

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

---

## 18. Session Continuity

GTT supports controlled continuity between development sessions.

Session continuity may preserve:

- current work;
- decisions;
- unresolved items;
- proposals;
- changes;
- relevant project state.

Session state is not automatically authoritative evidence.

A handoff or session summary must not silently become part of the grounding corpus merely because it exists.

---

## 19. CLI Role

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
- session continuity.

The CLI must not redefine GTT independently of the canonical methodology.

---

## 20. ADE and Agent Independence

GTT is designed to work with different AI development environments.

Examples include:

- Claude Code;
- Kiro;
- Codex;
- GitHub Copilot;
- other agents and ADEs.

The ADE is an execution environment.

GTT is the governance methodology.

---

## 21. GTT Services / Capabilities

GTT 2.1 provides a governance model covering:

1. Governed Context
2. Evidence and Provenance
3. Architectural Intent
4. Dossier
5. Reasoning / Workers
6. Human Decision Boundary
7. Proposals
8. Governed State / Freeze
9. Session Continuity
10. GTTGuard
11. CLI-based deterministic validation
12. ADE / Agent independence
13. Alignment with the canonical GTT definition

These capabilities describe the governance services GTT provides; they are not necessarily separate network services.

---

## 22. Current vs Future

This document defines **GTT 2.1**.

Anything not explicitly defined here must not be presented as an existing GTT 2.1 capability without a governed decision.

Future generations may extend the methodology.

---

## 23. Canonical Terminology

Preferred terms:

- GTT
- GTTGuard
- Governed Context
- Evidence Boundary
- Evidence Dossier
- Architectural Intent
- Human Decision Boundary
- Governed State
- Freeze
- Proposal
- Session Continuity
- ADE
- Agent
- Provenance
- Alignment

Terminology should remain stable unless deliberately changed through governance.

---

## 24. Change Governance

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

## 25. Canonical Rule

For GTT 2.1:

> **If an implementation, website, manual, CLI, agent instruction, or other GTT artifact contradicts this canonical definition, the contradiction must be surfaced and resolved rather than silently ignored.**

---

## 26. Version Boundary

GTT 2.1 is the current released generation represented by this document.

Future generations are defined separately:

- `GTT-CANONICAL-v3.md`
- `GTT-CANONICAL-v4.md`

Those documents must not be treated as current until their respective generations are formally released.
