<script setup>
import TotalBalance from './components/TotalBalance.vue';
import HistoryList from './components/HistoryList.vue';
import AddTransition from './components/AddTransition.vue';
</script>

<template>
  <h2>Expense Tracker</h2>
  <div class="container">
    <TotalBalance />
    <HistoryList />
    <AddTransition />
  </div>
</template>

<style scoped></style>
