# Sécurité

## Principe

Le repository GitHub peut être public.

Tout fichier versionné doit donc être considéré comme publiable.

`memory/` est une documentation, pas un coffre-fort.

## Interdit dans Git

Ne jamais versionner :

- mot de passe ;
- token ;
- clé API ;
- clé SSH privée ;
- clé privée Snowflake ;
- fichier `.env` réel ;
- credentials dans `profiles.yml` ;
- certificats ou clés privées ;
- chaîne de connexion contenant un secret ;
- exports professionnels ;
- données clients ;
- données personnelles sensibles ;
- datasets confidentiels.

## Autorisé

- architecture ;
- décisions ;
- roadmap ;
- documentation ;
- commandes sans secrets ;
- noms génériques de schemas/tables/warehouses ;
- données fictives ;
- placeholders.

Exemple :

```text
<SNOWFLAKE_ACCOUNT>
<SNOWFLAKE_USER>
<WAREHOUSE_NAME>
<DATABASE_NAME>
```

## `.gitignore` recommandé pour le projet

Le `.gitignore` du projet doit au minimum couvrir :

```gitignore
# Secrets
.env
.env.*
!.env.example
*.pem
*.key
*.p12
*.pfx
credentials*.json
profiles.yml

# Python
.venv/
venv/
__pycache__/
*.py[cod]

# dbt
target/
logs/
dbt_packages/

# Données locales / sensibles
data/private/
data/raw/

# OS
.DS_Store
Thumbs.db
*Zone.Identifier
```

Cette liste doit évoluer avec les outils du projet.

## Nouvelle brique = nouvelle vérification

À l'ajout de Snowflake, dbt, Docker, Airflow ou d'un autre outil :

1. identifier les fichiers générés ;
2. identifier les credentials ;
3. identifier les fichiers locaux ;
4. décider ce qui doit être ignoré ;
5. mettre à jour `.gitignore` ;
6. mettre à jour ce fichier si nécessaire ;
7. ajouter une procédure récurrente au `PLAYBOOK.md` si utile.

## Contrôle avant commit

Toujours vérifier :

```bash
git status
git diff
git diff --staged
```

Questions :

- uniquement les fichiers attendus ?
- aucun secret ?
- aucune donnée professionnelle ?
- aucun fichier de configuration locale ?
- aucun dataset non publiable ?

## Fichier déjà suivi

Ajouter un fichier au `.gitignore` ne l'empêche pas d'être suivi s'il est déjà dans Git.

Pour arrêter de suivre un fichier tout en le gardant localement :

```bash
git rm --cached chemin/du/fichier
```

Puis vérifier le staging.

## Secret publié accidentellement

Si un secret atteint GitHub :

1. le considérer compromis ;
2. le révoquer ou le régénérer ;
3. ne pas considérer sa suppression dans un nouveau commit comme suffisante ;
4. nettoyer l'historique si nécessaire ;
5. documenter l'incident sans recopier le secret.

## Règle Snowflake

Avant une opération susceptible de consommer :

- préciser l'action ;
- expliquer ce qui déclenche le compute ;
- vérifier la taille du warehouse ;
- vérifier l'auto-suspend ;
- vérifier les limites/budgets disponibles ;
- travailler sur un faible volume ;
- signaler explicitement le risque avant exécution.


## Séparation des comptes Snowflake

Le compte personnel du laboratoire et un éventuel compte professionnel sont des environnements distincts.

Avant toute opération d'administration ou de compute :

1. vérifier l'identifiant du compte actif dans Snowsight ;
2. confirmer qu'il s'agit du compte personnel du projet ;
3. vérifier le rôle actif ;
4. vérifier le warehouse actif ou sélectionné.

Ne jamais stocker dans Git un mot de passe, token, secret, clé privée ou credential issu de l'un ou l'autre compte.

La mémoire peut documenter la règle de vérification, mais ne doit pas recopier un identifiant réel lorsque celui-ci n'est pas nécessaire à la compréhension du projet.

## Données de test versionnées

`data/sample/` est réservé aux datasets explicitement fictifs et publiables.

Le fait qu'un CSV d'exemple soit versionné ne change pas l'interdiction concernant :
- exports professionnels ;
- données clients ;
- données personnelles sensibles ;
- datasets confidentiels.

## dbt — profil local et credentials

La configuration de connexion dbt doit vivre dans :

```text
~/.dbt/profiles.yml
```

Ce fichier est **hors du repository** et ne doit jamais être copié dans `memory/` avec des valeurs réelles.

Règles :
- ne jamais versionner un mot de passe, token, clé privée ou secret Snowflake ;
- conserver `profiles.yml` hors Git même si une règle `.gitignore` existe ;
- privilégier les variables d'environnement ou mécanismes d'authentification sûrs lorsqu'ils sont adaptés ;
- limiter les permissions du fichier local lorsque le profil sera créé ;
- ne versionner qu'un éventuel exemple avec placeholders ;
- ne jamais recopier dans une conversation ou dans la mémoire un credential réel ;
- avant `dbt debug`, `dbt run`, `dbt test`, `dbt build` ou toute commande pouvant utiliser Snowflake, appliquer la barrière compte personnel / rôle / warehouse / Resource Monitor / volume.

Les fichiers `requirements.txt`, `requirements.lock.txt` et `dbt_project.yml` sont publiables tant qu'ils ne contiennent aucun secret. `.venv/`, `target/`, `logs/` et `dbt_packages/` doivent rester hors Git.

## Snowflake CLI — si ajouté plus tard

Snowflake CLI n'est pas nécessaire à l'état actuel du projet. S'il devient utile plus tard :

- évaluer d'abord son impact architectural ;
- conserver configuration et credentials hors du repository ;
- ne jamais copier un secret dans `memory/`, un script versionné ou un exemple de commande ;
- privilégier les mécanismes d'authentification sûrs proposés par Snowflake ;
- appliquer la même barrière de sécurité et de coût que dans Snowsight et dbt.
