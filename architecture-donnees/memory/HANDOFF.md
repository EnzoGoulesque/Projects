# Handoff — État actuel du projet

## Projet
**Nom :** Projet Architecture de données

## Objectif
```text
CSV / Parquet
    ↓
Snowflake
    ↓
dbt
    ↓
marts / sources BI
    ↓
Tableau Desktop
```

## État technique
- Windows 11 : prêt
- WSL2 : fonctionnel
- Ubuntu 24.04 LTS : fonctionnel
- VS Code ↔ WSL : fonctionnel
- Git : fonctionnel
- GitHub : fonctionnel
- SSH GitHub : validé
- Repository `Projects` : présent sous `/home/enzo/projects`

## Priorité actuelle
1. Versionner cette mémoire.
2. Démarrer Snowflake par les fondamentaux.
3. Mettre en place les protections de coût.
4. Charger un petit dataset.
5. Passer ensuite à dbt.

## Décisions à respecter
- Ne pas recréer Snowflake localement.
- Ne pas prioriser PostgreSQL.
- Ne pas installer Docker ou Airflow sans besoin concret.
- Signaler explicitement tout risque de coût Snowflake avant une action consommatrice.
- Conserver les secrets hors de GitHub.

## Ordre de lecture
1. `CONTEXT.md`
2. `ARCHITECTURE.md`
3. `ROADMAP.md`
4. `DECISIONS.md`
5. `LEARNINGS.md`
