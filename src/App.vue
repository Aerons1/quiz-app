<template>
  <div class="app" v-touch:swipe.left="nextQuestion" v-touch:swipe.right="prevQuestion">
    <div class="card" @click="revealAnswer">
      <h2>{{ currentQuestion.question }}</h2>

      <transition name="fade" mode="out-in">
        <p :key="showAnswer" class="tap answer">
          {{ showAnswer ? currentQuestion.answer : 'Tap to reveal' }}
        </p>
      </transition>
    </div>

    <div class="controls">
      <button @click="prevQuestion" :disabled="currentIndex === 0">Previous</button>
      <button @click="nextQuestion" :disabled="currentIndex === questions.length - 1">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentIndex: 0,
      showAnswer: false,
      questions: [
        { question: "What is the capital of France?", answer: "Paris" },
        { question: "What is 2 + 2?", answer: "4" },
        { question: "What language is Vue.js written in?", answer: "JavaScript" }
      ]
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentIndex];
    }
  },
  methods: {
    revealAnswer() {
      this.showAnswer = !this.showAnswer;
    },
    nextQuestion() {
      if (this.currentIndex < this.questions.length - 1) {
        this.currentIndex++;
        this.showAnswer = false;
      }
    },
    prevQuestion() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.showAnswer = false;
      }
    }
  }
};
</script>
