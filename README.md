# GTT Method

**GTT — Governance Through Thinking.** A governed methodology for AI-assisted software development.

> **Agent reasoning ≠ Change authorization.**

AI agents write code faster than humans can review it. Without a governed reference, the code becomes the specification and architectural intent erodes one reasonable-looking change at a time. GTT gives the project a single source of truth that the human ratifies. Agents reason over it, propose changes to it and build freely inside it. Only the human changes it.

```text
Human decides → GTT governs → Agent/ADE executes within the boundary
Agent proposes → Human decides → GTT applies
```

GTT is a methodology, not an agent framework. It works with any agent or ADE (Claude Code, Kiro, Copilot, Cursor, Antigravity, Codex, OpenHands…) without depending on one.

## How it works

1. **Setup:** choose the participating ADEs, how much GTT may do without asking (autonomy profile), and how the solution is organized (development method).
2. **Elicitation:** declare your design documents, or answer a guided questionnaire.
3. **THINK:** an agent proposes the governed context and one design per Epic, with every statement traced to evidence or marked as a gap, conflict or proposal.
4. **Freeze:** you ratify it.
5. **Build:** the agent works freely. GTT observes drift and blocks only what you declared blocking.
6. **Change:** you write a change request; the agent proposes; you decide; GTT applies it and you freeze again.

## This repository

| File | What it is |
|---|---|
| [`GTT-CANONICAL.md`](GTT-CANONICAL.md) | **Start here.** Names the canonical generation in force |
| [`GTT-CANONICAL-v3.0.md`](GTT-CANONICAL-v3.0.md) | **GTT 3.0: the current canonical definition** |
| [`GTT-CANONICAL-v2.1.md`](GTT-CANONICAL-v2.1.md) | GTT 2.1: superseded, kept for existing references |
| [`GLOSSARY.md`](GLOSSARY.md) | Canonical terms: user terms, internal terms, deprecated terms |
| [`CONFORMANCE.md`](CONFORMANCE.md) | What the reference implementation (the GTT suite) implements, per capability |
| [`VERSIONING.md`](VERSIONING.md) | Methodology generation vs suite version |
| [`CHANGELOG.md`](CHANGELOG.md) | Changes between generations |
| [`planned/`](planned/) | Future generations (not released) |

The canon defines **what GTT is**. The reference implementation demonstrates it:

- [GTT Bootstrap](https://github.com/GTT-Community/gtt-bootstrap): the engine and agent contract installed into a project;
- [GTT CLI](https://github.com/GTT-Community/gtt-cli): install, setup, validation, freeze and the change flow;
- [gtt-method.org](https://gtt-method.org): documentation and quickstart.

When an implementation, the website or a manual contradicts the canon, the contradiction is surfaced and resolved, not ignored. Whether a canonical capability is available in a given release is stated only in `CONFORMANCE.md`.

## License

This repository is licensed under the Apache License 2.0. See [`LICENSE`](LICENSE).
