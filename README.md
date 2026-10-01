# Quiz des formes

Application Vue.js de quiz en images. Thème : **formes géométriques** (triangle, carré, cercle, étoile, rectangle).

Étudiante : Rougayatou Diallo

## Lancer le projet

```bash
npm install
npm run dev
```

Ouvrir l’adresse affichée dans le terminal (souvent http://localhost:5173/).

## Construire pour la production

```bash
npm run build
```

Le dossier `dist` est généré localement. Il n’est pas versionné (voir `.gitignore`).

## Contenu du dépôt

- `src/App.vue` : questions, logique du jeu, affichage et styles
- `public/images/` : images locales du quiz
- `captures/` : captures d’écran des situations demandées
- `compte-rendu/` : compte rendu du travail

## Règles du jeu

- 5 questions, 3 choix, 1 seule bonne réponse
- Une bonne réponse ajoute 1 point ; une mauvaise réponse n’ajoute rien
- La barre de progression avance à chaque réponse, juste ou fausse
- On ne peut pas changer de question avant d’avoir répondu
- **Rejouer** remet le score et la progression à zéro
