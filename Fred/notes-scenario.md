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
