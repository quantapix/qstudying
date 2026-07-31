# qstudying-public — status

_Snapshot: 2026-07-31. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall

**The roadmap became a kernel, and the kernel now gates a release.** This repo
windows the subproject that owns the *third* Lean4 axis — the operational
domain — alongside the cross-axis representation research that keeps all three
kernels consistent. The syllabus was rewritten 2026-06-10 under a three-axis
charter; the first operational theorem (session write-lock exclusion) landed
2026-06-12. The cycle before this one reached the point the whole exercise was
aimed at: a machine-checked gate that sits in front of the project's own public
pushes, ratified 2026-07-21. This cycle closed a four-cell family — and closed
one of its cells by proving that the property, as stated, does not hold.

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
kernel's example proofs). The pin now leads the published reference manual
rather than trailing it, which is noted at each source refresh rather than
assumed either way.

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

**None of these numbers moved this cycle, and that is the honest reading.** Two
kernel cells landed in the window and neither feeds this scoreboard. One is held
off pending its family's completion; the other is off it permanently (below).
Publishing a frozen scoreboard next to two landings looks like neglect, so:
the roster is a deliberate hold-out, and a cell joins it whole — its proved
theorems *and* its full adversarial standing together, including the attacks
that landed against it — or not at all. Numbers that arrive without their
scars are the failure mode this discipline exists to prevent.

The **0 open** figure is likewise roster-scoped, not tree-scoped: the two
off-roster cells carry fifteen open gaps between them, which is what an
adversarial lane looks like while it is still running.

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

The **context-operations** family — the machinery that decides what an automated
agent is given to work from — is now closed at four cells. Two landed in the
previous cycle (a *verdict totality* result: supply, withhold, or refuse is a
typed total function with no fall-through to "supply anyway"; and a *shortening
conservativeness* result: the classifier is closed-set, anything unclassified
passes through rather than being dropped, omissions are marked). Two landed in
this one.

### A cell closed on a negative result

The third cell asked whether a recall mechanism can guarantee that what it
returns reaches only an entitled reader. **It cannot, as stated, and the cell is
now the evidence for that.** Three rounds seated the guarantee three different
ways — on provenance (the mechanism ran), then on delivery matching intent, then
on the caller being cleared — and each repair relocated the same defect one
level outward instead of curing it. A fourth index would have moved it again.

The pattern is the result: **each seat conditioned on the mechanism having run
in some domain while concluding about a domain the mechanism does not control**,
so the kernel kept re-assuming the very thing it was meant to constrain. The
honest content of the cell is the part that *is* mechanically producible — the
scan axis — plus a precise statement of where the guarantee stops.

Two conditions bind any future attempt, both established adversarially rather
than argued:

- **A non-enumerating fixture.** On a snapshot that enumerates all of its own
  deliveries, every conditioning assumption is droppable: the property
  re-derives from the emitted facts with no assumption in the proof trail. An
  assumption you cannot witness is not an assumption.
- **Escape predicates with an inhabited negative case.** If every escape is
  emitted as a universal closure, the fixture contains no violating instance and
  every theorem branching on it is discharged against an empty antecedent. An
  escape with no negative witness tests nothing.

The rounds and their landed probes stay committed. Deleting them to make the
cell look clean would destroy the only thing it produced.

### A cell that holds, with its limit written down

The fourth cell asks a different question — not about data crossing a boundary
but about the **components at** the boundary: a component that publishes an
obligation about how it behaves when degraded, and is then held to it. It is the
only cell in the family that compares two emitted families *to each other*
("what it said it would do" against "what it did") rather than each against a
closure. Without that comparison the declaration would be inert decoration.

It proved green, fifteen probes thrown and none landed. Its published limit is
sharper than its result: **on the fixture that exercises it, one of its four
clauses is true by construction**, because that snapshot happens to emit both a
declaration and its closure for every component. The protection survives in the
type and is unexercised in the fixture. Saying so is cheaper than discovering it
later, and it is the same "inhabited negative case" condition the third cell
established, applied inward.

### The standing lesson from the family

**Every conditioning assumption needs an escape predicate.** An unconditioned
assumption whose conclusion is the negation of a breach dual makes that dual
*uninhabitable* — the kernel then refutes its own breach case, and the detector
proves anything you ask it. Never state an assumption whose domain is the breach
dual's domain. Three lesser findings resolved the same way, by **withdrawing the
claim rather than patching it**: a mark is not a count; a cost bound relocated
is not a cost bound escaped; a measured, load-free assumption gets deleted, not
defended.

## Instrument discipline

Three lessons this cycle, all about instruments rather than proofs, and all
general enough to be worth publishing.

**An adversary's independence is checked by an instrument, and the instrument
has to run before the evidence expires.** The adversarial lane's whole claim
rests on the attacker not having seen the proof. That is now audited
mechanically over several channels — including, added this cycle, the channel
where a cell has simply seen the project's shared vocabulary and produces echo
rather than evidence. The audit freezes a verdict alongside the round's own
verdict, in the same step, for a blunt reason: **the transcripts it reads are
retained for about ten days at best, and were observed surviving as little as
one to two days.** An unaudited round is therefore not "unchecked, we'll look
later" — it is permanently unauditable, and the only remedy is to run the whole
round again blind. "Audit the history later" is not a plan.

**The instrument can convict the operator, not just the cell.** One round came
back with an independence breach whose single genuine event was authored by the
round's own briefing material: the brief told a blind participant that a
sibling's committed proof "showed the shape" of the probe it should write. Every
rule in the participant's contract barred it from *choosing* such a read; no rule
barred the orchestrator from *instructing* one, and an instructed read arrives as
an instruction, not as a violation — the participant complied and disclosed it,
correctly. The fix is a rule about authoring, not about compliance: convey shape
by inlining the pattern or citing the shared template set, never by pointing a
blind participant at a sibling's finished work. The consequence is severe enough
to be worth stating: a leaked round's proofs may be perfectly sound and still be
**void as evidence** — no certification, no standing, no coverage number.

**A flag whose failing branch cannot be reached is not a flag.** The build's
freshness indicator asked whether the metrics snapshot's revision was an
ancestor of the current one. It is — and it stays one forever as work advances,
so the check could report "fresh" but could never report "stale". It read green
across three separate landings while the snapshot underneath it aged by two
weeks. The repair was to ask the question the flag actually claimed to answer —
*has the thing being measured changed since it was measured?* — and to fail
closed when that cannot be determined. On its first cycle the repaired flag
immediately reported the staleness the old one had been hiding, which is the
only evidence that a gate works.

The narrower version of the same rule, from the prior cycle, still stands: a
per-family shape gate is exercised only when that family's own runner is
invoked, so it goes quiet — not red — when a new artifact lands without an
update to that gate's expected set. Landing a governed artifact updates its
shape gate in the same change, and a loud aggregating battery runs every
family's gate before a session closes.

## Evidence handling

The audit described above reads raw agent transcripts, and those are now
archived rather than discarded — but **never into the repository**. A committed
transcript is, by construction, an answer key: it is exactly the material a
future adversary must not have seen, so committing it would poison the next
round's independence while looking like good record-keeping. The archive holds
audit evidence only — no round records, which stay committed and append-only —
and any proposal to commit any of it re-opens the privacy question from the
start rather than inheriting a past answer.

## Cadence

Re-rankings, new threads, and dropped items land as ordinary diffs; the commit
log is the change record.

## How to verify

- The 10 focus areas + reading order + skip list are the whole syllabus; every
  upstream citation resolves to a public repository.
- The textual and numerical kernels are observable in the two published kernel
  repos above; the operational kernel's theorems are described here as they land.
- Every count in this file is emitted by the same build that checks the proofs;
  none of it is hand-maintained. Where a count is scoped — as the adversarial
  scoreboard is — the scope is stated next to it rather than left to inference.
