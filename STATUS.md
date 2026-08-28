# qstudying-public — status

_Snapshot: 2026-08-28. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

Release-narrative status of the Lean4 focus-area syllabus. Companion to the
[README](./README.md), not a substitute.

## Overall
The scoreboard moved this cycle, after four cycles of not moving. Most of it
moved the wrong way, which is the point of publishing it.

Two figures this file withheld last cycle now exist, because the fold that
produces them was refused by two consecutive breached rounds and has since been
allowed to run. Both are worse than the numbers they replace. A third figure —
the certification badge — **fell for the first time**, from twenty of
twenty-two to eighteen. It fell because a newly-worked family took real
adversarial damage, and because the badge is scored cumulatively: a cell that
ever had something land is disqualified permanently, repair or no repair. The
rule that makes the badge conservative is the same rule that makes it fall.

One figure moved the right way, and it is the one that has been stuck longest:
the freshness flag behind the public card now reports current, so the card reads
healthy for the first time since late July, and the coverage fraction is
publishable again.

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

All three pin the same Lean toolchain (`leanprover/lean4:v4.33.0`, three-way
lockstep since the 2026-08-15 bump; bumps move all three and replay each
kernel's example proofs).

## Adversarial standing
Numbers as of this snapshot, all machine-checked by the build:

- **105** theorems proved across the operational targets; **77** on the
  domain-coverage numerator.
- **65** adversarial rounds; **357** attack probes thrown, **12** landed,
  **11** adjudication items open.
- **18 of 22** numerator cells re-certified under the closed adversarial
  contract, each holding zero landed attacks against a fresh, independent
  adversary.
- **12 of 22** domain concepts covered, all at the strongest tier, none refuted.

**State the population in the same sentence as the count, so: these figures are a
sum over the thirty-one on-roster cells that carry a standing record, each taken
at its own last-folded round rather than at a common cut-off.** A round whose
adjudication is still gated is in none of them. Four further families are held
off the roster until each can arrive whole — proved theorems *and* full
adversarial standing in one change — and they are excluded in **both** directions:
their theorem counts are absent here too. Read every figure over that population,
never as a statement about the whole tree.

One more stamp is owed and is given here rather than assumed: the open-item
figure is folded under two attribution rules at once, because the ruling that
unifies them is adopted but the mechanism that implements it has not shipped.
Earlier rounds label an item's attribution from the judge's verdict; later rounds
derive it. The two disagree materially where they overlap. **A cleared ruling is
not a shipped mechanism**, and a figure produced across the seam should say so.

Two of these figures rose sharply — landed attacks from two to twelve, open items
from zero to eleven. **That rise mixes two different things and this file will not
pretend otherwise.** Part of it is the population cure this file described last
cycle finally reaching the projection: the fold had been seeded from an optional
field, so any round record that omitted it was silently subtracted while staying
schema-valid. Part of it is genuine new damage from two rounds on a newly-worked
records-substrate family. The instrument cannot separate the two, so the honest
statement is the total plus the ambiguity, not an attribution.

The certification figure deserves the sharpest note, because it went down and
because a page like this one is exactly where a falling number gets quietly
dropped. It was twenty of twenty-two. It is eighteen. The two cells that left it
had been certified, then took landed and open attacks in this cycle's rounds. The
badge gates on the **cumulative** record by standing ruling, so they are out
permanently — and the reading that would have kept them in was put up, and
rejected, before any of this happened. **The rule was adopted when it cost
nothing and it is being honoured now that it costs two cells.** That is the only
interesting thing about a badge.

Last cycle this file published a fallback reading of the same data that would
have shown twenty-two of twenty-two. **That sentence is now false as well as
rejected** — the newer damage defeats it on its own terms — and it is removed
rather than carried forward with a caveat.

Coverage is still reported as coverage, not as a blanket falsifiability claim:
rounds predating the mid-July oracle-independence fix did not certify the
adversary's independence, which is why re-certification is a separate, slower
number. The first verdict issued under the stricter evidence contract landed this
cycle; the census behind it is stated two different ways in two different places,
so no count of it is published here until one basis is named.

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
  both halves of every prefix-anchored filter. **This one is now cured.** The
  filter's internal-declaration test had been doing two jobs at once — excluding
  compiler-generated auxiliaries, which is correct, and silently excluding
  author-written private declarations, which is a hole that fails in the
  direction that makes the audit look cleaner. The cure separates them by
  reading the user-facing name.

The first three defeat source-derived instruments. The fourth defeats
environment-derived ones. So "enumerate from the environment" is necessary and
**not** sufficient: the filter is the other half of the instrument, and both the
population and the filter have to be derived from the same place.

Two things about that cure are worth more than the cure. Its witness does not
reconstruct the mangling — it declares its own private probe, asks the
elaborator which constant it actually minted, and then requires the new filter
to admit that constant while the old one rejects it. A witness that rebuilds the
mangled name by hand is testing your string arithmetic, not your guard. And a
companion assertion requires the widening to be **monotone**, so a cure to a
filter cannot quietly narrow what the guard grades — the failure mode a widening
invites.

Measured at the moment of widening, the module had no private declarations left,
so the cure moved no number at all. The hole was real when it was found and
empty when it was closed. That is the normal shape of a fix that arrives late,
and it is not an argument that the hole was never there.

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

## A figure is a claim about a population, and it will be quoted without one
Three separate instances this cycle, in three layers, and they are the same
defect the guard-admissibility rules already name — restated because the rules
did not prevent any of them.

A published summary quoted a carrier count under a label that excludes, by its
own wording, part of the population the count was taken over. The underlying
artifact had carried the smaller, correct number throughout; the prose around it
carried the larger one. A section published a split of one module's declarations
that the tree does not measure, and its own enumeration listed one fewer name
than its own count.

The third is the one that reaches this file. **A fold step seeded a published
adversarial figure from an optional field.** A round record that omitted the
field was still schema-valid, so the fold silently dropped that round's findings
out of the figure and nothing anywhere reported a problem. Measured after the
fact: the published number was half the honest one, and the omission touched a
small but real fraction of the adversarial moves across several rounds. The cure
derives the value from a required field instead, with a sabotage-verified
witness.

The general rule, which is cheap and which this project keeps re-learning: **a
count is a claim about a population, so state the population in the same
sentence as the count, and derive it from a field the schema requires.** An
optional field is a silent subtraction waiting for the one emitter that forgets
it.

## The artifact under test is itself a channel — and it is not the worst one
The adversarial claim rests on the attacker not having seen prior adjudications.
The proof file under test carries a substantial history of prior rounds,
including verbatim convictions, and the adversary cannot test the artifact
without reading it. That is recorded as a known open channel rather than cured:
the history is the constructive side's legitimate rationale for why the proof is
shaped the way it is, and the cure would cost more than the leak.

A worse one was found this cycle, and it is worse for a structural reason. **A
standing instruction file is delivered to an automated participant in system
context — not read, delivered.** Every channel the independence instrument
grades is a *read*, and the transcript records no delivery, so the instrument's
silence on that channel is indistinguishable from absence. It was not silent
because the channel was clean; it was silent because it cannot see the channel
at all.

That file was carrying the example's identifier, a long run of prior-round
history, the adjudication figures, and the locations of the very documents the
same file forbids a blind participant's brief from naming — the rule defeated by
the file that states it. The payload is now split out to documents the
participant never receives, and the file points at that document set by
behaviour rather than by naming a location, which is the first time the
non-naming rule has been enforceable at all.

The transferable form: **an instrument that grades reads cannot grade a
delivery, and "no finding" from an instrument that cannot look is not a
finding.** Before trusting a clean channel report, enumerate the channels the
instrument is structurally incapable of observing, and check those by
construction instead.

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

Two further instances this cycle, and the second is the sharper.

A promotion step refused correctly on a breach and returned its own exit code —
the mechanism working as designed. Then a **test** of that same lane folded the
live records directory as a side effect, overwrote the committed standing file,
and in doing so moved the very figure the production gate was at that moment
refusing to move. The gate was not defeated by a caller who ignored it; it was
defeated by a test that never went through it.

The rule that follows is not "add another gate". It is that **a witness may not
invoke a production writer against production state** — it folds a copy, and it
carries an arm that reds if the live file changes at all. A gate that lives in
the writer is a gate the writer's callers can walk around; the gate has to live
where the invocation is made.

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

Both halves of that lesson are now mechanized rather than remembered. The census
is host-aware: it resolves each record against the machine the round actually
ran on instead of reporting absence-here as gone. Re-run over the previously
suspect rounds, it declares **zero losses** — every record that had been counted
missing was present on a peer machine, exactly as the earlier correction
predicted. The rule survives the cure and is the reason to keep it stated: a
declared loss should be read as *absent here* until a peer location has been
checked.

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

## A verdict that is a function of where the shell was standing
The lane's independence audit convicted a participant this cycle for reading a
file outside its own surface. The file does not exist.

The command was compliant and used exactly the form the brief prescribes: change
into the working directory, then reference a neighbouring file relatively.
Resolved against that directory, the target is an ordinary, specific, permitted
file. But the resolver was handed the working directory **recorded in the
transcript** — which is the orchestrator's own shell at the moment it spawned
the participant, not the participant's. The orchestrator had stepped into a
directory the participant is banned from, in order to read source claims before
spawning it, and that location was stamped into the participant's record.
Driving the real resolver on the recorded location reproduces the convicting
string byte for byte. Both participants' recorded locations were, in fact,
backwards with respect to their own surfaces.

So a conduct verdict was a function of where an unrelated shell happened to be
standing. This is a known class — an orchestrator-authored conviction — one
layer deeper than the previous instance and invisible where that one was
legible.

Three things follow, and only the first is about this defect:

- **The verdict stands and is not argued down.** The audit is not re-run in
  search of a different answer; that is the one response the lane forbids. The
  defect is filed, not applied.
- **The instrument is shared across all three kernels**, so the other two axes
  are exposed identically. A defect in a shared component is not scoped to the
  axis that found it.
- **A shared-component fix is regression-measured against every consuming axis's
  own test suite, never against the changing axis's production run.** Measured
  this cycle: one such change reds three assertions in a sibling axis's suite,
  while the check the changing axis ran instead came back clean — because its
  production run declares none of the state the change moved. The cheaper check
  is not merely weaker; it is systematically blind to exactly this class.

One more, recorded because it is unflattering. The intervention chosen for that
round — restating the ban in full, as the opening content of the brief, with the
compliant recipe attached — did not prevent the twelfth instance of the
behaviour it was written to prevent, and was not what convicted anyway.
**Briefing has now been measured as not being the lever.** That is a result
about instructions, not about the participant.

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

## A description cannot falsify its own currency
The freshness flag behind the public card has now been repaired twice, and the
second defect is better than the first.

The first version asked whether the snapshot's revision was an *ancestor* of the
current one — which it is, and stays, forever. It could report fresh and could
never report stale.

The replacement asked a question with a reachable failing branch: has anything
changed since the snapshot was taken? It counted changed files between the
snapshot's revision and now. **The snapshot's own outputs live inside the region
it was counting, and they are committed at a revision strictly later than the
content they describe.** So the first time a regenerated snapshot was committed,
every later reading reported drift forever — a permanent, self-inflicted stale
signal, from a check that was correct in shape and wrong in domain. A description
that includes itself in the population it measures cannot report on its own
currency.

The cure is structural rather than a carve-out: recompute a digest over the
kernel sources and the compiler pin, and compare it against the digest the
snapshot stamped when it was taken. A follow-up correction was needed on the
**coordinate** — the right root is the build package, not the surrounding
project. Read from one level up, the population grows by an order of magnitude,
sweeps in every debate artefact, and picks up the wrong one of two compiler pins,
so the recomputed digest can never match the stamped one except by accident.
**Asking a well-formed question at the wrong coordinate is still the wrong
question**, and it fails in the direction that looks like a real finding.

The producer and its test now run the same code rather than two copies of it.

With that in place the flag reports current, the card reads healthy for the first
time since 2026-07-25, and the coverage fraction above is published — the first
one this file has been willing to print since the flag started telling the truth.
The previous snapshot's drift count is **retired, not improved**: it was the
output of the defective instrument, and restating it smaller would have implied a
trend that the number could not carry.

## Three instruments wrong toward a false clean, on one round
The first independently-audited round on a new family found three lane
instruments broken, and all three broke in the same direction — toward reporting
clean.

- A configuration file stated that the judge inspects one directory and does not
  descend into a scratch directory. The judge runs a recursive search. The
  discrepancy was proof-fired at twenty-two versus twenty-three items. **The cost
  of an instrument that under-describes itself lands on the participant who obeys
  its contract**, not on the one who ignores it.
- The judge's inference of which round a receipt belongs to mis-keys the *first*
  audited round of any example that predates the auditor — which is nearly every
  example. One code path reads the missing predecessor as "re-judge", another as
  "fold with a warning", and nothing reconciled them.
- A closure collector walked a theorem's statement and not its proof, because the
  underlying accessor returns nothing for a theorem. It reported eight names
  where the truth was eleven — and the three it missed were exactly the committed
  facts it existed to audit.

The transferable half: an instrument that fails toward clean is not found by
running it. All three were found by a round that had a reason to distrust them.

## The brief was refuted by the participant it briefed
The round after that one produced the most useful result of the cycle, and it is
a result about the orchestrator.

The session briefed the constructive side that a certain statement had a
"stronger, indexed form". That claim had been inherited from the previous round,
carried forward through a tracking item, written into the brief, and **never
checked by anyone**. The adversary derived the indexed statement
character-for-character from the unindexed one, then generalised to an arbitrary
index and instantiated it at a trivial value. **The index was inert.** The
strengthening had never existed.

The rule minted from it, and adopted across all three kernels: **a claim that one
form is stronger than another needs a proof in both directions.** Pricing only
the expensive direction is what makes the wrong answer feel measured.

A second result from the same round is less flattering and more useful. A brief
convened *specifically* on a recurring over-claiming defect produced a
generated hundred-and-eighteen-member enumeration and fourteen demotions in
place — and still shipped three fresh false ordering sentences, an
anti-vacuity binder that was inert, and a warrant that miscounted its own
environment. **A per-item audit measures diligence, not control of the
generator.** The only thing that grades control of a generator is the *next*
artefact it produces.

So the following round was deliberately reversed: its findings were **not**
pre-fixed the way the previous round's were. Cleaning up before the next round
spares the constructive side the cost of its own over-claiming, which is
precisely the thing being measured.

## Five instruments that reported green over a failure
Cured in one pass, and worth publishing as a set because the shape repeats:

- A new test suite reported PASS over a failed assertion. Its helper library
  exports one name and the suite called another, and an undefined command in a
  `|| fail` idiom does not abort — it succeeds quietly. A proof-of-fire arm was
  added; the suite had never once been shown to be capable of failing.
- A test that stages a copy of the code it exercises was staging a hand-written
  file list that had already gone stale once. It now derives the list, with an
  arm that reds if the derivation returns nothing.
- An ordering check compared the *first* textual match anywhere in a file, so a
  comment counted as a call site — and the silent inverse is worse: a comment
  mentioning the guard early would make a genuinely mis-ordered lane pass.
- Another arm pinned a retired *mechanism* rather than the property the mechanism
  was there to hold, so it kept passing after the mechanism was replaced.
- A de-duplication of four routines copied across five callers looked safe by
  reading and was not. Three of the "identical" copies differed — raw versus
  stripped command output, two string-quoting conventions that agree until the
  first value containing a quote, and a strict lookup versus a defaulting one.
  All three are preserved as separate entry points and documented as numbered
  traps. **A dedup verified by reading would have shipped all three.**

## A guard authorized and deliberately unbuilt
A kernel of machine-checked guards was authorized this cycle by minting its
contract — and it is **unstarted**, on purpose, owned by no work item. It is
recorded here because the authorization is the interesting part: the argument for
it is a failure mode, not a feature. If the guard set is built as a kernel,
incoherence in it becomes a red build; if it is built as a set of scripts,
incoherence becomes a smaller pass. **A coverage number over a stale population is
indistinguishable from one over a complete population**, which is the whole reason
to pay for the kernel.

The contract's own test suite is written to go red the moment the guard kernel
appears. The first build session has to rewrite that case into a real check
rather than let it quietly stop grading anything — which is itself the failure
mode the suite exists to name.

---

Rest of file unchanged.

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
- Where a figure is stale, this file says stale rather than restating it. Where
  a figure is *convicted* — produced by an instrument since found defective —
  this file publishes nothing in its place and says why, rather than publishing
  the convicted value with a caveat attached.
- Where two defensible bases exist for the same count, this file names both and
  says which one it publishes.
- Every falsifiable claim on this page is re-derived against the working tree at
  each refresh, not carried forward because it was published last week. Four
  claims were corrected this cycle on that basis, each of them true when it was
  written.

## Contact
[github.com/quantapix](https://github.com/quantapix) — open an issue on any repo
in the org. Answered in public; there is no contact email.
