<template>
  <h1>CH2 データの登録と更新</h1>

  <h2>テキストと属性のデータバインディング</h2>

  <h3>オブジェクトや配列要素の表示</h3>
  <!-- 1 オブジェクトのプロパティを表示 -->
  <p>{{ message }}</p>
  <!-- 2 文字列の長さを表示 -->
  <p>{{ message.length }}</p>
  <!-- 3 リストのインデックス2を表示 -->
  <p>{{ list[2] }}</p>
  <!-- 4 プロパティを組み合わせて使用 -->
  <p>{{ list[num] }}</p>

  <h3>クリックでカウンターを増やそう</h3>
  <div>
    <!-- countプロパティを表示する -->
    <p>{{ count }}回クリックしたよ！ </p>
    <!-- このボタンをクリックするとincrementメソッドが呼び出される -->
    <button @click="increment">カウントを増やす</button>
  </div>

  <h3>クラスとスタイルのデータバインディング</h3>
  <button @click="isActive=!isActive">isActive を切り替える</button>
  <p v-bind:class="{ child: data.isChild, 'is-active': isActive }" class="item">
    動的なクラス
  </p>
  <p v-bind:style="{ color: data.textColor, backgroundColor: data.bgColor }" class="item">
    動的なスタイル
  </p>

  <h3>SVGのデータバインディング</h3>
  <div>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <circle cx="100" cy="75" v-bind:r="radius" fill="lightpink" />
    </svg>
    <input type="range" min="0" max="100" v-model.number="radius">
  </div>

  <h2>リストデータの表示と更新</h2>

  <h3>要素を繰り返し描画する</h3>
  <ul>
    <li
      v-for="item in monsterList"
      :key="item.id"
      :class="{ tuyoi: isTuyoi(item.hp) }"
    >
      ID.{{ item.id }} {{ item.name }} HP.{{ item.hp }}
      <span v-if="isTuyoi(item.hp)">つよい！</span>
    </li>
  </ul>

  <h3>リストに要素を追加しよう</h3>
  <input v-model="monsterName">
  <button @click="doAddMonster">モンスターを追加</button>
  <ul>
    <li v-for="item in monsterList" v-bind:key="item.id">
      ID.{{ item.id }} {{ item.name }} HP.{{ item.hp }}
    </li>
  </ul>

  <h3>リスト要素を削除しよう</h3>
  <ul>
    <li v-for="(item, index) in monsterList" :key="item.id">
      ID.{{ item.id }} {{ item.name }} HP.{{ item.hp }}
      <!-- 削除ボタンをv-for内に作成 -->
      <button @click="doRemove(index)">モンスターを削除</button>
    </li>
  </ul>

  <h3>リスト要素プロパティを更新しよう</h3>
  <ul>
    <li v-for="(item, index) in monsterList" :key="item.id">
      ID.{{ item.id }} {{ item.name }} HP.{{ item.hp }}
      <span v-show="item.hp < 50">瀕死！</span>
      <!-- ボタンはv-for内に作成 -->
      <button @click="doAttack(index)">攻撃する</button>
    </li>
  </ul>

  <h3>外部からデータを取得する</h3>
  <ul>
    <li v-for="(item, index) in MHList" :key="item.id">
      ID.{{ item.id }} {{ item.name }} HP.{{ item.hp }}
    </li>
  </ul>

  <h2>DOM を直接参照する $el と $refs</h2>

  <h3>$elや$refsは一時的な変更！</h3>
  <button v-on:click="handleClick">カウントアップ</button>
  <span ref="refCount">0</span>
</template>

<script setup lang="ts">
import axios, { AxiosResponse } from 'axios'
import { defineComponent, reactive, ref, readonly } from 'vue'

defineComponent({
  name: 'Chapter2',
})

const message = ref('Hello Vue.js!')
const list = ref(['りんご', 'ばなな', 'いちご'])
const num = ref(1)

const count = ref(0)
const increment = (): number => count.value++

const data = readonly({
  isChild: true,
  textColor: 'red',
  bgColor: 'lightgray',
})
const isActive = ref(true)

const radius = ref(50)

type Monster = {
  id: number
  name: string
  hp: number
}

const monsterList = reactive<Monster[]>([
  { id: 1, name: 'スライム', hp: 100 },
  { id: 2, name: 'ゴブリン', hp: 200 },
  { id: 3, name: 'ドラゴン', hp: 500 },
])
const isTuyoi = (hp: number): boolean => hp > 300

const monsterName = ref('キマイラ')
const doAddMonster = () => {
  // リスト内で1番大きいIDを取得
  const max = monsterList.reduce((a, b) => {
    return a > b.id ? a : b.id
  }, 0)
  // 新しいモンスターをリストに追加
  monsterList.push({
    id: max + 1, // 現在の最大のIDに+1してユニークなIDを作成
    name: monsterName.value, // 現在のフォームの入力値
    hp: 500
  })
}
const doRemove = (index: number): void => {
  // 受け取ったインデックスの位置から1個要素を削除
  monsterList.splice(index, 1)
}
const doAttack = (index: number):void => {
  monsterList[index].hp -= 10 // HPを減らす
  if (!monsterList[index].hp) doRemove(index)
}

const MHList = reactive<Monster[]>([])

axios.get('MHList.json').then((response: AxiosResponse<Monster[]>) => {
  // 取得完了したら MHList リストに代入
  response.data.map(data => MHList.push(data))
}).catch((e) => {
  console.error(e)
})

const refCount = ref<HTMLSpanElement>()
const handleClick = () => {
  const count = refCount
  if (count.value) {
    count.value.innerText = (parseInt(count.value.innerText, 10) + 1).toString()
  }
}
</script>

<style scoped>
.item {
  padding: 4px 8px;
  transition: background-color 0.4s;
}
.is-active {
  background-color: #ffeaea;
}
.tuyoi {
  color: #eb516b;
}
</style>
