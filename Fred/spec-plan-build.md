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

> _To fill: paste Strategy agent output here after running the Spec prompt._

| Category | Content |
|---|---|
| Objective | |
| Primary users | |
| User value | |
| Constraints | |
| Assumptions | |
| Success criteria | |
| Out of scope | |

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
