# Harness Comparison — M365 Copilot Studio vs GitHub Copilot CLI, projected to Microsoft Foundry

Lab section §7. Inputs: lab observations from Step 3 (M365 Agent Builder, observed only), Step 4 (Copilot CLI agents built), Step 5 (Spec → Plan → Build transfer).
Goal: name **architecture tradeoffs** (not feature compare, not UI preference), and project the same harness layers onto Microsoft Foundry.

> M365 column is intentionally **uplifted** to Copilot **Studio** (enterprise/pro surface) per worksheet §7, not Agent Builder. Where this profile only observed Agent Builder hands-on, statements about Studio are stated as platform facts, not personal observation.

---

## Workspace — 6 harness layers

| Layer | M365 Copilot Studio | GitHub Copilot CLI | What was made **explicit** in this lab? |
|---|---|---|---|
| **1. Model + execution runtime** | Microsoft-managed LLM orchestration (Copilot runtime, generative orchestrator/planner, multi-model abstraction). Model choice abstracted. | Explicit model selection (`/model`); LLM client + agent execution loop visible in CLI. | **CLI**: model is a first-class lever (we used Claude Opus 4.7 for CSA). **Studio**: opaque — orchestrator picks. |
| **2. Tool layer** | Connectors (M365 + 3rd-party), Power Automate flows, MCP servers, API plugins. Curated catalog. | `tools:` field in `.agent.md`, MCP servers, plugins, shell, Python, search, fetch, read, edit. Declarative per agent. | **CLI**: tool list **is the agent definition** (frontmatter). MCP `tools: ["*"]` required (we hit this). **Studio**: tools attached via UI catalog, governance-approved. |
| **3. Context / knowledge layer** | Dataverse, SharePoint, Outlook, Teams, Graph grounding, enterprise knowledge sources. **Tenant-scoped** by default. | Repo files, `.github/copilot-instructions.md`, `agents/*.agent.md`, workspace files, attachments. **Repo-scoped** by default. | Both surfaces gave the same conceptual layering — **but the scope unit differs**: tenant vs repo. This is a binding architectural choice, not a UI difference. |
| **4. Permission / governance** | DLP, Entra identity, connector classification, environment governance. Centrally enforced. | Tool restrictions in `.agent.md`, filesystem scope, approval model (`--yolo` vs gated). Enforced at agent file. | **CLI**: governance is **a markdown commit** (review-able in PR). **Studio**: governance is **a tenant policy** (review-able by admins, invisible to most agent authors). |
| **5. Memory layer** | Conversation history, Dataverse state, thread/session context. Persistent by default. | Session state, repo state via files+git, persisted CLI session memory. Per-session, per-repo. | **CLI**: memory across sessions is **opt-in via files** (we observed memories surfacing here). **Studio**: memory is closer to a default service. |
| **6. Execution / orchestration** | Topic routing, agent flows, connected agents, generative orchestration planner. | Subagents in `.agent.md`, parallel dispatch patterns, hooks, plan mode. | Both **support** orchestration; in this lab **neither was orchestrated** — the Strategy and CSA agents did **not** hand off to each other. The human (Fred) was the orchestrator. This is the most important observation of Step 4. |

---

## Synthesis

### What M365 Copilot Studio makes easier
- **Tenant-scoped context grounding** — SharePoint/Graph/Dataverse are first-class, no plumbing.
- **Centralized governance** (DLP, Entra, connector classification) without the agent author having to think about it.
- **Non-developer authoring** — the surface meets domain experts where they are.

### What GitHub Copilot CLI makes easier
- **Surgical control of context** — `.github/copilot-instructions.md` and `agents/*.agent.md` are diffable, reviewable, and version-controlled. Changes go through PR review.
- **Explicit tool surface** — what an agent can and can't do is visible in plain text in the agent file. No "did the admin approve this connector?" round-trip.
- **Model + reasoning style choice per agent** — different agents can use different models with different posture (Strategy vs CSA observed in this lab).

### Projection to Microsoft Foundry

The 6 harness layers all **carry forward** to Foundry — but the implementation shifts:

| Layer | How Foundry implements it | Source |
|---|---|---|
| Model + runtime | Foundry Agent Service is fully managed; **flexible model choice** per agent ("right model for the task"); model deployments per project. | [Foundry Agent Service overview](https://learn.microsoft.com/azure/foundry/responsible-ai/agents/transparency-note#capabilities) |
| Tools | **Tool Catalog** with Knowledge tools (File Search, AI Search, SharePoint, Fabric, Bing) and Action tools (Logic Apps, Functions, OpenAPI, MCP, Code Interpreter, Browser Automation). Tool selection via `tool_choice` (`auto`/`required`/`none`). | [Tool catalog](https://learn.microsoft.com/azure/foundry/agents/concepts/tool-catalog), [Tool best practices](https://learn.microsoft.com/azure/foundry/agents/concepts/tool-best-practice) |
| Context / knowledge | Knowledge tools above + on-behalf-of (OBO) auth for SharePoint/Fabric (agent only sees what the user has rights to). | [Transparency note — knowledge tools](https://learn.microsoft.com/azure/foundry/responsible-ai/agents/transparency-note#capabilities) |
| Permission / governance | Project-level RBAC (Azure AI Developer); tool-level connections require service-level access; Azure-native identity. | [Tool best practices — prerequisites](https://learn.microsoft.com/azure/foundry/agents/concepts/tool-best-practice) |
| Memory | **Threads** store messages; **Runs** activate the agent; **Run Steps** record what happened. Built-in memory management. | [Foundry Agent Service basics](https://learn.microsoft.com/azure/foundry/responsible-ai/agents/transparency-note#the-basics-of-foundry-agent-service) |
| Orchestration | **Connected Agents** (classic) or **Workflows** (declarative orchestration of multiple agents via graphical UI). Multi-agent is platform-native, not improvised. | [Connected Agents](https://learn.microsoft.com/azure/foundry-classic/agents/how-to/connected-agents) |

**Key projection insight:** the layer that changed most between CLI and Foundry is **orchestration**. In CLI the human was the orchestrator (Strategy → CSA hand-off was manual). In Foundry, Connected Agents/Workflows make multi-agent orchestration a **first-class declarative artifact** — which is good for reuse but **silently removes the human-in-the-orchestration check** that the CLI naturally enforced. This is a tradeoff to name, not to celebrate.

---

## Architecture tradeoffs (the worksheet asks for ≥2)

### Tradeoff 1 — Governance authoring locus
- **CLI**: governance is **agent-file commits in a repo**, reviewed via PR. Visible to anyone who reads the repo. Tightly coupled to the agent author.
- **Studio**: governance is **tenant policy**, reviewed by admins. Invisible to most agent authors. Decoupled from authoring.
- **Tradeoff**: PR-reviewable governance scales with developer culture; tenant policy scales with admin discipline. Neither is universally right; **the choice binds who gets blamed when something goes wrong**.

### Tradeoff 2 — Orchestration surface visibility
- **CLI**: no platform orchestration in this lab → the human did it. Naturally enforces a per-handoff review.
- **Studio / Foundry**: orchestration is declarative (topics, agent flows, Connected Agents, Workflows). Multi-step is reusable but the **human-in-the-orchestration check disappears unless explicitly designed in**.
- **Tradeoff**: declarative orchestration buys reuse and observability; manual orchestration buys per-step accountability. For the cost-estimate slice (Step D — B2 reviewer surface), the *manual* posture matches the spec ("single human-in-the-loop, no chained agents"). The architectural risk of moving to Foundry too early is **smuggling in orchestration that the spec said no to**.

### Tradeoff 3 — Context scope unit (bonus)
- **CLI**: context unit is the **repo**. Context follows the codebase. Easy to fork, audit, diff.
- **Studio / Foundry**: context unit is the **tenant / project**. Context follows identity. Easy to govern centrally, harder to fork or compare.
- **Tradeoff**: repo-scoped context maps cleanly to "this slice, this team, this jurisdiction." Tenant-scoped context maps cleanly to "all of M365." For the spec's *single-jurisdiction-first* posture, repo-scope is closer to the intent.

---

## The limitation that matters most for the first responsible slice

From Step D, the riskiest slice is **B2 (reviewer surface)** — it silently picks the tenant boundary. The harness comparison sharpens this:

- If the reviewer surface is built on **M365 Studio**, the surface decision is also a **tenant decision** and inherits Microsoft 365 governance for free — but the diff capture mechanism becomes opinionated by what Studio exposes.
- If built on **Copilot CLI / repo-scoped tooling**, the surface decision stays close to the team and the diff is whatever the team designs — but the governance falls back on what the team builds, not what the platform enforces.
- If built on **Foundry Agent Service**, the diff capture sits on top of Threads + Run Steps + agent tracing ([trace-agent](https://learn.microsoft.com/azure/foundry/observability/concepts/trace-agent-concept)), which is the most *operational-telemetry-rich* surface — but **operational telemetry is not the same as a business audit record** (called out in Step D, B3).

**Net:** the limitation that matters most is **the gap between what the harness gives you for free and what the spec requires you to build yourself**. Each surface gives you a different "free" and forces a different "build." None of them gives you the diff-as-business-record for free.

---

## Closing reflection

> If we succeed, what does this system make easier for humans, and what must it never decide on their behalf?

- **Easier for humans:** producing the *first* draft of a cost estimate — the cold-start tax, the boilerplate, the structurally-obvious line items. Humans get to spend their time on *judgment*, not on *typing*.
- **Must never decide on their behalf:**
  - whether the estimate is **safe to release**,
  - whether a regulatory interpretation **applies**,
  - whether an unknown should be **filled in or flagged**,
  - whether the estimate **passes review**,
  - and whether reviewer corrections should **change the model**.

The system's value is **inspectable contribution under explicit human authority**, not autonomy.
