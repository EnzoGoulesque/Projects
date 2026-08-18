# Playbook — Commandes et procédures réutilisables

Ce fichier est la bibliothèque technique du projet.

Il contient les commandes et procédures qui méritent d'être réutilisées.

Il ne doit contenir aucun secret.

## Règle d'enrichissement

Une commande ou procédure est ajoutée ici lorsqu'elle :
- est utilisée plusieurs fois ;
- est facile à oublier ;
- permet d'éviter une erreur ;
- accélère un workflow ;
- devient une pratique standard du projet.

Ne pas ajouter chaque commande ponctuelle.

---

# Git

## Vérifier l'état

```bash
cd ~/projects
git status
```

## Récupérer la dernière version distante

```bash
git pull --ff-only
```

## Voir les modifications locales

```bash
git diff
```

## Préparer uniquement le projet Architecture de données

```bash
git add architecture-donnees/
```

## Vérifier ce qui sera commité

```bash
git status
git diff --staged
```

## Retirer un fichier du staging

```bash
git restore --staged chemin/du/fichier
```

## Commit

```bash
git commit -m "type: description"
```

Types fréquents :

```text
docs:
feat:
fix:
refactor:
test:
chore:
```

## Push

```bash
git push
```

## Historique

```bash
git log --oneline -10
```

## Vérifier le remote

```bash
git remote -v
```

## Vérifier SSH GitHub

```bash
ssh -T git@github.com
```

---

# WSL / Ubuntu

## Aller aux projets

```bash
cd ~/projects
```

## Aller au projet Data

```bash
cd ~/projects/architecture-donnees
```

## Ouvrir le projet dans VS Code

```bash
code .
```

## Voir l'emplacement courant

```bash
pwd
```

## Voir les fichiers

```bash
ls -la
```

---

# Mémoire du projet

## Ouvrir directement le guide de session

Depuis le projet :

```bash
code memory/SESSION_GUIDE.md
```

## Fichiers à fournir en priorité à un agent

```text
memory/HANDOFF.md
memory/PROJECT.md
memory/ROADMAP.md
memory/DECISIONS.md
memory/SECURITY.md
```

---

# Snowflake

## Avant toute commande susceptible de consommer

Vérifier :
- identifiant du compte actif : compte personnel du projet ;
- rôle actif ;
- warehouse ;
- taille ;
- statut ;
- auto-suspend ;
- protection de coût ;
- volume traité.

Warehouse du projet :

```text
ARCHITECTURE_DONNEES_WH
X-Small / Gen1
auto-suspend = 300 s
statement timeout = 600 s
```

Ne pas utiliser `COMPUTE_WH` par défaut pour ce projet.

## Contexte de travail

```sql
USE ROLE ARCHITECTURE_DONNEES_ROLE;
USE DATABASE ARCHITECTURE_DONNEES;
USE SCHEMA RAW;
USE WAREHOUSE ARCHITECTURE_DONNEES_WH;
```

`USE WAREHOUSE` sélectionne le warehouse ; une opération nécessitant du compute peut ensuite déclencher son auto-resume.

## Vérifier le warehouse dédié

```sql
SHOW WAREHOUSES LIKE 'ARCHITECTURE_DONNEES_WH';
```

À contrôler notamment :
- état `SUSPENDED` hors utilisation ;
- taille X-Small ;
- auto-suspend ;
- auto-resume.

## Suspendre explicitement après une courte session de compute

```sql
USE ROLE SYSADMIN;
ALTER WAREHOUSE ARCHITECTURE_DONNEES_WH SUSPEND;
SHOW WAREHOUSES LIKE 'ARCHITECTURE_DONNEES_WH';
USE ROLE ARCHITECTURE_DONNEES_ROLE;
```

## Contrôle de la première table RAW

Après validation du chargement :

```sql
USE ROLE ARCHITECTURE_DONNEES_ROLE;
USE DATABASE ARCHITECTURE_DONNEES;
USE SCHEMA RAW;
USE WAREHOUSE ARCHITECTURE_DONNEES_WH;

SELECT COUNT(*) AS NUMBER_OF_ROWS
FROM ORDERS;

SELECT *
FROM ORDERS
ORDER BY ORDER_ID;
```

Ces `SELECT` consomment du compute si le warehouse doit être repris. Signaler le risque avant exécution et suspendre ensuite le warehouse.

## Diagnostiquer un chargement qui tarde

Dans Snowsight :

```text
Monitoring
  → Query History
  → filtrer sur ARCHITECTURE_DONNEES_WH
```

Vérifier le statut avant toute nouvelle tentative. Utiliser `Copy History` si nécessaire pour confirmer l'activité de chargement.

## Diagnostiquer le wizard `Load Data into Table`

Si le wizard tourne indéfiniment :

1. ne pas multiplier les clics `Load` / `Next` ;
2. vérifier `Monitoring → Query History` ;
3. confirmer que le rôle et le warehouse fonctionnent avec une petite requête directe ;
4. vérifier si un `COPY INTO` a réellement été soumis ;
5. si les tests SQL réussissent mais qu'aucun `COPY INTO` n'apparaît, considérer le problème comme situé avant la soumission du chargement et utiliser un autre chemin d'ingestion.

Test de contexte déjà validé pour ce projet :

```sql
USE ROLE ARCHITECTURE_DONNEES_ROLE;
USE WAREHOUSE ARCHITECTURE_DONNEES_WH;

SELECT
    CURRENT_ROLE(),
    CURRENT_WAREHOUSE(),
    1 AS TEST;
```

## État de la première ingestion

La première ingestion `orders.csv` a finalement réussi via Snowsight et `RAW.ORDERS` contient 12 lignes.

Snowflake CLI n'est donc pas une dépendance actuelle du projet. Ne l'ajouter que si un besoin futur concret justifie une méthode d'ingestion scriptée ou reproductible.

---

# Python / environnement dbt

## Activer l'environnement du projet

Depuis `~/projects/architecture-donnees` :

```bash
source .venv/bin/activate
```

Vérifier :

```bash
python --version
which python
dbt --version
```

Le Python actif doit pointer vers `.venv/bin/python`.

## Recréer l'environnement sur une nouvelle machine

Si le support `venv` manque sur Ubuntu 24.04 :

```bash
sudo apt update
sudo apt install python3.12-venv
```

Puis :

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.lock.txt
```

`requirements.txt` documente la dépendance principale choisie. `requirements.lock.txt` permet de reconstruire l'état exact validé.

---

# dbt

## Configuration locale

Répertoire local :

```bash
mkdir -p ~/.dbt
```

Le futur fichier :

```text
~/.dbt/profiles.yml
```

reste hors Git et ne doit contenir aucun secret versionné.

Projet dbt :

```text
dbt_project.yml
profile: architecture_donnees
models/staging
models/intermediate
models/marts
```

## Parsing local

Une fois le profil local présent :

```bash
dbt parse
```

Le parsing sert à valider la structure et le code du projet. Ne pas confondre une erreur de profil local avec une erreur de connexion Snowflake.

## Commandes connectées / potentiellement consommatrices

À traiter comme potentiellement consommatrices côté Snowflake :

```text
dbt debug
dbt run
dbt test
dbt build
```

Avant exécution :
- confirmer le compte personnel ;
- confirmer `ARCHITECTURE_DONNEES_ROLE` ;
- confirmer `ARCHITECTURE_DONNEES_WH` en X-Small ;
- vérifier le Resource Monitor ;
- vérifier le faible volume ;
- signaler explicitement le risque de coût.

Préparer le maximum localement, puis regrouper les commandes utiles dans une même session de compute. Après la séquence, suspendre explicitement le warehouse. Ne pas laisser le warehouse actif artificiellement pour « remplir » une minute.

Commandes à documenter après validation réelle :

```text
dbt compile
dbt docs generate
dbt docs serve
```

---

# SQL

> Section destinée aux requêtes ou patrons réutilisables.

Exemples futurs :
- inspection du grain ;
- détection de doublons ;
- contrôle des valeurs nulles ;
- comparaison avant/après transformation ;
- analyse du volume ;
- validation d'une clé.

Ne conserver ici que les patrons génériques sans données confidentielles.

---

# Tableau

> Section à enrichir lors de la phase Tableau.

Procédures futures possibles :
- connexion Snowflake ;
- vérification d'une source ;
- checklist performance ;
- Live vs Extract ;
- validation d'un mart.

---

# Docker

Non utilisé actuellement.

Si Docker entre dans le projet, créer ici les procédures récurrentes réellement nécessaires.

---

# Airflow

Non utilisé actuellement.

Si Airflow entre dans le projet, créer ici les procédures récurrentes réellement nécessaires.

---

# Règle de maintenance

À la fin d'une session, poser la question :

> Une commande ou une procédure utilisée aujourd'hui mérite-t-elle d'être retrouvée instantanément dans six mois ?

Si oui :
- l'ajouter dans la bonne catégorie ;
- expliquer brièvement son rôle ;
- ne jamais inclure de secret.
