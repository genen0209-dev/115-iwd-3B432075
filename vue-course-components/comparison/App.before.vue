<script setup>
import { computed, ref } from 'vue'

// 拆分前示意：商品資料與重複卡片標記集中在同一個檔案。
const products = ref([
  { id: 'book-vue', title: 'Vue 3 實戰手冊', desc: '元件、props 與專案實作，九成新。', price: 320, tags: ['程式書', '九成新'], available: true },
  { id: 'keyboard-87', title: '無線機械鍵盤 87 鍵', desc: '茶軸、藍牙與 USB 雙模，附原廠線材。', price: 1200, tags: ['3C 周邊', '九成新'], available: true },
  { id: 'lamp-vintage', title: '復古桌燈', desc: '宿舍書桌尺寸，暖光燈泡已附。', price: 480, tags: ['宿舍生活', '保存良好'], available: false },
])
const query = ref('')
const favorites = ref([])
const shownProducts = computed(() => products.value.filter((product) => product.title.includes(query.value.trim())))
function toggleFavorite(id) { favorites.value = favorites.value.includes(id) ? favorites.value.filter((item) => item !== id) : [...favorites.value, id] }
</script>

<template>
  <main class="market-page">
    <h1>好物不該被留在角落</h1>
    <input v-model="query" placeholder="搜尋物品" />
    <div class="product-grid">
      <article v-for="product in shownProducts" :key="product.id" class="product-card">
        <span>{{ product.tags.join(' · ') }}</span>
        <h2>{{ product.title }}</h2>
        <p>{{ product.desc }}</p>
        <strong>NT$ {{ product.price.toLocaleString() }}</strong>
        <button type="button" @click="toggleFavorite(product.id)">{{ favorites.includes(product.id) ? '♥ 已收藏' : '♡ 收藏' }}</button>
        <button type="button" :disabled="!product.available">{{ product.available ? '查看好物' : '已售出' }}</button>
      </article>
    </div>
  </main>
</template>
