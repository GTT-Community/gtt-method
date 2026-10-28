# Changelog — GTT Canon

## 3.0 — Released (2026-10-28)

GTT 3.0 redefines the canon from an end-to-end review of the suite at 1.7.0. The 2.1 canon had fallen behind the implementation: Method Governance, the change flow, the questionnaire, THINK Depth and Epics were shipped and advertised but not canonical, which the 2.1 canon itself forbids (§31, §33, §34). GTT 3.0 defines the **target** methodology. Implementation status lives in `CONFORMANCE.md`.

### Identity
- GTT = **Governance Through Thinking** (§1). Replaces three inconsistent expansions in circulation.
- One authority flow, in English: *Agent proposes → Human decides → GTT applies* (§3). 2.1 had two contradictory forms ("Usuario propone…" in §3 vs "Agent proposes…" in §36).
- New §2, *The Problem GTT Solves*.

### Principles
- Added **4.11 Low-friction governance**, adopted from the CLI's Developer Friction Minimization rule.
- Added **4.12 Honest verification** (verified / partial / declared-only).
- Added **4.13 Cooperative integration**.

### Model
- New **§6 Two planes** (governance vs work), already the basis of the Bootstrap contract.
- Governed Context, Execution Context and Context Efficiency merged into §7.
- Evidence Boundary and Dossier merged into §8.
- **Provenance tags are now English-canonical** (`[SOURCE] [GAP] [CONFLICT] [PROPOSAL]`), and the Spanish forms are accepted as aliases.
- New **§10 Work breakdown**: Epics governed, Stories free.

### Lifecycle (new Part III)
- Explicit lifecycle (§12): Setup → Elicitation → THINK → Freeze → Build/Observe → Change → Re-freeze, plus Design Reset.
- New sections:
  - **Setup** (§13): ADE participation, Primary ADE, language first, autonomy profile, development method, handoff.
  - **Elicitation and Design Assessment** (§14): multiple sources, minimum floor, questionnaire, readiness, THINK Depth, proactive start and resumption.
  - **THINK** (§15): defined for the first time (2.1 used the term without defining it).
- **Freeze** (§16) lists its refusal conditions.
- New **Build and Observation** (§17): four observation levels and the governance backlog.
- **Change flow** (§18) replaces 2.1 §13 and §19. It adds promotion sets, atomic promote and undo.
- New **Design reset** (§19).

### Capabilities
- New **Method Governance** (§20): Method Packs, Method Lock, Method Conformance as fitness functions using native tools.
- New **Autonomy profiles** (§21), formerly "Method Plans".
- GTTGuard consolidated from 2.1 §15–19 into §22.
- **Enforcement** (§23) adds per-ADE enforcement levels and shared decision logic.
- New **Next action** in §24.

### Integration
- **MCP** named as the preferred vendor-neutral adapter. **CI** recognized as an adapter (§27).
- New **Cooperative instruction files** (§28): GTT writes an owned block, never takes over `AGENTS.md`.
- New **Relationship to other methods** (§29): SDD, fitness functions, ADR/MADR, C4/arc42, agile.

### Governance of the canon
- New **Artifact roles** (§31) and **Annex A Reference Layout**. The canon names roles; the implementation publishes paths.
  - This withdraws the 2.1 paths `gtt/proposals/` and `gtt/protection/registry.yaml`, which the reference implementation never used (it uses `gtt-domain/proposals/` and `.gtt/protection/registry.yaml`).
- Terminology moved to `GLOSSARY.md`, split into user and internal terms, with a list of deprecated terms.
- Versioning moved to `VERSIONING.md`: two public numbers only.
- New `CONFORMANCE.md`, the per-capability implementation status required by §34.

### Repository
- Added `GTT-CANONICAL.md` (stable pointer), `GLOSSARY.md`, `CONFORMANCE.md`, `VERSIONING.md`, this changelog.
- `GTT-CANONICAL-v3-planned.md` is removed (absorbed by 3.0).
- `GTT-CANONICAL-v4-planned.md` is moved to `planned/GTT-CANONICAL-v4.md`.
- The duplicate `GTT Method — README.md` is removed.
- `GTT-CANONICAL-v2.1.md` is kept at its path: about 150 references in the suite point to it.

## 2.1 — Superseded by 3.0 (2026-10-28)

The first canonical generation published in this repository. See `GTT-CANONICAL-v2.1.md`.
