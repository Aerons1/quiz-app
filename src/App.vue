<script setup>
import { ref, onMounted } from 'vue'
import './style.css'

const questions = ref([
  {
    question: 'What is the capital of France?',
    correct: 'right',
    left: 'Berlin',
    right: 'Paris',
  },
  {
    question: '2 + 2 = ?',
    correct: 'left',
    left: '4',
    right: '5',
  },
  {
    question: 'What color is the sky?',
    correct: 'right',
    left: 'Green',
    right: 'Blue',
  },
])

const current = ref(0)
const isComplete = ref(false)
const retry = ref(false)
const cardRef = ref(null)

let touchStartX = 0
let touchEndX = 0

function handleGesture() {
  const diff = touchEndX - touchStartX
  if (Math.abs(diff) < 50) return

  const direction = diff > 0 ? 'right' : 'left'
  handleChoice(direction)
}

function handleChoice(dir) {
  const question = questions.value[current.value]
  if (dir === question.correct) {
    retry.value = false
    if (current.value < questions.value.length - 1) {
      current.value++
    } else {
      isComplete.value = true
    }
  } else {
    retry.value = true
  }
}

function restartQuiz() {
  current.value = 0
  isComplete.value = false
  retry.value = false
}

onMounted(() => {
  const card = cardRef.value
  card.addEventListener('touchstart', (e) => {
    touchStartX = e.changedTouches[0].screenX
  })

  card.addEventListener('touchend', (e) => {
    touchEndX = e.changedTouches[0].screenX
    handleGesture()
  })
})
</script>

<template>
  <div class="app">
    <div v-if="isComplete" class="complete-screen">
      <h2>🎉 Quiz Completed!</h2>
      <button @click="restartQuiz">Restart Quiz</button>
    </div>

    <div v-else class="card-container">
      <div class="card swipe-area" ref="cardRef">
        <h2>{{ questions[current].question }}</h2>
        <div class="options">
          👈 {{ questions[current].left }} | {{ questions[current].right }} 👉
        </div>
        <div v-if="retry" class="retry-message">Incorrect. Please try again!</div>
      </div>

      <div class="controls">
        <button @click="handleChoice('left')">Swipe Left</button>
        <button @click="handleChoice('right')">Swipe Right</button>
      </div>
    </div>
  </div>
</template>
