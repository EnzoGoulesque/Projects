# Architecture du projet

## Architecture cible
```text
CSV / Parquet
    ↓
Snowflake RAW
    ↓
dbt
    ↓
STAGING
    ↓
INTERMEDIATE
    ↓
MARTS / SOURCES BI
    ↓
Tableau Desktop
```

## Environnement de développement
```text
Windows 11
├── Tableau Desktop
├── VS Code
│    ↓
│  WSL2 / Ubuntu 24.04
│    ├── Git
│    ├── SQL
│    ├── dbt
│    └── scripts éventuels
└── Navigateur
     ├── GitHub
     └── Snowflake
```

## Rôle des briques
### Snowflake
Stockage analytique, schemas/tables/vues, compute via warehouse, exécution du SQL généré par dbt.

### dbt
Sources, transformations SQL, dépendances, tests, documentation, lineage, matérialisations et préparation de la couche BI.

### Tableau Desktop
Connexion aux données préparées, KPI, visualisations, dashboards et validation des performances des marts.

## Briques futures
PostgreSQL, Docker, Airflow, Tableau Cloud, CI/CD et IA locale pourront être ajoutés selon les besoins.

## Règle d'évolution
Toute nouvelle brique doit répondre à :
1. Quel problème concret résout-elle ?
2. Où se place-t-elle dans le flux ?
3. Quel impact a-t-elle sur coûts, complexité, sécurité et autres briques ?
