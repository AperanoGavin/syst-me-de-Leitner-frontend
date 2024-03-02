<template>
    <div>
      <div v-for="card in cards" :key="card.id">
        <h2>{{ card.category }}</h2>
        <p>Question: {{ card.question }}</p>
        <p>Tag: {{ card.tag }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        cards: [],
      };
    },
    async created() {
      const response = await axios.get('http://localhost:8000/cards');
      this.cards = response.data;
    },
  };
  </script>