# qresev-public — status

_Snapshot: 2026-08-07. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

This is the release-narrative status of the financial-domain slice: what
has landed, where each of the three surfaces stands, and what is next. It
is a companion to the [README](./README.md), not a substitute.

Most of what follows is a defect report. That is deliberate. This window's
useful output was almost entirely negative results — gates that were green and
wrong, counts that could not be re-derived, a certification frozen as a breach,
and two diagnoses withdrawn after being re-checked against the code rather than
re-read from the note that asserted them. Those are the findings worth
publishing; the parts that merely worked are one line each.

## Correction — 2026-07-14, and the re-verification has now answered

On 2026-07-14 our own adversarial review lane found leak channels in the
cross-axis **agreement oracle** — the mechanism that scores the top-tier
promotions this program reports. Independent verification cells could, in
principle, observe one another's outputs, so agreement-based tier figures
minted before that date are not established as blind.

The correction stands, and it now has an outcome:

- The **"seven top-tier names"** figure and the "re-tiered up on a fresh blind
  re-slice" claim remain **withdrawn**. The public status board reports these
  tiers as *withheld*, not as a number.
- **The blind re-slice completed on 2026-07-15 and did not re-establish those
  figures.** Of the names still eligible on that tape, it reproduced **no
  top-tier result** — with zero bear vetoes, the cleanest possible
  non-promotion. Two further blind re-slices under the corrected fan-out
  contracts, on 2026-07-19 and 2026-07-21, each landed below the top tier as
  well. The count of top-tier names re-earned under corrected contracts is
  **zero**, and that is what the board publishes.
- **The known leak-channel count went from four to six**, not because two new
  breaches happened but because two more *ways to leak* were identified and
  written into the contracts: a shared kernel namespace, and a reconciler's
  access to prior adjudications. Bounding the channel ("check a name, don't
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
- **The corrected number is still not published, and not for a good reason —
  for an honest one.** The internal rollup schema has no representation for
  *encoded and sound with agreement tier withdrawn*, so it still derives the
  old count. Publishing a public zero against an internal seven would trade one
  honest gap for two inconsistent numbers. The number rides a schema migration
  and a second review; the withdrawal ships now.
- **Unaffected: kernel soundness and the encoded census.** Every `lake build`
  is green and every committed encoding is `sorry`-free. How many symbols are
  encoded is a mechanical count, not an agreement figure. The
  accepting-and-refusing gate behaviour and the defined-risk options refusal
  are kernel facts — they stand.

We surfaced this because the cells check one another and one lane caught it —
not because the earlier numbers were right all along.

## Overall

**Pre-beta — endpoint deployed, beta not yet declared.** The hosted
evaluator at `qresev.quantapix.com` is deployed and reachable: the app
shell and its API answer publicly, and the worked example streams a kernel
verdict back that matches the committed ground truth. The drive's
early-beta opening — the hosted verification-service deliverable — remains
scheduled for Month 3+ of the window. Until it is declared, the deployment
is an engineering surface, not the launch deliverable.

## Formal evaluator (`accounting/`)

- **Eight judgment frameworks kernel-green, plus two program frameworks.**
  The five golden frameworks (Trend, Momentum, OptionsRisk, Sector, Drawdown)
  are joined by Regime, ExitDiscipline and Hedging under a chartered expansion;
  the bull-vs-bear gate and the new factor decomposition are program
  frameworks rather than portfolio judgments. All elaborate under `lake build`,
  and the spec battery is green.
- **A framework with no model in the loop.** The factor decomposition is the
  first kernel judgment whose facts are lifted deterministically from a fit
  report rather than decided by a model. The kernel checks the exact
  quantize-then-residualize identity as integer arithmetic and refuses on a
  length or day-count mismatch; the emitter is statically free of model calls
  and fails closed if the report's content hash is not the one it was told to
  read. Method detail in the [README](./README.md).
- **Hedging: eight of eleven predicates in, three recorded as bound.** Both
  top-level judgments elaborate as whole structures with no omittable field.
  The remaining three are **bound on a portfolio-schema extension**, not on
  kernel work, and are recorded as blocked rather than stubbed — a predicate
  that cannot yet read its evidence does not get a placeholder. The schema
  extension itself has now landed on the producer side; what is still missing
  is the wiring that writes it, which is stated below rather than counted as
  done.
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
- **A wave ran, passed every mechanical gate, and was not promoted.** The
  window's multi-sector wave covered four sectors, every cell elaborated, and
  the trust, coherence, oracle-guard, parity and theorem-attestation gates all
  passed. Its **blindness certification came back a breach verdict, and was
  frozen as a breach** — not downgraded — even though the substantive findings
  measured **zero** information disclosure: the one shared kernel file that was
  touched was touched by exactly one agent, the licensed cross-sector
  reconciler whose contract requires it, and no blind cell read it. The wave is
  held in its sandbox at the operator gate with its evidence bundle archived,
  and the encoded census is unchanged by it.

  The order of operations is the point. The verdict is frozen *before* anyone
  reacts to the auditor's non-zero exit, because an unfrozen breach ages off
  with the transcripts it rests on — that is precisely how one earlier wave
  became permanently un-certifiable. A certification adjusted after the fact
  would be evidence of nothing.
- **Five defects hit during that wave; a sixth was a misdiagnosis and is
  recorded as one.** Publishing the list because each generalises:
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
     all three then diverged *downward*, which is why the wave survives as
     anti-echo evidence rather than being void. Scrubbed to ordinal, date,
     sector and closure shape; waves are now cited by date, because three
     historical run names encoded the verdict in the name itself.

  The sixth was not a defect: four cells believed a data-reading flag narrowed
  their view of the tape when it is additive by design. It is recorded as a
  misdiagnosis so the next session does not "fix" working code — the shared raw
  tape vocabulary every axis reads is not a blindness channel.
- **Two ways a green build proves nothing, both now closed.** A Lean option
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
  arm passes it while the tree arm rejects it.
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
  week. The aggregator's roster is now derived from the tree.
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
- **Refusing to run is a reported outcome.** The deterministic pre-filter that
  targets a run — pure indicator and relative-strength arithmetic, no model
  calls — has returned zero eligible candidates on recent tapes, so no run was
  launched and no model spend was incurred. Those refusals are first-class rows
  in the program's history rather than prose, so the axis does not read as idle
  when it is in fact correctly holding. A monitoring alarm for tape staleness
  now exists alongside it, and is **dark** until it is enabled on the machine
  that would fire it — recorded as dark rather than described as running.

## Axiomatize-trading program

- **Encoded census: 10 symbols across four sectors**, each committed into
  the kernel library as an explicit build root, each `sorry`-free, with the
  golden reference untouched. Counting only committed universe encodings
  (the golden calibration portfolios are scaffolding, not universe
  members), that is on the order of **1.9%** of the ~526-symbol GICS
  universe. This is a mechanical census, unaffected by the agreement
  correction above and unchanged by the held wave.
- **Tier counts: withheld.** See the correction. Where tiers are reported at
  all, they are derived from per-symbol enumeration and never carried as a
  standalone number.
- **Three blind re-slices, none promoting.** Each ran the full pipeline —
  seven independent signal-family cells, a per-sector reconciliation, a
  cross-sector pass, and the admissibility gate — and each landed below the top
  tier because only two independent directional axes were confirmed at
  single-sector scope; the cross-section axis stays provisional until a
  cross-sector pass. All had **zero bear vetoes**. In each, the gate correctly
  **refuse-closed** with no incomplete proof: the admissibility structure is
  unconstructable without the top-tier confluence claim. One of the names is the
  program's original *accepting* exemplar, and it now correctly refuses. That is
  the anti-echo property working — a re-verification that could only ever
  confirm the prior answer would be measuring nothing.
- **The oracle caught a real defect in-flight.** In one re-slice, a
  reconciliation build-guard caught a corroborator cell reporting success on a
  claim that had discharged *vacuously*. The cell was tier-neutral, so the
  verdict did not move — but the guard, not a human review, is what found it.
- **Two genuine mechanism violations were found on re-adjudication, and the
  earlier convictions were withdrawn.** Re-running the corrected grader over
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
- **Sharing across axes is measured, not assumed.** A shared predicate library
  backs the per-sector slices; each new run reports how many of its candidate
  predicates re-witness existing shared cores versus mint new ones, so
  consolidation is observable rather than claimed.

Full-universe scale-out remains gated on a separate programmatic lane.

## Market inspection (`analyzing/`)

- **The local inspection surface is feature-complete for its first workflow
  set,** and gained optional factor-decomposition overlays this window at the
  symbol, cohort and ribbon scopes. It runs on loopback only, is absent from
  every deploy lane, and renders **ground-truth tape only** — no predicate
  verdict, framework judgment, or proof node appears in an inspection panel.
  Anything exported off it becomes a financial-clearance subject.
- **Honesty rules are mechanical, not stylistic.** A correlation pair with
  fewer overlapping returns than the window requires is reported as undecidable
  rather than as a short-window number. Symbols with too few bars for a cohort
  are listed as excluded rather than silently composited. Books render
  separately and are never pooled. A book's beta reference lines render as
  **values only** — there is no in-band / out-of-band verdict word, because the
  kernel predicate that would license one does not exist yet. And no
  fit-quality or confidence field crosses the overlay seam at all: a factor
  contribution is shown, the model's opinion of itself is not.
- **Restatement classification is compositional.** A vendor refresh that
  changes historical values is decomposed into world-fact components —
  split-like, dividend-like, volume-settlement, and residual — instead of a
  single opinion word. Recent full refreshes came back with **zero unmodelled
  residuals**, which is what licenses the claim that no new supplier-change
  class needs naming. A refinement: rescale uniformity is judged over a
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
  restate them, but may never reduce the set of dates held for a symbol; a
  vendor drop is refused, the file left byte-identical, and the run exits
  non-zero rather than letting a downstream program rebase on a truncated
  series. One symbol has been in that refused state since the guard was cut in
  July 2026 — refused on **every** refresh since, byte-identical each time,
  held on the record rather than repaired. A **second symbol entered the refused
  state on 2026-08-06**: the vendor dropped bars we hold while adding newer
  ones, which is plausibly structural rather than a flap given a pending
  corporate action on that name. Its disposition is explicitly open. If the
  security's listing has ended, the correct answer is retirement-in-place
  through the universe lane, never a rewrite: splicing a pre-action series onto
  a post-action security fabricates price continuity and poisons every indicator
  computed over it. Accepting a genuine reset is per-symbol and on the record,
  or the tape does not change. Universe membership is monotone too, and the
  freshness manifest's drop detection was widened by exactly the
  retired-and-still-held set — a per-symbol intersection, never a tolerance — so
  a retirement and a genuine silent drop in the same refresh do not cancel each
  other out.
- **The planned automatic alarm for that refusal has been redesigned before
  shipping, because the obvious version could not work.** The original
  requirement was to surface the refusal exit code as a routine failure. That
  alarm would have shipped red on its first fire and every fire after, because a
  standing refusal *is* the steady state of this lane — and a channel that is
  always red cannot report the new regression the alarm exists to catch. The arm
  must fire on a **diff against a recorded baseline** of expected-blocked
  symbols: a symbol newly entering the refused set, or a refused symbol whose
  signature changes. The second refused symbol above is exactly the event that
  design exists to catch, and it was caught by a person rather than by the
  alarm, which is the honest state of it.
- **A staleness gate was reading the wrong timestamp, on both sides.** A
  consumer-side freshness check compared against a field that a manifest
  regeneration could advance without any new data, which meant the cure for a
  staleness alarm was to regenerate the manifest — silently dropping the
  non-clean list the gate depended on and trading a false abort for a vacuous
  gate. Both halves are now fixed: the check reads the build timestamp and
  falls back aloud for older manifests, an absent basis is a loud
  *unchecked* rather than a pass, and an unknown one aborts as
  couldn't-look. The standing rule that survives: **never regenerate the
  manifest to clear a staleness alarm.**
- **Coverage status** surfaces through the framework's `/status` board —
  symbol count, indicator-reference count, GICS-mapping presence, and
  newest-bar freshness.

**Next:** a backtest-summary surface; the expected-blocked baseline the
refusal alarm needs; and the disposition of the newly refused symbol.

## Portfolio management (`trading/`)

**Two coverage gates were green and wrong at the same time this window, and
one of them caused real damage on a paper book.** Both are worth publishing in
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
  window that book has ever had** — about twenty-six seconds. The remedy text
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
- **The schema for all this landed; the wiring did not, and that is stated
  rather than counted.** A leg-pairing identifier minted at ticket time, a
  signed net premium per option leg, and a per-book append-only premium ledger
  are all in place. **Nothing yet writes the ledger on a real fill.** Until a
  routine turns a fill into a ledger row and stamps the identifier onto the
  positions it opens, the budget predicate has nothing to read and the collar
  predicate sees a null identifier on every existing option position. The
  backfill question is deliberately open, because the honest options are "leave
  them null and let the predicates abstain" or "operator-attested pairing" —
  never "infer", which is the reconstruction the identifier exists to prevent.
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
  window: while an interactive session holds the write-lock, every scheduled
  fire on that machine refuses and spends nothing, which also means a session
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
  its hand, declined to write and reported no problem. It now refuses loudly.
  The cost itself is never lost; it is reconstructible from the logs. **The
  narrative is deliberately not backfilled**: a closing section written days
  later is not a contemporaneous record, and manufacturing one is the same
  failure class the session-calendar guard exists to prevent. Record the gap.
- **Two of our own tracked diagnoses were withdrawn this window after being
  re-derived against the code.** One asserted a blocker that had already been
  cleared and a dependency count that the graph tool did not produce; another
  had quoted a machine-readable clause verbatim inside prose to narrate that the
  clause was *gone*, and the parser does not distinguish a quoted clause from a
  live one — so the tooling showed a phantom dependency on an item whose own
  text said the dependency was released. Both are recorded as corrections rather
  than quietly rewritten. Re-reading the note that makes a claim is not
  re-verifying the claim.

**Next:** wire fills into the premium ledger at execution time and put the
backfill question to the operator; finish the write-fence and session-calendar
migration for the remaining routine producers.

## How to verify

- Clone, `lake build`, and watch the kernel either accept or reject the
  worked examples.
- Every indicator predicate's authority resolves to a TA-Lib reference.
- Replay the worked example at the deployed endpoint and read the streamed
  trace back. Freeform portfolio submission opens with the beta; nothing
  un-redacted will be accepted.
- Where this page reports a tier, check it against the status board first —
  during the correction the board's *withheld* is the authority, and no
  number on this page outranks it.
- Where this page reports a count, prefer the mechanical ones. The encoded
  census is a count of committed build roots. Anything derived from
  cross-axis agreement is withdrawn, and anything derived from how many times
  a scheduled job has done the same thing is machine-local and therefore a
  partial view — which is why the standing refusal above is stated as
  "every refresh since" rather than as an ordinal.
