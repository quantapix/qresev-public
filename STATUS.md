# qresev-public — status

_Snapshot: 2026-10-04. The previous snapshot was 2026-08-07. This page changes
only when a financial sign-off clears the new text, so it can lag the weekly
refresh the other repositories in the organisation get; between those two dates
it did, and several statements here went stale. They are corrected below and
dated where the date matters._

This is the release-narrative status of the financial-domain slice: what
has landed, where each of the three surfaces stands, and what is next. It
is a companion to the [README](./README.md), not a substitute.

Most of what follows is a defect report. That is deliberate. This window's
useful output was almost entirely negative results — gates that were green and
wrong, counts that could not be re-derived, a certification frozen as a breach,
and two diagnoses withdrawn after being re-checked against the code rather than
re-read from the note that asserted them. Those are the findings worth
publishing; the parts that merely worked are one line each.

## Correction — 2026-07-14, the re-verification's answer, and what was found after it

On 2026-07-14 our own adversarial review lane found leak channels in the
cross-axis **agreement oracle** — the mechanism that scores the top-tier
promotions this program reports. Independent verification cells could, in
principle, observe one another's outputs, so agreement-based tier figures
minted before that date are not established as blind.

The correction stands, and it now has an outcome:

- The **"seven top-tier names"** figure and the "re-tiered up on a fresh blind
  re-slice" claim remain **withdrawn**. The public status board shows the tier
  as *withheld* and states the withdrawal, not a number.
- **A re-slice under the corrected contracts completed on 2026-07-15 and did
  not re-establish those figures.** Of the names still eligible on that tape,
  it reproduced **no top-tier result**. Two further re-slices under the
  corrected fan-out contracts, on 2026-07-19 and 2026-07-21, each landed below
  the top tier as well. The count of top-tier names re-earned under corrected
  contracts is **zero**, and it was still zero on 2026-10-04.
- **Two things an earlier version of this page said about those re-slices were
  wrong, and a third needs a caveat.** It called the first one "the cleanest
  possible non-promotion, with zero bear vetoes". That is false: on both names
  the limiting factor was a fired momentum-overextension disqualifier. It
  called all three *blind*. The first was later graded a breach on mechanism,
  and a channel found in September was open when all three ran: the briefings
  every cell is required to read named the reference judgments. A required
  input is graded by no blindness check, so none of the three is called blind
  here any longer. The direction matters and is the caveat: a leak toward the
  reference answers can only inflate agreement, and none of the three reached
  it. The negative result stands; the adjective does not.
- **The list of known leak channels has kept growing**, not because new
  breaches keep happening but because more *ways to leak* keep being
  identified and written into the contracts. This page once reported the count
  going from four to six — a shared kernel namespace, and a reconciler's access
  to prior adjudications. More have been named since, the latest on 2026-09-26,
  and at least one has no instrument yet and is recorded as ungraded. The count
  is left out on purpose: it moves. Bounding the channel ("check a name, don't
  list the directory") turned out not to bound the *breadth* — a cell may
  check only names its own brief hands it, and may not enumerate, roster, glob
  or walk the environment by any mechanism. Publishing a rising count of known
  channels is more honest than publishing a stable one; the number tracks how
  hard we have looked, not how safe the mechanism is.
- **We re-labelled our own withdrawal, because the old label was itself
  slightly dishonest.** It read "withheld — agreement oracle under
  re-verification". That told readers a question was open which had in fact
  been answered three weeks earlier. Carrying "under re-verification" past the
  answer reads as caution while overstating what is unsettled. The label now
  states the negative finding directly. Two phrasings were drafted and
  rejected: "re-verification complete" (on a status page, "complete" scans as
  "passed" — the exact inverse of the finding) and an unqualified "reproduced
  no top-tier result" (which sounds more conclusive while over-claiming a sweep
  that did not happen — only two names were still eligible on that tape).
  Under-claiming the scope of a negative finding is the right direction of
  error here.
- **The corrected number is still not on the board, for a different reason
  than before.** The reason this page used to give — the internal rollup could
  not represent *encoded and sound with agreement tier withdrawn* — stopped
  being true on 2026-08-21: the rollup now records the withdrawal and derives
  zero blind-agreement top-tier names. The numeric figure was then re-scanned
  for the financial sign-off on 2026-10-04 and refused as a set, because the
  counts below the top tier still include rows minted before the correction.
  The board keeps the withdrawal string.
- **Unaffected: kernel soundness and the encoded census.** As of the last
  recorded build (2026-09-22) and axiom-closure check (2026-09-29), the kernel
  libraries build and no committed encoding depends on `sorry`. How many symbols are
  encoded is a mechanical count, not an agreement figure. The
  accepting-and-refusing gate behaviour and the defined-risk options refusal
  are kernel facts — they stand.

We surfaced this because the cells check one another and one lane caught it —
not because the earlier numbers were right all along.

## Overall

**Pre-beta — endpoint deployed, beta not declared, and no date is set for it.**
The hosted evaluator at `qresev.quantapix.com` is reachable, and its API answers
on a separate host. The API serves the committed report for each of three worked
examples and will replay one through the kernel on request.

The app page does not show that yet, and an earlier version of this paragraph
said it did. The page's stream panel renders a fixed illustrative trace; it is
not connected to the API's event stream. Its report panel asks the page's own
host for the report, and the API lives on a different one, so on the hosted page
that request fails. The per-run pages are built from the committed results and
do render them. An earlier dated beta target passed unmet and has been
withdrawn; none replaces it. Until a beta is declared, the deployment is an
engineering surface, not the launch deliverable.

## Formal evaluator (`accounting/`)

- **Eight judgment frameworks kernel-green, plus two program frameworks.**
  The five golden frameworks (Trend, Momentum, OptionsRisk, Sector, Drawdown)
  are joined by Regime, ExitDiscipline and Hedging under a chartered expansion;
  the bull-vs-bear gate and the new factor decomposition are program
  frameworks rather than portfolio judgments. All elaborated at the last
  recorded build, and the spec battery was last recorded green on 2026-10-01.
- **A framework with no model in the loop.** The factor decomposition is the
  first kernel judgment whose facts are lifted deterministically from a fit
  report rather than decided by a model. The kernel checks the exact
  quantize-then-residualize identity as integer arithmetic and refuses on a
  length or day-count mismatch; the emitter contains no model call
  and fails closed if the report's content hash is not the one it was told to
  read. Method detail in the [README](./README.md).
- **Hedging: the whole chartered predicate roster is in the kernel as of
  2026-08-22.** Both top-level judgments elaborate as whole structures with no
  omittable field. The last three predicates had been recorded as blocked
  rather than stubbed — a predicate that cannot yet read its evidence does not
  get a placeholder — and landed once the fills ledger they read existed. They
  abstain on a position that carries no ledger facts.
- **The evidence tape is pinned into every run, and now pinned across
  machines.** Three gates sit between the kernel program and its market-data
  ground truth: a **corpus pin** (each archived run records the content hash of
  the exact tape it read), a scoped **trust gate** (a run refuses to start if a
  symbol *in its own scope* carries an unmodelled restatement, an unreconciled
  refusal or a frozen series), and a **coherence gate** (if the tape's hash
  changed *during* a run, its cells measured different worlds and its agreement
  is void — the remedy is a re-run against a pinned tape, **never** a re-score).
  A committed **tape-of-record pin** was added this window: it names the hash a
  checkout is supposed to hold, so a run on a lagging machine aborts rather
  than proving something against older data.

  The coherence check is deliberately not a self-report. Asking each cell which
  tape it read makes the check exactly as reliable as the cell's compliance;
  instead the single data-access boundary stamps every read, and the archiver
  asks whether that set of stamps is split. A cell cannot forget to be honest
  about something it never touches.
- **A wave ran, cleared the gates that could look, and was not promoted.** The
  2026-07-31 multi-sector wave covered four sectors, every cell elaborated, and
  the coherence, oracle-guard, parity and theorem-attestation gates passed. One
  further gate recorded as a pass at the time has since been restated as
  *nothing to check*: it ran over an empty population. Its **blindness
  certification came back a breach verdict, and was frozen as a breach** — not
  downgraded.

  An earlier version of this bullet added that the findings measured zero
  information disclosure. That was the reading on the channels the auditor
  graded then: the one shared kernel file that was touched was touched only by
  the licensed cross-sector reconciler whose contract requires it. The zero
  does not survive. A later re-grade convicted more cells on mechanism, and two
  channels found afterwards — context injected into every cell, and the
  required briefings — were never graded for this wave; blind-cell outputs
  from it do name the reference judgments those briefings carried. The wave was
  never promoted, its archive and evidence bundle are retained, and nothing in
  the encoded census came from it.

  The order of operations is the point. The verdict is frozen *before* anyone
  reacts to the auditor's non-zero exit, because an unfrozen breach ages off
  with the transcripts it rests on — that is precisely how one earlier wave
  became permanently un-certifiable. A certification adjusted after the fact
  would be evidence of nothing.
- **Five defects hit during that wave, and a sixth report first recorded as a
  misdiagnosis turned out to be a defect too.** The list is as of mid-August
  2026 and more were found on the same wave afterwards. Publishing it because
  each generalises:
  1. An archive step ran the trust gate **unscoped**, so one permanently-stale
     symbol that was *outside the run's scope* aborted every archive — and
     reported the failure under the wrong gate's banner while that gate printed
     ok. Fixed by scoping the gate, not by disabling it.
  2. A re-archive **destroyed the evidence subtree** — the one child not
     rebuildable from the sandbox, since raw transcripts age off in a day or
     two — while still printing "archived". A certified wave silently became
     un-certifiable along the sanctioned certification path.
  3. The durable append to the coverage store was skipped under the wrong
     Python interpreter. The store is the source of truth and the flat file its
     projection, so a skipped append drops the wave from any future re-render.
  4. A scoring flag was unreachable for an unpromoted sector and, worse,
     **silently scored the previous wave's result** for an already-promoted one
     — a laundered pass on exactly the answer a re-earn wave exists to
     re-derive.
  5. Build-configuration comments carried **prior tier verdicts, the encoded
     symbol, the directional call and fired flags with values** — a surface
     every reconciler must read. Three reconcilers hit it and self-disclosed;
     all three then diverged *downward*, which is why the wave was kept as
     evidence rather than voided. Scrubbed to ordinal, date,
     sector and closure shape; waves are now cited by date, because three
     historical run names encoded the verdict in the name itself.

  The sixth: four cells believed a data-reading flag narrowed their view of
  the tape. It was additive, and this page recorded their report as a
  misdiagnosis so that nobody would "fix" working code. That was reversed on
  2026-08-22. A flag that only widens exposes other signal families' columns to
  every single-axis cell, and it now narrows as well.
- **Ways a green build proves nothing: two were known in early August, and at
  least three more had been found by late September.** Each has a gate or a
  preflight now; none is described here as closed for good, because the earlier
  version of this bullet said "both now closed" two days before the third was
  found. A Lean option
  left on would silently bind an unqualified type as a phantom implicit
  variable: the module builds green while its axioms range over an invented
  type, and a bridge between two phantom-typed composites **discharges
  vacuously** — so anything resting on it measured nothing. That option stays
  off, permanently. The second hole is structural: an **unrooted file is never
  elaborated at all**, and an incomplete-proof marker is a warning rather than
  an error, so both leave the build "completed successfully". A gate scoped to a
  hand-maintained roster cannot reach an unrooted file by construction; the
  scan is now tree-derived, and a witness proof-of-fires the width by planting
  an incomplete proof in an unimported module and confirming the roster-scoped
  arm passes it while the tree arm rejects it. The later three: the same
  unrooted hole in the worked-examples library, where fewer than half the
  example proofs had been rooted; a second vacuity mechanism that no compiler
  option prevents; and an attestation step that could read stale build output
  on a tree that no longer built.
- **A guard that cannot be observed failing is not a guard.** A new guard's
  test passed green against a guard that never ran — the assertion matched text
  the tool emits on *any* misuse, so it proved nothing. The rule now is to
  assert on a string only the code under test can emit, and to drive the tool
  with a complete invocation. Two related findings this window: a hermetic-test
  seat was accepted only after a **polarity witness** was added, because a
  suite that only ever checks "the sentinel is armed" is green whether the
  sentinel works or the client underneath is simply dead; and arming an
  aggregator does not arm its members — a rostered battery sat unguarded when
  invoked directly, and another suite sat in no aggregated run at all for a
  week. The aggregator's roster is now derived from the tree, with one explicit
  exception.
- **Lost evidence is declared, and a declaration is not a shrug.** Archived
  waves are paired against the durable coverage history, restricted to rows
  that actually adjudicated something — pairing over *every* row would red every
  correct decision-not-to-run, and checking the inverse direction would mint
  false losses for every archive predating the ledger. Three dispositions, not
  two: undeclared gaps are errors, unrecoverable ones are declared losses, and
  archives **present on the other workstation** are a third class of their own.
  Filing a recoverable archive as lost permanently mutes the check for that
  run, which is worse than not looking; and a certification naming no agents is
  reported *uncheckable*, never passed over.
- **A window of audit verdicts is marked untrusted rather than re-asserted.**
  The shared transcript auditor gained a subject-scoping rule that this axis had
  not registered, and the unregistered state failed **open** — so a run of
  verdicts recorded between 2026-08-02 and 2026-08-04 demoted nothing and are
  carried as untrusted rather than counted. The reason the rule cannot be a
  lexical guess is worth stating: in the legal-domain sibling the subject marker
  is unambiguous, and a ticker is not; an all-caps pattern convicts the
  program's own capitalised emphasis. The subject is now supplied as data — a
  run's symbol roster — so any hit inside the window re-convicts whatever the
  phrasing. A recorded verdict never updates itself, so the standing obligation
  after any change to the grader is to re-run the whole invocation rather than
  trust the verdict on file, and a verdict that moves is superseded **in the
  open**, with the prior certificate retained beside the new one.
- **Refusing to run is a reported outcome, and since 2026-08-22 it is no
  longer the end of the session.** The deterministic pre-filter that targets a
  run — pure indicator and relative-strength arithmetic, no model calls — still
  usually returns zero top-tier-eligible candidates; the latest recorded read,
  on 2026-09-30, did. Those reads are first-class rows in the program's history
  rather than prose. What changed is what follows one: a zero read now routes
  to a wave that tries to *refute* a candidate, or to a depth increment on an
  existing framework, and several such waves have run. The earlier version of
  this bullet said no run was launched and no model spend incurred, which was
  true when written and is not now. The tape-staleness alarm it described as
  dark has been firing on its host since 2026-08-18.

## Axiomatize-trading program

- **Encoded census, as of 2026-10-04: 11 symbols across five sectors** (health
  care joined on 2026-08-22), each encoded in a sector module that is an
  explicit build root, none depending on `sorry` at the last recorded check,
  with the golden reference untouched. Counting only committed universe
  encodings (the golden calibration portfolios are scaffolding, not universe
  members), that is about **2%** of the ~526-symbol GICS universe. This is a
  mechanical census, unaffected by the agreement correction above and unchanged
  by the unpromoted wave.
- **Tier counts: withheld.** See the correction. Where tiers are reported at
  all, they are derived from per-symbol enumeration and never carried as a
  standalone number.
- **Three re-slices under the corrected contracts, none promoting.** Each ran
  seven independent signal-family cells and a per-sector reconciliation; the
  two later ones also ran the cross-sector pass and the admissibility gate. The
  first was held below the top tier by a fired momentum disqualifier on each
  name. The two later, single-sector ones were held there because only two
  independent directional axes were confirmed at that scope — the cross-section
  axis stays provisional until a cross-sector pass — and fired no bear
  disqualifier. In the two where the gate ran, it **refuse-closed** with no
  incomplete proof: the admissibility structure is unconstructable without the
  top-tier confluence claim. One of the names is the program's original
  *accepting* exemplar, and it now refuses. A re-verification that could only
  ever confirm the prior answer would be measuring nothing; this one did not
  confirm it. See the correction above for why these are no longer called
  blind.
- **The oracle caught a real defect in-flight.** In one re-slice, a
  reconciliation build-guard caught a corroborator cell reporting success on a
  claim that had discharged *vacuously*. The cell was tier-neutral, so the
  verdict did not move — but the guard, not a human review, is what found it.
- **Two genuine mechanism violations were found on the 2026-07-30
  re-adjudication, and the earlier convictions were withdrawn.** A later
  re-grade of another wave convicted more cells on the same mechanism class. Re-running the corrected grader over
  older waves moved verdicts in both directions. A batch of earlier
  "convictions" **evaporated**: they had graded the tool rather than the
  content, and that wave downgrades wholesale to review. Two real blind-cell
  violations surfaced in a July re-slice — both cells staged their mandated
  shared-library copies by **wildcard glob**, which is enumeration "by any
  mechanism", the exact barred conduct — with incremental disclosure measured at
  **zero**, because the globbed directories held only the files the contract
  named. The violation is therefore the *mechanism*, and that is what the next
  wave's fence has to catch. One older wave's adjudication can never be
  re-graded at all: its evidence aged off, so its frozen finding is permanent
  and bounded, and anything resting on it requires a fresh blind wave rather
  than a re-score.
- **Sharing across axes is not being measured at present.** A shared predicate
  library backs the per-sector slices, and it has not grown since June 2026.
  Recent waves are single-sector and skip the cross-sector pass, so no
  consolidation figure is reported for them; the earlier claim here that each
  new run reports one was not true of those waves.

Full-universe scale-out remains gated on a separate programmatic lane, which is
parked with no planned resumption date.

## Market inspection (`analyzing/`)

- **The local inspection surface is feature-complete for its first workflow
  set,** and carries optional factor-decomposition overlays at the symbol,
  cohort and ribbon scopes. It runs on loopback only, is absent from
  every deploy lane, and renders **ground-truth tape only** — no predicate
  verdict, framework judgment, or proof node appears in an inspection panel.
  Anything exported off it becomes a financial-clearance subject.
- **Honesty rules are mechanical, not stylistic.** A correlation pair with
  fewer overlapping returns than the window requires is reported as undecidable
  rather than as a short-window number. Symbols with too few bars for a cohort
  are listed as excluded rather than silently composited. Books render
  separately and are never pooled. A book's beta reference lines render as
  **values only** — there is no in-band / out-of-band verdict word, because the
  viewer carries no entry binding that word to a kernel predicate, and until it
  does the surface shows the numbers and nothing else. And no
  fit-quality or confidence field crosses the overlay seam at all: a factor
  contribution is shown, the model's opinion of itself is not.
- **Restatement classification is compositional.** A vendor refresh that
  changes historical values is decomposed into world-fact components —
  split-like, dividend-like, volume-settlement, frontier-settlement, and
  residual — instead of a single opinion word. The fourth was added on
  2026-08-25, and the earlier version of this bullet is why it is worth
  stating: it reported zero unmodelled residuals and concluded that no new
  class needed naming. After the refresh moved to run after the close, most of
  each run's restatements were unmodelled — the vendor finalises a session's
  open, high and low by the next day — until that class was named by ruling.
  Since then residuals are a handful per run, not zero, and each is read as a
  finding. A refinement: rescale uniformity is judged over a
  column's *changed* bars only. A corporate action rescales bars before its
  effective date, and mixing the unchanged tail into the ratio set had been
  mis-classing routine dividends as irregular.
- **We measured whether a restatement class matters, instead of adding a
  warning because it sounded like it might.** Several hundred restatements were
  replayed through the affected indicator from their pre-write backups.
  Dividend-like restatements are **exactly invariant** for it (they rewrite the
  adjusted close; the indicator reads the raw close), and volume-settlement
  restatements shift it by a **constant** from the restated bar forward, so
  every increment, slope and divergence read after that bar is unchanged — with
  no sign flips across the universe. **No warning tier is warranted for either
  class.** The bound is stated with the measurement: it does *not* cover a
  level read whose window spans the restated bar, nor repeated settlements
  landing on the same bar. A measurement that does not name what it failed to
  cover is an assertion wearing a number.
- **History and membership are both monotone.** A refresh may add bars and may
  restate them, but may never reduce the set of dates held for a symbol. How
  that is enforced changed on 2026-08-18. A small vendor drop is now absorbed by
  a union write: the bars we hold survive, the vendor's new bars land, and the
  event is recorded at warning level. A larger loss is still refused and the
  file left byte-identical, so a downstream program never rebases on a truncated
  series. The scheduled run exits non-zero only when a refusal is new or has
  changed.

  The refused set is not fixed, and the earlier version of this bullet counted
  it as one symbol and then two. It peaked at roughly a fifth of the universe in
  mid-August 2026 — mostly an artifact of fetching before the session had
  settled, cured by moving the run after the close and by the union write — and
  stood at a handful on 2026-10-03. One symbol has not advanced since July 2026:
  first refused, later simply unanswered by the vendor, its file unchanged, and
  carried as a standing expected entry. A second, refused in early August, was
  retired in place through the universe lane a few days later once the end of
  its listing was confirmed; the held bars stay on disk and nothing was
  rewritten. Retirement-in-place is the correct answer when a listing ends,
  never a rewrite: splicing a pre-action series onto
  a post-action security fabricates price continuity and poisons every indicator
  computed over it. Accepting a genuine reset is per-symbol and on the record,
  or the tape does not change. Universe membership is monotone too, and the
  freshness manifest's drop detection was widened by exactly the
  retired-and-still-held set — a per-symbol intersection, never a tolerance — so
  a retirement and a genuine silent drop in the same refresh do not cancel each
  other out.
- **The automatic alarm for that refusal shipped on 2026-08-09, in redesigned
  form, because the obvious version could not work.** The original
  requirement was to surface the refusal exit code as a routine failure. That
  alarm would have shipped red on its first fire and every fire after, because a
  standing refusal *is* the steady state of this lane — and a channel that is
  always red cannot report the new regression the alarm exists to catch. The arm
  fires on a **diff against a recorded baseline** of expected-blocked symbols: a
  symbol newly entering the refused set, or a refused symbol whose signature
  changes. The early-August refusal above is exactly the event that design
  exists to catch; it predates the alarm and was caught by a person. Since the
  alarm shipped, a newly refused symbol makes the run exit non-zero once and
  then joins the baseline.
- **A staleness gate was reading the wrong timestamp, on both sides.** A
  consumer-side freshness check compared against a field that a manifest
  regeneration could advance without any new data, which meant the cure for a
  staleness alarm was to regenerate the manifest — silently dropping the
  non-clean list the gate depended on and trading a false abort for a vacuous
  gate. Both halves are fixed: the check reads the build timestamp and
  falls back aloud for older manifests, an absent basis is a loud
  *unchecked* rather than a pass, and an unknown one aborts as
  couldn't-look. A regeneration that loses its basis now marks the non-clean
  list unknown rather than leaving it silently empty. The standing rule that
  survives: **never regenerate the
  manifest to clear a staleness alarm.**
- **Coverage status** surfaces through the framework's `/status` board —
  symbol count, indicator-reference count, GICS-mapping presence, and
  newest-bar freshness.

**Next:** the first live end-of-day options-chain snapshot and its place in
the schedule, and a watch on two vendor-side series that have stopped
answering.

## Portfolio management (`trading/`)

**The scheduled trading routines have been switched off, by operator
decision, since 2026-09-20.** Nothing below is running on a schedule today. The mechanisms described stay in place for a re-arm, and the incidents
are reported as what they were.

**In early August 2026 two coverage gates were green and wrong at the same
time, and one of them caused real damage on a paper book.** Both are worth publishing in
full, because between them they cover the two ways a safety check fails.

- **A gate asserted a breach it could not see.** A bracket order's protective
  stop leg hangs off its parent, and the parent is terminal once the entry
  fills — so fetching only *open* orders omits the parent and its live stop,
  and **every bracket-protected position reads fully naked.** The coverage
  check had no way to know how its caller had fetched, so it asserted a breach
  on a payload that structurally could not have shown the stop. The fetch mode
  is now a required input, only the complete fetch can yield a breach, and
  anything narrower degrades a naked reading to *unknown* rather than to a
  finding. Seeing a stop is still positive evidence under any filter, so
  covered positions stay covered. **Coverage is a claim about the broker, not
  about the ticket** — what the agent intended is recorded on the position and
  is not the same question.
- **And its remedy was harmful.** The breach text said to submit the missing
  stop immediately, before any other work. On 2026-08-05 a midday routine did
  exactly that. The stops had been resting the whole time; the duplicate
  submission cancelled them through the broker's one-cancels-other linkage,
  **permanently destroyed two take-profit legs, and opened the only naked
  window that book had had to that date** — about twenty-six seconds. The remedy text
  now names the hazard and says to stop and re-verify on a broker refusal;
  improvising around a refusal is the route the damage actually took. A false
  alarm whose prescribed response is destructive is worse than no alarm.
- **A book-level gate cleared a naked call, and had done so since it landed.**
  This one was found while auditing state, not by a test. The check summed
  existing short-call quantities *raw*, and the broker reports a short position
  with a negative quantity — so on a real book holding 22 shares and one short
  call, a proposed second call against those shares computed a required-share
  count of **zero** and was allowed. The check whose entire reason for existing
  is that leg-wise validation cannot see a second call written against
  already-committed shares was **failing open.** Counts are magnitudes now and
  the same ticket is refused. Fixing the sign alone would have over-demanded on
  the other real shape on that book — a short call that is the short leg of a
  debit spread, covered by its long leg rather than by shares — so a short call
  is exempted from the share requirement only when its leg-pairing identifier
  matches a long call of the same underlying, and one long leg cannot be
  credited twice. Unpaired legacy short calls stay share-charged: inferring a
  spread from "same underlying, opposite sides" would **release** a coverage
  requirement on a guess, which is the one direction the rule forbids. Four
  regression tests, one written against the live book's exact shape.
- **The schema for all this landed on 2026-08-07 and the wiring followed on
  2026-08-18.** A leg-pairing identifier minted at ticket time, a signed net
  premium per option leg, and a per-book append-only premium ledger are in
  place, and execution now records every option fill as a ledger row and stamps
  the identifier onto the positions it opens. The earlier version of this
  bullet said nothing wrote the ledger on a real fill; that stopped being true
  eleven days after it was written. One book's ledger has real rows; the other
  two have had no option fills. No share-covered short call has been written by
  any book, so the collar path is still unexercised on live data.

  The backfill question was ruled on 2026-08-09: positions that predate the
  identifier stay null and the predicates abstain — never "infer", which is the
  reconstruction the identifier exists to prevent. One such position was
  nevertheless stamped retroactively by a routine a month later, shortly before
  it closed. That is recorded here as a departure from the ruling, not as an
  attested pairing. Nothing yet reconciles the ledger against open positions
  either: in September a close whose rows were not written left a stale open
  short leg on the ledger's projection for about twenty hours before
  compensating rows corrected it.

  The ledger is also **never pruned at position close** (a pruned ledger serves
  everything and then fails once, about a year in, when the first year-long
  window needs a closed position) and a malformed row makes the reader **raise
  rather than skip** — a skipped row lowers a spend figure, and a spend quietly
  too low clears a budget that was breached.
- **Refuse rather than fabricate.** The options-math helper raises at or past
  expiry instead of returning zero-filled Greeks, and failure text goes to
  standard error with standard output left empty, so a caller parsing output can
  never mistake an error message for a result.
- **Scheduled routines are dependency chains, not independent fires.** A day's
  routines hand state to each other through tracked files, so a chain must run
  start-to-finish on one machine. That was never modelled, and a mid-morning
  handover between workstations split a day across two of them: every gate
  returned the correct verdict, and the day's records still ended up
  contradictory, with one pre-committed ticket never placed. The fix is a third
  invariant alongside exclusivity and capability — **continuity**: a tenure may
  only begin inside a fixed evening changeover hour, when no chain is in
  flight, and the incoming machine must fast-forward to the authoritative branch
  or refuse outright. A missed run is recoverable; a double run corrupts the
  book. We take the gap and make it loud. A same-machine half was added this
  window: while an interactive session holds a project's write-lock, every
  scheduled fire of that project on that machine refuses and spends nothing,
  which also means a session
  left open across a trading day **darkens that day's chain** — a constraint to
  design the cutover around, not to discover at the open.
- **Probes must import, not ask the packager.** A pre-flight check returned
  green on twenty-four of twenty-four capabilities on a machine that could not
  compute a single option Greek: a numerics library's metadata directory was
  present while its package directory was gone, so the package manager reported
  it installed and a reinstall was a silent no-op. A capability map that asks
  the installer what is installed measures the installer.
- **A process cannot diagnose its own run.** The scheduling wrapper writes a
  run's exit code and cost *after* the run returns, so a routine inspecting its
  own log always sees it truncated and can only conclude it crashed. One did,
  reported a phantom failure, and the false report propagated into a committed
  record before it was caught. The absence of a trailing line is structural, not
  evidence. Check a sibling fire instead — and note that "declined, not the
  current seat holder" means declined *on this machine*, not "did not run
  anywhere".
- **A missing narrative on a session day is a finding, not a no-op.** The
  cost-accounting step used to return "nothing written" with a success exit,
  which reads as success. One evening a closing routine died mid-run after real
  spend, leaving no closing section and no summary; the next morning caught the
  missing valuation mark and backfilled it, but nothing noticed the missing
  narrative — and the last step of the day, the one holding the cost figure in
  its hand, declined to write and reported no problem. It now refuses loudly —
  when it runs. The refusal sits inside the chain it grades, so days on which
  the chain itself went dark were again unrecorded for a week, and a later
  audit found sixteen gaps no detector had reported. A schedule-derived
  completeness check has since been added; a detector that lives outside the
  chain is still the open part. The cost itself is never lost; it is reconstructible from the logs. **The
  narrative is deliberately not backfilled**: a closing section written days
  later is not a contemporaneous record, and manufacturing one is the same
  failure class the session-calendar guard exists to prevent. Record the gap.
- **Two of our own tracked diagnoses were withdrawn in the summer of 2026 after being
  re-derived against the code.** One asserted a blocker that had already been
  cleared and a dependency count that the graph tool did not produce; another
  had quoted a machine-readable clause verbatim inside prose to narrate that the
  clause was *gone*, and the parser does not distinguish a quoted clause from a
  live one — so the tooling showed a phantom dependency on an item whose own
  text said the dependency was released. Both are recorded as corrections rather
  than quietly rewritten. Re-reading the note that makes a claim is not
  re-verifying the claim.

**Next:** reconcile the premium ledger against open positions, and give each
routine a named artifact contract so a run that exits cleanly having written
nothing is detected. Both wait on the routines being re-armed.

## How to verify

- This repository carries prose only. The kernel source is not published
  here, so there is nothing to build from a clone, and an earlier version of
  this list said otherwise.
- Every indicator predicate's authority resolves to a column of the committed
  indicator reference, with TA-Lib parity where TA-Lib defines the indicator.
- The API host serves the committed report for each worked example and will
  replay one on request. The app page's stream panel is illustrative and is not
  that replay. Freeform portfolio submission is not open; nothing un-redacted
  will be accepted when it is.
- Where this page reports a tier, check it against the status board first —
  during the correction the board's *withheld* is the authority, and no
  number on this page outranks it.
- Where this page reports a count, prefer the mechanical ones. The encoded
  census is a count of symbols with a committed, rooted encoding. Anything derived from
  cross-axis agreement is withdrawn, and anything derived from how many times
  a scheduled job has done the same thing is machine-local and therefore a
  partial view — which is why the standing refusals above are stated by date
  rather than by count.
