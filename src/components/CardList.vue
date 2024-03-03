<template>
    <div>
      <Card v-for="card in cards" :key="card.id" :data="card" />
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import Card from './Card.vue';
  
  export default {
    data() {
      return {
        cards: [],
      };
    },
    mounted() {
      axios.get('http://localhost:8000/cards/quizz')
        .then(response => {
          this.cards = response.data;
        })
        .catch(error => {
          console.error('Error fetching cards:', error);
        });
    },
    components: {
      Card,
    },
  };
  </script>