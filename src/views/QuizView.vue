<script setup>
import Question from '../components/Question.vue'
import QuizHeader from '../components/QuizHeader.vue'
import Result from '@/components/Result.vue'
import { useRoute } from 'vue-router'
import { ref, computed, watch } from 'vue'
import quizes from '../data/quizzes.json'
import { RouterLink } from 'vue-router'

const route = useRoute()
const quizId = parseInt(route.params.id)
const quiz = quizes.find((q) => q.id === quizId)
const currentQuestionIndex = ref(0)
const numberOfCorrectAnswers = ref(0)
const showResults = ref(false)

//  const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

//  watch(()=>currentQuestionIndex.value,()=>{
//         questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
//  })

const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)

const barPercentage = computed(
  () => `${(currentQuestionIndex.value / quiz.questions.length) * 100}%`,
)

const onOptionSelected = (isCorrect) => {
  console.log('emit is call', isCorrect)
  if (isCorrect) {
    numberOfCorrectAnswers.value++
  }

  if (quiz.questions.length - 1 === currentQuestionIndex.value) {
    showResults.value = true
    console.log('show result')
  } else {
    currentQuestionIndex.value++
  }
}

// const onOptionSelected = (isCorrect) => {
//     if(isCorrect){
//         numberOfCorrectAnswers.value++;
//     }

//     if(quiz.questions.length - 1 === currentQuestionIndex.value){
//         showResults.value = true
//     }

//     currentQuestionIndex.value++;
// }
</script>

<template>
  <div>
    <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />
    <div>
      <!-- v-if="!showResults"
                :question="quiz.questions[currentQuestionIndex]"
                @selectOption="onOptionSelected" -->
      <Question
        v-if="!showResults"
        :question="quiz.questions[currentQuestionIndex]"
        @selectOption="onOptionSelected"
      />
      <Result
        v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswers="numberOfCorrectAnswers"
      />
    </div>
    <RouterLink v-if="!showResults" to="/">Go Back</RouterLink>
  </div>
</template>
