<template>
  <div v-if="ring" class="glow-ring" :style="disc(+r + 0.38)" aria-hidden="true" />
  <div class="glow" :style="disc(+r)" aria-hidden="true" />
</template>

<script setup>
// The deck's accent: a translucent SDSU-red disc, usually with a faint outer ring.
// Centre and radius in inches.
const props = defineProps({
  cx: [Number, String],
  cy: [Number, String],
  r: [Number, String],
  ring: { type: Boolean, default: true },
})
const disc = r => ({
  left: `${(props.cx - r) * 100}px`,
  top: `${(props.cy - r) * 100}px`,
  width: `${2 * r * 100}px`,
  height: `${2 * r * 100}px`,
})
</script>

<style scoped>
.glow, .glow-ring { position: absolute; border-radius: 50%; box-sizing: border-box; }
.glow { background: rgba(166, 25, 46, 0.72); }
.glow-ring { border: 1.75px solid rgba(59, 60, 67, 0.7); }
</style>
