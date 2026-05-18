# qstudying

> Lean4 expert-track focus areas + open-source contribution targets,
> selected for the *axiomatic-kernel + LLM-generated facts + lake-build-as-verification*
> architecture that backs Qnarre and Qresev.

A weekly-refreshed window into the learning roadmap that runs alongside
the private working repository. Pure-mathlib paths
(Topology, MeasureTheory, Analysis, CategoryTheory) are deliberately
out of scope. Toolchain pin: `leanprover/lean4:v4.30.0-rc2`.

- Parent organisation: <https://github.com/quantapix>
- Engineering output: <https://quantapix.com>
- Motivational record: <https://femfas.net>

## Source-language note

Every `lib/lean4/<path>` reference below is to the upstream
[lean4](https://github.com/leanprover/lean4) repository. Clone it
separately if you want to follow along:

```bash
git clone https://github.com/leanprover/lean4 ~/clone/lean4
```

…and re-interpret the paths as `~/clone/lean4/<path>`.

---

## 1. Kernel semantics: `axiom` / `opaque` / `noncomputable` / `Prop`

The trust boundary the entire architecture rests on. Read
`lib/lean4/src/kernel/type_checker.{h,cpp}`, `declaration.cpp`,
`inductive.cpp` — `is_def_eq_core`, `whnf_core`, `quick_is_def_eq`.
Pair with the Lean Reference §4 (Type System), §7 (Definitions), §8
(Axioms) and *Theorem Proving in Lean* Ch.12. This explains *why*
`act_i ≠ act_j` needs an axiom, why opaque predicates must be
`Prop`-valued (not `Bool`), and why `noncomputable` is correct — not a
smell — for axiom-list witnesses.

**Difficulty:** Deep. Non-negotiable. **Order:** start here.

## 2. Decidability and Classical reasoning on opaque domains

`lib/lean4/src/Init/Decidable.lean`, `Init/Classical.lean`,
`Init/PropLemmas.lean` + TPL Ch.10. Codify *exactly* when `decide`
fires (closed enums with `deriving DecidableEq`) versus when it cannot
(any opaque element). This is the root cause of half the painful
lessons the project keeps hitting. The right output is a one-page rule
for the Python prompt template and for human reviewers.

**Difficulty:** Mid. **Order:** immediately after #1.

## 3. Domain modeling: `structure ... : Prop` vs `inductive ... : Prop`

TPL Ch.7 + Ch.9. The 7-field-conjunction (`Section1962c`) and
4-case-disjunction (`ValidCivilRicoComplaint`) shapes the kernel
already uses; deepen this to a habit. Anonymous-constructor literals
are the most LLM-robust intro form; tactic blocks regress under
whitespace. Add `deriving DecidableEq, Repr` reflexively on every
closed enum (`Strategy`, `GicsSector`, etc.) so failure messages stay
legible to the LLM debug loop.

**Difficulty:** Mid.

## 4. List membership and finite-domain proof patterns

`lib/lean4/src/Init/Data/List/Basic.lean`, `BasicAux.lean`,
`Lemmas.lean`. The daily proof shape is a `List.Mem.head / .tail`
chain. Master it, then write *one* helper theorem per kernel —
`mem_sampleActs_cases : a ∈ sampleActs ↔ a = act₁ ∨ … ∨ a = act₇` — and
have the LLM consume the helper instead of regenerating chains. This
area is also the most-merged Mathlib slice (`Data/List`), so it
doubles as a contribution surface.

**Difficulty:** Entry → Mid.

## 5. Elaborator API and error-message diagnostics

`lib/lean4/src/Lean/Elab/Term.lean`, `BuiltinCommand.lean`,
`Declaration.lean`, `DeclModifiers.lean`. The keyword collision
(`protected` / `private` / `partial` as field names) lives here, and
so does every error message the LLM debug loop has to interpret.
Improving an unclear elaboration error is a sanctioned external-PR
path (no RFC required) and directly improves the pipeline. Pair with
`src/Lean/Linter/`.

**Difficulty:** Mid–Deep. **Contribution path:** add a regression case
in `tests/elab_fail/` plus a clarified diagnostic.

## 6. Metaprogramming: replace the Python codegen with a Lean elaborator

*Metaprogramming in Lean 4* Ch.3 (Expressions) → Ch.4 (MetaM) → Ch.7
(Elaboration) → Ch.8 (DSLs) → Ch.9 (Tactics). The strategic move:
design a `facts%` macro / custom command that consumes a structured
input (JSON or a Lean-native syntax) and emits the axiom block
directly. This kills the two most fragile parts of the current
architecture — namespace resolution and string-level Lean codegen — by
moving them inside the elaborator. Likely the single highest-leverage
area on this list.

**Difficulty:** Deep.

## 7. Namespace, `open`, `export` discipline that survives codegen

`lib/lean4/src/Lean/Elab/BuiltinCommand.lean` (handlers for
`namespace`/`open`). The manifest's `setup_namespaces` is currently
doing semantic work that should be syntactic. Specific bug surfaced by
the patterns review: `Sector.Tech` (driver-emitted) ≠
`Accounting.GicsSector.InformationTechnology` (kernel) — current build
only passes by `open` ordering. Fix with
`abbrev Tech := GicsSector.InformationTechnology` *or* drop the whole
layer once #6 lands.

**Difficulty:** Entry. **Order:** before #6 if #6 is more than two
weeks out; otherwise fold into #6.

## 8. Tactics framework + Aesop rule sets

`lib/lean4/src/Lean/Elab/Tactic/`, `src/Lean/Meta/Tactic/`, plus the
[Aesop](https://github.com/leanprover-community/aesop) README. The
payoff: replace explicit `List.Mem` chains with one `aesop` call once
the axiom set is taught as rules; later, write a domain-specific
`prove_predicate` tactic for RICO predicate enumeration. Fits
naturally into the LLM loop because the LLM only has to emit the
high-level call; Aesop searches the rule set.

**Difficulty:** Mid.

## 9. Lake + golden-output regression scaffolding

`lib/lean4/src/lake/Lake/` (BuildIndex, BuildInfo) + `tests/lean/run/`
and `tests/elab_fail/` patterns (`.lean` + `.expected.out`). Codify a
regression suite per framework so an LLM-generated facts file that
"happens to build" doesn't silently break a cross-framework invariant.
Also: `script/lean-bisect` for finding when an axiomatization
regressed across releases.

**Difficulty:** Mid.

## 10. Mathlib4 + std4 / Batteries contribution lifecycle

Read `lib/lean4/doc/std/{naming,style}.md` and the Mathlib guides at
[leanprover-community.github.io/contribute](https://leanprover-community.github.io/contribute/index.html).
Land 2–3 `easy`-labeled Mathlib4 PRs in `Data/List`, `Logic`, `Order`,
`Tactic` — *not* in Topology / MeasureTheory / Analysis /
CategoryTheory. Use this to learn the review loop before pitching a
substantive RFC. Once fluent, the
[FRO Y3 roadmap](https://lean-lang.org/fro/roadmap/y3/) explicitly
invites "demonstration case studies"; the path is a public mirror +
writeup + Zulip pitch on `#lean4`.

**Difficulty:** Entry → Deep over time.

---

## Reading order (collapsed)

1. Reference §4 / §7 / §8 + TPL Ch.12 ⇒ **#1, #2**
2. TPL Ch.7 + Ch.9 + std/naming.md ⇒ **#3, #4**
3. Aesop README + one wired-in proof ⇒ **#8**
4. Metaprogramming Ch.3 → 4 → 8 → 9 ⇒ **#6** (also fixes #7)
5. Reference §24 (Lake) + tests/ patterns ⇒ **#9**
6. Mathlib contribute guide + 2 easy PRs ⇒ **#10**
7. `src/Lean/Elab/` deep dive when an LLM error message is
   unactionable ⇒ **#5**

## Active threads to track (lurk first)

- **Coinductive predicates** — FRO Y3 priority #3, Zulip
  `#lean4 > coinductive predicates`. Direct fit for ongoing-pattern
  RICO and rolling-window TREND / MOMENTUM.
- **RFC #12216 — One axiom per native computation** —
  [issue 12216](https://github.com/leanprover/lean4/issues/12216).
  Touches the build-as-signal pattern.
- **Std v1.0 stabilisation** — FRO Y3 #1; doc/spec PRs land easily.
- **LeanCopilot** —
  [github.com/lean-dojo/LeanCopilot](https://github.com/lean-dojo/LeanCopilot);
  an opaque-domain integration is publishable.
- **Verso adoption** —
  [github.com/leanprover/verso](https://github.com/leanprover/verso);
  document predicate libraries with Verso to surface usability gaps.

## Skip / deprioritise

- Mathematics in Lean Ch.8–13 (analysis-heavy, off-topic).
- FPiL Ch.1–3 and TPL Ch.2–4 (entry material).
- Mathlib `Topology/`, `MeasureTheory/`, `Analysis/`,
  `CategoryTheory/`.
- `src/lake/` and `src/Lean/Server/` PR work (owner-gated, churning).
- `stage0/` (read-only snapshot; teaches nothing `src/` doesn't).

---

## Cadence

Refreshed weekly from the private working tree. Re-ranking, new
threads, and dropped items are committed as ordinary diffs — the
commit log is the change record.

## Contact

[`quantapix@gmail.com`](mailto:quantapix@gmail.com)

## License

MIT (`LICENSE`). Content-class repo — focus-area prose plus the
OSS-contribution roadmap. Short embedded snippets ride the same
MIT grant.
