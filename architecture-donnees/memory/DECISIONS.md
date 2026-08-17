# Décisions structurantes

Ce fichier conserve les décisions qui doivent rester compréhensibles plusieurs mois plus tard.

Format recommandé :

```text
## YYYY-MM-DD — Décision

Contexte :
...

Décision :
...

Pourquoi :
...

Impact :
...
```

---

## 2026-08-17 — Windows + WSL2

**Contexte**  
Le PC doit rester adapté au gaming et aux applications Windows tout en servant de poste de développement.

**Décision**  
Windows reste le système principal et WSL2 / Ubuntu 24.04 sert d'environnement technique.

**Pourquoi**  
Séparer proprement l'usage quotidien Windows et l'environnement Linux de développement.

**Impact**  
Les projets techniques sont stockés dans `~/projects`.

---

## 2026-08-17 — GitHub comme mémoire versionnée

**Décision**  
Git gère l'historique local et GitHub conserve l'état distant validé.

**Impact**  
Les fichiers de mémoire sont versionnés comme le code.

---

## 2026-08-17 — Une mémoire par projet

**Décision**  
Chaque projet important possède son propre dossier `memory/`.

**Pourquoi**  
Conserver le contexte et les décisions au plus près du projet concerné.

---

## 2026-08-17 — Mémoire simplifiée

**Décision**  
La mémoire utilise huit fichiers fonctionnels :
- `PROJECT.md`
- `ROADMAP.md`
- `HANDOFF.md`
- `DECISIONS.md`
- `LEARNINGS.md`
- `SECURITY.md`
- `SESSION_GUIDE.md`
- `PLAYBOOK.md`

**Pourquoi**  
Éviter les doublons tout en séparant clairement vision, état, connaissances, sécurité, routine et commandes.

**Impact**  
`RUNBOOK.md` est supprimé.  
Les procédures de session vont dans `SESSION_GUIDE.md`.  
Les commandes et procédures techniques réutilisables vont dans `PLAYBOOK.md`.

---

## 2026-08-17 — PostgreSQL hors du chemin critique

**Décision**  
Utiliser d'abord CSV ou Parquet.

**Pourquoi**  
La priorité pédagogique est Snowflake → dbt → Tableau.

**Impact**  
PostgreSQL et Docker sont différés.

---

## 2026-08-17 — Utiliser le vrai Snowflake

**Décision**  
Ne pas simuler Snowflake localement.

**Pourquoi**  
Comprendre les connexions, rôles, warehouses et coûts réels fait partie de l'apprentissage.

**Impact**  
Toute action Snowflake susceptible de consommer doit être précédée d'un contrôle coût.

---

## 2026-08-17 — dbt comme cœur pédagogique

**Décision**  
Concentrer la majorité de l'apprentissage Data sur dbt.

**Impact**  
Le projet devra approfondir :
- sources ;
- staging ;
- intermediate ;
- marts ;
- tests ;
- documentation ;
- matérialisations ;
- optimisation ;
- impacts Snowflake.

---

## 2026-08-17 — Tableau Desktop comme validation de la couche Data

**Décision**  
Tableau Desktop consomme les marts produits par dbt.

**Pourquoi**  
Un bon modèle Data doit être évalué par son utilité réelle pour la BI.
