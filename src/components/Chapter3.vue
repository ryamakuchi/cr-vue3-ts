<template>
  <h1>CH3 イベントとフォーム入力の受け取り</h1>
  <h2>イベントハンドリング</h2>

  <h3>メソッドイベントハンドラ</h3>
  <button @click="handleClick">クリック</button>
  <p>※ ボタンを押すとアラートが出現します</p>

  <h3>フォーム入力の取得</h3>
  <input :value="message" @change="handleInput">

  <h3>イベント修飾子</h3>
  <div @click.right="handler('右クリック')">example</div>
  <!-- 右ボタンを押した時のコンテキストメニューを表示させない -->
  <div @click.right.prevent="handler('右クリック')">example</div>

  <h3>.stop</h3>
  <div @click="handler('div1')">
    div1
    <a href="#" @click.stop="handler('div2')">div2</a>
  </div>

  <h3>.prevent</h3>
  <div @click="handler('div1')">
    div1
    <a href="#" @click.prevent="handler('div2')">div2</a>
  </div>

  <h3>.capture</h3>
  <div @click.capture="handler('div1')">
    div1
    <div @click="handler('div2')">
      div2
      <div @click="handler('div3')">div3</div>
    </div>
  </div>

  <h3>.native 修飾子は削除されました</h3>
  <MyComponent @close="handler('emit')" />
  <a
    href="https://v3.vuejs.org/guide/migration/v-on-native-modifier-removed.html#_3-x-syntax"
    target="_blank"
    rel="noopener noreferrer"
  >
    v-on.native modifier removed | Vue.js
  </a>

  <h2>フォーム入力バインディング</h2>

  <h3>複数行テキスト</h3>
  <textarea v-model="message"></textarea>
  <pre>{{ message }}</pre>

  <h3>チェックボックス</h3>
  <h4>単一要素</h4>
  <label>
    <input type="checkbox" v-model="val"> {{ val }}
  </label>

  <h4>複数要素</h4>
  <label><input type="checkbox" v-model="valList" value="A">A</label>
  <label><input type="checkbox" v-model="valList" value="B">B</label>
  <label><input type="checkbox" v-model="valList" value="C">C</label>
  <p>{{ valList }}</p>

  <h3>ラジオボタン</h3>
  <label><input type="radio" value="a" v-model="radioVal"> A</label>
  <label><input type="radio" value="b" v-model="radioVal"> B</label>
  <label><input type="radio" value="c" v-model="radioVal"> C</label>
  <p>{{ radioVal }}</p>

  <h3>セレクトボックス</h3>
  <h4>単一要素</h4>
  <select v-model="selectVal">
    <option :disabled="true">選択してください</option>
    <option value="a">A</option>
    <option value="b">B</option>
    <option value="c">C</option>
  </select>
  <p>{{ selectVal }}</p>

  <h4>複数要素</h4>
  <select v-model="selectValMultiple" multiple>
    <option value="a">A</option>
    <option value="b">B</option>
    <option value="c">C</option>
  </select>
  <p>{{ selectValMultiple }}</p>

  <h3>画像ファイル</h3>
  <input type="file" @change="handleChange">
  <img v-if="preview" :src="preview">

  <h2>マウント要素外のイベントと操作</h2>
  <h3>スムーススクロールの実装</h3>
  <button @click="scrollTop">
    ページ上部へ移動
  </button>
</template>

<script setup lang="ts">
import { defineComponent, ref } from 'vue'
import MyComponent from './MyComponent.vue'

defineComponent({
  name: 'Chapter3',
  components: {
    MyComponent,
  }
})

const handleClick = () => {
  alert('クリックしたよ')
}

const message = ref('')
const val = ref(true)
const valList = ref<string[]>([])
const radioVal = ref('')
const selectVal = ref('')
const selectValMultiple = ref<string[]>([])

const handleInput = (event: Event): void => {
  if (event.target instanceof HTMLInputElement) {
    // 代入前に何か処理を行う...
    console.log(event.target.value)
    message.value = event.target.value
  }
}

const handler = (comment: string): void => {
  console.log(comment)
}

const preview = ref('')
const handleChange = (event: Event): void => {
  if (event.target instanceof HTMLInputElement) {
    const file = event.target.files?.[0]
    if (file && file.type.match(/^image\/(png|jpeg)$/)) {
      preview.value = window.URL.createObjectURL(file)
    }
  }
}

const scrollTop = (): void => scroll({ top: 0, behavior: 'smooth'})
</script>
