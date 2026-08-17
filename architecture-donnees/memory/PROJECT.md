# Projet — Architecture de données

## Vision

Construire un laboratoire Data personnel, à petite échelle, mais suffisamment proche d'une architecture professionnelle pour comprendre les interactions entre les différentes briques.

Le but n'est pas de reproduire les volumes de production.

Le but est de reproduire :
- les responsabilités ;
- les connexions ;
- les transformations ;
- les bonnes pratiques ;
- les impacts de performance ;
- les impacts de coût ;
- la consommation BI.

## Architecture cible actuelle

```text
Données de test
CSV / Parquet
      │
      ▼
Snowflake
RAW / sources
      │
      │ dbt
      ▼
STAGING
      │
      ▼
INTERMEDIATE
      │
      ▼
MARTS / SOURCES BI
      │
      ▼
Tableau Desktop
      │
      ▼
Reporting / dashboards
```

## Priorités pédagogiques

### Priorité 1 — Snowflake

Comprendre suffisamment Snowflake pour :
- comprendre database / schema / table / view ;
- comprendre users / roles ;
- comprendre les connexions ;
- comprendre les warehouses ;
- comprendre ce qui consomme du compute ;
- maîtriser les principales protections de coût ;
- charger un petit dataset de test.

Snowflake n'est pas le sujet pédagogique principal, mais il est la plateforme réelle sur laquelle dbt travaillera.

### Priorité 2 — dbt

dbt est le cœur pédagogique du projet.

Objectifs :
- déclarer des sources ;
- utiliser `ref()` ;
- comprendre le lineage ;
- créer des modèles staging ;
- créer des modèles intermediate ;
- créer des marts ;
- écrire des tests ;
- documenter ;
- comprendre les matérialisations ;
- comprendre l'impact d'un modèle sur Snowflake ;
- optimiser la logique SQL ;
- produire une couche BI réellement utile.

### Priorité 3 — Conception BI

Comprendre comment préparer les données pour Tableau :
- grain ;
- dimensions ;
- mesures ;
- faits ;
- jointures ;
- pré-calculs ;
- logique métier ;
- réutilisabilité ;
- performances.

### Priorité 4 — Tableau Desktop

Utiliser les marts produits pour :
- créer des KPI ;
- créer des visualisations ;
- construire un dashboard ;
- évaluer la simplicité d'utilisation des sources ;
- identifier les transformations qui devraient être déplacées vers dbt.

## Données d'entrée

La première version du projet utilise de petits fichiers :
- CSV ;
- ou Parquet.

Une source PostgreSQL pourra être ajoutée plus tard.

L'ingestion depuis PostgreSQL n'est actuellement pas prioritaire.

## Briques différées

Les briques suivantes font partie de l'architecture potentielle mais ne sont pas dans le chemin critique :

- PostgreSQL ;
- Docker ;
- Airflow ;
- Tableau Cloud ;
- CI/CD ;
- IA locale.

Elles ne doivent être ajoutées que lorsqu'un besoin concret le justifie.

## Environnement de développement

```text
Windows 11
│
├── Tableau Desktop
├── VS Code
│    │
│    ▼
│  WSL2 / Ubuntu 24.04
│    ├── Git
│    ├── SQL
│    ├── dbt
│    ├── scripts éventuels
│    └── outils par projet
│
└── Navigateur
     ├── GitHub
     └── Snowflake
```

## Organisation locale

Repository Git principal :

```text
~/projects
```

Projet :

```text
~/projects/architecture-donnees
```

La mémoire se trouve dans :

```text
~/projects/architecture-donnees/memory
```

## Principes d'architecture

### Modularité

Une nouvelle brique ne doit pas être installée uniquement parce qu'elle est populaire.

Elle doit résoudre un problème concret.

### Reproductibilité

Le code et les configurations publiables doivent être versionnés.

### Isolation

Les dépendances et secrets locaux doivent rester séparés du code publié.

### Sécurité

Le repository peut être public.

Les secrets sont donc toujours considérés comme externes à Git.

### Coût Snowflake

Avant toute opération susceptible d'utiliser du compute Snowflake :
1. identifier ce qui va consommer ;
2. vérifier le warehouse ;
3. vérifier les protections de coût ;
4. signaler le risque avant exécution ;
5. privilégier un dataset réduit.

## Critère de réussite

Le projet atteint une première version réussie lorsque le flux suivant fonctionne :

```text
petit dataset
      ↓
Snowflake RAW
      ↓
dbt
      ↓
marts propres
      ↓
Tableau Desktop
      ↓
dashboard exploitable
```

et que chaque brique est comprise suffisamment pour expliquer son rôle, ses limites et son impact sur les autres.
