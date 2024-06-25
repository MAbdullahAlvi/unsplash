<template>
    <div class="photo-list">
      <PhotoItem v-for="photo in photos" :key="photo.id" :photo="photo" />
    </div>
  </template>
  
  <script>
  import PhotoItem from './PhotoItem.vue';
  
  export default {
    name: 'PhotoList',
    props: ['photos'],
    components: {
      PhotoItem,
    },
  };
  </script>
  
  <style scoped>
  .photo-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  </style>
  