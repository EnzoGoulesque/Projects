# Handoff — État actuel

Dernière mise à jour : 2026-08-17.

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
- WSL2 / Ubuntu 24.04
- VS Code ↔ WSL
- Git / GitHub / SSH
- repository `Projects`
- mémoire du projet simplifiée et versionnée

## Snowflake — socle validé

Un compte Snowflake personnel de test est opérationnel.

Protections et organisation mises en place :

```text
ARCHITECTURE_DONNEES_ROLE
        │
        ├── ARCHITECTURE_DONNEES
        │       └── RAW
        │              └── ORDERS (structure créée, données non chargées)
        │
        └── ARCHITECTURE_DONNEES_WH
                X-Small / Gen1
                auto-resume = true
                auto-suspend = 300 s
                statement timeout = 600 s
```

Éléments validés :

- un Resource Monitor de niveau compte a été configuré avant les premiers usages de compute ;
- `COMPUTE_WH` est suspendu et n'est pas le warehouse du projet ;
- `ARCHITECTURE_DONNEES_ROLE` est le rôle de travail du projet ;
- le rôle projet est rattaché à `SYSADMIN` ;
- la database `ARCHITECTURE_DONNEES` existe ;
- le schema `RAW` existe ;
- la table `ARCHITECTURE_DONNEES.RAW.ORDERS` a été créée manuellement ;
- le warehouse dédié `ARCHITECTURE_DONNEES_WH` existe avec une configuration prudente ;
- le rôle projet peut utiliser ce warehouse : le test `CURRENT_ROLE() / CURRENT_WAREHOUSE() / SELECT 1` a réussi ;
- le warehouse a été remis à l'état `SUSPENDED` en fin de session ;
- les commandes SQL sont exécutées dans Snowsight Workspaces.

## Donnée de test

Un dataset synthétique `orders.csv` de 12 lignes a été créé pour le projet.

Emplacement Git prévu / recommandé :

```text
data/sample/orders.csv
```

Ce fichier est fictif et peut être versionné. Vérifier avec `git status` qu'il est bien au bon emplacement et qu'il s'agit uniquement du dataset synthétique avant publication.

Aucun export professionnel ou dataset privé ne doit être ajouté à sa place.

## Incident actuel — chargement CSV via Snowsight

Le chargement de `orders.csv` dans :

```text
ARCHITECTURE_DONNEES.RAW.ORDERS
```

n'a **pas** abouti via l'assistant web Snowsight `Load Data into Table`.

Constats validés :

- le wizard reste en chargement indéfini ;
- le même comportement se produit pour la création depuis fichier et pour le chargement dans la table existante ;
- le warehouse peut être démarré manuellement et fonctionne ;
- `ARCHITECTURE_DONNEES_ROLE` peut exécuter du compute sur `ARCHITECTURE_DONNEES_WH` ;
- `CREATE TABLE ... RAW.ORDERS` a réussi ;
- Query History montre les requêtes SQL de diagnostic et de création comme `Success` ;
- aucune opération `COPY INTO` correspondant au chargement du CSV n'apparaît dans Query History ;
- Snowsight affiche aussi `Failed to update the default warehouse` lorsqu'on tente de sélectionner le warehouse dédié dans le wizard ;
- modifier le warehouse par défaut n'a pas résolu le blocage.

Conclusion de travail : le problème se situe vraisemblablement dans le chemin d'upload / wizard Snowsight avant la soumission du vrai chargement SQL, plutôt que dans la table, le rôle ou le fonctionnement du warehouse. L'emplacement WSL du CSV n'est pas démontré comme cause.

Ne pas continuer à relancer le wizard au hasard lors de la prochaine session.

## Prochaine étape immédiate

Contourner le wizard web et réaliser la première ingestion depuis WSL avec **Snowflake CLI** :

1. vérifier que l'identifiant Snowflake actif correspond au compte personnel ;
2. vérifier que `ARCHITECTURE_DONNEES_WH` est `SUSPENDED` avant de commencer ;
3. installer Snowflake CLI dans WSL de manière isolée (`pipx` si disponible) ;
4. configurer une connexion Snowflake hors du repository et sans secret versionné ;
5. valider la connexion CLI ;
6. utiliser un stage interne et envoyer `data/sample/orders.csv` depuis WSL ;
7. lancer ensuite `COPY INTO ARCHITECTURE_DONNEES.RAW.ORDERS` ;
8. vérifier que 12 lignes sont présentes ;
9. contrôler quelques lignes ;
10. suspendre explicitement le warehouse ;
11. seulement après validation de l'ingestion, clôturer la Phase 2 Snowflake et démarrer dbt.

### Avertissement coût pour la reprise

L'installation du CLI, sa configuration, la création d'un stage interne et l'envoi local vers un stage ne doivent pas être traités comme du compute de warehouse.

En revanche, **avant `COPY INTO`, tout `SELECT` de validation ou toute autre opération nécessitant le warehouse, signaler explicitement le risque de coût**, vérifier le compte personnel, le warehouse X-Small, le Resource Monitor et l'état du warehouse.

## Étape suivante après l'ingestion

Démarrer la Phase 3 **dbt** :

- environnement Python isolé ;
- adaptateur Snowflake ;
- connexion sécurisée ;
- `dbt debug` ;
- initialisation du projet ;
- déclaration de `RAW.ORDERS` comme source ;
- puis modèles staging, intermediate et marts.

## Barrière de sécurité Snowflake

Un compte Snowflake professionnel existe séparément avec la même adresse e-mail que le compte personnel.

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
