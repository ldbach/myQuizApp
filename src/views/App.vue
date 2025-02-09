<script setup>
import { ref } from 'vue'
import Quiz from './Quiz.vue'

const gameStarted = ref(false)

const startQuiz = () => {
  gameStarted.value = true
}
</script>

<template>
  <div class="home">
    <div v-if="!gameStarted">
      <h2>Welcome to the Football Quiz!</h2>
      <p>Test your knowledge about football. Are you ready?</p>
      <button @click="startQuiz">Start Game</button>
      <p>Good luck!</p>
    </div>

    <Quiz v-if="gameStarted" @restartGame="startQuiz" />
  </div>
</template>

<style scoped>
.home {
  text-align: center;
}
</style>
