<template>
    <div class="card" @click="flipCard">
      <div class="front">
        <!-- Afficher la question -->
        <p>{{ data.question }}</p>
      </div>
      <div class="back" v-if="flipped">
        <!-- Afficher la réponse -->
        <p>{{ data.answer }}</p>
        <button @click="checkAnswer">Valider</button>
        <button @click="forceValidation">Forcer Validation</button>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: ['data'],
    data() {
      return {
        flipped: false,
        answered: false,
      };
    },
    methods: {
      flipCard() {
        this.flipped = !this.flipped;
      },
      checkAnswer() {
        // Logique pour vérifier la réponse avec l'API
        axios.patch(`http://localhost:8000/cards/${this.data.id}/answer`, { isValid: true })
          .then(response => {
            this.answered = true;
          })
          .catch(error => {
            console.error('Error checking answer:', error);
          });
      },
      forceValidation() {
        // Logique pour forcer la validation
        axios.patch(`http://localhost:8000/cards/${this.data.id}/answer`, { isValid: true })
          .then(response => {
            this.answered = true;
          })
          .catch(error => {
            console.error('Error forcing validation:', error);
          });
      },
    },
  };
  </script>
  