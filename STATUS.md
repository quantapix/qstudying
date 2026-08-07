# qstudying-public — status

_Snapshot: 2026-08-07. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall

**This cycle produced almost no proofs and a great deal of instrumentation, and
the reason is worth publishing: the project caught its own governing documents
mandating the leak that voids its evidence.** Three of the cycle's adversarial
rounds are void — not wrong, void: the Lean may be sound and it does not count.
The scoreboard below has not moved for two cycles running. What moved instead is
the set of gates that decide whether a number is allowed to move at all, and
several of them turned out to have been reporting green over sets they were never
checking.

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

Numbers as of this snapshot, all machine-checked by `lake build`:

- **98** theorems proved across the operational targets; **70** on the
  domain-coverage numerator.
- **63** adversarial rounds; **341** attack probes thrown, **2** landed, **0**
  open.
- **20 of 22** numerator cells re-certified under the closed adversarial
  contract, each holding zero landed attacks against a fresh, independent
  adversary.

**None of these numbers moved this cycle either, and the reason has changed.**
Last cycle the freeze was a deliberate hold-out: a cell joins the roster whole —
its proved theorems *and* its full adversarial standing, including the attacks
that landed against it — or not at all. That rule still holds. But this cycle
three of the five rounds that ran are **void as evidence**, and a void round
cannot contribute to any of these figures no matter how green its build is. The
off-roster cells carry seventeen open attacks and nine open gaps between them,
which is what an adversarial lane looks like while it is still running.

The 20/22 figure remains deliberately conservative by ruling: the badge gates on
the *cumulative* record, so a cell that ever had an attack land is disqualified
permanently, even after the repair. A latest-round-scoped reading of the same
data would show 22/22; it was rejected, not deferred. A cell that once had
something land has been shown to be repairable, not un-landable, and the public
figure should say the weaker of the two things.

Coverage is reported as coverage, not as a blanket falsifiability claim: rounds
predating the 2026-07-14 oracle-independence fix did not certify the adversary's
independence, which is why re-certification is a separate, slower number.

## The instruction layer is a channel too

The adversarial lane's entire claim rests on the attacker not having seen the
proof. Last cycle published the discovery that **the instrument can convict the
operator, not just the cell**: one round's own briefing material had told a blind
participant that a sibling's committed proof "showed the shape" of the probe it
should write. The stated cure was a rule about authoring — convey shape by
inlining the pattern or citing the shared template set, never by pointing a blind
participant at finished work.

**That cure was not enough, and the way it failed is the finding.** A later round
removed every brief-side cause and the channel stayed open, because the conflict
had been authored one level further up — in the *standing instructions* the
orchestrator itself runs under. The project's own governing document required a
read that the blindness contract forbade. Every rule in the participant's
contract constrained what the participant could *choose*; no rule constrained
what the instructions could *require*; and an instructed read arrives as an
instruction, not as a violation, so it is the one breach a participant cannot
self-police. Three rounds are void on that basis.

Two things changed as a result, and only the second is a mechanism:

- **State instrument requirements behaviourally.** Not "use tool X", but a
  description of what the tool must do and the failure mode it must avoid — for
  example, *audit axiom closures through a wrapper that checks the constant
  exists, because the bare library call returns an empty closure for a constant
  that does not exist.* That sentence carries the entire requirement and names
  nothing. This document is written under the same rule, which is why no
  instrument or module is named anywhere in it.
- **Grep your own instructions before you spawn.** A pre-spawn check now scans
  both the round's brief and the standing instructions for any repository path
  and blocks the launch on a hit. It deliberately cannot tell a prohibition from
  a direction, so a hit is a review obligation on the author rather than a
  verdict. Filing the first instance as a one-off anecdote about one channel is
  precisely why the second instance recurred on a different channel; the rule is
  now stated in its general form, with its dated instances under it.

## Gates that were green over sets they never checked

Four separate instruments failed the same way this cycle. Each was working
exactly as written. None of them was measuring what its name claimed.

**A soundness gate scoped by a roster audits what somebody remembered to add.**
The axiom-closure check ran over a hand-maintained list of kernel cells. Because
an incomplete proof is only a *warning* at the pinned release, an unsoundness
axiom sitting in an unlisted cell left the judge green on every arm — reproduced
by injecting one before repairing anything. The check now sweeps the whole kernel
tree, and an inconclusive attestation exit is red rather than skipped. Two
defects in the shared attester were fixed with it and apply to all three axes:
it now strips comments, so prose can no longer mint targets that do not exist;
and a directory scan that had been silently dropping files outside its declared
root now refuses loudly. **A silently smaller attested set is the one failure
mode a soundness gate cannot have.**

**A ban announced with a false claim.** The same area held one pattern
alternation that treated three unlike things as one: incomplete proofs,
trust-base extensions, and two of Lean's three *standard* axioms. The first two
are never declarable. The third is not unsound at all — classical choice and
quotient soundness are part of the standard library's own foundation. The old
form banned one of them, silently permitted the other, and announced the ban
with a statement that was not true. There are now three classes, with a
per-example declared opt-in for the standard pair. Two inherited escapes closed
with it: a decide-by-evaluation tactic mints a **fresh axiom name per
declaration**, so the match has to be a shape rule and no literal roster can
ever name it; and one trust-base axiom had simply never been listed. One gap is
published open rather than quietly carried: the probe is built from a keyword
scan for theorem declarations, so a proposition-valued definition is outside its
reach — reported, not reproduced.

**A per-example gate leaves closed examples unguarded by construction.** The
judge runs per example, so a *closed* cell is never judged again and its fixtures
stop being watched — which is how six attack fixtures sat non-elaborating for
days with every gate green. A whole-tree fixture sweep now answers one narrow
question, "does the fixture still build", and leaves adjudication where it
belongs. Three details in it are the transferable part: it reads breakage from
the **exit code**, never by grepping output for the word "error", which misreads
any fixture whose own report text contains it; it counts **verdicts, not output
lines**, because a sweep that reports success over a set it never checked is the
same vacuity class the sweep exists to catch; and a missing sweep is a failure
rather than a pass. It also only started biting when it was wired into the
aggregating battery that runs before a session closes — **shipping an instrument
does not run it, and an unrun instrument is the gap re-made.**

**A saturated channel carries no signal.** The battery that runs every family's
gate has stood at two dozen-odd reds for weeks. A new red entering a standing
roster of reds is invisible: a count gate needs a baseline diff, not a count.
Widening the same battery to cover the governance tree immediately exposed a
suite nobody had seated and a set of dangling references, both cured in the same
change — which is the argument for widening, and also the argument for the diff.

## Ordering is a gate, and prose cannot hold it

Three ordering defects landed in five weeks and all three are the same shape:
**a step that promotes results ran upstream of the oracle that can void them.**
The measurement was booked before adjudication; the standing fold ran before the
independence verdict; a booking receipt was keyed per example rather than per
round. Each was found, each was fixed correctly, and none of the fixes prevented
the next one — because the order is enforced by an operator following a document,
not by the tool. Two of the three cures are themselves still prose today. The
general rule is worth stating even where it is not yet mechanized: **nothing that
promotes may run before the oracle that can void it.**

The same family shows up as documentation rot. Scheduling and sequencing prose
rots faster than it is consulted: a calendar three weeks behind its own sibling
table in the same file, a backlog row describing shipped work as "next", a
retention figure carried in one place after its owning document had corrected it.
These are all *restatements* — the single-owner discipline the project applies
rigorously to code gates had not been applied to its own planning prose. A
pre-registered escalation clause that fired and that nobody checked is the same
class: an unread clause is a remembered rule, not a guard.

## An archive is evidence only once something checks it is there

The independence audit reads raw agent transcripts, and those decay within days,
so its verdicts are frozen alongside the round they judge and the raw evidence is
archived outside the repository in the same step. The retention window is the
reason the freeze has to be immediate: **an un-audited round is not "unchecked,
we will look later" — it is permanently unauditable**, and the only remedy is to
run the whole round again blind. "Audit the history later" is not a plan.

This cycle asked, for the first time mechanically, whether the archived evidence
was actually there.

**Seven frozen verdicts had nothing underneath them.** Every consumer read the
verdict files; nothing had ever looked beneath one. The evidence was unguarded by
construction, and the only guard was a rule a person had to recall at citation
time — which three committed records then broke, in both directions. An integrity
check now runs inside the fold that promotes standing. Its presence arm is
declarable: a declared loss prints a warning on **every** run, which is what
keeps it visible, and declaration is not absolution. Its subject arm — the
archive's participant roster must *equal* the verdict's participant set — is
never declarable, because a verdict computed over the wrong participants is null,
not lost.

**And then the census that called them lost turned out to be wrong, which is the
better half of the story.** Four of the seven were never lost. They were sitting
on a second machine's backup prefix, participant sets matching their verdicts
exactly, because those rounds had *run on the other machine*. The census had
enumerated by tree, on one host, and reported absence-here as gone. They were
pulled back, checksum-verified, and re-checked on both arms including the subject
arm; the declarations were then **retracted on verification rather than on a
filename**. The genuine permanent losses are three, all pre-dating the archiver —
and for two of them a later re-grade's evidence does exist, but it backs the
re-grade, not the frozen base verdict, and the checker is right to refuse it as a
substitute.

Two rules came out of that:

- **A gate has a coordinate system, not a target set.** "Scope by tree" fixes
  membership and leaves host, instrument version and corpus vintage silently
  partial. Fixing one axis of a scope is not fixing the scope.
- **Mechanizing a rule fixes who has to remember it, not whether the question was
  scoped right.** A declared loss should be read as *absent on this host* until a
  peer location has been checked, and a loss claim should never be hardened
  without one. Declarations warn rather than mute precisely so that a wrong one
  stays recoverable.

The archive itself still never enters the repository. A committed transcript is,
by construction, an answer key — exactly the material a future adversary must not
have seen — so committing it would poison the next round's independence while
looking like good record-keeping. Round records stay committed and append-only;
the archive holds evidence only.

## The cost of the trust

One finding from a month-long review of the lane's own daily operation belongs
here because it is unflattering and load-bearing: **the fixed per-round overhead
now exceeds the proving work.** A round is roughly a dozen orchestrator-sequenced
steps, and nearly every one of them is scar tissue from a specific dated defect,
each individually justified. The composition is the problem. The month's honest
summary is peak-to-close deflation on the headline numbers and a strong net gain
in how much those numbers can be trusted — deliberate, ruled, and correct — with
a cost curve for maintaining that trust that is currently super-linear. The
review was itself run adversarially, and forfeits fired against its own draft's
figures before it was adopted.

## A polarity trap worth publishing

One recurring mechanical trap, because it will recur anywhere a test asserts an
absence. A gap probe asserts that a diagnostic *is* produced. Close the gap and
the probe compiles clean — so the attack lane reads clean-on-annotated as
*landed*, and **the round that fixes a named gap reds itself.** There is no
"discharged" verdict to reach for. The resolution is to retire the probe into a
standing discharge record: drop the directive, keep the measurement.

## Build freshness

The metrics snapshot behind the public status card was taken on 2026-07-25.
Nearly two hundred files under the kernel tree have changed since, so the
freshness flag reports stale and the public card reads **degraded** rather than
green. That flag is the one repaired last cycle, after the previous version asked
whether the snapshot's revision was an *ancestor* of the current one — which it
is, and stays, forever, so it could report fresh but could never report stale.
Two weeks of continuous degraded is not the flag failing; it is the first honest
reading the project has had of how far its published metrics trail its tree. No
refreshed coverage percentage is published in this snapshot, deliberately:
publishing a number that the project's own gate calls stale is precisely the
failure the repair was for.

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
- Where a figure is stale, this file says stale rather than restating it.

## Contact

[github.com/quantapix](https://github.com/quantapix) — open an issue on any repo
in the org. Answered in public; there is no contact email.
