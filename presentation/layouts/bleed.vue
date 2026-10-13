<template>
  <div class="vbr vbr-bleed">
    <img v-if="image" class="bleed-photo" :src="base + image" :alt="alt" />
    <div v-if="image" class="bleed-overlay" :style="{ background: overlay }" />
    <slot />
    <div v-if="credit" class="bleed-credit">Photo: {{ credit }} / Unsplash</div>
  </div>
</template>

<script setup>
// "VBR Full Bleed" layout: the cover, the mantis shrimp photo slide and the closing slide.
// No number tab, footer or circuit traces; content is placed with <Box>.
defineProps({
  image: String,
  alt: { type: String, default: '' },
  credit: String,
  overlay: {
    type: String,
    default: 'linear-gradient(90deg, rgba(12,12,14,0.92) 0%, rgba(35,35,40,0.72) 45%, rgba(166,25,46,0.18) 100%)',
  },
})
const base = import.meta.env.BASE_URL
</script>

<style scoped>
.bleed-photo, .bleed-overlay { position: absolute; inset: 0; width: 100%; height: 100%; }
.bleed-photo { object-fit: cover; }
.bleed-credit { position: absolute; right: 20px; top: 533px; font-size: 9px; color: #E6E6EA; }
</style>
