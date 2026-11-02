<script setup>
import { computed, ref } from 'vue'
import CourseCard from './components/CourseCard.vue'
import ActionButton from './components/ActionButton.vue'

const courses = ref([
  { id: 'ux101', title: '介面設計入門', desc: '從使用者需求出發，練習資訊架構、版面配置與可用性檢查。', price: 1800, tags: ['設計', '入門'], available: true, category: '設計' },
  { id: 'vue201', title: 'Vue 元件實戰', desc: '用 props、事件與列表渲染組裝可維護的互動式前端介面。', price: 2400, tags: ['前端', 'Vue'], available: true, category: '前端' },
  { id: 'data110', title: '資料視覺化基礎', desc: '把資料整理成容易閱讀的圖表，並學會選擇合適的視覺形式。', price: 2100, tags: ['資料', '入門'], available: false, category: '資料' },
])
const query = ref('')
const sortOrder = ref('recommended')
const favorites = ref([])
const notice = ref('')
const searchInput = ref('')

const visibleCourses = computed(() => {
  const term = query.value.trim().toLocaleLowerCase()
  const matched = courses.value.filter((course) =>
    !term || [course.title, course.desc, course.category, ...course.tags].some((value) => value.toLocaleLowerCase().includes(term)),
  )
  if (sortOrder.value === 'price-asc') return [...matched].sort((a, b) => a.price - b.price)
  if (sortOrder.value === 'price-desc') return [...matched].sort((a, b) => b.price - a.price)
  return matched
})

function toggleFavorite(courseId) {
  favorites.value = favorites.value.includes(courseId)
    ? favorites.value.filter((id) => id !== courseId)
    : [...favorites.value, courseId]
}
function addCourse() {
  const title = searchInput.value.trim()
  if (!title) {
    notice.value = '請先輸入課程名稱，再新增模擬課程。'
    return
  }
  const id = `course-${Date.now()}`
  courses.value.unshift({ id, title, desc: '這是一筆新加入的模擬課程，可用來展示父層資料更新。', price: 1600, tags: ['新課程', '模擬資料'], available: true, category: '其他' })
  searchInput.value = ''
  query.value = ''
  notice.value = `已新增「${title}」。`
}
function handleEnroll(course) {
  notice.value = course.available ? `已選擇「${course.title}」，這是課堂展示，不會產生實際報名或付款。` : `「${course.title}」目前額滿，已停用報名。`
}
</script>

<template>
  <main class="page-shell">
    <header class="topbar">
      <a class="brand" href="#top" aria-label="學習所首頁"><span class="brand-mark">學</span><span>學習所<span class="brand-en"> / LEARN LAB</span></span></a>
      <nav aria-label="主選單"><a class="nav-active" href="#courses">探索課程</a><a href="#about">學習指南</a><a href="#about">關於我們</a></nav>
      <button class="avatar" type="button" aria-label="使用者帳戶">林</button>
    </header>

    <section id="top" class="hero">
      <div class="hero-copy">
        <p class="eyebrow"><span class="eyebrow-dot"></span> LEARN SOMETHING NEW</p>
        <h1>把好奇心，<br /><span>變成新能力。</span></h1>
        <p class="hero-text">精選實用課程，從今天開始，替自己的下一步做好準備。</p>
        <a class="hero-link" href="#courses">探索所有課程 <span>↘</span></a>
      </div>
      <div class="hero-art" aria-label="學習與創意插圖" role="img">
        <div class="art-orbit orbit-one"></div><div class="art-orbit orbit-two"></div>
        <div class="art-sun"></div><div class="art-card card-back"><span>IDEAS</span><b>✳</b></div>
        <div class="art-card card-front"><span>LEARN</span><strong>Make<br />it real.</strong><i>✦ &nbsp; 2025</i></div>
        <span class="art-spark spark-a">✳</span><span class="art-spark spark-b">✦</span><span class="art-label">小步前進，也算進步。</span>
      </div>
      <div class="hero-foot"><span>01 — 03</span><span>為每一種好奇，找到下一堂課</span><span>SCROLL TO EXPLORE ↓</span></div>
    </section>

    <section id="courses" class="catalog">
      <div class="section-heading"><div><p class="eyebrow">COURSE CATALOGUE</p><h2>找到你的下一堂課</h2><p class="section-lead">三門精選課程，陪你從想法走到實作。</p></div><div class="catalog-count"><strong>{{ visibleCourses.length.toString().padStart(2, '0') }}</strong><span>門課程</span></div></div>

      <div class="toolbar">
        <label class="search-box"><span aria-hidden="true">⌕</span><input v-model="query" type="search" placeholder="搜尋課程、主題或技能…" aria-label="搜尋課程" /><kbd>⌘ K</kbd></label>
        <label class="sort-control"><span>排序</span><select v-model="sortOrder" aria-label="課程排序"><option value="recommended">推薦順序</option><option value="price-asc">價格：由低到高</option><option value="price-desc">價格：由高到低</option></select></label>
      </div>

      <div class="course-grid" v-if="visibleCourses.length">
        <CourseCard v-for="(course, index) in visibleCourses" :key="course.id" :course="course" :index="index + 1" :favorite="favorites.includes(course.id)" @toggle-favorite="toggleFavorite" @enroll="handleEnroll" />
      </div>
      <div v-else class="empty-state"><span>⌕</span><h3>找不到符合的課程</h3><p>試試其他關鍵字，或清除搜尋條件。</p><ActionButton label="清除搜尋" variant="secondary" @click="query = ''" /></div>

      <form class="add-course" @submit.prevent="addCourse"><div><span class="add-icon">＋</span><strong>加入一筆模擬課程</strong><p>展示父層如何更新資料並重新渲染列表。</p></div><div class="add-controls"><input v-model="searchInput" type="text" placeholder="輸入課程名稱" aria-label="新課程名稱" /><ActionButton label="新增課程" type="submit" /></div></form>
      <p v-if="notice" class="notice" role="status"><span>✓</span>{{ notice }}</p>
    </section>

    <section id="about" class="note-strip"><div><p class="eyebrow">BUILT WITH COMPONENTS</p><h2>好介面，來自好的組裝方式。</h2></div><p>課程卡片與操作按鈕都是可重用元件。父層負責資料、搜尋、排序與收藏狀態，子元件則透過 props 接收資料、透過事件回報操作。</p><span class="note-star">✳</span></section>
    <footer><a class="brand" href="#top"><span class="brand-mark">學</span><span>學習所<span class="brand-en"> / LEARN LAB</span></span></a><span>互動式網頁設計｜第 8 週元件化作業</span><span>© 2025 LEARN LAB</span></footer>
  </main>
</template>
