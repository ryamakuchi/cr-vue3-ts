<template>
  <h1>CH4 データの監視と加工</h1>
  <h2>算出プロパティで処理を含むデータを作成</h2>
  <h3>算出プロパティの使い方</h3>
  <p>{{ data.width }} の半分は {{ halfWidth }}</p>

  <h3>算出プロパティを組み合わせて使用しよう</h3>
  <p>X: {{ halfPoint.x }}</p>
  <p>Y: {{ halfPoint.y }}</p>

  <h3>ゲッターとセッター</h3>
  <input v-model.number="data.width"> {{ data.width }}
  <input v-model.number="halfHeight"> {{ halfHeight }}

  <h3>算出プロパティのキャッシュ機能</h3>
  <p>算出プロパティ</p>
  <ol>
    <li>{{ computedData }}</li>
    <li>{{ computedData }}</li>
  </ol>
  <p>メソッド</p>
  <ol>
    <li>{{ methodsData() }}</li>
    <li>{{ methodsData() }}</li>
  </ol>

  <h3>リストの絞り込みに利用しよう</h3>
  <input v-model.number="budget"> 円以下に絞り込む
  <input v-model.number="limit"> 件を表示
  <p>{{ matched.length }} 件中 {{ limited.length }} 件を表示中</p>
  <ul>
    <!-- v-forでは最終結果、算出プロパティのlimitedを使用する -->
    <li v-for="item in limited" :key="item.id">
      {{ item.name }} {{ item.price }}円
    </li>
  </ul>

  <h3>ソート機能を追加しよう</h3>
  <input v-model.number="budget"> 円以下に絞り込む
  <input v-model.number="limit"> 件を表示
  <button @click="order=!order">切り替え</button>
  <p>{{ matched.length }} 件中 {{ limited.length }} 件を表示中</p>
  <ul>
    <!-- v-forでは最終結果、算出プロパティの limited を使用する -->
    <li v-for="item in sorted" :key="item.id">
      {{ item.name }} {{ item.price }}円
    </li>
  </ul>

  <h2>ウォッチャでデータを監視して処理を自動化</h2>
  <h3>ウォッチャの使い方</h3>
  <a
    href="https://v3.vuejs.org/guide/reactivity-computed-watchers.html#watch"
    target="_blank"
    rel="noopener noreferrer"
  >
    Computed and Watch | Vue.js
  </a>

  <h3>一度だけ動作するウォッチャ</h3>
  {{ edited }}
  <button @click="list[0].name = 'らいち'">りんごをらいちに変更！</button>

  <h3>フォームを監視してAPIからデータを取得しよう</h3>
  <select v-model="current">
    <option v-for="topic in topics" :value="topic.value">
      {{ topic.name }}
    </option>
  </select>
  <div v-for="item in repositories">{{ item.full_name }}</div>

  <h2>フィルタでテキストの変換処理を行う</h2>
  <p>フィルタは削除されました</p>
  <a
    href="https://v3.vuejs.org/guide/migration/filters.html"
    target="_blank"
    rel="noopener noreferrer"
  >
    Filters | Vue.js
  </a>

  <h2>カスタムディレクティブ</h2>
  <!-- <input type="text" v-focus> -->
  <h3>使用可能なフック</h3>
  <a
    href="https://v3.vuejs.org/guide/migration/custom-directives.html"
    target="_blank"
    rel="noopener noreferrer"
  >
  Custom Directives | Vue.js
  </a>

  <h2>nextTick で更新後の DOM にアクセスする</h2>
  <button @click="nextTickList.push(nextTickList.length + 1)">追加</button>
  <ul ref="nextTickRef">
    <li v-for="item in nextTickList">{{ item }}</li>
  </ul>
</template>

<script setup lang="ts">
import { defineComponent, computed, ref, reactive, watch, nextTick } from 'vue'
import axios, { AxiosResponse } from 'axios'

defineComponent({
  name: 'Chapter4',
  // directives: {
  //   focus: {
  //     mounted: (el) => {
  //       console.log(el)
  //     }
  //   }
  // }
})

const data = reactive({
  width: 800,
  height: 600,
})
const halfWidth = computed(() => data.width / 2)
const halfHeight = computed({
  get: () => data.height / 2,
  set: val => data.height = val * 2
})

const halfPoint = computed(() => {
  return {
    x: halfWidth.value,
    y: halfHeight.value
  }
})

const computedData = computed(() => Math.random())
const methodsData = () => Math.random()

const budget = ref(300) // フォームの入力と紐付けるデータ
const limit = ref(2) // 表示件数

const order = ref(false)
const edited = ref(false)

type Item = {
  id: number
  name: string
  price: number
}
const list = reactive<Item[]>([ // もとになるリスト
  { id: 1, name: 'りんご', price: 100 },
  { id: 2, name: 'ばなな', price: 200 },
  { id: 3, name: 'いちご', price: 400 },
  { id: 4, name: 'おれんじ', price: 300 },
  { id: 5, name: 'めろん', price: 500 }
])
// budget 以下のリストを返す算出プロパティ
const matched = computed(() => {
  return list.filter((el) => {
    return el.price <= budget.value
  })
})
// matched で返ったデータを limit 件返す算出プロパティ
const limited = computed(() => {
  return matched.value.slice(0, limit.value)
})
// sorted を新しく追加
const sorted = computed(() => {
  return limited.value.sort((a, b) => {
    if (order.value) {
      if(a.price < b.price) return -1
      if(a.price > b.price) return 1
      return 0
    } else {
      if(a.price > b.price) return -1
      if(a.price < b.price) return 1
      return 0
    }
  })
})

const unwatch = watch(() => list, () => {
  // listが編集されたことを記録する
  edited.value = !edited.value
  // 監視を解除
  unwatch()
}, { deep: true })

const current = ref('')
const repositories = ref<Repositorie[]>([])
const topics = reactive([
  { value: 'vue', name: 'Vue.js' },
  { value: 'react', name: 'React' },
])

type Repositorie = {
  full_name: string
}

watch(() => current.value, (val) => {
  axios.get('https://api.github.com/search/repositories', {
    params: {
      q: 'topic:' + val
    }
  }).then((response: AxiosResponse<{ items: Repositorie[] }>) => {
    repositories.value = response.data.items
  })
})

const nextTickList = reactive<number[]>([])
const nextTickRef = ref<HTMLUListElement>()
watch(() => nextTickList, () => {
  console.log('通常:', nextTickRef.value?.offsetHeight)
  // nextTick を使えばできる！
  nextTick(() => {
    console.log('nextTick:', nextTickRef.value?.offsetHeight)
  })
})
</script>
