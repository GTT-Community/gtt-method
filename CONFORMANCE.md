# GTT Conformance — Canon 3.0 vs the GTT suite

This file records, for every capability defined in [`GTT-CANONICAL-v3.0.md`](GTT-CANONICAL-v3.0.md), whether the reference implementation (GTT suite: Bootstrap + CLI + website) implements it. It is required by Canon §34.

**What was assessed (2026-10-28):**
- Suite **1.7.0**, as released (Bootstrap `fadb616`, CLI `7242492`).
- The **1.8.0 candidate**: the working trees after the 2026-10-28 review fixes. Not released yet; the version number is the maintainer's decision.

**Status values:**
- `implemented`: shipped in 1.7.0, with a deterministic check or a test as evidence;
- `implemented (1.8)`: done in the 1.8.0 candidate, with a test as evidence; available once released;
- `partial`: shipped in part; the gap is named;
- `planned`: not shipped; target release given.

**Rule:** user-facing material may present a capability as available only when its row says `implemented`, or `implemented (1.8)` once 1.8 is released (Canon §34).

**Evidence abbreviations:**
- **B:** a `gtt-bootstrap` script under `.gtt/scripts/`;
- **BT:** a `gtt-bootstrap` acceptance scenario in `.gtt/tests/bootstrap-acceptance.py`;
- **C:** a `gtt-cli` command;
- **CT:** a `gtt-cli` test (`test/integration/` or a unit test).

## Part II — Model

| § | Capability | Status | Evidence | Gap / target |
|---|---|---|---|---|
| 6 | Two planes (governance / work) | implemented | `contract/profiles.json` → `two_planes`, invariant `autonomous-work`; BT `observation`, `backlog_model` | — |
| 7 | Governed Context as source of truth | implemented | `gtt-domain/context/*`; B `gtt-check-integrity.sh`; ADR-001 | — |
| 7.1 | Execution Context selection | partial | B `gtt-session-context.sh`, `gtt-query.sh` | No per-operation context projection. Target: MCP retrieval tools (1.9) |
| 7.2 | Retrieval index | implemented | B `gtt-index.sh`, `gtt-query.sh`, `gtt-reconcile.sh`; skill `gtt-retrieve`; BT `guard_and_retrieval` | — |
| 8 | Evidence boundary, versioned hashed sources | implemented | B `gtt-source.sh`; C `gtt sources`, `gtt design add`; BT `sources`, `design_sources` | — |
| 8 | Evidence Dossier as an artifact | partial | Defined in the agent contract | No dossier artifact and no check. Target: decide whether one is needed (1.9) |
| 8.1 | Provenance tags well-formed; `[PROPOSAL]` never in ratified context; conflict/blocking gap refuse freeze | implemented | B `gtt-check-provenance.sh`, `gtt_provenance.py`, `gtt-freeze.sh` | — |
| 8.1 | Canonical English tags with Spanish aliases | planned | Only `[FUENTE] [VACÍO] [CONFLICTO] [PROPUESTA]` | Target 1.9 (touches every template, skill and check) |
| 9 | ADRs | implemented | `gtt-domain/adr/`, `ADR-TEMPLATE.md`; skill `gtt-adr` | — |
| 9 | MADR-compatible ADR format | partial | Template has Status / Context / Decision / Consequences | Field names not aligned with MADR. Target 1.9 |
| 10 | Epics governed, Stories free, backlog reconciled | implemented | B `gtt-check-backlog.sh`; BT `backlog_model`; the guide no longer says Stories need a change request (1.8) | — |
| 11 | No silent defaults; "help me choose" stays OPEN | implemented | `contract/methods.json`; CLI exit 3 with `Next:`; BT `method_governance`; CT `devmethod_test.go` | — |

## Part III — Lifecycle

| § | Capability | Status | Evidence | Gap / target |
|---|---|---|---|---|
| 12 | Lifecycle step derivable from state | implemented | B `gtt-project.sh detect`; C `gtt status` | — |
| 13 | ADE participation + one Primary ADE | implemented | B `gtt-ade.sh`; C `gtt init --ade --primary`, `gtt agents`; BT `ade`, `any_ade_alone`; CT `agents_test.go` | Interactive: an empty selection is asked again instead of aborting (1.8) |
| 13 | Language asked first | implemented (1.8) | `internal/app/init.go`; CT `TestInteractiveInitAsksAndRecords` | — |
| 13 | Language applied to all user interaction | implemented (1.8) | CLI output, prompts and errors in Spanish when the project language is `es` (`internal/core/i18n*.go`, ~330 messages; JSON never translated); Method Pack texts; the agent interviews in the human's language (elicitation directive `plain-language`); CT `i18n_test.go`, `TestInteractiveInitAsksAndRecords` | Cobra help, audit/doctor detail and Bootstrap-produced text stay English; the context templates are English |
| 13 | Setup joins an existing `AGENTS.md`, never refuses or overwrites it | implemented (1.8) | `internal/fsx/shared.go` (block markers); installer, `update`, `clean`, `export --clean`, rollback; B `gtt-check-agents.sh` caps the GTT block; CT `TestExistingAgentsFileIsJoinedNotTakenOver`, `TestFailedInstallKeepsTheHostAgentsFile`, `fsx/shared_test.go` | — |
| 13 | Handoff with explicit next action | implemented | C `gtt init`; CT `init_test.go` | — |
| 14.1 | Several design documents; consolidate or keep with precedence | implemented | C `gtt init --sources`, `gtt design add`; skill `gtt-bootstrap`; BT `design_sources` | — |
| 14.2 | Design assessment + minimum floor | implemented | template `gtt-design-assessment.md`; `contract/elicitation.json`; BT `design_sources` | — |
| 14.2 | Minimum floor enforced at freeze in every profile | implemented (1.8) | B `gtt_provenance.py` (`FLOOR_ROWS`: Language, Runtime, Datastore (primary) - FAIL pre-freeze) | — |
| 14.3 | Questionnaire, one question at a time, observable state | implemented | B `gtt-project.sh elicitation`; BT `questionnaire`, `elicitation_start` | — |
| 14.4 | Readiness + THINK Depth | implemented | `contract/elicitation.json`; BT `think_depth` | — |
| 14.5 | Proactive start at session start, every ADE | partial | Claude Code: SessionStart hook (verified). Antigravity: N3 session adapter (always-on rule) + next action in every state (1.8, BT `design_next_action`). Others: hooks declared, unverified | Delivery outside Claude Code depends on the ADE honouring its hook or rule. Target 1.9: MCP prompt |
| 14.5 | Resume without re-asking | implemented | BT `elicitation_start`, `continuity` | — |
| 15 | THINK proposes context + Epic designs per method; two confirmations | implemented | skill `gtt-bootstrap`; B `gtt-design.sh`, `gtt-approve.sh`, `gtt-check-design.sh` | — |
| 16 | Human-only freeze with full refusal conditions; digest; no unfreeze | implemented | B `gtt-freeze.sh`; C `gtt freeze`; BT `fresh_host`, `evolution`, `method_governance` | — |
| 16 | A frozen project passes validation (CI) | implemented (1.8) | Freeze normalizes the canonical reference line, registers new artifacts, refuses on integrity/markdown failures and retires the removed questionnaire; BT `freeze_validates` | — |
| 17 | Observation with four levels + governance backlog | implemented | B `gtt_observe.py`; BT `observation` | — |
| 17 | Out-of-design changes observed from the design itself (no hand-written rule) | implemented (1.8) | B `gtt_observe.py`: `GTT-STACK` (a datastore, cache, framework or broker the stack does not declare: npm, PyPI, Go, Maven, Gradle, NuGet), `GTT-MODULES` (code outside the declared modules); BT `out_of_design` | The backbone catalog covers the common packages; an unlisted one is ordinary work until a rule declares it |
| 17 | Governed state changed outside the change flow stops work | implemented (1.8) | `GTT-GOVERNED` is BLOCKING (a promotion re-freezes by itself; the human's re-freeze is never refused for it); BT `out_of_design` | — |
| 17 | Audit | implemented | skill `gtt-audit`; C `gtt audit` | Judgment-based by design |
| 18 | Change flow: request → proposal → promotion set → promote → undo | implemented | B `gtt-change.sh`, `gtt-stage.sh`, `gtt-promote.sh`; C `gtt change`, `gtt proposals`, `gtt promote`, `gtt undo`; BT `change_flow`; CT `changeflow_test.go` | — |
| 18 | No hand-written application scripts | implemented (1.8) | Rule in `AGENTS.md`; the staged-adapter tool is now an engine script (`.gtt/scripts/gtt-adapter-promote.sh`), denied to agents | — |
| 19 | Design reset before the first freeze, with backup | implemented (1.8) | C `gtt design reset` (also in the re-init menu); backup under `.gtt/local/design-reset/`; refused after a freeze; CT `TestDesignResetStartsOverBeforeTheFreezeOnly` | — |
| 19 | Add design documents later | implemented (1.8) | C `gtt design add <path>...` (also in the re-init menu); the next action names the candidates; BT `design_next_action` | — |
| 19 | Reset after freeze through change flow | implemented | Change flow §18; `gtt design reset` points to it | — |

## Part IV — Capabilities

| § | Capability | Status | Evidence | Gap / target |
|---|---|---|---|---|
| 20.1 | Method Packs as single source | implemented | `.gtt/methods/*.md`; `contract/methods.json`; C `gtt dev-method list` | — |
| 20.1 | Pack verification level declared | implemented | pack frontmatter `verification` | All 6 packs are `partial`; none `verified` |
| 20.2 | Method Lock ratified at freeze; ADR for styles | implemented | B `gtt-method.sh check --pre-freeze`; BT `method_governance` | — |
| 20.2 | Method change after freeze = change request | implemented | skill `gtt-propose-change`; BT `method_governance` | — |
| 20.3 | Conformance via native tool adapters | implemented (1.8) | C `gtt check`: JS/TS (dependency-cruiser), Go (`go list`), Java/Kotlin (`jdeps`, on compiled classes), PHP (deptrac), Python (import-linter); CT `internal/conformance/adapters_test.go` | .NET has no CLI-runnable tool (declared-only). The new adapters are tested against recorded tool output, not yet against the real tools |
| 20.3 | A BLOCKING `forbid` rule refused before the write | implemented (1.8) | Shared decision core (`core_content`) in `gtt_protect.py` and `.claude/hooks/protect-l0.py`; tested with Claude Code and Cursor payloads (BT `out_of_design`); Antigravity, OpenHands, Kiro and Copilot handlers pass their payload too | Applies where the ADE runs the hook (runtime-verified: Claude Code) and the payload carries the written text; otherwise observation and CI catch it after the write |
| 20.3 | Violation = reported conflict | implemented | `gtt check` → `conflict` (exit 1) | — |
| 21 | Autonomy Profiles Light / Medium / Hard / Team | implemented (1.8) | `contract/profiles.json` (labels Light/Medium/Hard/Team); C `gtt autonomy` (alias `gtt method`), `gtt init --autonomy` (`--method` kept); BT `profiles` | Internal JSON keys keep `plan` / `methodology` |
| 21 | Less strict profile after freeze refused | implemented | `profiles.json` → `after_freeze`; direct writes to `.gtt/methodology.json` denied (1.8); BT `profiles` | — |
| 22 | GTTGuard declaration, normalized registry, HUMAN_APPROVAL | implemented | B `gtt_guard.py`, `gtt-guard-sync.sh`, `gtt-check-protection.sh`; BT `protection_hooks`, `registry_safety` | — |
| 23 | Shared decision logic across adapters | implemented | decision core byte-identical in `gtt_protect.py` and `protect-l0.py`; BT `enforcement` | — |
| 23 | Shell bypasses of governed state treated as writes | implemented (1.8) | interpreter one-liners (`python -c`, `node -e`, ...) that write a governed path; `git checkout/restore` of governed paths; setup records (`.gtt/methodology.json`, `development-method.json`, `selected-sources.json`); BT `out_of_design` | A shell parser is a heuristic: observation (`GTT-GOVERNED`, BLOCKING) and CI remain the backstop |
| 23 | Enforcement level declared per ADE | partial | `manifest.yaml` → `enforcement` | Only Claude Code verified; level names differ from the canon. Target 1.9 |
| 24 | Single validation entry point | implemented | B `gtt-validate.sh`; C `gtt validate [--ci]` | — |
| 24 | Status from state, observations included | implemented (1.8) | C `gtt status` → *Observations* and *STOP* lines; "Open gaps / Blocking gaps" no longer confused with observations; CT `TestStatusPresentsBootstrapState` | — |
| 24 | Next action for every state | implemented (1.8) | B `gtt_project.py` `next_action()` (design / questionnaire / change request); BT `design_next_action` | — |
| 25 | Derived session continuity | implemented | B `gtt-session-context.sh`, `gtt-checkpoint.sh`; C `gtt resume`; BT `continuity` | The duplicate banner of the Claude Code hook was removed (1.8) |
| 26 | Working preferences (local, bounded) + team agreements | implemented | `.gtt/local/preferences.md`, `gtt-domain/working-agreements.md`; BT `profiles` | — |

## Part V — Integration

| § | Capability | Status | Evidence | Gap / target |
|---|---|---|---|---|
| 27 | Adapters for several ADEs | implemented (1.8) | 7 ADEs: Claude Code, Kiro, Copilot, Cursor, OpenHands, Antigravity (overlays), Codex (`AGENTS.md` + CI); 7 session-adapter declarations (Antigravity N3 added); BT `any_ade_alone`, `design_next_action`; the website lists exactly these | Only Claude Code is runtime-verified |
| 27 | MCP adapter | planned | — | Target 1.9 (`gtt mcp`) |
| 27 | CI gate | implemented (1.8) | `.gtt/scaffold/ci/github-actions-gtt.yml`; reusable action `GTT-Community/gtt-action` (validation, freeze gate, observation; file annotations, job summary, outputs; local test 30/30) | The action is not published yet; first run on GitHub pending |
| 28 | Cooperative instruction files (owned block) | implemented (1.8) | see §13 | — |
| 28 | Short root files, detail under `.gtt/docs/` | implemented (1.8) | `readme-gtt*.md` ~50 lines each (full text `.gtt/docs/guide*.md`); `AGENTS.md` 24 KB -> 16 KB, ten grouped invariants, every rule kept in `.gtt/docs/agents/rules.md` | — |
| 29 | Positioning vs SDD, fitness functions, C4 | partial | Canon §29; website page *GTT 3.0* | Dedicated website page. Target 1.9 |
| 30 | CLI does not redefine GTT; non-interactive explicit errors | implemented | CT `init_test.go` | — |
| 30 | No internal terminology in user-facing output | partial | "Method Plan" → "Autonomy Profile" (1.8) | Prompts still use "L0", "promotion set", "declared-only". Target 1.9 |

## Part VI — Governance of the canon

| § | Capability | Status | Evidence | Gap / target |
|---|---|---|---|---|
| 31 | Role → path map published | implemented | Canon Annex A; Bootstrap `manifest.yaml`; website FAQ and *GTT 3.0* page use the real paths (1.8) | — |
| 32 | Single glossary used everywhere | partial | [`GLOSSARY.md`](GLOSSARY.md) | Site and Bootstrap glossaries are still written by hand. Target 1.9 (generated from this repository) |
| 33 | Only two public version numbers | implemented (1.8) | "Bootstrap 1.0" labels replaced by "contract version 1"; the website shows "GTT 3.0 · Suite x.y.z" (`src/lib/links.ts`) | — |
| 34 | Conformance matrix maintained | implemented | this file | Check it in Bootstrap CI. Target 1.9 |
| 35 | Superseded generation kept readable | implemented | `GTT-CANONICAL-v2.1.md` kept, marked SUPERSEDED | — |

## Functional review — end-to-end (2026-10-28)

A real project was installed with `gtt init` (Claude Code, Light, Vertical Slice + Walking Skeleton), given a governed design (Node/Fastify/PostgreSQL), frozen, and then subjected to the changes an agent could make outside the design. The same run is now the acceptance scenario `out_of_design`.

| # | Finding | Status |
|---|---|---|
| F-01 | A freshly frozen project failed `gtt validate` | **Fixed (1.8)** - BT `freeze_validates` |
| F-02 | `init` aborted when `AGENTS.md` existed | **Fixed (1.8)** - joined as a block |
| F-03 | Antigravity with no questionnaire got no next action | **Fixed (1.8)** - next action in every state; N3 adapter |
| F-04 | Out-of-design changes with no declared rule were invisible | **Fixed (1.8)** - `GTT-STACK`, `GTT-MODULES` |
| F-05 | A new datastore weighed the same as any library | **Fixed (1.8)** - backbone dependencies compared with the stack |
| F-06 | Governed context changed outside the flow was only GOVERNANCE | **Fixed (1.8)** - BLOCKING |
| F-07 | BLOCKING reported after the write | **Fixed (1.8)** - refused before the write, by content |
| F-08 | Shell bypasses (`python -c`, `git checkout`, setup records) | **Fixed (1.8)**; `--no-verify` is denied only where the Git hook is installed, by design |
| F-09 | `status` hid observations | **Fixed (1.8)** |
| F-10 | A new project showed 3 proposals it never made: the staged session adapters of Codex, Copilot and Kiro | **Fixed (1.8)** - moved into the engine (`.gtt/session-adapters/staged/`); `gtt update` removes the untouched copies from upgraded projects |
| F-11 | Light froze with an empty stack | **Fixed (1.8)** |
| F-12 | Placeholder error without a line; `<slice>` in a path, and the format line of `gtt-boundaries`, taken as placeholders | **Fixed (1.8)** |
| F-13 | `--language es` left everything in English | **Fixed (1.8)** - language asked first; CLI output in Spanish (help texts and templates remain English) |
| F-14 | ~2,500 lines of documentation at the project root | **Fixed (1.8)** - ~100 lines; full guide under `.gtt/docs/` |
| F-15 | Hand-written context failed validation for a missing header | **Fixed (1.8)** - added at freeze |
| F-16 | Session-start disclaimer repeated four times | **Fixed (1.8)** - twice (the service header and the session file) |

## Known contradictions (from the 2026-10-28 review)

| Where | Contradiction | Status |
|---|---|---|
| Website, Bootstrap | "GTT" expanded three ways | **Resolved:** *Governance Through Thinking* everywhere |
| All repos | MIT / Apache-2.0 / CC BY 4.0 | **Resolved:** Apache-2.0 for the whole suite |
| All repos | References to canon v2.1 | **Resolved:** 3.0 / the stable pointer; the markdown gate accepts any generation |
| Bootstrap | "Works with 4 ADEs", `.github/copilot-instructions.md`, a `.gitignore` to merge that does not exist | **Resolved (1.8)** |
| gtt-cli CI | A comment referenced a private file | **Resolved (1.8)** |
| gtt-cli (dev repo) | Governed with Bootstrap 1.2.0 while pinning 1.7.0 | Open - its own governed files change through its own flow (`gtt update` in that repo) |
| Website | Governs itself with CDAD (`cdad/`), not GTT | Open - migrate the site's own governance to GTT |
