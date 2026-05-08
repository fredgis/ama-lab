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

> _To fill: paste Strategy agent output here after running the Plan prompt._

| Category | Content |
|---|---|
| Recommended approach | |
| Durable context needed | |
| Harness / control-surface assumptions | |
| Tradeoffs | |
| Validation / review method | |
| Revisit trigger | |

---

## Step D — Build slices (CSA agent output)

> _To fill: paste CSA agent output here after running the Build prompt. Aim for 3-5 slices._

| Slice | Artifact or decision output | Depends on | Review evidence |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
