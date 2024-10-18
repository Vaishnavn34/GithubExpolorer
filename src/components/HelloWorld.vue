<template>
    <div id="app">
      <h1>Github Explorer</h1>

    <input v-model="username" placeholder="Enter Github Username"/>
    <button @click="fetchRepos">Search</button>

    <div v-if="loading">Loading...</div>
    <div v-if="error">{{ error }}</div>

    
    <ul v-if="repos.length">
      <li v-for="repo in repos" :key="repo.id">
        <p>{{ repo.name }}</p>
      </li>
    </ul>
  </div>
  </template>
  
  <script>
import axios from 'axios';

  
  export default {
    name: 'App',
    data() {
      return {
        username : '',
        repos : [],
        error:null,
        loading:false,
      };
    },
    methods: {
      async fetchRepos() {
        this.loading=true;

        try {
          const response = await axios.get(`https://api.github.com/users/${this.username}/repos`);
          this.repos = response.data;
          console.log(response)
          
        }
        catch (error){
          this.error = "Failed"
        } finally {
          this.loading = false;
        }
        
      }
    },
  };
  </script>
  
  <style>
  /* Add any styles here */
  </style>
  