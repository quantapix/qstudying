# qstudying-public — status

_Snapshot: 2026-07-24. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall

**The roadmap became a kernel, and the kernel now gates a release.** This repo
windows the subproject that owns the *third* Lean4 axis — the operational
domain — alongside the cross-axis representation research that keeps all three
kernels consistent. The syllabus was rewritten 2026-06-10 under a three-axis
charter; the first operational theorem (session write-lock exclusion) landed
2026-06-12. This cycle the axis reached the point the whole exercise was aimed
at: a machine-checked gate that sits in front of the project's own public
pushes, ratified 2026-07-21.

## The three axes

The architecture the syllabus is selected *for* runs in three parallel kernels,
one per domain, never sharing ground truth:

- **Textual** — the legal-domain axiom sets published as
  [`qnarre-public`](https://github.com/quantapix/qnarre-public).
- **Numerical** — the financial-domain axiom sets published as
  [`qresev-public`](https://github.com/quantapix/qresev-public).
- **Operational** — git-grounded axioms over the agent constellation's own
  daily mechanics (this subproject; focus area #10), proved over synthetic and
  structurally-synthetic snapshots, never real-tree data.

All three pin the same Lean toolchain (`leanprover/lean4:v4.32.0`, three-way
lockstep since the 2026-07-14 bump; bumps move all three and replay each
kernel's example proofs).

## A kernel-checked gate over public pushes

The operational axis's newest theorem cell is about the project's own release
discipline: every payload that goes out to a public surface must be covered by
a clearance record, and *that claim is checked by the kernel*, not by a
convention.

The load-bearing artifact is a small committed map from **payload class** to
**required clearances**. Three properties make it hard to defeat:

- **The class is structural, not self-asserted.** It is derived from a prefix
  that is mandatory in every catalog key — the same string that appears in the
  distribution path and the upload trigger — so it cannot be omitted the way an
  optional per-entry boolean flag can.
- **An unregistered class refuses the push.** Failing *open* on an unrecognized
  class would reintroduce exactly the vacuity the gate exists to catch: a
  payload with no clearance record and no required clearances makes the coverage
  implication trivially true. Unknown class ⇒ refuse.
- **The map is a floor, never a ceiling.** Per-entry flags may widen the
  required set; nothing may narrow it below the committed floor.

Two independent readers consume the same committed file — the release tooling
that produces it, and the kernel-side extractor that turns it into a fact the
theorem quantifies over. Neither imports the other; the JSON is the only seam.
A malformed map raises rather than degrading to "no gates", and a map that names
a clearance which is not itself independently verifiable is rejected outright.

### The amendment: a hash is not a hash

Ratification carried a correction worth publishing, because the failure mode is
general. A clearance record anchors content at **two levels**: a roll-up hash
over the whole cleared payload, and a per-face table with one entry per surface
the payload lands on. The push record, by contrast, hashes **the bytes that
actually landed on one surface**. Both fields are 64 hex characters; both are
named something like "content hash"; they denote different objects. Joining them
by name made a properly cleared push read as an uncleared breach.

The fix was not to loosen the comparison but to **type the discharge**: a push
binds its clearance if its hash matches the roll-up *or* the face for the class
that push discharges, with the surface → class assignment supplied as a
committed fact and non-widening proved as a theorem rather than asserted as a
convention. Three negative closure facts travel with it — drop any one and the
breach detector silently stops biting while the build stays green. That pairing
(a positive theorem plus the negative closures its breach dual needs to remain
inhabitable) is now the standing shape for gate work on this axis.

## Adversarial standing

Numbers as of this snapshot, all machine-checked by `lake build`:

- **98** theorems proved across the operational targets; **70** on the
  domain-coverage numerator.
- **63** adversarial rounds; **341** attack probes thrown, **2** landed, **0**
  open.
- **20 of 22** numerator cells re-certified under the closed adversarial
  contract — each holding zero landed attacks against a fresh, independent
  adversary.

The 20/22 figure is deliberately conservative and was ruled so on 2026-07-22.
The badge is gated on the *cumulative* record: a cell that ever had an attack
land is disqualified permanently, even after the repair. A latest-round-scoped
reading of the same data would show 22/22; it was rejected, not deferred. The
rationale is worth stating plainly — a cell that once had something land has
been shown to be repairable, not un-landable, and the public figure should say
the weaker of the two things.

Coverage is reported as coverage, not as a blanket falsifiability claim: rounds
predating the 2026-07-14 oracle-independence fix did not certify the adversary's
independence, which is why re-certification is a separate, slower number.

## Representation research

The cross-axis representation guide (authored 2026-06-11, debate-verified) maps
each standard Theorem-Proving-in-Lean / reference-manual idiom to its kernel
counterpart with the three axes as parallel columns, and ships the
code-generation templates the proof-driving lanes consume.

Two cells landed this cycle in the **context-operations** family — the machinery
that decides what an automated agent is given to work from:

- **Verdict totality** — the decision to supply, withhold, or refuse context is
  a typed, total function over the gate state; there is no unhandled case that
  falls through to "supply anyway".
- **Shortening conservativeness** — the classifier that shortens material is
  closed-set, and anything unclassified **passes through** rather than being
  dropped; omissions are marked; the result is pinned never to be worse than the
  unshortened input at the measured gap.

The round that produced them landed nine attacks against the kernel and repaired
them in the same round. The sharpest one generalizes: **every conditioning
assumption needs an escape predicate.** An unconditioned assumption whose
conclusion is the negation of a breach dual makes that dual *uninhabitable* —
the kernel then refutes its own breach case, and the detector proves anything
you ask it. Never state an assumption whose domain is the breach dual's domain.
Three lesser findings resolved the same way, by **withdrawing the claim rather
than patching it**: a mark is not a count; a cost bound relocated is not a cost
bound escaped; a measured, load-free assumption gets deleted, not defended.

## Test-gate discipline

A second general lesson, from two gates found silently red this cycle: a
per-family shape gate is exercised only when that family's own runner is
invoked, so it goes quiet — not red — when a new artifact lands without an
update to that gate's expected set. Two rules followed: landing a governed
artifact updates its shape gate in the same change, and a loud aggregating
battery runs every family's gate before a session closes. A gate whose
accumulator is only asserted inside a wrapper is the adjacent trap: green and
dead.

## Cadence

Re-rankings, new threads, and dropped items land as ordinary diffs; the commit
log is the change record.

## How to verify

- The 10 focus areas + reading order + skip list are the whole syllabus; every
  upstream citation resolves to a public repository.
- The textual and numerical kernels are observable in the two published kernel
  repos above; the operational kernel's theorems are described here as they land.
- Every count in this file is emitted by the same build that checks the proofs;
  none of it is hand-maintained.
