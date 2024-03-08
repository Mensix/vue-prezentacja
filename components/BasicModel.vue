<script setup lang="ts">
import { ref } from 'vue'

const name = ref('')
</script>

<template>
  <input v-model="name" class="border-black border" />
  <p>{{ name }}</p>
</template>