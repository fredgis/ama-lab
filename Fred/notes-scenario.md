# Notes — Scenario: AI-Assisted Estimation and Compliance

> Source: [`class/scenario-handout.md`](../class/scenario-handout.md)
> Step 1 of the plan — capture tensions, **do not solve**.

## Three key tensions

### 1. Speed vs Safety/Compliance
The customer wants to compress cycles (days/weeks → minutes). But every misinterpretation of regulation creates human and legal risk.
→ *Going faster mechanically increases residual risk.*

### 2. Automation vs Human authority
Customer vision: an AI that *"reads plans, applies rules, generates the estimate."* Yet:
- **final approval must remain human-owned**;
- expert corrections cannot become automatic learning without governance and validation.
→ *The more the AI "decides," the harder it becomes to prove who is accountable.*

### 3. Model uniformity vs Local variability
Same standards, **different interpretations** depending on jurisdiction and regulator.
A generalist AI smooths over those differences.
→ *Treating local requirements as uniform is the dominant regulatory risk.*

## Open human-authority questions
- Who signs off on the final estimate produced with AI assistance?
- On what evidence can an expert challenge / overturn an AI recommendation?
- How do we trace expert corrections without turning them into self-learning?

## What we are **not** doing here
- No final system design.
- No architecture choice.
- No MVP commitment.
These tensions will resurface at Step 5 (Spec → Plan → Build) as evidence.

---

## Step 4 — Layering check (CLI files)

Outcome of Workbook §5 Step D for the three drafted files in `.github/`:
- `copilot-instructions.md` (shared)
- `agents/strategy.agent.md`
- `agents/cloud-solution-architect.agent.md`

| Guidance candidate | Keep in `copilot-instructions.md`? | Move to `strategy.agent.md`? | Move to `cloud-solution-architect.agent.md`? | Why? |
|---|:---:|:---:|:---:|---|
| Durable context (operating posture, risk ranking, evidence standard) | ✅ | ❌ | ❌ | Applies to every agent regardless of role. Duplicating it in role files would create drift. |
| General operating rules (ambiguity handling, escalation principles, response shape) | ✅ | ❌ | ❌ | Cross-cutting; both role agents inherit them. |
| Strategy-specific reasoning style (decision space mapping, "name the decision first", strategy vs. implementation split) | ❌ | ✅ | ❌ | Only meaningful when shaping a decision; would distort technical reviews if forced on CSA. |
| Microsoft portfolio technical depth (Azure / M365 / Security / Data / AI / Power Platform) | ❌ | ❌ | ✅ | Domain expertise specific to the CSA role; out of scope for shared and Strategy. |
| Microsoft Learn MCP Server access | ❌ | ❌ | ✅ | MCP is configured in CSA frontmatter; Strategy must not cite product docs. Scoping reduces noise and prevents accidental product-pitching. |
| Escalation rules — generic ("regulatory / no human owner / no exit strategy") | ✅ | ❌ | ❌ | True for any agent in this environment. |
| Escalation rules — role-specific (Strategy → CSA handoff; CSA → human authority for legal/regulatory) | ❌ | ✅ | ✅ | Each handoff direction belongs to the role that initiates it. |
| Communication fingerprint — generic structure (Goal / Assumptions / Tradeoffs / Risks / Rationale) | ✅ | ❌ | ❌ | Default response shape for any agent. |
| Communication fingerprint — Strategy compression style (lead with the decision, bullets over prose) | ❌ | ✅ | ❌ | Tone unique to strategy work; technical reviews need the longer architect shape. |
| Response shape — technical review (Proposal → Architecture review → Binding choices → Now vs. before-scale → Sources) | ❌ | ❌ | ✅ | Architecture-review-only; would over-formalize Strategy answers. |
| Evaluation expectations (real prod data > theory; cite sources when grounding tools available) | ✅ | ❌ | ❌ | Evidence standard applies to every agent; "cite sources" tightens automatically when Learn MCP is attached on the CSA. |

### One-liner takeaway
Shared instructions hold the **posture**; the Strategy agent owns **how to frame decisions**; the CSA agent owns **how to stress-test Microsoft architecture with cited evidence**. No rule lives in two files.

---

## Lab recap — all 6 steps delivered

Pushed to https://github.com/fredgis/AMA-lab on `main`.

| # | Step | Deliverable | Key outcome |
|---|---|---|---|
| 1 | Scenario tensions | `Fred/notes-scenario.md` (this file) | 3 named tensions (Speed/Safety, Automation/Authority, Uniformity/Local variability) |
| 2 | M365 Copilot personalization interview | `Fred/agent-profile-baseline.md` | Agent Operating Profile v1 (risk ranking Impact > Reversibility > Likelihood, anti cloud-agnostic stance, response shape) |
| 3 | M365 Agent Builder observation | (conversational) | Observed grounding wall, no MCP, tenant-scoped surface |
| 4 | GitHub Copilot CLI agents | `.github/copilot-instructions.md`, `.github/agents/strategy.agent.md`, `.github/agents/cloud-solution-architect.agent.md` | 3 files, repo-scoped, CSA wired to Microsoft Learn MCP (`tools: ["*"]` mandatory), layering check above |
| 5 | Spec → Plan → Build | `Fred/spec-plan-build.md` | Step A anchor + Spec slice (Strategy) + Plan slice (Strategy, diff-first) + 4 build slices (CSA, Learn-grounded: schema/reviewer surface/audit/evaluation) |
| 6 | Harness comparison + Foundry projection | `Fred/harness-comparison.md` | 6-layer table (Studio vs CLI), 3 architecture tradeoffs (governance locus, orchestration visibility, context scope), Foundry mapping (Tool Catalog / Threads-Runs / Connected Agents) |

### Cross-step anchors
- **Strategic anchor** (from Step 5 Plan): *value is only realized if the diff is captured.*
- **Architectural anchor** (from Step 5 Build): the riskiest build slice is the **reviewer surface** — it silently picks the tenant boundary.
- **Harness anchor** (from Step 6): low-code vs pro-code is **not** the real distinction. Control over context, tools, memory, permissions, reviewability — and where humans keep authority — is.

### What this scenario is **about** in the end
Producing **inspectable AI contribution under explicit human authority**, not autonomy. Every step refused to drift from that line.
