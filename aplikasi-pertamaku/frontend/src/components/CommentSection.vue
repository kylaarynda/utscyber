<script setup>
import { ref } from 'vue';

const comment = ref('');
const comments = ref([]);

const submitComment = () => {
  comments.value.push(comment.value);  
  comment.value = '';  
};
</script>

<template>
  <div>
    <h3>Comments</h3>
    <input v-model="comment" placeholder="Leave a comment" />
    <button @click="submitComment">Submit</button>
    <div v-for="comment in comments" :key="comment" v-text="comment"></div> 
  </div>
</template>
