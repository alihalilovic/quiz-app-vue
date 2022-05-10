<script setup>
defineProps(["data", "position"])

const emit = defineEmits(["answerClick"])
</script>

<template>
  <div class="answer"
    v-for="answer in data[position].answers"
    @click="emit('answerClick', {answer, position})"
  >
    {{ answer }}
  </div>
</template>

<style scoped>
.answer {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-radius: 10px;
  background: rgba(0, 0, 0, 0.45);
  font-weight: bold;
  padding: 1.5rem;
  margin: .1rem;
  transition: 0.2s ease;
}

@media(min-width: 1100px) {
  .answer:hover {
    cursor: pointer;
    background: rgba(0, 0, 0, 0.2);
  }
}

</style>
