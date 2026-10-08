# Petit plan — Gestionnaire de tâches

Application Vue 3 pour noter les tâches du jour, les marquer comme terminées et suivre le nombre de tâches restantes.

## Installation et lancement

```sh
npm install
npm run dev
```

Pour vérifier les types et construire le projet :

```sh
npm run build
```

## Fonctions réalisées

- Ajouter une tâche et ignorer les saisies vides.
- Cocher une tâche terminée; son texte est alors barré.
- Supprimer une tâche.
- Calculer automatiquement les tâches restantes et la progression.

## Notions Vue utilisées

- `ref` rend réactifs le texte saisi et la liste des tâches.
- `v-model` relie le champ texte et les cases à cocher aux données.
- `v-for` affiche chaque tâche.
- `:key` fournit un identifiant stable à chaque ligne.
- `computed` recalcule le compteur des tâches non terminées.
- `@submit.prevent` intercepte l’envoi du formulaire sans recharger la page.
- `@click` supprime la tâche sélectionnée.

## Captures

- `captures/taches-en-cours.png` : deux tâches et leur compteur.
- `captures/tache-terminee.png` : tâche cochée et barrée, compteur actualisé.
