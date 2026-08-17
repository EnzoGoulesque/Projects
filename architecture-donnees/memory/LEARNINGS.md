# Apprentissages, problèmes et solutions

Ce fichier conserve uniquement les informations qui évitent de répéter une erreur ou qui apportent une compréhension durable.

Format recommandé :

```text
## Sujet

Problème / découverte :
...

Cause / explication :
...

Solution / règle :
...
```

---

## WSL2 — emplacement des projets

**Découverte**  
Les projets Linux sont stockés sous `~/projects`.

**Règle**  
Éviter d'héberger les projets techniques principaux sous `/mnt/c/...`.

---

## VS Code — `code .`

**Découverte**  
La commande :

```bash
code .
```

ouvre dans VS Code le dossier courant.

**Utilité**  
Permet de lancer VS Code directement dans le bon projet WSL.

---

## GitHub — SSH

**Découverte**  
L'authentification SSH entre Ubuntu et GitHub est opérationnelle.

Commande de vérification :

```bash
ssh -T git@github.com
```

---

## Git — vérifier avant publication

**Règle**  
Avant un commit/push :

```bash
git status
git diff
git diff --staged
```

**Pourquoi**  
Éviter de publier :
- mauvais fichier ;
- secret ;
- donnée sensible ;
- modification non souhaitée ;
- modification d'un autre projet.

---

## Snowflake — modèle mental initial

```text
dbt
= organisation / génération du SQL

Snowflake
= stockage + exécution du SQL
```

Le compute Snowflake doit être traité comme une ressource potentiellement payante.

---

## Documentation — principe d'amélioration continue

Lorsqu'un comportement devient récurrent :

- nouvelle commande réutilisable → `PLAYBOOK.md`
- nouvelle routine de session → `SESSION_GUIDE.md`
- nouvelle décision structurante → `DECISIONS.md`
- nouvelle règle de sécurité → `SECURITY.md`
- nouveau problème résolu → `LEARNINGS.md`
- changement d'architecture → `PROJECT.md`
- changement de progression → `ROADMAP.md`
- changement d'état courant → `HANDOFF.md`

La documentation doit évoluer uniquement lorsque la récurrence ou l'importance le justifie.
