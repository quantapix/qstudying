# qstudying-public — status

_Snapshot: 2026-06-12. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall

**The roadmap became a kernel.** This repo windows the subproject that owns
the *third* Lean4 axis — the operational domain — alongside the cross-axis
representation research that keeps all three kernels consistent. On
2026-06-10 the syllabus was rewritten under a three-axis charter (every
area tagged to a workstream; the old open-source-contribution framing
retired). On 2026-06-12 the first operational theorem landed: a
kernel-checked proof that the constellation's session write-lock discipline
excludes conflicting concurrent sessions, driven end-to-end by an
adversarial coding-vs-testing LLM debate lane — no manual proof driving.

## The three axes

The architecture the syllabus is selected *for* runs in three parallel
kernels, one per domain, never sharing ground truth:

- **Textual** — the legal-domain axiom sets published as
  [`qnarre-public`](https://github.com/quantapix/qnarre-public).
- **Numerical** — the financial-domain axiom sets published as
  [`qresev-public`](https://github.com/quantapix/qresev-public).
- **Operational** — git-grounded axioms over the agent constellation's own
  daily mechanics (this subproject; focus area #10). First theorem proved
  2026-06-12: lock exclusion over an extracted repo snapshot, with the
  adversarial side's negation and axiom-audit probes committed alongside
  the constructive proof.

All three pin the same Lean toolchain (`leanprover/lean4:v4.30.0`,
three-way lockstep; bumps move all three in one commit and replay each
kernel's example proofs).

## Representation research

The cross-axis representation guide is now written (authored 2026-06-11,
debate-verified): it maps each standard Theorem-Proving-in-Lean /
reference-manual idiom to its kernel counterpart with the three axes as
parallel columns, plus the code-generation templates the proof-driving
lanes consume (facts skeletons, quartet module templates, membership-helper
shapes, a diagnostics decoder, and the round/report/verdict emit schemas).
The proof drivers read a document now, not folklore.

## Cadence

Re-rankings, new threads, and dropped items land as ordinary diffs; the
commit log is the change record.

## How to verify

- The 10 focus areas + reading order + skip list are the whole syllabus;
  every upstream citation resolves to a public repository.
- The textual and numerical kernels are observable in the two published
  kernel repos above; the operational kernel's theorems are described here
  as they land.
