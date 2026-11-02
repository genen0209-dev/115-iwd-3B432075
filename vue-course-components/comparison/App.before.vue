<script setup>
import { computed, ref } from 'vue'
const courses = ref([
  { id: 'ux101', title: '介面設計入門', desc: '練習資訊架構、版面配置與可用性檢查。', price: 1800, tags: ['設計', '入門'], available: true },
  { id: 'vue201', title: 'Vue 元件實戰', desc: '運用 props、事件與列表渲染組裝前端介面。', price: 2400, tags: ['前端', 'Vue'], available: true },
  { id: 'data110', title: '資料視覺化基礎', desc: '把資料整理成容易閱讀的圖表。', price: 2100, tags: ['資料', '入門'], available: false },
])
const query = ref('')
const favorites = ref([])
const shownCourses = computed(() => courses.value.filter((course) => course.title.includes(query.value.trim())))
function toggleFavorite(id) { favorites.value = favorites.value.includes(id) ? favorites.value.filter((item) => item !== id) : [...favorites.value, id] }
</script>
<template>
  <main class="course-page"><h1>找到你的下一堂課</h1><input v-model="query" placeholder="搜尋課程" /><div class="course-grid"><article v-for="course in shownCourses" :key="course.id" class="course-card"><span>{{ course.tags.join(' · ') }}</span><h2>{{ course.title }}</h2><p>{{ course.desc }}</p><strong>NT$ {{ course.price.toLocaleString() }}</strong><button type="button" @click="toggleFavorite(course.id)">{{ favorites.includes(course.id) ? '♥ 已收藏' : '♡ 收藏' }}</button><button type="button" :disabled="!course.available">{{ course.available ? '了解課程' : '已額滿' }}</button></article></div></main>
</template>
