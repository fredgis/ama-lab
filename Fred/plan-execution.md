# 🧭 Plan Fred — AMA Lab

```mermaid
flowchart LR
    A[1. Scénario]:::a --> B[2. M365 Copilot<br/>baseline]:::b --> C[3. M365 Agent<br/>Builder + MCP]:::c --> D[4. Copilot CLI<br/>+ agents]:::d --> E[5. Spec→Plan→Build]:::e --> F[6. Compare]:::f
    classDef a fill:#cfe2ff,stroke:#0d6efd,color:#0a3d91
    classDef b fill:#e2d4f0,stroke:#6f42c1,color:#3d1f6b
    classDef c fill:#ffe5b4,stroke:#fd7e14,color:#7a3a00
    classDef d fill:#c7ecc7,stroke:#198754,color:#0b5022
    classDef e fill:#fff3a3,stroke:#d4a017,color:#6b4f00
    classDef f fill:#f5b7b1,stroke:#c0392b,color:#641e16
```

## À faire dans l'ordre

1. **Lire** [`class/scenario-handout.md`](../class/scenario-handout.md). Noter 3 tensions, ne pas résoudre.
2. **M365 Copilot** : interview de personnalisation → sauver `agent-profile-baseline.md` (local).
3. **M365 Agent Builder** : coller la baseline, tester grounding, observer le mur **Learn MCP**.
4. **GitHub Copilot CLI** :
   - `~/.copilot/copilot-instructions.md` (baseline)
   - `~/.copilot/agents/strategy.agent.md`
   - `~/.copilot/agents/cloud-solution-architect.agent.md` (avec Learn MCP)
   - `/restart` puis `/env` pour vérifier.
5. **Spec → Plan → Build** : 1 spec, 1 plan, 3-5 slices.
6. **Comparer** les harnesses, projeter sur Foundry.

## Règles

- Workbook ouvert : [`class/worksheets.md`](../class/worksheets.md).
- Bloqué > 10 min sur un outil → observation/pair.
- Ne pas réordonner les étapes.
