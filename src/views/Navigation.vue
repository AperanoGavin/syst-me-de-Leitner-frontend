<template>
    <nav>
      <form @submit.prevent="search" class="search-form">
        <label for="startDate">Date:</label>
        <input v-model="startDate" type="date" id="startDate">
        <button type="submit">Rechercher</button>
      </form>
    </nav>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        startDate: '',
      };
    },
    methods: {
      async search() {
        const response = await axios.get(`http://localhost:8000/cards/quizz?date=${this.startDate}`);
        if(response.data.length === 0) {
          alert('No cards found');
        }
        this.$emit('search', response.data);
      },
    },
  };
  </script>
  <style scoped>
  nav {
    background-color: #007bff;
    padding: 10px;
    margin-bottom: 20px;
  }
  
  .search-form {
    display: flex;
    align-items: center;
  }
  
  .search-form label {
    color: white;
    margin-right: 10px;
  }
  
  .search-form input[type="date"],
  .search-form button {
    padding: 5px;
    border-radius: 5px;
  }
  
  .search-form button {
    background-color: white;
    color: #007bff;
    border: none;
    cursor: pointer;
  }
  </style>