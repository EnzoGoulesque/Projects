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

---

## 2026-08-17 — Rôle et warehouse Snowflake dédiés au projet

**Contexte**  
Le compte Snowflake contient des rôles système et des warehouses génériques qui ne doivent pas devenir le contexte de travail quotidien du laboratoire.

**Décision**  
Utiliser :
- `ARCHITECTURE_DONNEES_ROLE` comme rôle de travail ;
- `ARCHITECTURE_DONNEES_WH` comme warehouse du projet ;
- `ARCHITECTURE_DONNEES` comme database ;
- `RAW` comme première zone d'entrée.

Le warehouse du projet démarre en X-Small / Gen1, avec auto-resume, auto-suspend à 300 secondes et timeout de requête à 600 secondes.

**Pourquoi**  
Isoler le projet, appliquer le moindre privilège et rendre le coût du compute lisible et contrôlable.

**Impact**  
`ACCOUNTADMIN` et les autres rôles système ne sont utilisés que lorsque leurs privilèges sont nécessaires. `COMPUTE_WH` n'est pas le warehouse de travail du projet.

---

## 2026-08-17 — Vérification du compte Snowflake actif comme barrière de sécurité

**Contexte**  
Un compte Snowflake professionnel distinct et le compte personnel peuvent être associés à la même adresse e-mail et apparaître dans le même sélecteur Snowsight.

**Décision**  
Avant toute modification ou opération susceptible d'utiliser du compute, vérifier l'identifiant du compte actif et confirmer qu'il s'agit du compte personnel du projet.

**Pourquoi**  
Éviter une manipulation accidentelle sur l'environnement professionnel.

**Impact**  
Cette vérification devient une étape obligatoire du workflow Snowflake. Aucun identifiant de compte réel n'est stocké dans la mémoire versionnée.

---

## 2026-08-17 — Dataset synthétique versionnable

**Décision**  
Conserver le petit dataset fictif `orders.csv` dans `data/sample/` afin de disposer d'une source reproductible et évolutive pour les exercices Snowflake, dbt et BI.

**Pourquoi**  
Pouvoir ajouter des colonnes, cas limites et nouvelles lignes de test sans dépendre de données professionnelles.

**Impact**  
`data/sample/` peut contenir des données explicitement fictives et publiables. Les données privées ou professionnelles restent interdites dans Git.

---

## 2026-08-18 — Environnement dbt isolé et reproductible dans WSL

**Contexte**  
Le projet doit rester stable dans Ubuntu sans polluer le Python système ni introduire une couche supplémentaire sans besoin concret.

**Décision**  
Utiliser le Python 3.12 fourni par Ubuntu uniquement comme base, puis créer un environnement `.venv` propre au projet. Installer `dbt-snowflake` dans cet environnement et versionner :
- `requirements.txt` pour la dépendance principale choisie ;
- `requirements.lock.txt` pour les versions exactes réellement installées.

Version de départ validée :
- Python 3.12.3 ;
- dbt Core 1.12.2 ;
- dbt-snowflake 1.12.0.

**Pourquoi**  
Obtenir une installation simple, isolée et reconstruisible sans ajouter Docker, Conda, Poetry ou `uv` alors qu'aucun besoin actuel ne le justifie.

**Impact**  
`.venv/` reste local et ignoré par Git. Les fichiers de dépendances sont versionnés. Sur une nouvelle machine, l'environnement peut être recréé à partir des fichiers du projet.

---

## 2026-08-18 — Projet dbt à la racine du projet Architecture de données

**Contexte**  
Le repository contient actuellement un seul laboratoire Data et dbt en est la couche centrale de transformation.

**Décision**  
Placer `dbt_project.yml` à la racine de `architecture-donnees` et organiser les modèles sous :

```text
models/
├── staging/
├── intermediate/
└── marts/
```

Le profil dbt se nomme `architecture_donnees` et sa configuration locale restera dans `~/.dbt/profiles.yml`, hors du repository.

**Pourquoi**  
Éviter un sous-projet dbt artificiel et garder une lecture simple du flux complet données → Snowflake → dbt → BI.

**Impact**  
Le dépôt reste mono-projet pour l'instant. Si plusieurs projets dbt apparaissent plus tard, cette décision pourra être réévaluée explicitement.
