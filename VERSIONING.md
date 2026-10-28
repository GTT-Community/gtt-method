# GTT Versioning

GTT has **two public version numbers**, and only two (Canon §33).

| Number | What it versions | Example | Changes when |
|---|---|---|---|
| **Generation** | The methodology: this repository's canon | `GTT 3.0` | The canon is revised and ratified. Major = principles or model change; minor = capabilities added or clarified |
| **Suite version** | The reference implementation: Bootstrap, CLI and website, released together under one number | `1.7.0` | Any release of the implementation (SemVer) |

## Compatibility statement

Each suite release states which generation it implements, and how far, in [`CONFORMANCE.md`](CONFORMANCE.md):

| Suite | Implements | Notes |
|---|---|---|
| 1.0 – 1.6 | GTT 2.1 | — |
| 1.7.0 | GTT 2.1, plus capabilities later canonized in 3.0 (Method Governance, change flow) | Assessed against 3.0 in `CONFORMANCE.md` |
| 1.8 (candidate, unreleased) | GTT 3.0 — functional review closed | Out-of-design observation from the design itself, pre-write blocking, cooperative `AGENTS.md`, design reset/add, Autonomy Profile, next action in every state (see `CONFORMANCE.md`) |
| 1.9 (planned) | GTT 3.0 — integration | MCP adapter, official CI action, more conformance adapters, CLI i18n, English provenance tags |

The website shows both numbers together, for example **"Suite 1.8 · GTT 3.0"**.

## Internal numbers (never user-facing)

These exist in the implementation and must not appear in READMEs, CLI prompts, errors or the website:

| Internal number | Where | Meaning |
|---|---|---|
| `contract_version` | `.gtt/contract/*.json` | Version of the Bootstrap↔CLI machine contract. Formerly shown as "Bootstrap 1.0" |
| `scaffold.version` | `.gtt/scaffold/manifest.yaml` | Version of the installed layout |
| `schema_version` / per-overlay `version` | contracts, manifest | Data-format versions |

## Canonical references from implementations

Implementations reference the canon by URL. To avoid breaking references at each generation:

- New references should point to the stable pointer, [`GTT-CANONICAL.md`](GTT-CANONICAL.md).
- Generation-specific files (`GTT-CANONICAL-v2.1.md`, `GTT-CANONICAL-v3.0.md`) are never moved or deleted. A superseded one is marked **SUPERSEDED** at the top (Canon §35).
