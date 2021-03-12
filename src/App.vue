<template>
  <header class="header">
    <h1>Vue 3 + TypeScript + Vite inspired「基礎から学ぶ Vue.js」</h1>
  </header>

  <div class="container">
    <side class="menu">
      <ul>
        <li v-for="(menu, index) in menuList">
          <button
            :class="Number(index) === activeMenu ? 'is-active' : ''"
            @click="changeMenu(Number(index))"
          >
            {{ menu }}
          </button>
        </li>
      </ul>
    </side>

    <main class="main">
      <template v-if="activeMenu === 0">
        <img alt="Vue logo" src="./assets/logo.png" />
        <HelloWorld />
      </template>

      <template v-else>
        <component :is="evalFunc(activeMenu)" />
      </template>
    </main>
  </div>
</template>

<script setup lang="ts">
import { defineComponent, readonly, ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import Chapter1 from './components/Chapter1.vue'
import Chapter2 from './components/Chapter2.vue'

defineComponent({
  name: 'App',
  components: {
    HelloWorld,
    Chapter1,
    Chapter2,
  },
})

type Chapter = typeof Chapter2

const menuList = readonly({
  1: 'CHAPTER 1',
  2: 'CHAPTER 2',
  3: 'CHAPTER 3',
  4: 'CHAPTER 4',
  5: 'CHAPTER 5',
  6: 'CHAPTER 6',
  7: 'CHAPTER 7',
  8: 'CHAPTER 8',
  9: 'CHAPTER 9',
})
const activeMenu = ref(0)

const changeMenu = (menu: number): void => {
  activeMenu.value = menu
}
const evalFunc = (menu: number): Chapter => eval(`Chapter${menu}`)
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>

<style lang="scss" scoped>
.header {
  padding: 0 2rem;
}
.container {
  display: flex;
}
.menu {
  ul {
    list-style: none;
    padding: 0 2rem 2rem;

    li {
      margin: 1rem;
    }

    button {
      color: #2c3e50;
      font-size: 1rem;
      font-weight: bold;
      border: none;
      outline: none;
      background-color: white;

      &:hover {
        cursor: pointer;
      }
      &.is-active {
        color: #3eaf7c;
      }
    }
  }
}
</style>
