<template>
  <div>
    <div v-if="selectedCard === null">
      <div v-for="card in cards" :key="card.question" @click="selectCard(card)" class="card">
        <h2>{{ card.category }}</h2>
        <p class="question">Question: {{ card.question }}</p>
        <p>Answer: {{ card.answer }}</p>
        <p>Tag: {{ card.tag }}</p>
      </div>
    </div>
    <div v-else class="selected-card">
      <h2>{{ selectedCard.category }}</h2>
      <p class="question">Question: {{ selectedCard.question }}</p>
      <input v-model="answer" type="text" placeholder="Your answer">
      <button @click="checkAnswer">Submit</button>
      <button @click="selectedCard = null">Back to list</button>
    </div>
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h2>Confirmation</h2>
        <p>La réponse est incorrecte  la reponse est {{ selectedCard.answer }} et votre reponse est {{ answer }}.
           Était-ce une faute de frappe ?</p>
        <button @click="forceSubmit">Oui</button>
        <button @click="submitAnswer(false)">Non</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cards: [],
      selectedCard: null,
      answer: '',
      showModal: false,
      searchDate: '',
    };
  },
  methods: {
    selectCard(card) {
      this.selectedCard = card;
    },
    checkAnswer() {
      if (this.answer === this.selectedCard.answer) {
        this.submitAnswer(true);
      } else {
        this.showModal = true;
      }
    },
    async submitAnswer(isValid) {
      const card = this.cards.find(card => card.question === this.selectedCard.question);
      if (card) {
        await axios.patch(`http://localhost:8000/cards/${card.id}/answer`, {
          isValid,
        });
      }
      this.answer = '';
      this.selectedCard = null;
      this.showModal = false;
    },
    forceSubmit() {
      this.submitAnswer(true);
    },
  },
  async created() {
    const response = await axios.get('http://localhost:8000/cards/quizz');
    if (response.data && typeof response.data === 'object') {
      this.cards = Object.values(response.data);
    } else {
      console.error('API did not return an object');
    }
  },
  async search() {
      const response = await axios.get('http://localhost:8000/cards/quizz', {
        params: {
          startDate: this.startDate,
        },
      })
    },
};
</script>

<style scoped>
.card {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
}

.selected-card {
  border: 1px solid #ccc;
  padding: 10px;
}

.question {
  font-weight: bold;
  color: #333;
}
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0,0,0,0.4);
}

.modal-content {
  background-color: #888;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.nav {
  background-color: #f4f4f4;
  padding: 10px;
  margin-bottom: 20px;
}
</style>