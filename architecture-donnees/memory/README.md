# Memory — Projet Architecture de données

Ce dossier est la **mémoire officielle et versionnée** du projet.

Il doit permettre à une personne ou à un agent IA de comprendre rapidement :
- pourquoi le projet existe ;
- comment il est construit ;
- où il en est ;
- quelles décisions ont été prises ;
- quels problèmes ont déjà été rencontrés ;
- quelles règles de sécurité doivent être respectées ;
- quelles commandes et procédures sont réutilisables ;
- comment commencer et terminer correctement une session.

## Fichiers

| Fichier | Rôle |
|---|---|
| `PROJECT.md` | Vision, périmètre, objectifs et architecture globale |
| `ROADMAP.md` | Étapes du projet et progression |
| `HANDOFF.md` | État courant condensé pour reprendre immédiatement |
| `DECISIONS.md` | Décisions structurantes et leurs raisons |
| `LEARNINGS.md` | Problèmes, découvertes et solutions réutilisables |
| `SECURITY.md` | Règles de sécurité et données interdites dans Git |
| `SESSION_GUIDE.md` | Mode opératoire complet d'une session |
| `PLAYBOOK.md` | Bibliothèque de commandes/procédures par domaine |

## Ordre de lecture recommandé pour un agent IA

1. `HANDOFF.md`
2. `PROJECT.md`
3. `ROADMAP.md`
4. `DECISIONS.md`
5. `SECURITY.md`

Puis, selon le besoin :
- `LEARNINGS.md`
- `PLAYBOOK.md`
- `SESSION_GUIDE.md`

## Principe de version

Ne jamais créer de fichiers `V1`, `V2`, `final`, `final_v2`, etc.

Git conserve l'historique.

```text
mémoire actuelle
    ↓
modification validée
    ↓
commit Git
    ↓
nouvelle mémoire officielle
```

## Principe de publication

Tout ce dossier doit être considéré comme **publiable**.

Aucun secret, credential ou donnée professionnelle confidentielle ne doit y être stocké.
