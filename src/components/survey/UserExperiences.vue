<script setup>
import { ref, onMounted } from 'vue'
import SurveyResult from './SurveyResult.vue'

// defineProps(['results'])
const results = ref([])

const loadExperiences = async () => {
  isLoading.value = true
  error.value = null
  try {
    const response = await fetch(
      'https://vue-http-demo-c8a60-default-rtdb.firebaseio.com/surveys.json'
    )

    if (response.ok) {
      const data = await response.json()
      isLoading.value = false
      const result = []
      for (const id in data) {
        result.push({ id: id, name: data[id].name, rating: data[id].rating })
      }
      results.value = result
    }
  } catch (err) {
    isLoading.value = false
    error.value = 'Error: ' + err.message
  }
}
const isLoading = ref(false)
const error = ref(null)
onMounted(() => loadExperiences())
</script>
<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading.....</p>
      <p v-else-if="!isLoading && error" style="color: red">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No stored experiences found. Please add some
      </p>
      <ul v-else>
        <survey-result
          v-for="r in results"
          :key="r.id"
          :name="r.name"
          :rating="r.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
