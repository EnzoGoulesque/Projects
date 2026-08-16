# Contexte du projet — Architecture de données

## Objectif
Construire un laboratoire personnel reproduisant, à petite échelle, une architecture Data proche d'un projet professionnel.

Flux cible :
```text
Données de test
    ↓
Snowflake
    ↓
dbt
    ↓
Marts / sources BI
    ↓
Tableau Desktop
```

## Priorités
1. Comprendre Snowflake suffisamment pour l'utiliser proprement et limiter les coûts.
2. Approfondir dbt : sources, modèles, dépendances, tests, documentation, matérialisations et optimisation.
3. Concevoir des modèles adaptés à Tableau.
4. Utiliser Tableau Desktop pour valider la qualité et les performances des sources produites.

## Données d'entrée
La première version utilisera un petit jeu de données CSV ou Parquet. PostgreSQL pourra être ajouté plus tard.

## Hors du chemin critique
PostgreSQL, Docker, Airflow, Tableau Cloud, IA locale et CI/CD sont différés.

## Environnement déjà prêt
- Windows 11
- WSL2
- Ubuntu 24.04 LTS
- VS Code connecté à WSL
- Git dans Ubuntu
- GitHub connecté en SSH
- Repository `Projects`
- Dossier local `/home/enzo/projects`

## Règle permanente Snowflake
Avant toute opération susceptible d'utiliser du compute Snowflake :
1. identifier ce qui va consommer ;
2. vérifier la taille du warehouse ;
3. vérifier l'auto-suspend ;
4. signaler explicitement le risque de coût ;
5. privilégier de petits volumes.
