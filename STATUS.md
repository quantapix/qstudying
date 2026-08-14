# qstudying-public — status

_Snapshot: 2026-08-14. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall

Third cycle running in which the published scoreboard did not move, and the
third with a different reason. This one is the most useful of the three:
**an instrument was found measuring the wrong population four separate times,
in three different layers, inside one week** — and once that was named as one
finding rather than four incidents, the response stopped being a fix and became
a standing law about what a guard has to prove before it is permitted to run.

Nothing below changes a theorem. Everything below changes what this project is
willing to claim about one.

## The three axes

The architecture the syllabus is selected *for* runs in three parallel kernels,
one per domain, never sharing ground truth:

- **Textual** — the legal-domain axiom sets published as
  [`qnarre-public`](https://github.com/quantapix/qnarre-public).
- **Numerical** — the financial-domain axiom sets published as
  [`qresev-public`](https://github.com/quantapix/qresev-public).
- **Operational** — git-grounded axioms over the agent constellation's own
  daily mechanics (this subproject; focus area #10), proved over synthetic and
  structurally-synthetic snapshots, never real-tree data. Its consumer is
  local-only and only visualizes; nothing here is a deployed surface.

All three pin the same Lean toolchain (`leanprover/lean4:v4.32.0`, three-way
lockstep since the 2026-07-14 bump; bumps move all three and replay each
kernel's example proofs).

## Adversarial standing

Numbers as of this snapshot, all machine-checked by the build:

- **98** theorems proved across the operational targets; **70** on the
  domain-coverage numerator.
- **63** adversarial rounds; **341** attack probes thrown, **2** landed, **0**
  open.
- **20 of 22** numerator cells re-certified under the closed adversarial
  contract, each holding zero landed attacks against a fresh, independent
  adversary.

**Not one of these moved this cycle, and the reason is structural rather than
disappointing.** Four rounds ran. All four ran against a cell that is not on
the roster, and the roster is what these figures count. A cell joins whole — its
proved theorems *and* its full adversarial standing, including attacks that
landed against it — or not at all. Two of the four rounds were void besides.

The round count deserves a note, because there are two defensible ways to count
it and this file publishes the smaller one on purpose. One count is every round
that has been run. The other counts only rounds on fully adjudicated roster
entries. **The published figure is the adjudication-gated one**, on the same
principle as the certification badge: a public count gates on adjudication.
The two bases diverge by a few rounds continuously and neither is ever
corrected toward the other; they answer different questions.

The 20/22 figure remains deliberately conservative by ruling: the badge gates on
the *cumulative* record, so a cell that ever had an attack land is disqualified
permanently, even after the repair. A latest-round-scoped reading of the same
data would show 22/22; it was rejected, not deferred. A cell that once had
something land has been shown to be repairable, not un-landable, and the public
figure should say the weaker of the two things.

Coverage is reported as coverage, not as a blanket falsifiability claim: rounds
predating the 2026-07-14 oracle-independence fix did not certify the adversary's
independence, which is why re-certification is a separate, slower number.

## An instrument is only as good as the population it enumerates

Four failures in one week, three layers apart, one shape:

- A reporting column published **zero theorems proved for every round that had
  ever run**, while a cumulative figure in the same table read two thirds
  complete. The join matched an identifier against build targets by substring
  and nothing had ever put one there. It survived for months because a zero
  looks like a modest round. The sibling join — the same defect, on the attack
  side — had been found and fixed earlier, and a comment recording that fix sat
  a few lines above the broken one. **A fix applied where it was noticed is not
  a fix of its class.**
- An axiom-floor probe reached 143 of the 282 declarations in its own module.
- An open-findings counter could only ever grow, because a conceded finding had
  no retirement path at all.
- Guards throughout an example **hand-copied the thing they were checking**, so
  they could never observe the repair they demanded.

The unifying rule: **a guard that restates its target instead of deriving from
it cannot observe the repair it demands, and a one-round finding becomes
permanent prose.** It was validated the round after it was minted. An attack
that had copied eight names out of a manifest into its own source was rebuilt to
derive them; it then saw the correction and stood down — the first time an
attack in that cell retired by the fix it demanded rather than by an operator
deleting it. The round after that, seven more conceded findings were re-keyed
the same way and retired together, one of them completing the full
observe-repair-retire cycle inside a single round.

**None of that is in the numbers above.** That round breached, so the fold
refused, and seven retirements exist in the tree and in no metric until the next
clean round. That is the honest cost of a breach: it withholds the number, not
the work.

## Enumerate by the environment — necessary, and not sufficient

The axiom-floor probe was rebuilt to enumerate every constant the example's
modules contribute, internal ones included, with two populations reported per
run. Population 310 against the old text scan's 143, a strict superset, verdicts
unchanged, and zero false refusals across the live roster.

The rebuild is worth publishing for what it *did not* fix. Four distinct escape
forms are witnessed in one example, and they split into two families that defeat
opposite instruments:

- An **anonymous declaration** contributes no constant at all, so it is
  invisible to a text scan, to an axiom-print, and to a roster check alike.
- A **structure** mints a derived constant that appears nowhere in the file's
  text.
- A **proposition-valued definition** sits outside any keyword roster.
- A **private declaration** is name-mangled outside its own namespace and fails
  both halves of every prefix-anchored filter — five live constants of one
  module are rejected today.

The first three defeat source-derived instruments. The fourth defeats
environment-derived ones. So "enumerate from the environment" is necessary and
**not** sufficient: the filter is the other half of the instrument, and both the
population and the filter have to be derived from the same place.

The measured consequence, which is the sharpest statement of why any of this
matters: one example's classical footprint was **ten, not the eight its own
prose enumerated**. A footprint counted over named declarations is blind by
construction to an anonymous one carrying the same dependence — so the gap and
the mismeasurement had a single cause, and finding either finds both.

Last cycle this file published one of those forms open, as *reported but not
reproduced*. It is now reproduced with live members, and cured.

**The first measurement of the cure was void.** The build environment read
week-old compiled artifacts without complaint and reported 229 of 310 constants,
with 51 names the old scan had found simply missing. The thing that caught it
was the superset check, not the compiler — nothing errored. Build before you
measure is now written down beside the rate, because a measurement taken over
stale artifacts does not fail, it just answers a different question.

## What a guard has to prove before it is allowed to run

Adopted this cycle as a standing law rather than another individual repair. A
guard is admissible only once it discharges all of these, the first three as
committed, suite-exercised tests:

1. **A known-bad witness it must reject.** A witness that a later run cannot
   re-fire unattended does not discharge the obligation.
2. **Reachability** — the failing branch demonstrably reachable, not dead by
   construction.
3. **A false-refusal rate against a named, frozen corpus**, recorded as
   numerator *and* denominator with its population declared. Live session
   transcripts are not a corpus: they evaporate within a day or two, and **a
   test over evaporating input passes vacuously forever.** An empty or
   near-empty denominator grades *unmeasured* — never zero, never clean.
4. **Mandate compatibility, checked before arming.** Enumerate what the
   surrounding instruction surfaces *require* on the guarded surface and replay
   each against the guard. A guard that refuses a mandated act may not arm, and
   the conflict goes to the author of the instruction, not to the participant.
5. **An excusal predicate keys on what transferred, never on how it was
   expressed.** Enumerate the ways the conduct can be phrased, then key on none
   of them.

The fifth is a law because it was a bill. One over-convicting arm voided three
rounds by convicting two structurally unrelated and entirely safe commands — a
recursive search and a status query — whose path arguments provably excluded the
sandbox in both cases. The arm read the verb and the working directory and
discarded the path arguments. A syntax-keyed excusal systematically punishes
exactly the careful caller who scoped the command properly.

The symmetry is the point, and it took a while to see: **an over-convicting
instrument voids a round precisely as a real leak does.** In one of those
rounds, the same verdict that convicted a cell on a command that transferred
nothing graded the other cell clean on zero channels while it had disclosed a
listing that named four internal documents. The instrument inverted the two
cells relative to what actually transferred. It was not superseded, because an
orchestrator correcting a breach that voids its own round is self-interested and
does not get to.

## The artifact under test is itself a channel

New this cycle, and published open rather than cured. The adversarial claim
rests on the attacker not having seen prior adjudications. But the proof file
under test carries **136 references to prior rounds**, including verbatim
convictions, and the adversary cannot test the artifact without reading it. One
round graded clean with that channel wide open.

This is not fixed by deleting the history. That history is the constructive
side's legitimate rationale for why the proof is shaped the way it is. It is
recorded as a known open channel, which is the honest disposition when the cure
would cost more than the leak.

## Ordering is a gate, and prose could not hold it

Last cycle published three ordering defects in five weeks, all the same shape:
*a step that promotes results ran upstream of the oracle that can void them.*
Each was fixed correctly and none of the fixes prevented the next, because the
order was enforced by an operator following a document.

One of the three is now a mechanism rather than a document. The step that folds
a round's results into standing refuses on a breach with its own exit code, and
a single verb chains the closing steps in their required order instead of
leaving the operator to sequence them. The rule it enforces is still worth
stating in general, because the remaining cures are still prose: **nothing that
promotes may run before the oracle that can void it.**

## An archive is evidence only once something checks it is there

The independence audit reads raw agent transcripts, and those decay within days,
so verdicts are frozen alongside the round they judge and the raw evidence is
archived outside the repository in the same step. The retention window is why
the freeze has to be immediate: **an un-audited round is not "unchecked, we
will look later" — it is permanently unauditable**, and the only remedy is to
run the whole round again blind. "Audit the history later" is not a plan.

Last cycle's census of that archive found seven frozen verdicts with nothing
underneath them, then found that four of the seven had never been lost at all —
they were sitting on a second machine, because those rounds had run there. A
census that enumerates on one machine and reports absence-here as gone is not a
stale census; it is a census of the wrong universe.

The integrity check itself was corrected twice this cycle, in the same class it
exists to catch. Its comparison of an archive's participants against a verdict's
participants had been treating a one-sided-empty comparison as a match — that is
now **unverifiable**, which is a distinct verdict from *checked and agreed*, and
it exits accordingly. The correction landed in the shared component rather than
the local copy, after the first attempt landed in the wrong layer, at the wrong
scope, with the wrong exit semantics.

Two rules survive from all of it:

- **A gate has a coordinate system, not a target set.** Scoping by tree fixes
  membership and leaves machine, instrument version and corpus vintage silently
  partial. Fixing one axis of a scope is not fixing the scope.
- **Mechanizing a rule fixes who has to remember it, not whether the question
  was scoped right.** A declared loss should be read as *absent here* until a
  peer location has been checked.

The archive itself still never enters the repository. A committed transcript is,
by construction, an answer key — exactly the material a future adversary must
not have seen — so committing it would poison the next round's independence
while looking like good record-keeping. Round records stay committed and
append-only; the archive holds evidence only.

## Instruments that check themselves before they report

A small pattern, stated because it is cheap and it has now caught three real
defects at first fire. Every arm of a periodic reporting check is preceded by a
self-sabotage pass over the check's own logic; if the sabotaged copy does not
fail, the run aborts before producing any verdict at all. Its first fire
convicted its own arm design twice — one arm mass-refused historical records
from before the contract it was grading existed, and a self-assertion caught a
generated baseline directory that had never been excluded from version control.
Both were found before the check ever published a number.

The same discipline applies to the witnesses: a proof-of-fire arm can be
simultaneously correct — it fails — and wrong, because it fails for a different
reason than the one it was written for. Run the sabotage and read *why* it went
red, not just that it did.

## The cost of the trust

One finding from a review of the lane's own daily operation belongs here because
it is unflattering and load-bearing: **the fixed per-round overhead now exceeds
the proving work.** A round is roughly a dozen orchestrator-sequenced steps, and
nearly every one is scar tissue from a specific dated defect, each individually
justified. The composition is the problem. The honest summary is deflation on
the headline numbers and a strong net gain in how far those numbers can be
trusted — deliberate, ruled, and correct — with a cost curve for maintaining
that trust that is currently super-linear.

## A polarity trap worth publishing

One recurring mechanical trap, because it will recur anywhere a test asserts an
absence. A gap probe asserts that a diagnostic *is* produced. Close the gap and
the probe compiles clean — so the attack lane reads clean-on-annotated as
*landed*, and **the round that fixes a named gap reds itself.** There is no
"discharged" verdict to reach for. The resolution is to retire the probe into a
standing discharge record: drop the directive, keep the measurement.

## Build freshness

The metrics snapshot behind the public status card is unchanged from last
cycle's, and the tree has moved further away from it: **229 files** under the
kernel tree now differ from the snapshot revision, up from just under two
hundred. The freshness flag reports stale and the public card reads **degraded**
rather than green.

That flag is the one repaired two cycles ago, after the previous version asked
whether the snapshot's revision was an *ancestor* of the current one — which it
is, and stays, forever, so it could report fresh but could never report stale.
Three weeks of continuous degraded is not the flag failing; it is the first
honest reading this project has had of how far its published metrics trail its
tree. No refreshed coverage percentage is published in this snapshot,
deliberately: publishing a number that the project's own gate calls stale is
precisely the failure the repair was for.

## Cadence

Re-rankings, new threads, and dropped items land as ordinary diffs; the commit
log is the change record.

## How to verify

- The 10 focus areas + reading order + skip list are the whole syllabus; every
  upstream citation resolves to a public repository.
- The textual and numerical kernels are observable in the two published kernel
  repos above; the operational kernel's theorems are described here as they
  land.
- Every count in this file is emitted by the same build that checks the proofs;
  none of it is hand-maintained. Where a count is scoped — as the adversarial
  scoreboard is — the scope is stated next to it rather than left to inference.
- Where a figure is stale, this file says stale rather than restating it.
- Where two defensible bases exist for the same count, this file names both and
  says which one it publishes.

## Contact

[github.com/quantapix](https://github.com/quantapix) — open an issue on any repo
in the org. Answered in public; there is no contact email.
