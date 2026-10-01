# Compte rendu — Quiz des formes

- **Nom :** Rougayatou Diallo
- **Thème choisi :** formes géométriques (triangle, carré, cercle, étoile, rectangle)

## Présentation de l’interface

Tout le quiz se trouve dans un seul composant : `src/App.vue`.

En haut, le titre **Quiz des formes** et le **score** restent visibles. En dessous, une barre HTML `progress` et le texte `Progression : x / 5` indiquent le nombre de réponses déjà données.

Pendant le quiz, on voit :

- le numéro de question (`Question 2 sur 5`)
- l’image (`img` avec `:src` et `:alt`)
- le texte de la question
- trois boutons créés avec `v-for`
- un message après la réponse
- le bouton **Question suivante**, ou **Voir le résultat** à la dernière question

À la fin, `v-if` / `v-else` masquent les choix et affichent le score final, plus le bouton **Rejouer**.

### Directives Vue utilisées

- `{{ }}` pour afficher le score, le numéro de question et les textes
- `:src` et `:alt` pour l’image
- `v-for` et `:key` pour les trois choix
- `@click` pour répondre, avancer et rejouer
- `:disabled` pour bloquer les boutons après une réponse
- `:value` et `:max` sur `progress`
- `v-if` / `v-else` pour passer du quiz au résultat

## Structure du tableau des questions

Le tableau `questions` est déclaré dans la section `script` de `App.vue`. Chaque objet contient :

- `texte` : la question
- `image` : chemin local, par exemple `/images/triangle.svg`
- `choix` : un tableau de 3 chaînes
- `bonneReponse` : l’indice 0, 1 ou 2 du bon choix
- `descriptionImage` : texte alternatif de l’image

Les images sont dans `public/images`. Vite les sert à la racine, donc `/images/carre.svg` fonctionne.

La bonne réponse n’est pas toujours sur le même bouton (indices 1, 0, 2, 1, 0).

## Événements, score et progression

L’état réactif est :

- `indexQuestion` : question affichée
- `score` : nombre de bonnes réponses
- `reponseChoisie` : indice cliqué, ou `null` si pas encore répondu
- `termine` : `true` quand on affiche le résultat

### Comment un clic sait quel choix a été choisi

`v-for="(choix, index) in questionCourante.choix"` donne l’indice 0, 1 ou 2.  
`@click="repondre(index)"` envoie cet indice à la fonction.

### Comment on évite plusieurs points pour une question

Au début de `repondre`, une **garde** vérifie `reponseChoisie !== null`. Si c’est déjà le cas, la fonction s’arrête. Les boutons sont aussi `:disabled`.

### Différence entre score et progression

- **Score** : uniquement les bonnes réponses (+1 ou 0)
- **Progression** : toutes les réponses données, justes ou fausses

`nombreReponses` est calculé avec `computed` à partir de `indexQuestion`, `reponseChoisie` et `termine`. On ne modifie pas le DOM avec `document.getElementById`.

### Dernière question et résultat

Si `indexQuestion` est 4 (la 5e question), le bouton affiche **Voir le résultat**. La fonction `avancer` met `termine` à `true` sans dépasser le tableau. Les choix sont alors masqués.

`rejouer` remet `indexQuestion`, `score`, `reponseChoisie` et `termine` à zéro.

## Tests réalisés

| Test | Résultat |
| --- | --- |
| Nouvelle partie | Question 1, 3 choix, score 0/5, progression 0/5 |
| Bonne réponse | Score +1 et progression +1 |
| Mauvaise réponse | Score inchangé, progression +1 |
| Plusieurs clics | Aucun point en plus, boutons désactivés |
| Question suivante | Image, texte et choix changent, score conservé |
| 5 bonnes réponses | Score 5/5, progression 5/5 |
| 5 mauvaises réponses | Score 0/5, progression 5/5 |
| Mélange juste/faux | Score = nombre exact de bonnes réponses |
| Rejouer | Retour à la question 1, score et progression à 0 |
| Petit écran | Boutons accessibles, textes lisibles |

## Difficultés

Au départ, les images n’étaient pas dans `public/images` et le tableau n’était pas dans `App.vue`. Le projet a été aligné sur l’énoncé : images locales, état `termine`, bouton **Rejouer**, et progression calculée à partir des réponses données.
