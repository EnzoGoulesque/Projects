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
- [ ] Charger `orders.csv` et valider les 12 lignes
- [ ] Finaliser la compréhension table / view à partir du dataset chargé

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

Terminer la première ingestion Snowflake sans réutiliser le wizard Snowsight qui bloque :

1. vérifier que le compte actif est le compte personnel ;
2. vérifier que `ARCHITECTURE_DONNEES_WH` est `SUSPENDED` ;
3. installer et configurer Snowflake CLI dans WSL, hors Git pour les credentials ;
4. charger `data/sample/orders.csv` via stage interne + `PUT` + `COPY INTO` ;
5. confirmer exactement 12 lignes dans `ARCHITECTURE_DONNEES.RAW.ORDERS` ;
6. suspendre le warehouse après contrôle ;
7. clôturer la Phase 2 uniquement lorsque l'ingestion est validée.

Ensuite, démarrer la Phase 3 dbt : environnement isolé, adaptateur Snowflake, connexion, `dbt debug`, initialisation et déclaration de la source RAW.

**Coût :** signaler explicitement le risque avant `COPY INTO` et les requêtes de validation, car ces opérations peuvent démarrer / utiliser le warehouse.
