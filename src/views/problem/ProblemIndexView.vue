<template>
    <div class="problem-list">
      <h1>Problem List</h1>
      <div v-if="loading" class="loading">Loading...</div>
      <div v-if="error" class="error">{{ error }}</div>
      <div v-if="problems.length > 0" class="problems">
        <ul>
          <li v-for="problem in problems" :key="problem.identity" class="problem-item">
            <h2>{{ problem.title }}</h2>
            <p class="content">{{ problem.content }}</p>
            <p><strong>Category:</strong> {{ problem.category_id }}</p>
            <p><strong>Max Runtime:</strong> {{ problem.max_runtime }}ms</p>
            <p><strong>Max Memory:</strong> {{ problem.max_mem }}MB</p>
          </li>
        </ul>
      </div>
      <div v-else-if="!loading && !error">
        <p class="no-problems">No problems available.</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        problems: [],
        loading: false,
        error: null,
      };
    },
    created() {
      this.fetchProblems();
    },
    methods: {
      async fetchProblems() {
        this.loading = true;
        try {
          const response = await axios.get('http://127.0.0.1:8888/problem/list');
          if (!response.data.err) {
            this.problems = response.data.data;
          } else {
            this.error = 'Failed to load problems.';
          }
        } catch (error) {
          this.error = 'An error occurred: ' + error.message;
        } finally {
          this.loading = false;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');
  
  .problem-list {
    font-family: 'Roboto', sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f7f9fc;
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    color: #333;
    font-weight: 700;
    margin-bottom: 20px;
    text-align: center;
  }
  
  .loading,
  .error {
    text-align: center;
    color: #ff5252;
    font-size: 18px;
    font-weight: 500;
  }
  
  .problems {
    list-style-type: none;
    padding: 0;
  }
  
  .problem-item {
    background-color: #ffffff;
    margin-bottom: 20px;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    color: #007BFF;
    font-weight: 500;
    margin-bottom: 10px;
  }
  
  .content {
    margin: 10px 0;
    color: #555;
    line-height: 1.6;
  }
  
  .no-problems {
    text-align: center;
    color: #666;
    font-size: 18px;
  }
  </style>
  