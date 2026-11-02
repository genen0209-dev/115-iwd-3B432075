<script setup>
import ActionButton from './ActionButton.vue'

defineProps({
  product: { type: Object, required: true },
  index: { type: Number, required: true },
  favorite: { type: Boolean, default: false },
})
const emit = defineEmits(['toggle-favorite', 'view-product', 'delete-product'])
</script>

<template>
  <article class="product-card" :class="{ 'is-unavailable': !product.available }">
    <div class="product-card-top"><span class="product-number">0{{ index }}</span><div class="card-tools"><button class="favorite-button" type="button" :aria-label="favorite ? `取消收藏 ${product.title}` : `收藏 ${product.title}`" :aria-pressed="favorite" @click="emit('toggle-favorite', product.id)">{{ favorite ? '♥' : '♡' }}</button><button class="delete-button" type="button" :aria-label="`刪除刊登：${product.title}`" title="刪除刊登" @click="emit('delete-product', product)"><svg viewBox="0 0 20 20" aria-hidden="true"><path d="M4 6h12M8 6V4h4v2m3 0-.7 10H5.7L5 6m3 3v4m4-4v4" /></svg></button></div></div>
    <div class="product-art" :class="`art-${product.art}`" role="img" :aria-label="product.title"><span class="product-art-label">{{ product.artLabel }}</span><span class="object-illustration" aria-hidden="true"><i></i><b></b></span><span class="art-caption">GOOD THINGS, AGAIN</span></div>
    <div class="product-card-body"><div class="tag-row"><span v-for="tag in product.tags" :key="tag" class="tag">{{ tag }}</span><span class="availability" :class="product.available ? 'available' : 'unavailable'"><i></i>{{ product.available ? '刊登中' : '已售出' }}</span></div><p class="product-category">{{ product.category }}</p><h3>{{ product.title }}</h3><p class="product-desc">{{ product.desc }}</p><div class="product-card-bottom"><div class="price"><small>參考售價</small><strong>NT$ {{ product.price.toLocaleString('zh-TW') }}</strong></div><div class="product-actions"><ActionButton :label="product.available ? '查看好物' : '已售出'" :disabled="!product.available" :variant="product.available ? 'primary' : 'muted'" @click="emit('view-product', product)" /></div></div></div>
  </article>
</template>

