<template>
  <div class="vbr vbr-content">
    <Circuits />
    <div class="num-tab">{{ $page }}</div>
    <div v-if="eyebrow" class="eyebrow top-eyebrow">{{ eyebrow }}</div>
    <slot />
    <div class="footer">VBR MECHANICAL TEAM &nbsp; · &nbsp; SDSU YANG LAB</div>
  </div>
</template>

<script setup>
// "VBR Content" layout: gradient, circuit traces, red slide-number tab, eyebrow over the
// title (the slide's # heading) and the footer.
import { useSlideContext } from '@slidev/client'
import Circuits from '../components/Circuits.vue'

defineProps({ eyebrow: String })
const { $page } = useSlideContext()
</script>

<style scoped>
.num-tab {
  position: absolute; left: 45px; top: 0; width: 52px; height: 58px;
  background: var(--red-fill);
  display: flex; align-items: flex-end; justify-content: center; padding-bottom: 5px; box-sizing: border-box;
  font-family: Kanit, sans-serif; font-size: 19.4px; line-height: 1; color: #fff;
}
.top-eyebrow { position: absolute; left: 75px; top: 74px; width: 500px; }
.footer {
  position: absolute; left: 75px; top: 524px;
  font-size: 9.7px; letter-spacing: 2.8px; color: var(--red-text); white-space: nowrap;
}
</style>
