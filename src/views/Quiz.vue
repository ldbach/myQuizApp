<script setup>
import { ref, computed } from 'vue'
import '../style.css'

const questions = ref([
  {
    question: 'Which country won the FIFA World Cup in 2018?',
    answer: 2,
    options: [
      'Brazil',
      'Germany',
      'France',
      'Argentina'
    ],
    selected: null
  },
  {
    question: 'Who has won the most Ballon d\'Or awards?',
    answer: 1,
    options: [
      'Cristiano Ronaldo',
      'Lionel Messi',
      'Diego Maradona',
      'Pele'
    ],
    selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  }
  else {
    quizCompleted.value = true
  }
}

const restartGame = () => {
  // Reset game state
  quizCompleted.value = false
  currentQuestion.value = 0
  questions.value.forEach(q => {
    q.selected = null
  })
  // Emit event to restart game in App.vue
  emit('restartGame')
}

</script>

<template>
  <main class = "app">
  <div class="quiz">
  <h1>The Football Quiz</h1>

    <section class = "quiz" v-if = "!quizCompleted">
      <div class="quiz-info">
        <span class = "question">{{ getCurrentQuestion.question }}</span>
        <span class = "score">Score {{ score }}/{{ questions.length }} </span>
      </div>

      <div class = "option">
        <label 
        v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
          <input 
          type = "radio"
          :name = "getCurrentQuestion.index"
          :value = "index" 
          v-model = "getCurrentQuestion.selected" 
          :disabled= "getCurrentQuestion.selected" 
          @change = "SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

      <button 
      @click = "NextQuestion" 
      :disabled = "!getCurrentQuestion.selected">
    {{
      getCurrentQuestion.index == questions.length - 1 
      ? 'Finish' 
      : getCurrentQuestion.selected == null 
      ? 'Select an option' 
      : 'Next Question'
    }}
    </button>
    </section>

    <section v-else>
      <h2>Finish the quiz!</h2>
      <p>Your score is : {{ score }}/{{ questions.length }}</p>
      <button @click="restartGame">Restart Game</button>
    </section>
  </div>
  </main>
</template>

