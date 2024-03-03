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
    };
  },
  methods: {
    selectCard(card) {
      this.selectedCard = card;
    },
    async checkAnswer() {
      const card = this.cards.find(card => card.question === this.selectedCard.question);
      if (card) {
        await axios.patch(`http://localhost:8000/cards/${card.id}/answer`, {
          isValid: this.answer === this.selectedCard.answer,
        });
      }
      this.answer = '';
      this.selectedCard = null;
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
</style>