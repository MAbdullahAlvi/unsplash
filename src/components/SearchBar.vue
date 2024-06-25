<template>
    <div>
      <input v-model="query" placeholder="Search for photos..." @keyup.enter="search" />
      <button @click="search">Search</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        query: '',
      };
    },
    methods: {
      search() {
        this.$emit('search', this.query);
      },
    },
  };
  </script>
  
  <style scoped>
  input {
    padding: 8px;
    margin-right: 8px;
    margin-left: 494px;
    width: 250px;
  }
  button {
    padding: 8px;
  }
  </style>
  