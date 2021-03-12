<template>
  <h2>CH1 Vue.js とフレームワークの基礎知識</h2>

  <h3>テキストのバインディング</h3>
  {{ data.message }}

  <h3>繰り返しの描画</h3>
  <ol>
    <li v-for="item in data.list">{{ item }}</li>
  </ol>

  <h3>イベントの利用</h3>
  <button @click="handleClick">Click</button>
  <p>※ ボタンを押すとアラートが出現します</p>

  <h3>フォーム入力との同期</h3>
  <input v-model="message">
  <p>{{ message }}</p>

  <h3>条件分岐</h3>
  <button @click="show1=!show1">切り替え</button>
  <p v-if="show1">Hello Vue.js!</p>

  <h3>条件分岐</h3>
  <button v-on:click="show2=!show2">切り替え</button>
  <transition>
    <p v-if="show2">Hello Vue.js!</p>
  </transition>
</template>

<script setup lang="ts">
import { defineComponent, readonly, ref } from 'vue'
defineComponent({
  name: 'Chapter1',
})

const data = readonly({
  message: 'Hello Vue.js!',
  list: ['りんご', 'ばなな', 'いちご'],
})

const handleClick = (event: Event): void => {
  alert(event.target)
}

const message = ref('初期メッセージ')

const show1 = ref(true)
const show2 = ref(true)
</script>

<style scoped>
.v-enter-active, .v-leave-active {
  transition: opacity 1s;
}
/* opacity:0から1へのフェードイン＆フェードアウト */
.v-enter-from, .v-leave-to {
  opacity: 0;
}
</style>
