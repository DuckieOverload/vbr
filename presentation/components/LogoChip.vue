<template>
  <div class="chip" :style="{ left: px(x), top: px(y), width: px(width), height: px(+h + 0.2) }">
    <img v-for="l in logos" :key="l.src" :src="base + l.src" :alt="l.alt" :style="{ height: px(h), width: px(h * l.ratio) }" />
  </div>
</template>

<script setup>
// White pill with the SDSU, Yang Lab and Victory Bell logos (cover and closing slides).
import { computed } from 'vue'

const props = defineProps({ x: [Number, String], y: [Number, String], h: { type: [Number, String], default: 0.3 } })
const base = import.meta.env.BASE_URL
const logos = [
  { src: 'deck/logo-sdsu.png', alt: 'San Diego State University', ratio: 3.0 },
  { src: 'deck/logo-yanglab.jpg', alt: 'SDSU Yang Lab', ratio: 1103 / 339 },
  { src: 'deck/logo-vbr.png', alt: 'Victory Bell Education & Research', ratio: 1275 / 439 },
]
const px = v => `${v * 100}px`
const width = computed(() => logos.reduce((s, l) => s + props.h * l.ratio, 0) + 0.18 * 2 + 0.4)
</script>

<style scoped>
.chip {
  position: absolute; box-sizing: border-box; background: #fff; border-radius: 999px;
  display: flex; align-items: center; gap: 18px; padding: 0 20px;
}
.chip img { display: block; object-fit: contain; }
</style>
