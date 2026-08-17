# Guide de session

> Ce fichier est conçu pour rester ouvert dans VS Code pendant le travail.

Il doit être suffisamment complet pour ne pas avoir besoin de redemander la routine Git de début ou de fin de session.

---

# 1. Principe général

```text
GitHub
= dernière version validée

Local
= travail en cours

Conversation IA
= espace temporaire

memory/
= compréhension durable du projet
```

Un agent peut proposer ou modifier.

La publication reste sous contrôle humain.

---

# 2. Début de session

## Étape A — Aller à la racine Git

```bash
cd ~/projects
```

## Étape B — Vérifier l'état local

```bash
git status
```

### Si le dépôt est propre

Continuer.

### Si des modifications sont présentes

Ne pas lancer automatiquement un pull.

Identifier :
- modification volontaire ?
- ancienne session ?
- nouveau fichier ?
- secret ?
- modification d'un autre projet ?

## Étape C — Synchroniser GitHub

Lorsque l'état local est compris et propre :

```bash
git pull --ff-only
```

Si la commande échoue, ne pas forcer.

Comprendre la divergence avant toute action.

## Étape D — Ouvrir le projet

```bash
cd ~/projects/architecture-donnees
code .
```

---

# 3. Préparer l'agent IA

## Si l'agent peut lire le repository

Utiliser :

```text
Tu travailles sur le projet "Architecture de données".

Avant toute proposition :
1. lis memory/HANDOFF.md ;
2. lis memory/PROJECT.md ;
3. lis memory/ROADMAP.md ;
4. lis memory/DECISIONS.md ;
5. lis memory/SECURITY.md.

Utilise ces fichiers comme source de vérité.

Ensuite :
- résume l'état actuel ;
- identifie la prochaine étape ;
- signale les incohérences éventuelles ;
- ne change pas une décision structurante sans l'expliquer ;
- ne place jamais de secret ou donnée privée dans Git ;
- signale explicitement les risques de coût Snowflake avant toute opération consommatrice ;
- évalue toute nouvelle brique par son impact sur l'architecture globale.
```

## Si l'agent ne peut pas lire le repository

Fournir au minimum :
- `HANDOFF.md`
- `PROJECT.md`
- `ROADMAP.md`
- `DECISIONS.md`
- `SECURITY.md`

Ajouter `LEARNINGS.md` et `PLAYBOOK.md` si la tâche le nécessite.

---

# 4. Pendant la session

## Travailler d'abord, documenter ensuite

Ne pas transformer la mémoire en transcription de conversation.

Documenter uniquement ce qui aura une valeur future.

## Quand mettre à jour quel fichier ?

### Vision / périmètre / architecture
→ `PROJECT.md`

### Progression
→ `ROADMAP.md`

### État actuel pour le prochain agent
→ `HANDOFF.md`

### Choix structurant
→ `DECISIONS.md`

### Problème ou découverte réutilisable
→ `LEARNINGS.md`

### Nouvelle règle sécurité
→ `SECURITY.md`

### Nouvelle commande récurrente
→ `PLAYBOOK.md`

### Nouvelle routine de travail
→ `SESSION_GUIDE.md`

---

# 5. Amélioration continue

À chaque récurrence, se demander :

## Commande répétée ?

Exemple :
- commande dbt ;
- requête SQL de diagnostic ;
- commande Git ;
- inspection Snowflake.

Si elle sera utile à nouveau :
→ ajouter à `PLAYBOOK.md`, dans la bonne catégorie.

## Nouvelle routine répétée ?

Exemple :
- nouvelle procédure de validation dbt ;
- nouvelle procédure avant un run Snowflake ;
- nouvelle checklist Tableau.

Si elle structure une session :
→ ajouter à `SESSION_GUIDE.md`.

Si elle est purement technique :
→ ajouter à `PLAYBOOK.md`.

## Nouveau type de fichier sensible ?

→ mettre à jour :
- `.gitignore`
- `SECURITY.md`
- éventuellement `SESSION_GUIDE.md`

## Nouvelle brique technique ?

Se demander :

```text
PROJECT.md ?
DECISIONS.md ?
SECURITY.md ?
PLAYBOOK.md ?
SESSION_GUIDE.md ?
```

Ne modifier que ce qui est réellement impacté.

---

# 6. Commandes de diagnostic utiles pendant une session

## Emplacement

```bash
pwd
```

## Fichiers

```bash
ls -la
```

## État Git

```bash
cd ~/projects
git status
```

## Modifications non stagées

```bash
git diff
```

## Modifications stagées

```bash
git diff --staged
```

## Historique

```bash
git log --oneline -10
```

Pour davantage de commandes :
→ `PLAYBOOK.md`

---

# 7. Avant de clôturer une étape significative

Demander à l'agent :

```text
Nous arrivons à la fin de cette étape.

À partir uniquement de ce qui a été réellement validé :

1. identifie les fichiers memory qui doivent être mis à jour ;
2. ne modifie pas les fichiers inutiles ;
3. mets HANDOFF.md à jour avec l'état réel et la prochaine étape ;
4. mets ROADMAP.md à jour uniquement pour les étapes réellement terminées ;
5. ajoute les décisions structurantes à DECISIONS.md ;
6. ajoute les apprentissages réutilisables à LEARNINGS.md ;
7. modifie PROJECT.md seulement si la vision ou l'architecture change ;
8. modifie SECURITY.md si de nouveaux secrets, fichiers sensibles ou risques apparaissent ;
9. ajoute à PLAYBOOK.md les commandes/procédures qui deviennent récurrentes ;
10. modifie SESSION_GUIDE.md si notre manière de travailler évolue durablement ;
11. ne place aucun secret, credential ou donnée privée dans la mémoire.

Termine par une liste courte des fichiers modifiés et pourquoi.
```

---

# 8. Contrôle humain avant commit

Retour :

```bash
cd ~/projects
```

## Étape A — Voir tout ce qui a changé

```bash
git status
git diff
```

## Étape B — Ajouter uniquement le projet Data

```bash
git add architecture-donnees/
```

## Étape C — Contrôle obligatoire

```bash
git status
git diff --staged
```

Vérifier :

### Périmètre
- [ ] uniquement le projet attendu ;
- [ ] aucun ancien projet modifié par erreur ;
- [ ] aucun fichier généré inutilement.

### Secrets
- [ ] aucun mot de passe ;
- [ ] aucun token ;
- [ ] aucune API key ;
- [ ] aucune clé privée ;
- [ ] aucun `.env` réel ;
- [ ] aucun credential Snowflake ;
- [ ] aucune chaîne de connexion sensible.

### Données
- [ ] aucune donnée professionnelle confidentielle ;
- [ ] aucune donnée client ;
- [ ] aucune donnée personnelle sensible ;
- [ ] dataset publié explicitement fictif ou publiable.

### Mémoire
- [ ] `HANDOFF.md` reflète l'état réel ;
- [ ] `ROADMAP.md` n'anticipe pas des étapes non terminées ;
- [ ] décisions importantes documentées ;
- [ ] problèmes réutilisables documentés ;
- [ ] nouveaux risques de sécurité couverts ;
- [ ] nouvelles commandes récurrentes rangées dans `PLAYBOOK.md`.

---

# 9. Si un fichier ne doit pas partir

```bash
git restore --staged chemin/du/fichier
```

Puis :

```bash
git status
git diff --staged
```

---

# 10. Commit

Exemple :

```bash
git commit -m "docs: update project memory"
```

Préfixes utiles :

```text
docs:
feat:
fix:
refactor:
test:
chore:
```

---

# 11. Push

```bash
git push
```

Puis :

```bash
git status
```

État recherché :

```text
nothing to commit, working tree clean
```

---

# 12. Vérification GitHub après changement important

Après une étape significative :

1. ouvrir le repository dans le navigateur ;
2. vérifier le dernier commit ;
3. vérifier les fichiers modifiés ;
4. confirmer l'absence de fichier sensible.

Ce contrôle est particulièrement recommandé après :
- ajout d'un nouvel outil ;
- ajout de données ;
- ajout de configuration Snowflake ;
- ajout de dbt ;
- modification du `.gitignore`.

---

# 13. Fin de session

Avant de quitter :

```text
État Git propre ?
Mémoire à jour ?
Prochaine étape claire ?
Nouvelle commande récurrente ajoutée au Playbook ?
Nouvelle règle sécurité documentée ?
Aucun secret publié ?
```

Si oui, la session est terminée.

---

# 14. Routine condensée

```bash
cd ~/projects
git status
git pull --ff-only

cd architecture-donnees
code .
```

Agent :
```text
Lis HANDOFF, PROJECT, ROADMAP, DECISIONS et SECURITY.
```

Travail.

Puis :

```bash
cd ~/projects
git status
git diff
git add architecture-donnees/
git status
git diff --staged
```

Contrôle humain.

Puis :

```bash
git commit -m "type: description"
git push
git status
```

---

# 15. Règle finale

Le guide doit évoluer si la routine réelle évolue.

Le Playbook doit évoluer si les commandes réelles deviennent récurrentes.

La mémoire ne doit jamais devenir une archive de tout ce qui s'est passé.

Elle doit rester une représentation courte, fiable et utile de ce qu'il faut savoir pour continuer correctement le projet.
