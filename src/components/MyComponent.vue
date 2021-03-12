<template>
  <button @click="$emit('close')">MyComponent</button>
</template>

<script setup lang="ts">
import { defineComponent } from 'vue'
defineComponent({
  name: 'MyComponent',
  emits: ['close'],
})
</script>
