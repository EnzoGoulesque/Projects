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

> Section à enrichir à mesure que les commandes réellement utilisées sont validées.

## Avant toute commande susceptible de consommer

Vérifier :
- warehouse ;
- taille ;
- statut ;
- auto-suspend ;
- protection de coût ;
- volume traité.

Les commandes Snowflake seront ajoutées ici uniquement après validation dans le projet.

---

# dbt

> Section évolutive.

Commandes qui seront documentées lorsqu'elles auront été réellement installées et validées :

```text
dbt debug
dbt run
dbt test
dbt build
dbt compile
dbt docs generate
dbt docs serve
```

Pour chacune, documenter à terme :
- objectif ;
- contexte d'utilisation ;
- impact Snowflake ;
- risque de coût ;
- exemple sûr.

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
