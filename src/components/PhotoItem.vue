<template>
    <div class="photo-item">
      <img :src="photo.urls.small" :alt="photo.alt_description" />
      <p>{{ photo.alt_description || 'No description' }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PhotoItem',
    props: ['photo'],
  };
  </script>
  
  <style scoped>
  .photo-item {
    margin: 10px;
    text-align: center;
  }
  img {
    max-width: 300px;
    border-radius: 10px;
  }
  </style>
  