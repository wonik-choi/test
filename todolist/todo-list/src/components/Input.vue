<template>
  <div class="form-control">
    <label for="text">{{ message }}</label>
    <input type="text" id="text" :value="modelValue" :placeholder="placeholderText" @input="emitValue" />
  </div>
</template>
<script setup>
const emit = defineEmits(['update:modelValue']);
defineProps(['modelValue', 'message', 'placeholderText']);

const emitValue = (e) => {
  let value = e.target.value;
  emit('update:modelValue', value);
};
</script>
<style scoped></style>
