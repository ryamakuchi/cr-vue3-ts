<template>
  <h1>CH5 コンポーネントで UI 部品を作る</h1>
  <h2>コンポーネント間の通信 / 親から子</h2>
  <CompChild val="これは子 A" />
  <CompChild :val="'これは子 B'" />

  <h2>コンポーネントをリストレンダリング</h2>
  <CompChild @childsEvent="parentsMethod()" />
</template>

<script setup lang="ts">
import { defineComponent } from 'vue'
import CompChild from './CompChild.vue'

defineComponent({
  name: 'Chapter5',
  components: {
    CompChild,
  },
})

const parentsMethod = (): void => alert('イベントをキャッチ！')
</script>
