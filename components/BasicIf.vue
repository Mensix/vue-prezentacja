<script setup lang="ts">
import { ref } from 'vue'

const number = ref(0)
</script>

<template>
  <p v-if="number > 0">Liczba większa niż zero!</p>
  <p v-else-if="number < 0">Liczba mniejsza niż zero!</p>
  <p v-else>Liczba równa zero!</p>
</template>