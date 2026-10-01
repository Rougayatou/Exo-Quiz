<script setup>
import { computed, ref } from 'vue'

 
const questions = [
  {
    texte: 'Quel est le nom de ce fruit ?',
    image: '/images/pomme.png',
    choix: ['Pomme', 'Poire', 'Orange'],
    bonneReponse: 0,
    descriptionImage: 'Une pomme rouge'
  },
  {
    texte: 'Quelle est la couleur de ce fruit ?',
    image: '/images/banane.png',
    choix: ['Jaune', 'Rouge', 'Vert'],
    bonneReponse: 0,
    descriptionImage: 'Une banane jaune'
  },
  {
    texte: 'Quel est le nom de ce fruit ?',
    image: '/images/fraise.png',
    choix: ['Cerise', 'Fraise', 'Framboise'],
    bonneReponse: 1,
    descriptionImage: 'Une fraise rouge avec des pépins jaunes'
  },
  {
    texte: 'Quel est le nom de ce fruit ?',
    image: '/images/ananas.png',
    choix: ['Ananas', 'Noix de coco', 'Mangue'],
    bonneReponse: 0,
    descriptionImage: 'Un ananas avec des feuilles vertes sur le dessus'
  },
  {
    texte: 'Quelle est la couleur principale de ce fruit ?',
    image: '/images/pasteque.png',
    choix: ['Rouge', 'Bleu', 'Orange'],
    bonneReponse: 0,
    descriptionImage: 'Une tranche de pastèque verte, blanche et rouge avec des pépins noirs'
  }
]


const indexQuestion = ref(0)
const score = ref(0)
const reponseChoisie = ref(null)
const termine = ref(false)

const questionCourante = computed(function () {
  return questions[indexQuestion.value]
})


const aRepondu = computed(function () {
  return reponseChoisie.value !== null
})

const nombreReponses = computed(function () {
  if (termine.value === true) {
    return questions.length
  }
  if (reponseChoisie.value === null) {
    return indexQuestion.value
  }
  return indexQuestion.value + 1
})

const estDerniereQuestion = computed(function () {
  return indexQuestion.value === questions.length - 1
})


function repondre(index) {
  if (reponseChoisie.value !== null) {
    return
  }
  if (termine.value === true) {
    return
  }

  reponseChoisie.value = index

  if (index === questionCourante.value.bonneReponse) {
    score.value = score.value + 1
  }
}


function avancer() {
  if (reponseChoisie.value === null) {
    return
  }

  if (indexQuestion.value >= questions.length - 1) {
    termine.value = true
    return
  }

  indexQuestion.value = indexQuestion.value + 1
  reponseChoisie.value = null
}


function rejouer() {
  indexQuestion.value = 0
  score.value = 0
  reponseChoisie.value = null
  termine.value = false
}


function classeBouton(index) {
  if (aRepondu.value === false) {
    return ''
  }
  if (index === questionCourante.value.bonneReponse) {
    return 'juste'
  }
  if (index === reponseChoisie.value) {
    return 'faux'
  }
  return ''
}
</script>

<template>
  <main class="page">
    <header class="entete">
      <h1>Quiz des formes</h1>
      <p class="score">Score : {{ score }} / {{ questions.length }}</p>
    </header>

    <p class="progression-texte">Progression : {{ nombreReponses }} / {{ questions.length }}</p>
    <progress class="barre" :max="questions.length" :value="nombreReponses"></progress>

    <section v-if="!termine" class="carte">
      <p class="numero">Question {{ indexQuestion + 1 }} sur {{ questions.length }}</p>

      <div class="cadre-image">
        <img class="image-question" :src="questionCourante.image" :alt="questionCourante.descriptionImage" />
      </div>

      <h2 class="question">{{ questionCourante.texte }}</h2>

      <div class="choix">
        <button
          v-for="(choix, index) in questionCourante.choix"
          :key="index"
          type="button"
          class="bouton-choix"
          :class="classeBouton(index)"
          :disabled="aRepondu"
          @click="repondre(index)"
        >
          {{ choix }}
          <span v-if="aRepondu && index === questionCourante.bonneReponse"> (bonne réponse)</span>
          <span v-else-if="aRepondu && index === reponseChoisie"> (votre choix)</span>
        </button>
      </div>

      <p v-if="aRepondu && reponseChoisie === questionCourante.bonneReponse">Bonne réponse !</p>
      <p v-else-if="aRepondu">
        Mauvaise réponse. La bonne réponse était : {{ questionCourante.choix[questionCourante.bonneReponse] }}.
      </p>

      <button v-if="aRepondu" type="button" class="bouton-nav" @click="avancer">
        <span v-if="estDerniereQuestion">Voir le résultat</span>
        <span v-else>Question suivante</span>
      </button>
    </section>

    <section v-else class="carte">
      <h2>Résultat</h2>
      <p>Score final : {{ score }} / {{ questions.length }}</p>
      <p>Progression : {{ nombreReponses }} / {{ questions.length }}</p>
      <button type="button" class="bouton-nav" @click="rejouer">Rejouer</button>
    </section>
  </main>
</template>

<style scoped>
.page {
  min-height: 100vh;
  max-width: 640px;
  margin: 0 auto;
  padding: 16px;
}

.entete {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

h1 {
  font-size: 1.4rem;
  font-weight: 700;
}

.score {
  font-weight: 700;
}

.progression-texte {
  margin-top: 12px;
}

.barre {
  width: 100%;
  height: 14px;
  margin-bottom: 16px;
}

.carte {
  background: #ffffff;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.cadre-image {
  display: flex;
  justify-content: center;
  margin: 12px 0;
  background: #f1f5f9;
  border-radius: 8px;
  padding: 8px;
}

.image-question {
  max-width: 100%;
  height: 180px;
  object-fit: contain;
}

.question {
  font-size: 1.1rem;
  margin-bottom: 12px;
}

.choix {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.bouton-choix,
.bouton-nav {
  width: 100%;
  padding: 12px;
  font-size: 1rem;
  border-radius: 8px;
  cursor: pointer;
  border: 1px solid #94a3b8;
  background: #f8fafc;
  color: #0f172a;
  text-align: left;
}

.bouton-choix:disabled {
  cursor: default;
}

.juste {
  background: #dcfce7;
  border-color: #16a34a;
}

.faux {
  background: #fee2e2;
  border-color: #dc2626;
}

.bouton-nav {
  margin-top: 16px;
  border: none;
  background: #334155;
  color: #ffffff;
  text-align: center;
}

@media (max-width: 480px) {
  h1 {
    font-size: 1.2rem;
  }
  .image-question {
    height: 140px;
  }
  .bouton-choix,
  .bouton-nav {
    font-size: 0.95rem;
    padding: 14px 12px;
  }
}
</style>