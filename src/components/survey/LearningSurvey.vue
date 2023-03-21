<script setup>
import { ref } from 'vue'

const enteredName = ref('')
const chosenRating = ref(null)
const invalidInput = ref(false)
const error = ref(null)

// const emit = defineEmits(['survey-submit'])

const submitSurvey = () => {
  if (enteredName.value === '' || !chosenRating.value) {
    invalidInput.value = true
    return
  }
  invalidInput.value = false

  // emit('survey-submit', { userName: enteredName.value, rating: chosenRating.value })
  error.value = null
  fetch('https://vue-http-demo-c8a60-default-rtdb.firebaseio.com/surveys.json', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: enteredName.value, rating: chosenRating.value })
  }).catch((err) => {
    console.log(err)
    error.value = 'Error: Failed to post survey'
  })

  enteredName.value = ''
  chosenRating.value = null
}
</script>

<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input type="radio" id="rating-poor" value="poor" name="rating" v-model="chosenRating" />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <p v-if="invalidInput">
          One or more input fields are invalid. Please check your provided data.
        </p>
        <p v-if="error" style="color: red">{{ error }}</p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>
