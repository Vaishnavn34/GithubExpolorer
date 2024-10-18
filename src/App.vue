<template>
  <div id="app">
    <h1>GitHub Repository Explorer</h1>
    <input
      v-model="username"
      type="text"
      placeholder="Enter GitHub username"
    />
    <button @click="fetchRepos">Get Repos</button>

    <div v-if="loading">Loading...</div>
    <div v-if="error" style="color: red;">{{ error }}</div>

    <ul>
      <li
        v-for="repo in repos"
        :key="repo.id"
        @click="selectRepo(repo)"
      >
        {{ repo.name }} - ⭐ {{ repo.stargazers_count }}
      </li>
    </ul>

    <div v-if="selectedRepo">
      <h2>Description:</h2>
      <p>{{ selectedRepo.description || 'No description available' }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      repos: [],
      selectedRepo: null,
      loading: false,
      error: '',
    };
  },
  methods: {
    async fetchRepos() {
      this.loading = true;
      this.error = '';
      this.selectedRepo = null;
      try {
        const response = await fetch(`https://api.github.com/users/${this.username}/repos`);
        if (!response.ok) {
          throw new Error('User not found');
        }
        const data = await response.json();
        this.repos = data;
      } catch (err) {
        this.error = err.message;
      } finally {
        this.loading = false;
      }
    },
    selectRepo(repo) {
      this.selectedRepo = repo;
    },
  },
};
</script>

<style>
#app {
  text-align: center;
  margin: 20px;
}

input {
  padding: 10px;
  margin-right: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  cursor: pointer;
  padding: 10px;
  border: 1px solid #ccc;
  margin: 5px 0;
}

li:hover {
  background-color: #f0f0f0;
}
</style>
