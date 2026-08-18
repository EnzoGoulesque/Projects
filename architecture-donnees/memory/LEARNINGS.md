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

## Snowflake — wizard Snowsight bloqué avant `COPY INTO`, puis ingestion réussie

**Problème / découverte**  
Lors d'une première session, l'assistant `Load Data into Table` est resté indéfiniment en chargement pour `orders.csv`. Le comportement persistait avec une nouvelle table comme avec une table existante.

**Diagnostic validé à ce moment-là**  
Les requêtes SQL de contrôle fonctionnaient :
- création de `RAW.ORDERS` réussie ;
- démarrage / suspension du warehouse réussis ;
- `CURRENT_ROLE()` / `CURRENT_WAREHOUSE()` / `SELECT 1` réussis avec le rôle projet et le warehouse dédié.

Query History ne montrait cependant aucun `COPY INTO` issu du wizard. Le chargement semblait donc bloquer avant la soumission du vrai SQL de copie.

**Résolution observée le 2026-08-18**  
Lors de la reprise, le même chemin Snowsight a finalement permis de charger `orders.csv`. La table `RAW.ORDERS` contient bien 12 lignes et le warehouse a été suspendu après validation.

La cause exacte du blocage initial n'a pas été identifiée.

**Règle**  
Conserver Query History comme preuve de diagnostic et ne pas multiplier les clics lorsqu'un wizard semble bloqué. Ne pas transformer pour autant un incident ponctuel en décision architecturale permanente : Snowflake CLI n'est ajouté que si un besoin reproductible futur le justifie.

---

## Snowflake — Query History comme preuve de diagnostic

**Découverte**  
Lorsqu'un assistant graphique semble bloqué, Query History permet de distinguer un problème de requête Snowflake d'un problème qui survient avant la soumission SQL.

**Règle**  
Si aucune requête attendue (`COPY INTO`, DML, etc.) n'apparaît alors que les tests SQL directs réussissent, ne pas conclure immédiatement à un problème de warehouse ou de rôle. Documenter ce qui est réellement visible et choisir un autre chemin d'exécution si nécessaire.

---

## Ubuntu 24.04 — `venv` peut manquer malgré Python installé

**Problème / découverte**  
`python3 --version` retournait Python 3.12.3, mais `python3 -m venv .venv` échouait car `ensurepip` n'était pas disponible.

**Cause / explication**  
Sur Ubuntu, le support des environnements virtuels peut être fourni par un paquet séparé.

**Solution / règle**  
Installer le paquet correspondant puis recréer l'environnement incomplet :

```bash
sudo apt update
sudo apt install python3.12-venv
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
```

Vérifier ensuite `python --version` et `which python`.

---

## dbt — le répertoire de profil doit exister avant `dbt parse`

**Problème / découverte**  
Une première exécution de `dbt parse` a échoué avec une erreur indiquant que `~/.dbt` n'existait pas.

**Solution / règle**  
Créer le répertoire local avec :

```bash
mkdir -p ~/.dbt
```

La configuration `profiles.yml` doit ensuite rester dans ce répertoire, hors du repository. Cette erreur ne prouve pas un problème de connexion Snowflake : aucune connexion n'avait encore été configurée.

---

## Git — les dossiers vides ne sont pas suivis

**Découverte**  
Créer `models/staging`, `models/intermediate` et `models/marts` ne les fait pas apparaître dans `git status` tant qu'ils ne contiennent aucun fichier.

**Règle**  
Ne pas interpréter leur absence dans Git comme un échec de création. Ils seront versionnés naturellement lorsque les premiers fichiers dbt seront ajoutés.
