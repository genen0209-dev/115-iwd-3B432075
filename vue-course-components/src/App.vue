<script setup>
import { computed, ref } from 'vue'
import ProductCard from './components/ProductCard.vue'
import ActionButton from './components/ActionButton.vue'

const products = ref([
  { id: 'book-vue', title: 'Vue 3 實戰手冊', desc: '從元件、props 到專案實作，適合正在練習前端的同學。', price: 320, tags: ['程式書', '九成新'], category: '書籍', available: true, art: 'book', artLabel: 'LEARN BY BUILDING' },
  { id: 'keyboard-87', title: '無線機械鍵盤 87 鍵', desc: '茶軸手感、藍牙與 USB 雙模，附原廠線材。', price: 1200, tags: ['3C 周邊', '九成新'], category: '3C', available: true, art: 'keyboard', artLabel: 'TYPE SOMETHING GOOD' },
  { id: 'lamp-vintage', title: '復古桌燈', desc: '宿舍書桌尺寸，暖光燈泡已附，外觀保存良好。', price: 480, tags: ['宿舍生活', '保存良好'], category: '生活', available: false, art: 'lamp', artLabel: 'A LITTLE WARMTH' },
])
const query = ref('')
const category = ref('全部')
const sortOrder = ref('recommended')
const favorites = ref([])
const newProductTitle = ref('')
const notice = ref('')
const categories = ['全部', '書籍', '3C', '生活']

const visibleProducts = computed(() => {
  const term = query.value.trim().toLocaleLowerCase()
  const matched = products.value.filter((product) => {
    const matchesCategory = category.value === '全部' || product.category === category.value
    const matchesQuery = !term || [product.title, product.desc, product.category, ...product.tags].some((value) => value.toLocaleLowerCase().includes(term))
    return matchesCategory && matchesQuery
  })
  if (sortOrder.value === 'price-asc') return [...matched].sort((a, b) => a.price - b.price)
  if (sortOrder.value === 'price-desc') return [...matched].sort((a, b) => b.price - a.price)
  return matched
})

function toggleFavorite(productId) {
  favorites.value = favorites.value.includes(productId)
    ? favorites.value.filter((id) => id !== productId)
    : [...favorites.value, productId]
}
function addProduct() {
  const title = newProductTitle.value.trim()
  if (!title) {
    notice.value = '先輸入物品名稱，就能新增一筆模擬刊登。'
    return
  }
  products.value.unshift({
    id: `item-${Date.now()}`,
    title,
    desc: '這是一筆新加入的模擬刊登，可用來展示父層資料更新。',
    price: 250,
    tags: ['新上架', '模擬刊登'],
    category: '生活',
    available: true,
    art: 'new',
    artLabel: 'A NEW FIND',
  })
  newProductTitle.value = ''
  query.value = ''
  category.value = '全部'
  notice.value = `已新增「${title}」的模擬刊登。`
}
function removeProduct(product) {
  products.value = products.value.filter((item) => item.id !== product.id)
  favorites.value = favorites.value.filter((id) => id !== product.id)
  notice.value = '已移除「' + product.title + '」的刊登。'
}
function showProduct(product) {
  notice.value = product.available
    ? `「${product.title}」是課堂展示資料，尚未連接聯絡或交易服務。`
    : `「${product.title}」已售出，暫停刊登。`
}
</script>

<template>
  <main class="page-shell">
    <header class="topbar">
      <a class="brand" href="#top" aria-label="拾物市集首頁"><span class="brand-mark">拾</span><span>拾物<span class="brand-en"> / CAMPUS REUSE</span></span></a>
      <nav aria-label="主選單"><a class="nav-active" href="#market">逛逛市集</a><a href="#how-it-works">交換指南</a><a href="#how-it-works">關於拾物</a></nav>
      <button class="avatar" type="button" aria-label="使用者帳戶">同</button>
    </header>

    <section id="top" class="hero">
      <div class="hero-copy">
        <p class="eyebrow"><span class="eyebrow-dot"></span> GIVE GOOD THINGS ANOTHER STORY</p>
        <h1>好物不該<br /><span>被留在角落。</span></h1>
        <p class="hero-text">讓閒置的書、用品與小家電，在校園裡遇見下一位需要它的人。</p>
        <a class="hero-link" href="#market">看看大家在賣什麼 <span>↘</span></a>
      </div>
      <div class="hero-art" aria-label="二手好物與循環使用插圖" role="img">
        <div class="art-orbit orbit-one"></div><div class="art-orbit orbit-two"></div>
        <div class="art-sun"></div><div class="art-card card-back"><span>RE:USE</span><b>↻</b></div>
        <div class="art-card card-front"><span>GOOD FINDS</span><strong>Use it.<br />Love it.</strong><i>✳ &nbsp; PASS IT ON</i></div>
        <span class="art-spark spark-a">✳</span><span class="art-spark spark-b">✦</span><span class="art-label">一件好物，繼續一段故事。</span>
      </div>
      <div class="hero-foot"><span>LOCAL — CIRCULAR — THOUGHTFUL</span><span>校園好物交換小市集</span><span>SCROLL TO DISCOVER ↓</span></div>
    </section>

    <section id="market" class="catalog">
      <div class="section-heading"><div><p class="eyebrow">THE CAMPUS MARKET</p><h2>幫好物找下一個主人</h2><p class="section-lead">每件物品都有自己的故事，從校園裡再出發。</p></div><div class="catalog-count"><strong>{{ visibleProducts.length.toString().padStart(2, '0') }}</strong><span>件好物</span></div></div>

      <div class="toolbar">
        <label class="search-box"><span aria-hidden="true">⌕</span><input v-model="query" type="search" placeholder="搜尋物品、分類或標籤…" aria-label="搜尋物品" /><kbd>⌘ K</kbd></label>
        <label class="sort-control"><span>排序</span><select v-model="sortOrder" aria-label="物品排序"><option value="recommended">最新上架</option><option value="price-asc">價格：由低到高</option><option value="price-desc">價格：由高到低</option></select></label>
      </div>
      <div class="category-tabs" role="group" aria-label="物品分類篩選"><button v-for="item in categories" :key="item" type="button" :class="{ selected: category === item }" @click="category = item">{{ item }}<span v-if="item === '全部'"> {{ products.length }}</span></button></div>

      <div class="product-grid" v-if="visibleProducts.length">
        <ProductCard v-for="(product, index) in visibleProducts" :key="product.id" :product="product" :index="index + 1" :favorite="favorites.includes(product.id)" @toggle-favorite="toggleFavorite" @view-product="showProduct" @delete-product="removeProduct" />
      </div>
      <div v-else class="empty-state"><span>⌕</span><h3>沒有找到這件好物</h3><p>換個關鍵字，或看看其他分類。</p><ActionButton label="清除篩選" variant="secondary" @click="query = ''; category = '全部'" /></div>

      <form class="add-product" @submit.prevent="addProduct"><div><span class="add-icon">＋</span><strong>刊登一件模擬好物</strong><p>新增資料只會在這個課堂展示頁中顯示。</p></div><div class="add-controls"><input v-model="newProductTitle" type="text" placeholder="輸入物品名稱" aria-label="新物品名稱" /><ActionButton label="新增刊登" type="submit" /></div></form>
      <p v-if="notice" class="notice" role="status"><span>✓</span>{{ notice }}</p>
    </section>

    <section id="how-it-works" class="note-strip"><div><p class="eyebrow">A SMALLER FOOTPRINT</p><h2>讓好物繼續被好好使用。</h2></div><p>商品卡片和操作按鈕以 Vue 元件重用。搜尋、分類、排序、收藏、新增與刪除刊登由父層管理；所有商品都是模擬資料，頁面不會進行聯絡、付款或實際交易。</p><span class="note-star">↻</span></section>
    <footer><a class="brand" href="#top"><span class="brand-mark">拾</span><span>拾物<span class="brand-en"> / CAMPUS REUSE</span></span></a><span>互動式網頁設計｜Vue 元件化專題</span><span>給好物一個新故事</span></footer>
  </main>
</template>
