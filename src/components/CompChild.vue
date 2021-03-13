<template>
  <p v-if="val">{{ val }}</p>
  <button v-if="!val" @click="handleClick()">イベント発火</button>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    val: {
      type: String,
      default: '',
    },
  },
  emits: ['childsEvent'],
  setup(_, context) {
    return {
      handleClick: () => {
        context.emit('childsEvent')
      }
    }
  },
})
</script>
