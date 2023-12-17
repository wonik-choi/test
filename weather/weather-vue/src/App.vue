<template>
  <div>
    <SiteNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router';
import SiteNavigation from './components/SiteNavigation.vue';

import { ref } from 'vue';
</script>
<style></style>
