# qstudying

> Lean4 expert-track focus areas + open-source contribution targets,
> selected for the *axiomatic-kernel + LLM-generated facts + lake-build-as-verification*
> architecture that backs Qnarre and Qresev.

A weekly-refreshed window into the learning roadmap that runs alongside the
private working repository. Pure-mathlib paths (Topology, MeasureTheory,
Analysis, CategoryTheory) are deliberately out of scope. Toolchain pin (both
kernels, unified 2026-06-05): `leanprover/lean4:v4.30.0`. The Lean Language
Reference tracks v4.31-rc1, so the docs lead the toolchain with no functional
skew against the pin.

- Parent organisation: <https://github.com/quantapix>
- Engineering output: <https://quantapix.com>
- Motivational record: <https://femfas.net>

## Source-language note

Every `lib/lean4/<path>` reference below is to the upstream
[lean4](https://github.com/leanprover/lean4) repository, no longer vendored
here. Clone it separately to follow along:

    git clone https://github.com/leanprover/lean4 ~/clone/lean4

…and re-interpret the paths as `~/clone/lean4/<path>`.

The list is re-ranked by leverage: acute daily-loop pain and not-yet-started
deliverables lead; areas whose premise the code already resolved were retired.

---

## 1. Kernel semantics: `axiom` / `opaque` / `noncomputable` / `Prop`

The trust boundary the entire architecture rests on. Read
`lib/lean4/src/kernel/type_checker.{h,cpp}`, `declaration.cpp`,
`inductive.cpp` — `is_def_eq_core`, `whnf_core`, `quick_is_def_eq`. Pair with
the Lean Reference §4 (Type System), §7 (Definitions), §8 (Axioms) and
*Theorem Proving in Lean* Ch.12. This explains *why* `act_i ≠ act_j` needs an
axiom, why opaque predicates must be `Prop`-valued (not `Bool`), and why
`noncomputable` is correct — not a smell — for axiom-list witnesses.

Re-validate against v4.30.0 PR #12973 — "makes theorems opaque in almost all
ways, including in the kernel." This strengthened proof/opaque isolation at
the kernel level; with both kernels now pinned to v4.30.0, a green `lake build`
of the legal-domain and financial-domain axiom sets is the load-bearing
post-bump confirmation that `Prop`-valued opaque predicates and
`noncomputable` witnesses still reduce in def-eq the way the proofs assume.

**Difficulty:** Deep. Non-negotiable. **Order:** start here.

## 2. List membership and finite-domain proof patterns

`lib/lean4/src/Init/Data/List/Basic.lean`, `BasicAux.lean`, `Lemmas.lean`.
This is the #1 daily pain: the legal-domain example proofs carry hand-built
`List.Mem` witness towers many `.tail`s deep plus an unrolled
`cases … with | head | tail` ladder. Because the action type is opaque,
`decide` can't close membership, so every added predicate re-indents the
ladder and renumbers every tower by hand.

Master the `List.Mem.head / .tail` shape, then write *one* helper per kernel —
`mem_sampleActs_cases : a ∈ sampleActs ↔ a = act₁ ∨ … ∨ a = act₇` (plus an
index-keyed `mem_of_index`) — and have the LLM consume the helper instead of
regenerating chains. `Data/List` is also the most-merged Mathlib slice, so
this doubles as a contribution surface.

**Difficulty:** Entry → Mid. **Status:** not started. **Deliverable:**
`mem_*_cases` + `mem_of_index` helpers, plus the analogous helpers for the
financial-domain framework lists.

## 3. Metaprogramming: replace the string codegen with a Lean elaborator

*Metaprogramming in Lean 4* Ch.3 (Expressions) → Ch.4 (MetaM) → Ch.7
(Elaboration) → Ch.8 (Embedding DSLs By Elaboration) → Ch.9 (Tactics). The
book is pinned to v4.17.0-rc1 (~12 minors behind the kernels) — trust it for
*shape*, verify every Meta/Elab signature against current source or Zulip.

The strategic move: a `facts%` macro / custom command that consumes a
structured input (JSON or Lean-native syntax) and emits the axiom block
directly. The current codegen is pure string assembly — it concatenates the
negated predicate calls and resolves namespaces by emitting `open` directives,
so a wrong `open` order or a stale call string fails only at `lake build`,
never at codegen. Moving this inside the elaborator kills the two most fragile
parts of the architecture. Likely the single highest-leverage area on this
list.

**Difficulty:** Deep. **Status:** not started. **Deliverable:** a `facts%`
prototype for one framework (Title VI is small) before generalizing.

## 4. Decidability and Classical reasoning on opaque domains

`lib/lean4/src/Init/Core.lean` (where `Decidable`/`DecidableEq` now live —
`Init/Decidable.lean` was removed), `Init/Classical.lean`, `Init/PropLemmas.lean`
+ TPL Ch.10 (Type Classes) and Ch.12 (Classical reasoning). Codify *exactly*
when `decide` fires (closed enums with `deriving DecidableEq`) versus when it
cannot (any opaque element — the structural gap behind half the pinned painful
lessons). The right output is a one-page rule for the prompt template and for
human reviewers. Track the FRO `grind` / counterexample-gen MVP as the emerging
discharge path for the opaque-membership cases `decide` can't reach.

**Difficulty:** Mid. **Order:** alongside #2 (shared opaque-domain root cause).

## 5. Domain modeling: `structure ... : Prop` vs `inductive ... : Prop`

TPL Ch.9 (Structures) + Ch.7 (Inductive Types). The 7-field-conjunction and
4-case-disjunction shapes the legal-domain kernel already uses — conjunctions
as `structure … : Prop where`, disjunctions as `inductive … : Prop`, proofs
assembled via `refine { field := ?_ … }`. Deepen this to a habit. Add
`deriving DecidableEq, Repr` reflexively on every closed enum (now pervasive
across the statutory-corpus types, absent on the legacy opaque types) so
failure messages stay legible to the LLM debug loop.

**Difficulty:** Mid. **Deliverable:** the per-element intro-lemma duplication
in the financial-domain Sector and OptionsRisk theorems (hand-written identity
packagers, one per `Prop` field) is the live cleanup target — generate or
`@[simp]`-derive them instead of hand-maintaining.

## 6. Coinductive predicates

`coinductive` shipped in Lean 4.25.0 (2025-11-14) via a PartialFixpoint/lattice
encoding — no kernel extension. Read the FRO Y3 roadmap entry plus the Zulip
`#lean4 > Core support for coinductive predicates` thread (active through
2026-02). This is the cleanest *new* fit on the list: continuing-enterprise
RICO patterns (an ongoing scheme, not a one-shot act) and rolling-window
TREND / MOMENTUM are naturally coinductive — they are currently forced into
inductive/finite shapes. Prototype one coinductive predicate per domain and
compare against the current finite encoding.

**Difficulty:** Mid–Deep. **Deliverable:** one coinductive predicate in the
legal-domain kernel (ongoing-enterprise) and one in the financial-domain
kernel (rolling-window) as feasibility spikes.

## 7. Lake + golden-output regression + per-axiom audit

`lib/lean4/src/lake/Lake/` (Build, CLI, Config, DSL) + `tests/lean/run/` and
`tests/elab_fail/` patterns (`.lean` + `.expected.out`). Codify a regression
suite so an LLM-generated facts file that "happens to build" doesn't silently
break a cross-framework invariant. Also `script/lean-bisect` for finding when
an axiomatization regressed across releases.

Adopt the #12216 / PR #12217 landing: `#print axioms` now emits per-computation
axioms instead of collapsing to one shared axiom, and #13117 re-enabled it
under the module system. That turns `#print axioms` into a per-Fact audit
signal — pin it as a golden check so an injected or mis-generated Fact shows
up as an unexpected axiom, not just a build pass.

**Difficulty:** Mid. **Status:** not started. **Deliverable:** golden test
dirs with at least one `.expected.out` per framework, plus a `#print axioms`
allow-list snapshot per kernel.

## 8. Tactics framework + Aesop / `grind` rule sets

`lib/lean4/src/Lean/Elab/Tactic/`, `src/Lean/Meta/Tactic/`, plus the
[Aesop](https://github.com/leanprover-community/aesop) README (`@[aesop]`
safe/unsafe + custom rule builders). The payoff: replace explicit `List.Mem`
chains with one `aesop` call once the axiom set is taught as rules; later, a
domain-specific `prove_predicate` tactic for RICO predicate enumeration.
Dependency note: `aesop` usage is currently zero and `@[simp]` essentially
absent — the automation gap is upstream (membership helpers #2 + codegen #3).
Land those first; this area has no live surface until then. Aesop has no
documented opaque-axiom rule-builder, so a custom builder over domain axioms
is itself a publishable gap.

**Difficulty:** Mid. **Deliverable:** once #2 lands, wire `aesop` into one
legal-domain theorem; file an issue if rule-tagging on opaque axioms
misbehaves.

## 9. Elaborator API and error-message diagnostics

`lib/lean4/src/Lean/Elab/Term.lean`, `BuiltinCommand.lean`, `Declaration.lean`,
`DeclModifiers.lean`. The keyword collision (`protected` / `private` /
`partial` as field names) lives here, and so does every error message the LLM
debug loop has to interpret. Clarifying an unclear elaboration error or linter
diagnostic is a sanctioned external-PR path (no RFC required, per
CONTRIBUTING.md) and directly improves the pipeline. Pair with
`src/Lean/Linter/`.

**Difficulty:** Mid–Deep. **Contribution path:** add a regression case in
`tests/elab_fail/` plus a clarified diagnostic. Avoid lake / Lean/Server /
stage0 for first PRs — owner-gated and churning.

## 10. Mathlib4 + Batteries contribution + FRO case study

Read the std naming conventions and the Mathlib contribute guide
([leanprover-community.github.io/contribute](https://leanprover-community.github.io/contribute/index.html));
Batteries follows the same conventions, docs at
[github.com/leanprover-community/batteries](https://github.com/leanprover-community/batteries)
(the renamed ex-`std4`). Land 2–3 `easy` / `good first issue` Mathlib4 PRs in
`Data/List`, `Logic`, `Order`, `Tactic` — *not* Topology / MeasureTheory /
Analysis / CategoryTheory — to learn the squash-merge review loop. The bigger
prize: the FRO Y3 roadmap runs an explicit "Case Studies: real-world
software/hardware verification demonstrations" stream — the legal-domain and
financial-domain axiomatizations are exactly that. The path is a public mirror
+ writeup + a Zulip pitch on `#lean4`.

**Difficulty:** Entry → Deep over time. **Deliverable:** 2 Mathlib `easy` PRs
by month 2; case-study pitch into the FRO Y3 stream by month 6.

---

## Reading order (collapsed)

1. Reference §4/§7/§8 + TPL Ch.12 ⇒ **#1** (re-validate #12973 opaque-in-kernel)
2. `Init/Data/List/` + `Init/Core.lean`/`Classical.lean` + TPL Ch.10 ⇒ **#2, #4**
3. TPL Ch.9 + Ch.7 + std/naming.md ⇒ **#5**
4. Metaprogramming Ch.3→4→7→8→9 (verify APIs vs source) ⇒ **#3**
5. FRO coinductive entry + Zulip thread ⇒ **#6**
6. Reference §24 (Build Tools) + tests/ patterns + `#print axioms` ⇒ **#7**
7. Aesop README + one wired-in proof (after #2/#3) ⇒ **#8**
8. `src/Lean/Elab/` deep dive when an LLM error message is unactionable ⇒ **#9**
9. Mathlib contribute guide + 2 easy PRs ⇒ **#10**

## Active threads to track (lurk first)

- **`grind` / counterexample-gen MVP** — FRO Y3 proof-automation stream.
  Emerging discharge path for opaque-domain goals `decide` can't reach.
- **Std v1.0 stabilization** — FRO Y3 headline; RC not yet shipped
  (async/await + HTTP). Doc/spec PRs land easily.
- **LeanCopilot** —
  [github.com/lean-dojo/LeanCopilot](https://github.com/lean-dojo/LeanCopilot)
  (active, v4.27.0, 2026-02-11); an opaque-domain integration is publishable.
- **LeanDojo-v2** —
  [github.com/lean-dojo/LeanDojo-v2](https://github.com/lean-dojo/LeanDojo-v2)
  (NeurIPS 2025; original LeanDojo now deprecated). Trace→dataset→train
  framework; relevant to any LLM+Lean training loop.
- **Verso adoption** —
  [github.com/leanprover/verso](https://github.com/leanprover/verso) (v4.29.0,
  now powers the official docs). Documenting predicate libraries with Verso
  surfaces usability gaps — coordinate with maintainers before unsolicited PRs.
- **lean4-mlir** —
  [github.com/brettkoonce/lean4-mlir](https://github.com/brettkoonce/lean4-mlir);
  a different-domain mirror of the string-codegen→elaborator ambition (#3).

## Skip / deprioritise

- Mathematics in Lean Ch.8–13 (analysis-heavy, off-topic).
- FPiL Ch.1–3 and TPL Ch.2–4 (entry material; FPiL Ch.4 Monads is the only
  early bit worth a glance for MetaM plumbing).
- Mathlib `Topology/`, `MeasureTheory/`, `Analysis/`, `CategoryTheory/`.
- `src/lake/` and `src/Lean/Server/` PR work (owner-gated, churning).
- `stage0/` (read-only snapshot; teaches nothing `src/` doesn't).

---

## Cadence

Refreshed weekly from the private working tree. Re-ranking, new threads, and
dropped items are committed as ordinary diffs — the commit log is the change
record.

## Contact

[`quantapix@gmail.com`](mailto:quantapix@gmail.com)

## License

MIT (`LICENSE`). Content-class repo — focus-area prose plus the OSS-contribution
roadmap. Short embedded snippets ride the same MIT grant.
