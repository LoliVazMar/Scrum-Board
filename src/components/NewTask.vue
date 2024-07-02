<script setup>
import { ref } from 'vue'
//const props = defineProps(["onNewItem"]);
const text = ref('')
const emits = defineEmits(['onNewItem'])

function handleKeyDown(evt) {
  if (evt.key === 'Enter') {
    emits('onNewItem', text)
    text.value = ''
  }
}
</script>

<template><input v-model="text" @keydown="handleKeyDown" placeholder="+ New task" /></template>

<style lang="scss" scoped>
@import '../assets/base.scss';

input {
  background-color: rgba(255, 255, 255, 0.8);
  color: $secondary-color;
  border: none;
  border-radius: 0.25rem;
  width: 100%;
}
</style>
