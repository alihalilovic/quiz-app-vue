<script setup>
import { ref } from "vue";

const customQuestion = ref();
const customAnswer = ref();
const customAnswers = ref();

const emit = defineEmits(["addQuestionBlock", "hideForm"]);

const handleSubmitQuestionBlock = () => {
  if (!customQuestion.value || !customAnswer.value || !customAnswers.value)
    return;

  emit("addQuestionBlock", {
    customQuestion: customQuestion.value,
    customAnswer: customAnswer.value,
    customAnswers: customAnswers.value,
  });

  customQuestion.value = "";
  customAnswer.value = "";
  customAnswers.value = "";
};
</script>

<template>
  <div class="form-container">
    <form @submit.prevent="handleSubmitQuestionBlock">
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
        <button class="btn submit">Submit</button>
      </label>
      <label>
        <div class="btn hide-form" @click="emit('hideForm')">Hide form</div>
      </label>
    </form>
  </div>
</template>

<style scoped>
.form-container {
  background: linear-gradient(45deg, #17225c, #2b998e);
  width: 100vw;
  height: 100vh;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 999;

  display: flex;
  align-items: center;
  justify-content: center;
}

form {
  display: flex;
  flex-direction: column;
  max-width: 550px;
}

form label {
  display: flex;
  flex-direction: column;
  padding: 0.5rem;
}

form input,
textarea {
  padding: 0.5rem;
  font-size: 1.2rem;
  border: none;
  outline: none;
  border-radius: 7px;
  margin-top: .5rem;
}

.submit {
  background: green;
  color: white;
}

.hide-form {
  background: red;
  text-align: center;
}
</style>
