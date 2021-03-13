<template>
  <p v-if="val">{{ val }}</p>
  <button v-if="!val" @click="handleClick()">イベント発火</button>
</template>

<script setup lang="ts">
import { defineProps, defineEmit } from 'vue'

defineProps({
  val: {
    type: String,
    default: '',
  },
})

const emit = defineEmit(['childsEvent'])

const handleClick = () => {
  emit('childsEvent')
}
</script>
