<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

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
      categories: ['FIRST', 'SECOND', 'THIRD', 'FOURTH', 'FIFTH', 'SIXTH', 'SEVENTH', 'DONE'],
    };
  },
  methods: {
    async selectCard(card) {
      this.selectedCard = card;
    },
    async checkAnswer() {
      if (this.answer === this.selectedCard.answer) {
        const index = this.categories.indexOf(this.selectedCard.category);
        if (index < this.categories.length - 1) {
          const card = this.cards.find(card => card.question === this.selectedCard.question);
          if (card) {
            await axios.patch(`http://localhost:8000/cards/${card.id}/answer`, {
              isValid: true,
            });
            this.selectedCard.category = this.categories[index + 1];
          }
        }
      }
      this.answer = '';
      this.selectedCard = null;
    },
  },
  async created() {
    const response = await axios.get('http://localhost:8000/cards/quizz');
    this.cards = response.data;
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