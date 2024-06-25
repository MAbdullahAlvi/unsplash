<template>
  <div :class="theme">
    <div id="app">
      <header class="header">
        <div>
          <h1>Unsplash</h1>
          <button @click="toggleTheme" class="theme-toggle">
            {{ theme === 'light' ? 'Night Mode' : 'Day Mode' }}
          </button>
        </div>
        <div class="header-actions">
          <button class="notification-bell" @click="toggleNotifications">
            <i class="fas fa-bell"></i>
            <span class="notification-badge" v-if="notificationsEnabled && notificationsCount > 0">{{ notificationsCount }}</span>
          </button>
          <input type="file" @change="handleFileUpload" accept="image/*" class="upload-input">
          <button class="upload-button" @click="uploadImage">Upload Image</button>
        </div>
      </header>
      <nav class="navbar">
        <button class="nav-arrow left" @click="scrollLeft">&#9664;</button>
        <div class="nav-container" ref="navContainer">
          <ul class="nav-list">
            <li><a href="#" @click.prevent="searchCategory('Wallpapers')">Wallpapers</a></li>
            <li><a href="#" @click.prevent="searchCategory('Nature')">Nature</a></li>
            <li><a href="#" @click.prevent="searchCategory('3D Renders')">3D Renders</a></li>
            <li><a href="#" @click.prevent="searchCategory('Travel')">Travel</a></li>
            <li><a href="#" @click.prevent="searchCategory('Architecture & Interiors')">Architecture & Interiors</a></li>
            <li><a href="#" @click.prevent="searchCategory('Textures & Patterns')">Textures & Patterns</a></li>
            <li><a href="#" @click.prevent="searchCategory('Street Photography')">Street Photography</a></li>
            <li><a href="#" @click.prevent="searchCategory('Film')">Film</a></li>
            <li><a href="#" @click.prevent="searchCategory('Village')">Village</a></li>
            <li><a href="#" @click.prevent="searchCategory('Animals')">Animals</a></li>
            <li><a href="#" @click.prevent="searchCategory('People')">People</a></li>
            <li><a href="#" @click.prevent="searchCategory('Business & Work')">Business & Work</a></li>
            <li><a href="#" @click.prevent="searchCategory('Health & Food')">Health & Food</a></li>
            <li><a href="#" @click.prevent="searchCategory('Sport')">Sport</a></li>
            <li><a href="#" @click.prevent="searchCategory('Current Events')">Current Events</a></li>
            <li><a href="#" @click.prevent="showFavorites">Favorites</a></li>
          </ul>
        </div>
        <button class="nav-arrow right" @click="scrollRight">&#9654;</button>
      </nav>
      <SearchBar @search="handleSearch" />
      <PhotoList :photos="photos" @favorite="addToFavorites" />

      
      <div v-if="favoritesVisible" class="modal">
        <div class="modal-content">
          <span class="close" @click="favoritesVisible = false">&times;</span>
          <h2>Your Favorites</h2>
          <ul class="favorites-list">
            <li v-for="photo in favorites" :key="photo.id">
              <img :src="photo.urls.small" :alt="photo.alt_description" />
              <button @click="removeFromFavorites(photo.id)">Remove</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import PhotoList from './components/PhotoList.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    SearchBar,
    PhotoList,
  },
  data() {
    return {
      photos: [],
      favorites: [],
      favoritesVisible: false,
      theme: 'light', 
      notificationsEnabled: false,
      notificationsCount: 0,
    };
  },
  methods: {
    handleSearch(query) {
      this.searchPhotos(query);
    },
    searchCategory(category) {
      this.searchPhotos(category);
    },
    searchPhotos(query) {
      axios
        .get(`https://api.unsplash.com/search/photos`, {
          params: {
            query,
            client_id: 'o4dXygNTyfW5uZjylhwdOumyP90uScYSnChIROOVzXE',
          },
        })
        .then((response) => {
          this.photos = response.data.results;
        });
    },
    addToFavorites(photo) {
      if (!this.favorites.some(fav => fav.id === photo.id)) {
        this.favorites.push(photo);
      }
    },
    removeFromFavorites(photoId) {
      this.favorites = this.favorites.filter(photo => photo.id !== photoId);
    },
    showFavorites() {
      this.favoritesVisible = true;
    },
    scrollLeft() {
      const container = this.$refs.navContainer;
      container.scrollLeft -= 100; 
    },
    scrollRight() {
      const container = this.$refs.navContainer;
      container.scrollLeft += 100; 
    },
    toggleTheme() {
      this.theme = this.theme === 'light' ? 'dark' : 'light';
    },
    toggleNotifications() {
      this.notificationsEnabled = !this.notificationsEnabled;
      if (this.notificationsEnabled) {
        
        this.notificationsCount = Math.floor(Math.random() * 10); 
      } else {
        this.notificationsCount = 0;
      }
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        if (file.size <= 5000 && file.type.includes('image')) {
          
          console.log('File is valid:', file);
          
        } else {
          alert('Please upload an image that is less than 5MB and in image format.');
        }
      }
    },
    uploadImage() {
      
      const uploadInput = document.querySelector('.upload-input');
      uploadInput.click(); 
    }
  },
  mounted() {
    this.searchCategory('Wallpapers'); 
  }
};
</script>

<style scoped>

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--header-bg);
  padding: 10px 20px;
  border-bottom: 1px solid var(--text-color);
}

.header h1 {
  margin: 0;
  margin-left: 358%;
}

.theme-toggle {
  background-color: transparent;
  border: 1px solid var(--text-color);
  color: var(--text-color);
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
}

.theme-toggle:hover {
  background-color: var(--text-color);
  color: var(--bg-color);
}


.notification-bell {
  background: none;
  border: none;
  font-size: 24px;
  color: var(--text-color);
  cursor: pointer;
  position: relative;
}

.notification-bell .fa-bell {
  position: relative;
}

.notification-badge {
  position: absolute;
  top: -5px;
  right: -10px;
  background-color: red;
  color: white;
  border-radius: 50%;
  padding: 3px 6px;
  font-size: 10px;
}


.upload-button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 5px;
}

.upload-button:hover {
  background-color: #0056b3;
}

.upload-input {
  display: none;
}


.navbar {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  background-color: var(--nav-bg);
}

.nav-container {
  overflow-x: auto;
  white-space: nowrap;
  display: flex;
  flex: 1;
  max-width: 80%;
}

.nav-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
}

.nav-list li {
  margin: 0;
}

.nav-list li a {
  text-decoration: none;
  color: var(--text-color);
  font-weight: bold;
  padding: 5px 10px;
  border-radius: 5px;
}

.nav-list li a:hover {
  color: #007bff;
  background-color: #f0f0f0;
}

.nav-arrow {
  background: none;
  border: none;
  font-size: 24px;
  color: var(--text-color);
  cursor: pointer;
}

.nav-arrow:hover {
  color: #007bff;
}

.nav-arrow.left {
  margin-right: 10px;
}

.nav-arrow.right {
  margin-left: 10px;
}


.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: var(--modal-bg);
  padding: 20px;
  border-radius: 10px;
  max-width: 80%;
  max-height: 80%;
  overflow: auto;
  position: relative;
  text-align: center;
  color: var(--modal-text);
}

.modal-content .close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  font-size: 24px;
  color: var(--text-color);
}


.favorites-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.favorites-list li {
  margin: 10px;
  position: relative;
}

.favorites-list li img {
  width: 150px;
  height: auto;
  border-radius: 5px;
}

.favorites-list li button {
  position: absolute;
  top: 5px;
  right: 5px;
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
}

.favorites-list li button:hover {
  background-color: #c82333;
}

/* General styles */
body, #app {
  background-color: var(--bg-color);
  color: var(--text-color);
  font-family: Arial, sans-serif;
}

.dark {
  --bg-color: #1c1c1c;
  --text-color: #f1f1f1;
  --header-bg: #333;
  --nav-bg: #444;
  --modal-bg: #2a2a2a;
  --modal-text: #f1f1f1;
}

.light {
  --bg-color: white;
  --text-color: black;
  --header-bg: #f8f9fa;
  --nav-bg: #e9ecef;
  --modal-bg: white;
  --modal-text: black;
}
</style>
