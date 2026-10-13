<template>
  <div class="box" :class="accent && `accent-${accent}`" :style="style">
    <img v-if="img" :src="url(img)" :alt="alt" :style="{ objectFit: fit }" />
    <video v-else-if="video" :src="url(video)" :poster="poster && url(poster)" :aria-label="alt"
           autoplay muted loop playsinline :style="{ objectFit: fit }" />
    <div v-else-if="svg" class="svg-inline" role="img" :aria-label="alt" v-html="svgText" />
    <slot />
  </div>
</template>

<script setup>
// One PowerPoint shape: position and size in inches (x, y, w, h), plus the
// fill, text size (pt), colour and anchoring the deck uses for it.
import { computed, ref, watchEffect } from 'vue'

const props = defineProps({
  x: [Number, String], y: [Number, String], w: [Number, String], h: [Number, String],
  size: [Number, String],            // font size in points
  color: String,                     // red | muted | faint | white | any CSS colour
  head: Boolean,                     // Kanit instead of Open Sans
  anchor: { type: String, default: 'top' },  // top | middle | bottom
  align: String,
  fill: String,                      // red (SDSU gradient) | card | card2 | white | any CSS colour
  accent: String,                    // top | left: the thin red bar on cards
  accentColor: String,
  pad: String,
  radius: String,
  line: [Number, String],            // line-height
  after: [Number, String],           // paragraph spacing in points
  bullet: String,                    // bullet colour
  strong: String,                    // colour of **bold** runs
  img: String, video: String, poster: String, alt: String,
  svg: String,                       // inline SVG figure, so its labels use the slide fonts
  fit: { type: String, default: 'cover' },
  z: [Number, String],
})

const base = import.meta.env.BASE_URL
const url = p => base + p.replace(/^\//, '')
const svgText = ref('')
watchEffect(async () => {
  if (props.svg) svgText.value = await (await fetch(url(props.svg))).text()
})
const tok = c => c && ({ red: 'var(--red-text)', muted: 'var(--muted)', faint: 'var(--faint)', white: '#fff' }[c] || c)
const fills = { red: 'var(--red-fill)', card: 'var(--card)', card2: 'var(--card2)', white: '#fff' }
const inch = v => (v === undefined ? undefined : `${parseFloat(v) * 100}px`)
const pt = v => (v === undefined ? undefined : `${(parseFloat(v) * 100) / 72}px`)

const style = computed(() => ({
  left: inch(props.x), top: inch(props.y), width: inch(props.w), height: inch(props.h),
  fontSize: pt(props.size),
  color: tok(props.color),
  fontFamily: props.head ? 'Kanit, sans-serif' : undefined,
  justifyContent: { top: 'flex-start', middle: 'center', bottom: 'flex-end' }[props.anchor],
  textAlign: props.align,
  background: props.fill && (fills[props.fill] || props.fill),
  borderRadius: props.radius,
  padding: props.pad,
  lineHeight: props.line,
  zIndex: props.z,
  '--accent': tok(props.accentColor),
  '--after': pt(props.after),
  '--bullet': tok(props.bullet),
  '--strong': tok(props.strong),
}))
</script>
