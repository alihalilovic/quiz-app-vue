<script setup>
import {ref} from "vue"

const customQuestion = ref()
const customAnswer = ref()
const customAnswers = ref()

const emit = defineEmits(["addQuestionBlock"])

const handleSubmitQuestionBlock = () => {
  emit("addQuestionBlock", {
    customQuestion: customQuestion.value,
    customAnswer: customAnswer.value,
    customAnswers: customAnswers.value
  })

  customQuestion.value = ""
  customAnswer.value = ""
  customAnswers.value = ""
}
</script>

<template>
  <p>Add your question and answer</p>
  <form
    @submit.prevent="handleSubmitQuestionBlock"
    >
    <label
      >Question:
      <input v-model="customQuestion" type="text" />
    </label>
    <label
      >Answer:
      <input v-model="customAnswer" type="text" />
    </label>
    <label
      >Possible answers (separate answers by comma):
      <textarea v-model="customAnswers" cols="30" rows="10"></textarea>
    </label>
    <label>
      <button>Submit</button>
    </label>
  </form>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  width: 50%;
}

form label {
  display: flex;
  flex-direction: column;
  padding: 0.5rem;
}
</style>
