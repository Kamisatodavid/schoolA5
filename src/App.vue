<script setup>
import { ref } from "vue";
</script>

<template>
  <div class="app-container">
    <Suspense>
      <RouterView />
    </Suspense>
  </div>
</template>

<style scoped>
</style>
