# Handoff — État actuel

## Projet

Projet Architecture de données.

## Vision courte

Construire un laboratoire :

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

## Environnement prêt

- Windows 11
- WSL2
- Ubuntu 24.04
- VS Code ↔ WSL
- Git
- GitHub
- SSH GitHub
- repository `Projects`

## État actuel

Le socle technique du PC et GitHub est terminé.

Le système de mémoire du projet est en cours de consolidation dans une version simplifiée.

## Prochaine étape fonctionnelle

Snowflake.

Avant tout run :
- comprendre les warehouses ;
- comprendre la consommation ;
- configurer les protections de coût ;
- utiliser de très faibles volumes.

## Priorités

1. Snowflake suffisamment maîtrisé pour travailler en sécurité.
2. dbt en profondeur.
3. Conception de marts adaptés à la BI.
4. Tableau Desktop.

## Non prioritaire

- PostgreSQL
- Docker
- Airflow
- Tableau Cloud
- IA locale

## Contraintes

- Aucun secret dans Git.
- Aucun dataset professionnel/confidentiel dans GitHub.
- Signaler explicitement les risques de coût Snowflake avant une action consommatrice.
- Toute nouvelle brique doit être évaluée par son impact sur l'architecture globale.

## Documents utiles

- Vision et architecture : `PROJECT.md`
- Progression : `ROADMAP.md`
- Décisions : `DECISIONS.md`
- Problèmes déjà rencontrés : `LEARNINGS.md`
- Règles sécurité : `SECURITY.md`
- Commandes : `PLAYBOOK.md`
- Routine de travail : `SESSION_GUIDE.md`
