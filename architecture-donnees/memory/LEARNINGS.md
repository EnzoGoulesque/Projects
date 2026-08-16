# Apprentissages, problèmes et solutions

## Environnement
### WSL2
Les projets Linux sont stockés sous `/home/enzo/projects`, pas sous `/mnt/c/...`.

### VS Code
`code .` ouvre dans VS Code le dossier courant du terminal.

### GitHub / SSH
Connexion SSH validée avec :
```bash
ssh -T git@github.com
```

## Snowflake
Snowflake sépare conceptuellement stockage et compute.

### Vigilance coût
Avant les tests :
- utiliser le plus petit warehouse adapté ;
- configurer l'auto-suspend ;
- éviter du compute inutile ;
- travailler sur de faibles volumes ;
- mettre en place des protections de consommation.

Les paramètres précis seront revalidés avec la documentation officielle au moment de la configuration.

## dbt
```text
dbt = organisation / génération / gouvernance du SQL
Snowflake = stockage + exécution du SQL
```

L'objectif est de comprendre le grain, les consommateurs, les matérialisations et l'impact coût/performance de chaque modèle.

## Tableau
Les marts doivent fournir des dimensions et mesures claires, limiter les jointures inutiles et déplacer les calculs au bon niveau.
