# GTT Method

**Governed methodology for AI-assisted software development.**

GTT provides a governance layer for software development assisted by AI agents and ADEs. It establishes the context, evidence, architectural intent, decision boundaries, and controlled change mechanisms required to keep AI-assisted development aligned with human decisions and project intent.

> **Agent reasoning ≠ Change authorization.**

## What is GTT?

GTT is a methodology, not an AI agent framework.

It can govern development workflows using different AI agents, models, tools, and AI Development Environments (ADEs), without depending on a specific vendor or technology.

GTT establishes a clear separation between:

```text
Evidence
   ↓
Grounding
   ↓
Reasoning
   ↓
Proposal
   ↓
Human Decision
   ↓
Governed Change
```

## Core Principles

- **Context before reasoning**
- **Evidence before assumption**
- **Human decision boundary**
- **Architectural intent**
- **Provenance and traceability**
- **Deterministic governance where possible**
- **Agent and ADE independence**
- **Controlled change**

## GTT 2.1

The current canonical generation is **GTT 2.1**.

GTT 2.1 includes:

- Governed Context
- Evidence Boundary
- Evidence Dossier
- Architectural Intent
- Human Decision Boundary
- Governed State and Freeze
- Proposals
- Session Continuity
- GTTGuard
- CLI-oriented governance
- ADE / Agent independence

### GTTGuard

GTTGuard establishes a governance boundary around protected files, classes, and methods.

> **User proposes → GTT implements → Agent respects.**

Protected changes can be routed through `gtt/proposals/` for human approval.

GTTGuard is a governance mechanism, not an operating-system security mechanism.

## Canonical Specification

The canonical definition of each GTT generation is maintained in its versioned canonical document:

- [`GTT-CANONICAL-v2.1.md`](GTT-CANONICAL-v2.1.md) — **Current**
- [`GTT-CANONICAL-v3.md`](GTT-CANONICAL-v3.md) — **Planned**
- [`GTT-CANONICAL-v4.md`](GTT-CANONICAL-v4.md) — **Planned**

[`GTT-CANONICAL.md`](GTT-CANONICAL.md) identifies the current canonical generation.

## Repository Purpose

This repository contains the **GTT methodology and its canonical specifications**.

Implementation projects, CLI tooling, websites, manuals, and other integrations may consume and implement the methodology, but should remain aligned with its canonical definition.

## Project Status

**Current generation:** GTT 2.1  
**Status:** Released

GTT 3 and GTT 4 are planned future generations and are not yet released.

## License

GTT is released under the **Apache License 2.0**.