# qstudying

> Lean4 focus areas for the *axiomatic-kernel + LLM-generated facts +
> lake-build-as-verification* architecture — now three kernels: the
> legal-domain and financial-domain axiom sets behind Qnarre and Qresev,
> plus a third, operational-domain kernel that turns the method inward,
> axiomatizing the agent constellation's own engineering mechanics
> (starting with git) into proofs a local consumer only visualizes.

This third axis is the method turned back on Quantapix itself — the only
self-referential one, and **partially exercised, not closed**: the method
*describes and distills* these engineering mechanics into proofs, but no
proof yet *governs* an operation, and the local consumer only visualizes
them. It is a deeper exposition of the **method**, the third axis of one
method — not a product, a service, or a feature.

A weekly-refreshed window into the working syllabus that runs alongside the
private working repository. Rewritten 2026-06-10 under a three-axis charter:
each area is tagged to the workstream it serves — **[A]** cross-axis
representation research (how to *represent* the kernel shape, the
facts-generation contract, the debate protocol, and the verification gate so
LLM proof-drivers consume a written guide instead of folklore) or **[B]**
operational axiomatization (git first). The former open-source-contribution
framing is retired; areas survive because they serve A or B. Pure-mathlib
paths (Topology, MeasureTheory, Analysis, CategoryTheory) remain deliberately
out of scope.

Toolchain pin (all three kernels, three-way lockstep):
`leanprover/lean4:v4.32.0` (bumped from v4.31.0 on 2026-07-14; the prior
v4.30.0 → v4.31.0 bump was 2026-06-29). Any bump moves all three kernels
in lockstep and replays each kernel's example proofs. "One commit" is not
always achievable — when the three kernels are each held by a separate
concurrent session, the bump lands as one commit per kernel, in lockstep
in time, each verified independently; the deviation is recorded rather
than papered over.

One non-obvious hazard, stated here because it voided real work: the pin
is read from the *current directory*. A bare compiler invocation from a
directory that has no pin file silently falls back to the default
toolchain — which then rejects the kernel's compiled artifacts as
incompatible, invites a "workaround" that recompiles shared modules from
source, and produces a green build against a compiler the kernel does not
use. That green means nothing. Always invoke through the package's own
build environment, or pin the invocation explicitly. A file in the
package root is not a pin for any process whose working directory is
elsewhere.

- Parent organisation: <https://github.com/quantapix>
- Engineering output: <https://quantapix.com>
- Motivational record: <https://femfas.net>

## Source grounding

Standard texts and templates ground on five read-only upstream clones,
refreshed manually and pinned by SHA in a committed manifest:

| clone | upstream | role |
|---|---|---|
| theorem_proving_in_lean4 | <https://github.com/leanprover/theorem_proving_in_lean4> | [A] standard proof idioms |
| reference-manual | <https://github.com/leanprover/reference-manual> | [A] normative language semantics |
| cslib | <https://github.com/leanprover/cslib> | [B] structural template (CS-formalization layout) |
| git | <https://github.com/git/git> | [B] ground truth for the git axiomatization |
| vscode-lean4 | <https://github.com/leanprover/vscode-lean4> | template source ONLY for code-generation patterns — never installed as an interactive proof surface (no manual proof driving, ever) |

Lean compiler/library paths below cite the upstream
[lean4](https://github.com/leanprover/lean4) repository (`src/<path>`).

---

## 1. [A] Kernel semantics: `axiom` / `opaque` / `noncomputable` / `Prop`

The trust boundary the entire architecture rests on. Read upstream
`src/kernel/type_checker.{h,cpp}`, `declaration.cpp`, `inductive.cpp` —
`is_def_eq_core`, `whnf_core`, `quick_is_def_eq`. Pair with the Lean
Reference §4 (Type System), §7 (Definitions), §8 (Axioms) and *Theorem
Proving in Lean* Ch.12. This explains *why* `act_i ≠ act_j` needs an axiom,
why opaque predicates must be `Prop`-valued (not `Bool`), and why
`noncomputable` is correct — not a smell — for axiom-list witnesses. The
representation guide's kernel-shape chapter starts here.

Re-validate against PR #12973 — "makes theorems opaque in almost all
ways, including in the kernel." With all three kernels now pinned to v4.32.0,
a green `lake build` of each axiom set is the load-bearing post-bump
confirmation that `Prop`-valued opaque predicates and `noncomputable`
witnesses still reduce in def-eq the way the proofs assume — re-confirmed on
the v4.31.0 → v4.32.0 bump.

**Difficulty:** Deep. Non-negotiable. **Order:** start here.

## 2. [A] List membership and finite-domain proof patterns

Upstream `src/Init/Data/List/Basic.lean`, `BasicAux.lean`, `Lemmas.lean`.
The #1 daily pain: the legal-domain example proofs carry hand-built
`List.Mem` witness towers many `.tail`s deep plus an unrolled
`cases … with | head | tail` ladder. Because the action type is opaque,
`decide` can't close membership, so every added predicate re-indents the
ladder and renumbers every tower by hand.

Master the `List.Mem.head / .tail` shape, then write *one* helper per
kernel — `mem_sampleActs_cases : a ∈ sampleActs ↔ a = act₁ ∨ … ∨ a = act₇`
(plus an index-keyed `mem_of_index`) — and have the LLM consume the helper
instead of regenerating chains. These helper shapes are exactly the
code-generation templates Workstream A ships to the proof-driving lanes —
the template set now exists, and the operational kernel's first proof
discharges its per-snapshot universals over a closed-enumeration helper of
precisely this shape.

**Difficulty:** Entry → Mid. **Status:** template shipped; consumption open.
The helper shapes exist as a packaged template set the proof-driving lanes
read at prompt-assembly time. What has not landed is the second half:
per-kernel instances of the helper, and the model consuming them instead of
regenerating chains. The deliverable is measurable rather than declarable —
the witness-tower depth in the legal-domain example proof coming down.

## 3. [A] Metaprogramming: replace the string codegen with a Lean elaborator

*Metaprogramming in Lean 4* Ch.3 (Expressions) → Ch.4 (MetaM) → Ch.7
(Elaboration) → Ch.8 (Embedding DSLs By Elaboration) → Ch.9 (Tactics). The
book is pinned to v4.17.0-rc1 (~12 minors behind the kernels) — trust it for
*shape*, verify every Meta/Elab signature against current source or Zulip.

The strategic move: a `facts%` macro / custom command that consumes a
structured input (JSON or Lean-native syntax) and emits the axiom block
directly. The current codegen is pure string assembly — it concatenates the
negated predicate calls and resolves namespaces by emitting `open`
directives, so a wrong `open` order or a stale call string fails only at
`lake build`, never at codegen. Moving this inside the elaborator kills the
two most fragile parts of the architecture. Likely the single
highest-leverage area on this list.

**Difficulty:** Deep. **Status:** not started *as scoped here* — and the way
the previous status line was wrong is worth keeping. It read "not started" on
the evidence that no metaprogramming token appeared anywhere in the tree.
That evidence is now false: a domain proof tactic shipped (#8), so the
metaprogramming substrate is present. The area is still untouched, because a
tactic is not a term elaborator and does not touch the code-generation seam
at all — but the *detector* had gone stale the moment something unrelated
landed nearby, and it would have reported "started" for the wrong reason just
as readily. A status line assembled from a search over the tree answers
"is this token present", never "is this work done".
**Deliverable:** a `facts%` prototype for one framework (Title VI is small)
before generalizing.

## 4. [A] Decidability and Classical reasoning on opaque domains

Upstream `src/Init/Core.lean` (where `Decidable`/`DecidableEq` now live —
`Init/Decidable.lean` was removed), `Init/Classical.lean`,
`Init/PropLemmas.lean` + TPL Ch.10 (Type Classes) and Ch.12 (Classical
reasoning). Codify *exactly* when `decide` fires (closed enums with
`deriving DecidableEq`) versus when it cannot (any opaque element — the
structural gap behind half the pinned painful lessons). The right output is
a one-page rule for the LLM prompt templates and for human reviewers — a
representation-guide chapter. Track the FRO `grind` / counterexample-gen MVP
as the emerging discharge path for the opaque-membership cases `decide`
can't reach.

**Difficulty:** Mid. **Order:** alongside #2 (shared opaque-domain root cause).

## 5. [A] Domain modeling: `structure ... : Prop` vs `inductive ... : Prop`

TPL Ch.9 (Structures) + Ch.7 (Inductive Types). The 7-field-conjunction and
4-case-disjunction shapes the legal-domain kernel already uses —
conjunctions as `structure … : Prop where`, disjunctions as
`inductive … : Prop`, proofs assembled via `refine { field := ?_ … }`.
Deepen this to a habit; it is also the modeling vocabulary the operational
git kernel (#10) starts from. Add `deriving DecidableEq, Repr` reflexively
on every closed enum (pervasive across the statutory-corpus types, absent on
the legacy opaque types) so failure messages stay legible to the LLM debug
loop.

**Difficulty:** Mid. **Deliverable:** the per-element intro-lemma
duplication in the financial-domain Sector and OptionsRisk theorem modules
(hand-written identity packagers, one per `Prop` field) is the live cleanup
target — generate or `@[simp]`-derive them instead of hand-maintaining.

## 6. [A/B] Coinductive predicates

`coinductive` shipped in Lean 4.25.0 (2025-11-14) via a
PartialFixpoint/lattice encoding — no kernel extension. Read the FRO Y3
roadmap entry plus the Zulip `#lean4 > Core support for coinductive
predicates` thread (active through 2026-02). The cleanest *new* fit on the
list: continuing-enterprise RICO patterns (an ongoing scheme, not a one-shot
act) and rolling-window TREND / MOMENTUM are naturally coinductive —
currently forced into inductive/finite shapes. The operational axis adds a
third candidate: long-lived session/lock lifecycles. Prototype one
coinductive predicate per domain and compare against the current finite
encoding.

**Difficulty:** Mid–Deep. **Deliverable:** one coinductive predicate in the
legal-domain kernel (ongoing-enterprise) and one in the financial-domain
kernel (rolling-window) as feasibility spikes.

## 7. [A] Lake + golden-output regression + per-axiom audit

Upstream `src/lake/Lake/` (Build, CLI, Config, DSL) + `tests/lean/run/` and
`tests/elab_fail/` patterns (`.lean` + `.expected.out`). Codify a regression
suite so an LLM-generated facts file that "happens to build" doesn't
silently break a cross-framework invariant — the debate lanes' judge needs
it, and the operational kernel joins the same suite shape. Also
`script/lean-bisect` for finding when an axiomatization regressed across
releases.

Adopt the #12216 / PR #12217 landing: `#print axioms` now emits
per-computation axioms instead of collapsing to one shared axiom, and
#13117 re-enabled it under the module system. That turns `#print axioms`
into a per-fact audit signal — pin it as a golden check so an injected or
mis-generated fact shows up as an unexpected axiom, not just a build pass.
The operational kernel's first proof round already carries an adversarial
`#print axioms` allow-list probe alongside the constructive proof, and the
audit has since widened from per-example to whole-tree: the axiom closure of
every declaration in the kernel is attested on every judged round, because a
soundness check scoped to a hand-maintained roster audits exactly what someone
remembered to add.

**Difficulty:** Mid. **Status:** shipped — in a different shape than this
entry named, which is the other half of the lesson in #3. The old status line
read "not started" on the evidence that no directory of the named shape
existed in any kernel. Literally true and substantively wrong: golden-output
regression shipped as a per-example byte-reproduction check wired into the
judge, the per-axiom audit shipped whole-tree, and the aggregating regression
battery shipped as a separate suite. "Was the named directory built" was the
wrong question. **Still open** — the upstream per-computation axiom landing
this entry flags is not yet exploited as a per-fact signal.
**Deliverable:** an allow-list snapshot keyed on the per-computation axiom
names, so an injected fact surfaces as an unexpected axiom rather than a
build pass.

## 8. [A] Tactics framework + Aesop / `grind` rule sets

Upstream `src/Lean/Elab/Tactic/`, `src/Lean/Meta/Tactic/`, plus the
[Aesop](https://github.com/leanprover-community/aesop) README (`@[aesop]`
safe/unsafe + custom rule builders). The payoff: replace explicit `List.Mem`
chains with one `aesop` call once the axiom set is taught as rules; later, a
domain-specific `prove_predicate` tactic for predicate enumeration.

**Landed (a first domain tactic, not aesop).** A Mathlib-free domain proof
tactic now ships — core `grind` + `constructor` + `assumption` composed via
`macro_rules`, with a `@[grind →]` doctrine-tagging convention that teaches
the axiom set to the prover as tagged intro rules. It first discharges a
hierarchical predicate decomposition, where the simple route closes by
`constructor` and the structured route genuinely needs the tagged `grind`
intro. The **aesop** arm stays deferred: aesop is not in core and is
charter-gated the same way Mathlib is — the operational axis rejects both, so
the explicit-`List.Mem`-chain → one-`aesop`-call payoff remains future work
behind membership helpers #2 + codegen #3. Aesop has
no documented opaque-axiom rule-builder, so a custom builder over domain
axioms is a representation-guide deliverable in its own right.

**Difficulty:** Mid. **Deliverable:** once #2 lands, wire `aesop` into one
legal-domain theorem and template the rule-tagging shape for all three
kernels.

## 9. [A] Elaborator API, diagnostics, and editor-tooling templates

Upstream `src/Lean/Elab/Term.lean`, `BuiltinCommand.lean`,
`Declaration.lean`, `DeclModifiers.lean`, paired with `src/Lean/Linter/`.
The keyword collision (`protected` / `private` / `partial` as field names)
lives here, and so does every error message the LLM debug loop has to
interpret — codify the recurring diagnostic shapes into the representation
guide so the proof-driving lanes recover instead of flailing. The
vscode-lean4 clone is the **template source** for code-generation patterns
(elaboration hooks, diagnostics shapes, file-watching conventions) — mined
for generation patterns, never installed as an interactive proof surface.

**Difficulty:** Mid–Deep. **Deliverable:** a diagnostics-decoder section in
the representation guide + extracted vscode-lean4 patterns for the codegen
templates.

## 10. [B] Operational axiomatization: git via the cslib template

The third axis's first target — and the first one to ship. Ground truth is
the git repository's own `Documentation/` tree (gitglossary,
gitrepository-layout, the object-model and ref docs — before any C source);
the structural exemplar is [cslib](https://github.com/leanprover/cslib)
(module layout, naming, and doc conventions for formalizing an operational
system rather than a statute or a market). Surface at v1: the object model
(blob / tree / commit / tag; content-addressing as an explicit, declared
collision-freedom assumption — never an implicit one), the commit DAG,
refs/branches, worktrees + the index. No packfile internals, no transport
protocol.

The angle is the point: the first theorems target the agent constellation's
*own* operational invariants — branch-presence-as-write-lock exclusion,
close-cascade safety, canonical-vs-worktree divergence — with git's
semantics as the axiom layer beneath them. These proofs *describe* the
constellation's mechanics; they do not yet *govern* them — the loop is
partially exercised (discovery and distillation ship and are audited), not
a closed self-governing one, and the consumer is local-only and only
visualizes. Proofs run through a
coding-vs-testing adversarial debate lane (paired LLM agents: one side
constructs the proof over synthetic repo-snapshot facts, the other commits
attack probes — embedded negations, axiom-audit checks — against the same
build gate), never manually.

**Status: shipped.** The operational lake package builds green on the
lockstep pin, and the git-axis invariant theorems are proved end-to-end over
**synthetic and structurally-synthetic repository snapshots — never
real-tree data** — each driven through the coding-vs-testing adversarial
debate lane, with the testing side's negation and axiom-audit probes
committed alongside the constructive proof:

- **T1, lock exclusion** — over a synthetic snapshot, any two
  write-locks held by distinct sessions have non-conflicting scopes;
  discharged over a closed lock-table enumeration plus
  scope-disjointness facts. A companion theorem proves the full five-field
  lock model (every held lock is a live, scope-named, session-opened
  branch; locks are exclusive per branch; one branch per session per scope).
- **T2, cascade safety** — the session-close merge-to-parent walk preserves
  committed-object reachability, over a stacked-branch fixture; reachability
  is derived by the kernel, never asserted.
- **T3, integrity breach** — the snapshot audit detects a session whose
  window commit leaks to a path outside its own working-tree root: the
  canonical bypass an editor-level guard structurally cannot see.

Additional operational-invariant theorems extend the same surface — a
substrate layer beneath git (files, processes, exit codes), a relational
ledger's append/replay/adoption semantics, and the governance gate described
in [STATUS](./STATUS.md). The hierarchical predicate decomposition already
used on the textual and numerical axes now extends to the operational kernel
— the same hierarchy/leaf-extraction method, applied inward. The syllabus
enumerates the architectural surface only; the axis's memory- and
session-substrate theorems are tracked privately and are deliberately absent
from the public syllabus. A fourth family — the context-operations cells,
which model what an automated agent is given to work from — closed in an
earlier cycle, one of them on a negative result; see [STATUS](./STATUS.md).

**Difficulty:** Mid–Deep.

---

## Reading order (collapsed)

1. Reference §4/§7/§8 + TPL Ch.12 ⇒ **#1** (re-validate #12973 opaque-in-kernel)
2. `Init/Data/List/` + `Init/Core.lean`/`Classical.lean` + TPL Ch.10 ⇒ **#2, #4**
3. TPL Ch.9 + Ch.7 ⇒ **#5**
4. Metaprogramming Ch.3→4→7→8→9 (verify APIs vs source) ⇒ **#3**
5. FRO coinductive entry + Zulip thread ⇒ **#6**
6. Reference §24 (Build Tools) + tests/ patterns + `#print axioms` ⇒ **#7**
7. Aesop README + one wired-in proof (after #2/#3) ⇒ **#8**
8. `src/Lean/Elab/` deep dive when an LLM error message is unactionable; mine vscode-lean4 ⇒ **#9**
9. cslib layout pass + git `Documentation/` ⇒ **#10**

## Active threads to track (lurk first)

- **`grind` / counterexample-gen MVP** — FRO Y3 proof-automation stream.
  Emerging discharge path for opaque-domain goals `decide` can't reach;
  track alongside #4/#8.
- **Std v1.0 stabilization** — FRO Y3 headline; RC not yet shipped
  (async/await + HTTP). Affects what the kernels can lean on.
- **LeanCopilot** —
  [github.com/lean-dojo/LeanCopilot](https://github.com/lean-dojo/LeanCopilot)
  (active, v4.27.0, 2026-02-11); candidate aid for the LLM-parallel debate
  lanes on opaque domains.
- **LeanDojo-v2** —
  [github.com/lean-dojo/LeanDojo-v2](https://github.com/lean-dojo/LeanDojo-v2)
  (NeurIPS 2025; the original LeanDojo is now deprecated).
  Trace→dataset→train framework; relevant to any LLM+Lean proving loop.
- **lean4-mlir** —
  [github.com/brettkoonce/lean4-mlir](https://github.com/brettkoonce/lean4-mlir);
  a different-domain mirror of the string-codegen→elaborator ambition (#3).
- **cslib growth** — young and moving; each refresh may add structures worth
  mirroring in the operational kernel (#10).
- **Operational-axis coverage denominator** — the operational kernel is the
  first of the three to carry a live, deterministic coverage denominator:
  the set of operational domain concepts it formalizes is extracted from the
  git glossary over a cited neighborhood, so coverage is a real fraction over
  a real ground-truth set rather than a hand-maintained estimate. Tracked as
  it grows.

## Skip / deprioritise

- Mathematics in Lean Ch.8–13 (analysis-heavy, off-topic).
- FPiL Ch.1–3 and TPL Ch.2–4 (entry material; FPiL Ch.4 Monads is the only
  early bit worth a glance for MetaM plumbing).
- Mathlib `Topology/`, `MeasureTheory/`, `Analysis/`, `CategoryTheory/`.
- `stage0/` (read-only snapshot; teaches nothing `src/` doesn't).
- git packfile internals + transport protocol (out of scope at v1 — #10).

---

## Cadence

Refreshed weekly from the private working tree. Re-ranking, new threads, and
dropped items are committed as ordinary diffs — the commit log is the change
record.

Authored by a sole developer working with an AI assistant (Claude Code) under written CLAUDE.md contracts — methodology in [qagents-public](https://github.com/quantapix/qagents-public).

## Contact

[github.com/quantapix](https://github.com/quantapix) — open an issue on any repo
in the org. Answered in public; there is no contact email.

## License

MIT (`LICENSE`). Content-class repo — focus-area prose plus the syllabus.
Short embedded snippets ride the same MIT grant.
