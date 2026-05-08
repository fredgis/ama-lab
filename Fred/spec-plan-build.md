# Spec → Plan → Build

Lab section §6. Inputs: scenario tensions, baseline profile, Agent Builder observations, Strategy + CSA agents.
Goal: separate intent / approach / reviewable slices — **not** produce the final solution.

---

## Step A — Anchor

| Question | Answer |
|---|---|
| Which slice am I attacking first? | **First-pass cost estimate** from a structured design plan, delivered as a **draft** to the senior reviewer. Creates value early (speed), preserves human authority (review), avoids the compliance-interpretation trap. |
| Which concern is most at risk? | **Evaluation + system boundary**. We cannot tell "this estimate is good" without the expert; and the line "where the AI stops / where the expert resumes" is undefined. (Not orchestration: single step. Not responsibility: human still approves.) |
| Which visible work product makes the improvement inspectable? | **Side-by-side diff** AI-estimate vs expert-estimate, with quantified deltas categorized (missing item / overestimated / local interpretation). Makes the AI inspectable item by item and feeds the revisit trigger. |

### Why this choice (risk logic)
- Speed without touching Safety — the expert always validates.
- No automation of compliance interpretation — jurisdictional gray zone.
- The diff **is** the validation evidence — no diff, no useful slice.
- Avoids "AI reads plans + applies rules + generates" — too many domains stacked.

### Explicit out-of-scope for this slice
Compliance check automation, feedback learning, final approval, multi-jurisdiction handling.

---

## Step B — Spec slice (Strategy agent output)

**Decision being shaped:** what the **first reviewable AI slice** must commit to (and refuse), so the customer can judge whether AI helps cost estimation **without eroding expert authority or compliance accountability**. This is a spec slice — not the system, not the architecture.

| Category | Content |
|---|---|
| **Objective** | Produce an AI-generated **first-pass cost estimate** from a structured design input, delivered as a **draft** to a senior reviewer, surfaced alongside a **side-by-side diff** against the reviewer's corrections, so AI contribution is inspectable item by item. The objective is **inspectability + speed**, not autonomous estimation. |
| **Primary users** | • **Senior reviewer (estimator)** — accountable human; receives the draft, edits it, owns the final number.<br>• **Design author / project lead** — submits the structured design input; consumes the reviewed estimate downstream.<br>• **Quality / compliance lead** — observer at this stage; gets visibility but does not act on AI output.<br><br>Explicitly **not** users at this stage: regulator, customer, field execution team. |
| **User value** | • **Reviewer:** fewer cold-start estimates; time shifts from typing to judging.<br>• **Design author:** faster turnaround on first draft, predictable inputs.<br>• **Organization:** the diff is the **evidence trail** — first measurable signal of where AI helps, where it misses, where it must not be trusted.<br><br>Value is **only realized if the diff is captured**. No diff → no slice. |
| **Constraints** | • Human approval is **non-negotiable** on every estimate leaving the system.<br>• **No** compliance interpretation generated, summarized, or implied by AI in this slice.<br>• **Single jurisdiction** for the first slice — local regulator variability deferred.<br>• AI output must be **structurally identical** to the reviewer's working format, so diffing is mechanical, not interpretive.<br>• **No expert correction is auto-fed back** into the model in this slice (no implicit learning loop). |
| **Assumptions** | • A **structured design input** exists or can be produced (the AI is not asked to read raw plans yet).<br>• The reviewer's current estimate format is **stable enough** to diff against.<br>• Reviewers tolerate reading drafts they may discard, in exchange for time savings on the keepers.<br>• Historical estimates are available as **calibration data** for evaluation, even if not as training data.<br>• Failure of a single estimate is **recoverable** — no estimate is binding before review.<br><br>These assumptions are the **first thing to invalidate** if the slice underperforms. |
| **Success criteria** | • **% of line items the reviewer accepts unchanged** on first review (target band agreed before build, not after).<br>• **Time-to-first-draft** measurably below current cold-start baseline.<br>• **Diff completeness:** every estimate has a captured diff, categorized (missing / overestimated / interpretation differs).<br>• **Zero estimates** leave the system without explicit reviewer acceptance.<br>• **Reviewers agree** the draft is worth reading — qualitative signal, not just acceptance rate.<br><br>Success is **not** "the AI estimate matches the expert estimate." Success is **inspectable contribution**. |
| **Out of scope** | • Compliance / regulatory interpretation of any kind.<br>• Multi-jurisdiction handling, local rule variability.<br>• Automated learning from expert corrections.<br>• Final approval workflow, sign-off routing, customer-facing output.<br>• Reading unstructured plans (PDFs, drawings, free text) — input is structured.<br>• Cost optimization or alternative-design suggestions.<br>• Confidence scoring per line item (deferred — needs evaluation foundation first).<br>• Any user other than the three named above. |

**Strategy note:** the sharpest line in this spec is *"value is only realized if the diff is captured."* If the build slices in Step D do not make the diff a first-class artifact, the whole thing collapses into a faster typewriter with no evidence trail. Push the CSA agent on that point.

---

## Step C — Plan slice (Strategy agent output)

**Decision being shaped:** what must be **decided and held stable** before any build slice is started, so that the spec's promise (inspectable contribution via diff) is not eroded by implementation drift. Direction and constraints — **not** an architecture brief.

| Category | Content |
|---|---|
| **Recommended approach** | • **Diff-first delivery model**: the diff between AI draft and reviewer-corrected estimate is the **primary deliverable** of every cycle. The estimate is a means; the diff is the asset.<br>• **Single human-in-the-loop pattern**: AI produces → reviewer edits → diff is computed and stored. No second AI pass, no chained agents, no automatic re-prompting.<br>• **Narrow domain, broad observation**: one estimate type, one jurisdiction, one structured input format. Spend the saved scope on **observability** instead of features.<br>• **Explicit "AI does not know" surface**: AI must produce a *flagged-unknowns* section per estimate. Reviewer's handling of those flags is itself measured. |
| **Durable context needed** | • Stable **estimation schema** (line item categories, units, expected fields). Diffability depends on it.<br>• Stable **reviewer working surface** (where they correct the draft). If reviewers correct outside the system, no diff is captured, no value lands.<br>• Stable **diff taxonomy** (missing / overestimated / underestimated / interpretation differs / out-of-schema).<br>• A small **calibration set** of historical reviewed estimates, agreed before build, that defines "good enough" empirically.<br>• Named **accountability owner** for the slice (one human, not a committee).<br><br>If any of these is "we'll figure it out later," the slice is **not ready to build**. |
| **Harness / control-surface assumptions** | • Reviewer interacts with the draft inside a **controlled surface** (not over email, not in personal spreadsheets) — otherwise the diff is lost.<br>• AI has **no autonomous tool access** in this slice (no live data fetches, no compliance lookups, no file actions).<br>• A **kill switch / fallback to manual** must be available at any time without ceremony.<br>• All AI runs are **logged with input + output + reviewer diff** as a single record — audit trail and evaluation dataset.<br>• **No memory across cases**. Each estimate stands alone; persistent memory would smuggle in implicit learning. |
| **Tradeoffs** | **Accept:**<br>• Slower scope expansion in exchange for clean evidence of AI contribution.<br>• Reviewer friction (reading drafts they may discard) in exchange for time savings on the keepers.<br>• Manual schema discipline in exchange for mechanical, non-interpretive diffs.<br><br>**Reject:**<br>• "Also flag compliance issues" — drags regulatory interpretation into a slice that explicitly excluded it.<br>• "Quietly retrain on reviewer corrections" — auto-learning loop without governance destroys reviewer trust and audit defensibility.<br>• "Confidence scores per line item" — with no evaluation foundation yet, confidence numbers are **fabricated trust**, worse than no number. |
| **Validation method** | • **Per-estimate**: was a complete diff captured in the agreed taxonomy? Yes/no, every time.<br>• **Per-cohort**: acceptance rate by line-item category, time-to-first-draft trend, reviewer qualitative signal ("worth reading: yes/no").<br>• **Per-failure**: every estimate the reviewer discards entirely is a **named failure case** examined manually. Discard rate is itself a metric.<br>• **Adversarial check**: at agreed cadence, a senior reviewer estimates from scratch *without* seeing the AI draft, then compares. Detects whether AI is **anchoring** reviewers rather than helping them. |
| **Revisit trigger** | Stop and re-plan if **any** of these fires:<br>• Diff capture rate drops below an agreed floor (reviewers correcting outside the controlled surface).<br>• Acceptance rate plateaus or degrades over consecutive cohorts.<br>• Reviewers report **anchoring** — accepting AI suggestions they would have rejected from a junior estimator.<br>• A reviewer-discarded estimate is found to have been used downstream anyway (control surface failure).<br>• Stakeholders propose adding compliance interpretation, multi-jurisdiction, or learning loop **inside this slice**. |

### Decisions required before build/task execution

| # | Decision | Owner type | Why it must be settled first |
|---|---|---|---|
| 1 | Estimation schema (fields, units, line-item taxonomy) | Domain expert | Defines what diff even means |
| 2 | Diff taxonomy (the categories of difference we count) | Domain expert + reviewer | Without it, diffs are noise |
| 3 | Controlled reviewer surface (where corrections happen) | Slice owner + IT | Without it, no diff is captured |
| 4 | Calibration / "good enough" target band | Senior reviewer + business owner | Otherwise success is renegotiated post-hoc |
| 5 | Single accountable human for the slice | Business sponsor | No accountability → no honest validation |
| 6 | Logging + retention policy for AI-input/output/diff records | Slice owner + compliance | Audit trail; cannot be retrofitted |

If decisions 1–3 are not stable, **do not start build**. Slices 4–6 must be named even if values are provisional.

**Strategy note:** the plan deliberately spends the budget on **observability and discipline**, not features. The most likely failure of this slice is *not* "the AI estimates badly" — it is "we built the AI but never captured the diff cleanly, so we have no evidence either way." Make that the first thing the CSA agent must defend in Step D.

---

## Step D — Build slices (CSA agent output)

> _To fill: paste CSA agent output here after running the Build prompt. Aim for 3-5 slices._

| Slice | Artifact or decision output | Depends on | Review evidence |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
