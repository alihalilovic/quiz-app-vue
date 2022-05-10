<script setup>
defineProps(["finished"])

const emit = defineEmits(["restart"])
</script>

<template>
  <div v-if="finished" class="finished-container">
    <h2>Quiz finished!</h2>
    <button class="btn" @click="emit('restart')">Restart</button>
  </div>
</template>

<style scoped>
.finished-container {
  display: flex;
  flex-direction: column;
  text-align: center;
}

.finished-container > * {
  padding: 1rem 1.5rem;
}

.finished-container > h2 {
  text-transform: lowercase;
  font-weight: 200;
  font-size: 1.2rem;
}

.btn {
  border-radius: 10px;
  border: none;
  outline: none;
  background: rgba(255, 255, 255, 0.5);
  color: white;
  text-transform: uppercase;
  font-size: 1.2rem;
  width: 50%;
  margin: auto;
}
</style>
