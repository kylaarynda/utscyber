<script setup>
import { ref } from 'vue';
import CommentSection from './components/CommentSection.vue';

// Fungsi debounce untuk membatasi frekuensi request
const debounce = (func, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), delay);
  };
};

const userId = ref('');
const users = ref(null);
const newEmail = ref('');

const getUser = async () => {
  try {
    const response = await fetch(`http://backend:3000/api/user/${userId.value}`);
    if (!response.ok) throw new Error('Failed to fetch user data');
    users.value = await response.json();
  } catch (error) {
    console.error('Error fetching user data:', error.message);
  }
};

const changeEmail = async () => {
  try {
    const response = await fetch('http://backend:3000/api/user/change-email', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `email=${newEmail.value}`,
    });
    if (!response.ok) throw new Error('Failed to change email');
    console.log('Email updated successfully');
  } catch (error) {
    console.error('Error changing email:', error.message);
  }
};

const debouncedGetUser = debounce(getUser, 2000); // Delay 2 detik
const debouncedChangeEmail = debounce(changeEmail, 2000); // Delay 2 detik
</script>

<template>
  <div id="app">
    <h1>User Dashboard</h1>
    <div>
      <input v-model="userId" placeholder="Enter User ID" />
      <button @click="debouncedGetUser">Get User Info</button>
    </div>
    <div v-if="users">
      <template v-for="user in users" :key="user.id">
        <h2>{{ user.name }}</h2>
        <p>Email: {{ user.email }}</p>
        <hr />
      </template>
    </div>
    <CommentSection />
    <form @submit.prevent="debouncedChangeEmail">
      <h3>Change Email</h3>
      <input v-model="newEmail" placeholder="New Email" />
      <button type="submit">Submit</button>
    </form>
  </div>
</template>
