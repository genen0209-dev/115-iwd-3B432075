<script setup>
import ActionButton from './ActionButton.vue'

defineProps({
  course: { type: Object, required: true },
  index: { type: Number, required: true },
  favorite: { type: Boolean, default: false },
})
const emit = defineEmits(['toggle-favorite', 'enroll'])
</script>

<template>
  <article class="course-card" :class="{ 'is-full': !course.available }">
    <div class="course-card-top"><span class="course-number">0{{ index }}</span><button class="favorite-button" type="button" :aria-label="favorite ? `取消收藏 ${course.title}` : `收藏 ${course.title}`" :aria-pressed="favorite" @click="emit('toggle-favorite', course.id)">{{ favorite ? '♥' : '♡' }}</button></div>
    <div class="course-illustration" :class="`art-${index % 3}`"><span class="illustration-shape shape-one"></span><span class="illustration-shape shape-two"></span><span class="illustration-label">{{ course.category }}</span><span class="illustration-symbol">{{ index === 1 ? '✳' : index === 2 ? '⌘' : '◒' }}</span></div>
    <div class="course-card-body"><div class="tag-row"><span v-for="tag in course.tags" :key="tag" class="tag">{{ tag }}</span><span class="availability" :class="course.available ? 'available' : 'unavailable'"><i></i>{{ course.available ? '開放報名' : '目前額滿' }}</span></div><h3>{{ course.title }}</h3><p class="course-desc">{{ course.desc }}</p><div class="course-card-bottom"><div class="price"><small>課程費用</small><strong>NT$ {{ course.price.toLocaleString('zh-TW') }}</strong></div><ActionButton :label="course.available ? '了解課程' : '已額滿'" :disabled="!course.available" :variant="course.available ? 'primary' : 'muted'" @click="emit('enroll', course)" /></div></div>
  </article>
</template>
