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

---

## Windows / Git — fichiers `Zone.Identifier`

**Problème / découverte**  
Des fichiers `*:Zone.Identifier` peuvent apparaître comme de vrais fichiers lors de certains passages Windows / WSL / archives alors qu'ils proviennent du marquage de sécurité Windows.

**Solution / règle**  
Ils ne font pas partie du projet. Les supprimer lorsqu'ils sont matérialisés comme fichiers et les ignorer dans Git avec une règle couvrant `*Zone.Identifier`.

Ne pas désactiver globalement les protections Windows uniquement pour éviter cet affichage.

---

## Snowflake — même e-mail, comptes distincts

**Découverte**  
La même adresse e-mail peut être utilisée sur plusieurs comptes Snowflake distincts affichés dans Snowsight.

**Règle**  
L'adresse e-mail ne suffit pas à identifier l'environnement. Vérifier l'identifiant du compte actif avant une opération d'administration ou de compute.

---

## Snowflake — warehouse sélectionné vs warehouse actif

**Découverte**  
Un warehouse affiché dans le contexte d'un Workspace ou d'un assistant Snowsight peut être sélectionné sans être nécessairement en cours d'exécution.

**Règle**  
Avant un chargement ou une requête consommatrice, vérifier explicitement :
- le nom du warehouse ;
- sa taille ;
- son statut ;
- l'auto-suspend ;
- le garde-fou de coût.

Pour ce projet, utiliser `ARCHITECTURE_DONNEES_WH` et non le warehouse générique `COMPUTE_WH`.

---

## Snowflake — chargement qui semble bloqué

**Découverte**  
La reprise d'un warehouse prend souvent quelques secondes mais peut parfois prendre plus longtemps pendant le provisionnement. Pour un fichier minuscule, un chargement de plusieurs minutes mérite toutefois une vérification plutôt qu'un second clic immédiat.

**Règle**  
Ne pas relancer un chargement tant que son état n'est pas connu. Vérifier d'abord `Monitoring > Query History` et, si nécessaire, `Copy History` afin d'éviter une seconde tentative alors que la première a déjà réussi.

---

## Snowflake — wizard Snowsight bloqué avant `COPY INTO`

**Problème / découverte**  
L'assistant `Load Data into Table` reste indéfiniment en chargement pour `orders.csv`. Le comportement persiste avec une nouvelle table comme avec une table existante.

**Diagnostic validé**  
Les requêtes SQL de contrôle fonctionnent :
- création de `RAW.ORDERS` réussie ;
- démarrage / suspension du warehouse réussis ;
- `CURRENT_ROLE()` / `CURRENT_WAREHOUSE()` / `SELECT 1` réussis avec le rôle projet et le warehouse dédié.

Query History ne montre cependant aucun `COPY INTO` issu du wizard. Le chargement semble donc bloquer avant la soumission du vrai SQL de copie.

Snowsight affiche également `Failed to update the default warehouse` lorsqu'on tente de sélectionner le warehouse dédié dans le wizard. Changer le warehouse par défaut n'a pas résolu le problème.

**Règle**  
Ne pas multiplier les tentatives du wizard lorsque SQL, rôle et warehouse ont déjà été validés. Passer à une méthode reproductible depuis WSL avec Snowflake CLI, stage interne, `PUT` puis `COPY INTO`.

L'emplacement du CSV dans WSL n'est pas considéré comme cause démontrée à ce stade.

---

## Snowflake — Query History comme preuve de diagnostic

**Découverte**  
Lorsqu'un assistant graphique semble bloqué, Query History permet de distinguer un problème de requête Snowflake d'un problème qui survient avant la soumission SQL.

**Règle**  
Si aucune requête attendue (`COPY INTO`, DML, etc.) n'apparaît alors que les tests SQL directs réussissent, ne pas conclure immédiatement à un problème de warehouse ou de rôle. Documenter ce qui est réellement visible et choisir un autre chemin d'exécution si nécessaire.
