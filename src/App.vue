<script setup>
import { ref, computed } from 'vue';
import Card from './components/Card.vue';
import CardBody from './components/CardBody.vue';

const items = ref([
  {
    id: 1,
    name: 'アボカドディップバケット',
    description:
      '刻んだ野菜をアボカドと混ぜてディップに。こんがり焼いたバゲットとお召し上がりください。',
    price: 480,
    image: '/images/item1.jpg',
    soldOut: false,
    isSelected: false,
    link: 'https://handson.vuejs-jp.org/'
  },
  {
    id: 2,
    name: 'あの日夢見たホットケーキ',
    description:
      '子供のころに食べたかった、あのホットケーキを再現しました。素朴でどこか懐かしい味をどうぞ。',
    price: 1180,
    image: '/images/item2.jpg',
    soldOut: false,
    isSelected: false
  },
  {
    id: 3,
    name: 'HOP WTR',
    description:
      'ロサンゼルス生まれのスパークリングウォーター。ノンカロリー、ノンアルコールの新感覚飲料です。',
    price: 320,
    image: '/images/item3.jpg',
    soldOut: true,
    isSelected: false
  },
  {
    id: 4,
    name: 'チーズフレンチフライ',
    description:
      'イタリア産チーズをたっぷりかけたアツアツのフレンチフライ。みんな大好きな一品です。',
    price: 670,
    image: '/images/item4.jpg',
    soldOut: false,
    isSelected: false
  }
])

const changeSoldOut = (id) => {
  const pickElm = items.value.find(item => item.id === id);
  if (pickElm) {
    pickElm.soldOut = true;
  }
}

const stockItem = (item) => {
    item.soldOut = false;
}

const stockQuantity = () => {
  return items.value.filter(item => item.soldOut === false).length;
}

const getDate = () => {
  const date = new Date();
  return date.toLocaleString('ja-JP', { timeZone: 'Asia/Tokyo' });
}

const stockQuantityComputed = computed(() => {
  return items.value.filter(item => item.soldOut === false).length;
})

const getDateComputed = computed(() => {
  const date = new Date();
  return date.toLocaleString('ja-JP', { timeZone: 'Asia/Tokyo' });
})
</script>

<template>
  <header class="header">
    <img 
      src="/images/logo.svg"
      alt="">
    <h1>Vue.js ハンズオン</h1>
  </header>
  <div>取扱商品数 (function): {{ stockQuantity() }}</div>
  <div>現在時刻 (function): {{ getDate() }}</div>
  <div>取扱商品数 (computed): {{ stockQuantityComputed }}</div>
  <div>現在時刻 (computed): {{ getDateComputed }}</div>
  <main class="main">
    <template
      v-for="item in items"
      :key="item.id">
      <div
        v-if="!item.soldOut"
        class="item"
        :class="{'selected-item': item.isSelected}"
        @click="item.isSelected = !item.isSelected"
      >
        <Card
          :id="item.id"
          :name="item.name"
          :price="item.price"
          :image="item.image"
          @sold-out="changeSoldOut"
        >
          <template #body>
            <CardBody
              :description="item.description"
              :link="item.link"
            />
          </template>
        </Card>
      </div>
      <div v-else>
        売り切れです！<button type="button" @click="stockItem(item)">入荷</button>
      </div>
    </template>
  </main>
</template>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin-top: 40px;
  margin-bottom: 40px;
}

.header > img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header > h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin-top: 0;
  margin-bottom: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

.selected-item {
  background-color: #e3f2fd;
}
</style>