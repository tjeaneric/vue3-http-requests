<script setup>
// import { ref } from 'vue'
import LearningSurvey from './components/survey/LearningSurvey.vue'
import UserExperiences from './components/survey/UserExperiences.vue'

// const savedSurveyResults = ref([])

// const storeSurvey = (surveyData) => {
//   const surveyResult = {
//     id: new Date().toISOString(),
//     name: surveyData.userName,
//     rating: surveyData.rating
//   }

//   savedSurveyResults.value.push(surveyResult)
//   console.log(surveyResult)
// }
</script>

<template>
  <learning-survey></learning-survey>
  <user-experiences></user-experiences>
</template>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>
