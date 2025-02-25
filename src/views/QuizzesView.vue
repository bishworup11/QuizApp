<script setup>
import q from '../data/quizzes.json'
import { ref, watch } from 'vue'
import gsap from 'gsap'
import Card from '../components/Card.vue'
const quizzes = ref(q)
const search = ref('')
watch(search, () => {
  quizzes.value = q.filter((quiz) => quiz.name.toLowerCase().includes(search.value.toLowerCase()))
})

function beforeEnter(el) {
  el.style.opacity = 0
  el.style.transform = 'translateY(-50px)'
}

function enter(el, done) {
  gsap.to(el, {
    opacity: 1,
    y: 0,
    duration: 0.5,
    delay: el.dataset.index * 0.3,
    onComplete: done,
  })
}

function afterEnter(el) {
  el.style.opacity = ''
  el.style.transform = ''
}

function beforeLeave(el) {
  el.style.opacity = 1
  el.style.transform = 'translateY(0)'
}

function leave(el, done) {
  gsap.to(el, {
    opacity: 0,
    y: 50,
    duration: 0.5,
    delay: el.dataset.index * 0.01,
    onComplete: done,
  })
}

//
</script>

<template>
  <div>
    <header>
      <h1>Quizzes</h1>
      <input v-model.trim="search" type="text" placeholder="Search..." />
    </header>
    <div class="options-container">
      <!-- name="card" -->
      <TransitionGroup
        appear
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
        @before-leave="beforeLeave"
        @leave="leave"
      >
        <Card v-for="(quiz, index) in quizzes" :key="quiz.id" :quiz="quiz" :data-index="index" />
      </TransitionGroup>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 1000px;
  margin: 0 auto;
}

header {
  margin-bottom: 10px;
  margin-top: 30px;
  display: flex;
  align-items: center;
}

header h1 {
  font-weight: bold;
  margin-right: 30px;
}

header input {
  border: none;
  background-color: rgba(128, 128, 128, 0.1);
  padding: 10px;
  border-radius: 5px;
}

.options-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 40px;
}

/* CARD */
.card-enter-from {
  opacity: 0;
  transform: translateY(-50px);
}

.card-enter-to {
  opacity: 1;
  transform: translateY(0);
}

.card-enter-active {
  transition: all 0.5s;
}

.card-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.card-leave-to {
  opacity: 0;
  transform: translateY(50px);
}

.card-leave-active {
  transition: all 0.5s;
}
</style>
