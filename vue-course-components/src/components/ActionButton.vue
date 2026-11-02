<script setup>
defineProps({
  label: { type: String, required: true },
  variant: { type: String, default: 'primary' },
  disabled: { type: Boolean, default: false },
  type: { type: String, default: 'button' },
})
</script>

<template>
  <button class="action-button" :class="`button-${variant}`" :type="type" :disabled="disabled"><span>{{ label }}</span><span v-if="!disabled" class="button-arrow" aria-hidden="true">↗</span></button>
</template>
