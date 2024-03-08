<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++" class="px-4 py-2 flex items-center justify-center rounded-lg border-4 border-gray-200 bg-gray-50 text-sm font-semibold">
    Licznik: {{ count }}
  </button>
</template>

<style>
button {
  /* ... */
}
</style>