# Roadmap

## Phase 0 — Poste de travail

- [x] Windows 11 prêt
- [x] BIOS mis à jour
- [x] WSL2 installé
- [x] Ubuntu 24.04 LTS installé
- [x] VS Code connecté à WSL
- [x] Git fonctionnel
- [x] GitHub configuré
- [x] Authentification SSH GitHub fonctionnelle
- [x] Repository `Projects` synchronisé en local

## Phase 1 — Système de mémoire

- [x] Mémoire versionnée dans le projet
- [x] Règles de sécurité définies
- [x] Guide de session créé
- [x] Bibliothèque de commandes structurée
- [ ] Remplacer l'ancienne structure memory par cette version optimisée
- [ ] Commit et push de la version optimisée

## Phase 2 — Snowflake : fondamentaux

- [ ] Préparer / vérifier le compte Snowflake
- [ ] Comprendre account / user / role
- [ ] Comprendre database / schema / table / view
- [ ] Comprendre warehouse / compute
- [ ] Identifier les mécanismes de contrôle des coûts
- [ ] Configurer les protections adaptées
- [ ] Charger un petit CSV ou Parquet
- [ ] Créer une première zone RAW

## Phase 3 — dbt : priorité principale

- [ ] Créer un environnement isolé pour dbt
- [ ] Installer l'adaptateur Snowflake
- [ ] Configurer la connexion Snowflake
- [ ] Valider la connexion
- [ ] Initialiser le projet dbt
- [ ] Déclarer les sources
- [ ] Créer les modèles staging
- [ ] Créer les modèles intermediate
- [ ] Créer les marts
- [ ] Ajouter les tests
- [ ] Ajouter la documentation
- [ ] Étudier les matérialisations
- [ ] Étudier coûts et performances
- [ ] Optimiser les requêtes pertinentes

## Phase 4 — Conception BI

- [ ] Définir le grain des tables finales
- [ ] Identifier dimensions et mesures
- [ ] Définir les KPI
- [ ] Identifier les transformations à faire dans dbt
- [ ] Identifier les calculs à laisser dans Tableau
- [ ] Réduire les jointures inutiles côté BI
- [ ] Préparer les sources Tableau

## Phase 5 — Tableau Desktop

- [ ] Installer / vérifier Tableau Desktop
- [ ] Connecter Tableau à Snowflake
- [ ] Créer un premier modèle Tableau
- [ ] Construire des KPI
- [ ] Construire un dashboard
- [ ] Évaluer les performances
- [ ] Corriger les marts dbt si nécessaire

## Extensions

- [ ] PostgreSQL
- [ ] Docker
- [ ] Airflow
- [ ] Tableau Cloud
- [ ] CI/CD
- [ ] IA locale

## Prochaine étape

Commencer Snowflake par :
1. le fonctionnement général ;
2. les objets principaux ;
3. le compute ;
4. les coûts ;
5. les protections anti-surcoût.

Aucun run Snowflake consommateur ne doit être effectué avant d'avoir validé ces points.
