<template>
  <h3>Add new transaction</h3>
  <Input :message="TextLabel" :placeholderText="TextPH" v-model="TextInput" />
  <Input :message="AmountLabel" :placeholderText="AmountPH" v-model="AmountInput" />
  <div>{{ test }}</div>
</template>
<script setup>
import Input from './Input.vue';
import { ref, computed } from 'vue';

const TextLabel = ref('Text');
const TextPH = ref('Enter Text...');
const TextInput = ref('');

const AmountLabel = ref('Amout');
const AmountPH = ref('Enter amount...');
const AmountInput = ref('');

const test = computed(() => {
  return TextInput.value + AmountInput.value;
});
</script>
<style scoped></style>
