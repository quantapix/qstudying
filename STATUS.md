# qstudying-public — status

_Snapshot: 2026-06-01. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 expert-track + open-source
contribution roadmap. Companion to the [README](./README.md), not a
substitute.

## Overall

**A roadmap, not a runtime** — so its public surface is the syllabus
itself: 10 ranked focus areas selected for the *axiomatic-kernel +
LLM-generated facts + lake-build-as-verification* architecture, with a
reading order, active upstream threads to track, and an explicit skip list.
Pure-mathlib paths (Topology, MeasureTheory, Analysis, CategoryTheory) are
deliberately out of scope. Toolchain pinned to the same Lean version both
kernels build against.

## What the roadmap now backs

The architecture the syllabus is selected *for* is no longer aspirational —
it is running in the two kernel subprojects published as
[`qnarre-public`](https://github.com/quantapix/qnarre-public) (legal) and
[`qresev-public`](https://github.com/quantapix/qresev-public) (financial),
including the redundant-encoding axiomatization programs that score blind
agent encodings against a hand-built golden reference via kernel-checked
Bridge lemmas. The focus areas read against real kernel pain points (the
`axiom`/`opaque`/`noncomputable`/`Prop` trust boundary, decidability on
opaque domains, `List.Mem` proof patterns, namespace discipline that
survives codegen, and the metaprogramming move to replace string-level
codegen with a Lean elaborator).

## Contribution surface

The highest-leverage upstream paths are flagged: an unclear elaboration
diagnostic (a sanctioned external-PR path, no RFC required); a handful of
`easy`-labeled standard-library PRs to learn the review loop; and, longer
term, a demonstration case study the language foundation's roadmap
explicitly invites. Active threads tracked (lurk-first): coinductive
predicates, native-computation axiom RFCs, standard-library stabilization,
an LLM-copilot opaque-domain integration, and documentation tooling.

## Cadence

Re-rankings, new threads, and dropped items land as ordinary diffs; the
commit log is the change record. The README itself changes when the
syllabus is re-ranked.

## How to verify

- The 10 focus areas + reading order + skip list are the whole artifact;
  every upstream citation resolves to the public language repository.
- The architecture they target is observable in the two published kernel
  repos.
