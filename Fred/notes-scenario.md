# Notes — Scenario : AI-Assisted Estimation and Compliance

> Source : [`class/scenario-handout.md`](../class/scenario-handout.md)
> Étape 1 du plan — noter les tensions, **ne pas résoudre**.

## 3 tensions clés

### 1. Vitesse vs Sécurité/Conformité
Le client veut compresser les cycles (jours/semaines → minutes). Mais chaque erreur d'interprétation réglementaire = risque humain et juridique.
→ *Aller plus vite augmente mécaniquement le risque résiduel.*

### 2. Automatisation vs Autorité humaine
Vision client : IA qui *"lit les plans, applique les règles, génère l'estimation"*. Or :
- l'**approbation finale doit rester humaine** ;
- les corrections d'experts ne peuvent pas devenir un apprentissage automatique sans gouvernance/validation.
→ *Plus l'IA "décide", moins on peut prouver qui est responsable.*

### 3. Uniformité du modèle vs Variabilité locale
Mêmes standards, **interprétations différentes** selon juridiction et régulateur.
Une IA généraliste lisse ces différences.
→ *Traiter les exigences locales comme uniformes = principal risque réglementaire.*

## Questions d'autorité humaine ouvertes
- Qui signe l'approbation finale d'une estimation produite avec assistance IA ?
- Sur quelles preuves un expert peut-il challenger / annuler une recommandation IA ?
- Comment trace-t-on les corrections d'experts sans les rendre auto-apprenantes ?

## Ce qu'on **ne fait pas** ici
- Pas de design de système final.
- Pas de choix d'architecture.
- Pas de promesse de MVP.
Ces tensions reviendront à l'étape 5 (Spec → Plan → Build) comme preuves.
