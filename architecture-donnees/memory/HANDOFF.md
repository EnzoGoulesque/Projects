# Handoff — État actuel

Dernière mise à jour : 2026-08-18.

## Projet

Projet Architecture de données.

## Vision courte

Construire un laboratoire :

```text
CSV / Parquet
    ↓
Snowflake RAW
    ↓
dbt
    ↓
staging / intermediate / marts
    ↓
Tableau Desktop
```

## Environnement prêt

- Windows 11
- WSL2 / Ubuntu 24.04
- VS Code ↔ WSL
- Git / GitHub / SSH
- repository `Projects`
- mémoire du projet simplifiée et versionnée
- Python 3.12.3 dans Ubuntu
- environnement Python local `.venv` créé pour le projet
- `dbt-snowflake` installé dans `.venv`

## Snowflake — première ingestion validée

Le compte Snowflake personnel de test est opérationnel.

Organisation du projet :

```text
ARCHITECTURE_DONNEES_ROLE
        │
        ├── ARCHITECTURE_DONNEES
        │       └── RAW
        │              └── ORDERS (12 lignes chargées)
        │
        └── ARCHITECTURE_DONNEES_WH
                X-Small / Gen1
                auto-resume = true
                auto-suspend = 300 s
                statement timeout = 600 s
```

Éléments validés :

- un Resource Monitor de niveau compte a été configuré avant les premiers usages de compute ;
- `COMPUTE_WH` n'est pas le warehouse du projet ;
- `ARCHITECTURE_DONNEES_ROLE` est le rôle de travail du projet ;
- la database `ARCHITECTURE_DONNEES` existe ;
- le schema `RAW` existe ;
- la table `ARCHITECTURE_DONNEES.RAW.ORDERS` existe ;
- `orders.csv` a finalement été chargé avec succès via Snowsight ;
- la table contient exactement 12 lignes et les valeurs ont été contrôlées visuellement ;
- le warehouse dédié `ARCHITECTURE_DONNEES_WH` a été suspendu manuellement après le chargement ;
- le rôle projet et le warehouse avaient déjà été validés avec `CURRENT_ROLE() / CURRENT_WAREHOUSE() / SELECT 1`.

### Incident Snowsight — état final

Lors d'une session précédente, le wizard `Load Data into Table` restait bloqué avant qu'un `COPY INTO` soit visible dans Query History. Le diagnostic était cohérent avec un blocage dans le chemin du wizard plutôt qu'avec un défaut de table, rôle ou warehouse.

Lors de la reprise du 2026-08-18, le chargement a finalement réussi via Snowsight. La cause exacte du blocage initial n'a pas été identifiée.

Conclusion durable : conserver Query History comme outil de diagnostic, mais ne plus considérer Snowflake CLI comme nécessaire pour cette première ingestion. Snowflake CLI reste une brique optionnelle à n'ajouter que si un besoin futur concret le justifie.

## dbt — état actuel

La Phase 3 est démarrée.

Environnement validé :

```text
Ubuntu 24.04
└── projet architecture-donnees
    └── .venv
        ├── Python 3.12.3
        ├── dbt Core 1.12.2
        └── dbt-snowflake 1.12.0
```

Travail réalisé :

- le paquet Ubuntu `python3.12-venv` a été installé car il manquait initialement ;
- `.venv` a été recréé proprement puis activé ;
- `which python` pointe bien vers `.venv/bin/python` ;
- `dbt-snowflake==1.12.0` est installé ;
- `requirements.txt` fixe la dépendance principale ;
- `requirements.lock.txt` conserve les versions exactes installées ;
- `dbt_project.yml` a été créé à la racine du projet avec le profil `architecture_donnees` ;
- la structure logique prévue est `models/staging`, `models/intermediate`, `models/marts` ;
- les trois fichiers dbt/reproductibilité ont été commités et poussés ;
- Git est revenu à un état propre ;
- `~/.dbt` existe désormais hors du repository.

Une première tentative de `dbt parse` a échoué uniquement parce que le répertoire `~/.dbt` n'existait pas encore. Ce répertoire a ensuite été créé. Aucun `profiles.yml` n'a encore été configuré et la connexion Snowflake dbt n'a donc pas encore été validée.

## Prochaine étape immédiate

Configurer la connexion dbt à Snowflake **hors Git** :

1. vérifier que `.venv` est actif ;
2. créer `~/.dbt/profiles.yml` avec une configuration sans secret versionné ;
3. utiliser le compte personnel du laboratoire, le rôle `ARCHITECTURE_DONNEES_ROLE`, la database `ARCHITECTURE_DONNEES` et le warehouse `ARCHITECTURE_DONNEES_WH` ;
4. protéger le fichier local de configuration ;
5. relancer `dbt parse` ;
6. préparer ensuite une session de connexion/compute cohérente ;
7. avant `dbt debug` ou toute commande pouvant utiliser Snowflake, vérifier compte, rôle, warehouse, Resource Monitor et état du warehouse ;
8. regrouper les commandes utiles dans une même fenêtre de compute puis suspendre explicitement le warehouse ;
9. déclarer `RAW.ORDERS` comme source dbt ;
10. créer le premier modèle `stg_orders`, puis les tests.

## Avertissement coût pour la reprise

La création ou modification locale de `~/.dbt/profiles.yml`, `dbt_project.yml`, YAML ou SQL ne consomme pas de compute Snowflake.

En revanche, **traiter `dbt debug`, `dbt run`, `dbt test`, `dbt build` et toute requête de validation comme potentiellement consommatrices**. Avant ces commandes, signaler explicitement le risque, vérifier le compte personnel, le warehouse X-Small, le Resource Monitor et le faible volume.

Pour limiter les reprises facturées, préférer une session cohérente : préparer localement d'abord, démarrer le compute une fois, enchaîner les vérifications utiles, puis suspendre. Ne pas maintenir le warehouse actif artificiellement lorsqu'il n'y a plus de travail utile.

## Barrière de sécurité Snowflake

Un compte Snowflake professionnel existe séparément du compte personnel.

Règle durable : **avant toute modification ou opération de compute, vérifier explicitement que l'identifiant du compte actif est celui du compte personnel du projet**.

Ne jamais copier dans la mémoire :
- mot de passe ;
- token ;
- clé ;
- identifiant privé inutile ;
- credential professionnel.

## Contraintes

- Aucun secret dans Git.
- Aucun dataset professionnel/confidentiel dans GitHub.
- `~/.dbt/profiles.yml` reste local et hors repository.
- `.venv/`, `target/`, `logs/` et `dbt_packages/` restent hors Git.
- Signaler explicitement les risques de coût Snowflake avant une action consommatrice.
- Utiliser le warehouse dédié du projet, pas `COMPUTE_WH`.
- Garder les volumes de test très faibles.
- Toute nouvelle brique doit être évaluée par son impact sur l'architecture globale.
- Ne pas changer une décision structurante sans expliquer pourquoi.

## Documents utiles

- Vision et architecture : `PROJECT.md`
- Progression : `ROADMAP.md`
- Décisions : `DECISIONS.md`
- Problèmes déjà rencontrés : `LEARNINGS.md`
- Règles sécurité : `SECURITY.md`
- Commandes : `PLAYBOOK.md`
- Routine de travail : `SESSION_GUIDE.md`
