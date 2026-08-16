# Journal des décisions

## 2026-08-17 — WSL2 / Ubuntu comme environnement technique
**Décision :** Windows reste le système principal ; WSL2 / Ubuntu 24.04 sert d'environnement de développement.  
**Impact :** projets techniques sous `/home/enzo/projects`.

## 2026-08-17 — GitHub comme système de version
**Décision :** Git local dans Ubuntu, GitHub comme dépôt distant.  
**Impact :** la mémoire du projet est versionnée comme le code.

## 2026-08-17 — Mémoire propre à chaque projet
**Décision :** chaque nouveau projet significatif possède son propre dossier `memory/`.  
**Pourquoi :** conserver le contexte, les décisions et l'évolution au plus près du projet.

## 2026-08-17 — PostgreSQL hors du chemin critique
**Décision :** commencer avec CSV/Parquet.  
**Pourquoi :** priorité à Snowflake → dbt → Tableau.  
**Impact :** PostgreSQL et Docker sont différés.

## 2026-08-17 — Snowflake réel
**Décision :** utiliser le vrai Snowflake et ne pas le recréer localement.  
**Impact :** contrôle explicite des coûts avant toute opération consommatrice.

## 2026-08-17 — dbt comme cœur pédagogique
**Décision :** concentrer l'apprentissage sur `sources → staging → intermediate → marts`, les tests, la documentation et l'optimisation.

## 2026-08-17 — Tableau Desktop comme validation finale
**Décision :** Tableau Desktop sert à tester que les marts sont simples, pertinents et performants pour le reporting.
