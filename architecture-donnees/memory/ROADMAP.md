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
- [x] Remplacer l'ancienne structure memory par cette version optimisée
- [x] Commit et push de la version optimisée

## Phase 2 — Snowflake : fondamentaux

- [x] Préparer / vérifier le compte Snowflake
- [x] Comprendre account / user / role
- [ ] Comprendre database / schema / table / view
- [x] Comprendre warehouse / compute
- [x] Identifier les mécanismes de contrôle des coûts
- [x] Configurer les protections adaptées
- [x] Créer la database `ARCHITECTURE_DONNEES` et le schema `RAW`
- [x] Créer le rôle projet `ARCHITECTURE_DONNEES_ROLE`
- [x] Créer et sécuriser le warehouse `ARCHITECTURE_DONNEES_WH`
- [x] Créer la structure de table `RAW.ORDERS`
- [x] Charger `orders.csv` et valider les 12 lignes
- [ ] Finaliser la compréhension table / view à partir du dataset chargé

> L'objectif pratique d'ingestion RAW est validé. La compréhension `table / view` reste un point pédagogique à clôturer ; elle ne bloque pas le démarrage de dbt.

## Phase 3 — dbt : priorité principale

- [x] Créer un environnement isolé pour dbt
- [x] Installer l'adaptateur Snowflake
- [ ] Configurer la connexion Snowflake
- [ ] Valider la connexion
- [x] Initialiser le projet dbt
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

Continuer la Phase 3 dbt sans lancer inutilement le compute Snowflake :

1. activer `.venv` ;
2. créer et sécuriser `~/.dbt/profiles.yml` hors Git ;
3. configurer le profil `architecture_donnees` avec le compte personnel, le rôle projet, la database et le warehouse dédiés ;
4. relancer `dbt parse` ;
5. préparer la source `RAW.ORDERS` et le premier modèle staging localement ;
6. avant la première commande connectée/consommatrice, vérifier compte personnel, rôle, warehouse X-Small, Resource Monitor et état `SUSPENDED` ;
7. lancer une séquence courte et cohérente de validation (`dbt debug`, puis commandes utiles) ;
8. suspendre explicitement le warehouse après la session ;
9. valider ensuite `stg_orders` et ses premiers tests.

**Coût :** les fichiers dbt locaux ne consomment pas de compute Snowflake. Signaler explicitement le risque avant `dbt debug`, `dbt run`, `dbt test`, `dbt build` ou toute requête de contrôle pouvant reprendre le warehouse.
