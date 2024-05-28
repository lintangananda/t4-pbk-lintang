<template>
    <div class="post-app">
      <h1>DATA USER</h1>
      <p>Select Option:</p>
      <select v-model="selectedUserLocal" @change="handleUserChange">
        <option value="" selected disabled>Pilih User</option>
        <option v-for="user in users" :value="user.id" :key="user.id">{{ user.name }}</option>
      </select>
  
      <div v-if="posts.length > 0">
        <ul>
          <li v-for="post in posts" :key="post.id">
            <slot :post="post">
              <!-- Default content if no slot content is provided -->
              <h3>{{ post.title }}</h3>
              <p>{{ post.body }}</p>
            </slot>
            <button @click="emitPostDetails(post)">Show Details</button>
          </li>
        </ul>
      </div>
      <div v-else>
        <p>Loading...</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue';
  
  const props = defineProps({
    users: Array,
    selectedUser: Number,
    posts: Array,
  });
  const emits = defineEmits(['update:selectedUser', 'fetchPosts', 'showPostDetails']);
  
  const selectedUserLocal = ref(props.selectedUser);
  
  watch(() => props.selectedUser, (newVal) => {
    selectedUserLocal.value = newVal;
  });
  
  function handleUserChange() {
    emits('update:selectedUser', selectedUserLocal.value);
    emits('fetchPosts');
  }
  
  function emitPostDetails(post) {
    emits('showPostDetails', post);
  }
  </script>
  
  <style scoped>
  .post-app {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40px;
    font-family: 'Arial', sans-serif;
    background-color: #fff;
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 100%;
    box-sizing: border-box;
  }
  
  .post-app h1 {
    color: #ff69b4;
    font-size: 40px;
    font-family: Georgia, serif;
    margin-bottom: 30px;
  }
  
  .post-app select {
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 6px;
    margin-bottom: 20px;
  }
  
  .post-app ul {
    list-style-type: none;
    padding: 0;
    width: 100%;
    max-width: 600px;
  }
  
  .post-app ul li {
    background-color: #f9f9f9;
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .post-app ul li h3 {
    margin: 0 0 10px;
    font-size: 24px;
    color: #333;
  }
  
  .post-app ul li p {
    margin: 0;
    color: #666;
  }
  </style>
  