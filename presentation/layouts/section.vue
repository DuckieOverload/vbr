<template>
  <Default>
    <Glow cx="3.85" cy="2.55" r="1.7" />
    <Box x="0.75" y="0.8" w="3.6" h="4.0" :img="img" :alt="alt" />
    <Box x="3.15" y="3.7" w="2.3" h="1.1" fill="red" pad="0 17px" anchor="middle" head size="11.5" line="1.15">{{ teaser }}</Box>
    <Box v-if="credit" x="0.75" y="4.88" w="3.5" class="credit">Photo: {{ credit }} / Unsplash</Box>
    <Box x="6.1" y="1.0" w="2.5" head size="60" color="red" line="1">{{ num }}</Box>
    <Box x="6.1" y="3.55" w="3.2" size="10.5" color="muted" after="3">
      <ul class="sq"><li v-for="t in topics" :key="t">{{ t }}</li></ul>
    </Box>
    <div class="section-title"><slot /></div>
  </Default>
</template>

<script setup>
// Section divider: photo with a red glow and teaser card, big red number, title and topics.
import Default from './default.vue'
import Glow from '../components/Glow.vue'
import Box from '../components/Box.vue'

defineProps({
  num: String,
  img: String,
  alt: { type: String, default: '' },
  credit: String,
  teaser: String,
  topics: { type: Array, default: () => [] },
})
</script>

<style scoped>
.section-title { position: absolute; left: 610px; top: 205px; width: 340px; z-index: 5; }
.section-title :deep(h1) { font-size: 47.2px; line-height: 1.1; }   /* 34 pt */
</style>
