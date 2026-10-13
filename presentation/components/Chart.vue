<template>
  <svg class="chart" :style="pos" :viewBox="`0 0 ${W} ${H}`" role="img" :aria-label="title || label">
    <text v-if="title" :x="W / 2" y="15" class="c-title" text-anchor="middle">{{ title }}</text>

    <g v-if="legend" class="c-legend">
      <g v-for="(s, i) in lines" :key="s.name" :transform="`translate(${legendX(i)}, 12)`">
        <line x1="0" x2="18" y1="-4" y2="-4" :stroke="s.color" :stroke-width="s.width" />
        <text x="22" y="0">{{ s.name }}</text>
      </g>
    </g>

    <!-- value axis: gridlines and labels -->
    <g>
      <line v-for="t in yTicks" :key="`g${t}`" :x1="L" :x2="W - R" :y1="sy(t)" :y2="sy(t)" stroke="#3B3C43" stroke-width="0.7" />
      <text v-for="t in yTicks" :key="`t${t}`" :x="L - 7" :y="sy(t) + 4" class="c-ytick" text-anchor="end">{{ num(t) }}</text>
    </g>
    <line :x1="L" :x2="W - R" :y1="sy(ymin)" :y2="sy(ymin)" stroke="#3B3C43" stroke-width="1" />
    <text v-if="ytitle" class="c-axis" text-anchor="middle" :transform="`translate(11, ${(T + H - B) / 2}) rotate(-90)`">{{ ytitle }}</text>
    <text v-if="xtitle" class="c-axis" text-anchor="middle" :x="(L + W - R) / 2" :y="H - 3">{{ xtitle }}</text>

    <!-- bar -->
    <template v-if="type === 'bar'">
      <g v-for="(v, i) in vals" :key="i">
        <rect :x="cx(i) - barW / 2" :y="sy(v)" :width="barW" :height="sy(ymin) - sy(v)" :fill="i === +best ? '#A6192E' : '#4A4C54'" />
        <text :x="cx(i)" :y="sy(v) - 6" class="c-label" text-anchor="middle">{{ v.toFixed(2) }}</text>
      </g>
    </template>

    <!-- line with markers -->
    <template v-if="type === 'line'">
      <polyline :points="vals.map((v, i) => `${cx(i)},${sy(v)}`).join(' ')" fill="none" stroke="#A9ABB3" stroke-width="3.1" />
      <g v-for="(v, i) in vals" :key="i">
        <circle v-if="i === +best" :cx="cx(i)" :cy="sy(v)" r="7.6" fill="#A6192E" stroke="#fff" stroke-width="1" />
        <circle v-else :cx="cx(i)" :cy="sy(v)" r="4.9" fill="#A9ABB3" />
        <text :x="cx(i)" :y="sy(v) - (i === +best ? 13 : 10)" class="c-label" text-anchor="middle">{{ v.toFixed(2) }}</text>
      </g>
    </template>

    <!-- category labels -->
    <template v-if="type !== 'xy'">
      <text v-for="(c, i) in cats" :key="c" :x="cx(i)" :y="sy(ymin) + 17" class="c-cat" text-anchor="middle">{{ c }}</text>
    </template>

    <!-- xy scatter lines -->
    <template v-else>
      <text v-for="t in xTicks" :key="`x${t}`" :x="sx(t)" :y="sy(ymin) + 16" class="c-ytick" text-anchor="middle">{{ num(t) }}</text>
      <polyline v-for="s in lines" :key="s.name" :points="s.pts.map(([x, y]) => `${sx(x)},${sy(y)}`).join(' ')"
                fill="none" :stroke="s.color" :stroke-width="s.width" stroke-linejoin="round" />
    </template>
  </svg>
</template>

<script setup>
// Charts from the team's workbook (data/charts.json), drawn the way the
// PowerPoint deck's native charts are: grey bars or lines, the winner in SDSU red.
import { computed } from 'vue'
import data from '../data/charts.json'

const props = defineProps({
  type: { type: String, default: 'bar' },      // bar | line | xy
  x: [Number, String], y: [Number, String], w: [Number, String], h: [Number, String],
  title: String,
  series: String,     // bar / line: path into charts.json, e.g. "r1.brk"
  labels: String,     // bar / line: category path, e.g. "r1.cats"
  best: [Number, String],
  preset: String,     // xy: finalForce | finalRes | shrimp | twoForce | twoRes
  ymin: { type: [Number, String], default: 0 },
  ymax: [Number, String], ymajor: [Number, String],
  xmax: [Number, String], xmajor: [Number, String],
  xtitle: String, ytitle: String,
  legend: Boolean,
})

const get = path => path.split('.').reduce((o, k) => o[k], data)
const W = computed(() => props.w * 100)
const H = computed(() => props.h * 100)
const pos = computed(() => ({ left: `${props.x * 100}px`, top: `${props.y * 100}px`, width: `${W.value}px`, height: `${H.value}px` }))

const vals = computed(() => (props.series ? get(props.series) : []))
const cats = computed(() => (props.labels ? get(props.labels) : []))

const RED = '#E0435A'
const runs = d => [
  ...d.runs.map((pts, k) => ({ name: `Run ${k + 1}`, pts, color: '#6E717A', width: 1 })),
  { name: 'Average', pts: d.avg, color: RED, width: 3.8 },
]
const shrimpColors = { '1 bump': '#9FA1A9', '2 bumps': RED, '3 bumps': '#7E8089', '4 bumps': '#5E6068', '5 bumps': '#A97880' }
const lines = computed(() => {
  switch (props.preset) {
    case 'finalForce': return runs(data.finalForce)
    case 'finalRes': return runs(data.finalRes)
    case 'shrimp': return Object.entries(data.shrimp).map(([name, pts]) => ({ name, pts, color: shrimpColors[name], width: name === '2 bumps' ? 3.8 : 2.1 }))
    case 'twoForce': return [{ name: 'Force', pts: data.twoBump.force, color: RED, width: 3.5 }]
    case 'twoRes': return [{ name: 'Resistance', pts: data.twoBump.res, color: '#C9CACF', width: 3.5 }]
    default: return []
  }
})
const label = computed(() => props.preset || props.series)

// plot area
const T = computed(() => (props.title ? 30 : props.legend ? 26 : 8))
const L = computed(() => (props.ytitle ? 50 : 34))
const R = 10
const B = computed(() => (props.xtitle ? 36 : 24))
const ymin = computed(() => +props.ymin)
const ymax = computed(() => +props.ymax)
const sy = v => T.value + (1 - (v - ymin.value) / (ymax.value - ymin.value)) * (H.value - T.value - B.value)
const sx = v => L.value + (v / props.xmax) * (W.value - L.value - R)
const n = computed(() => Math.max(vals.value.length, cats.value.length))
const band = computed(() => (W.value - L.value - R) / n.value)
const cx = i => L.value + band.value * (i + 0.5)
const barW = computed(() => band.value / 1.55)

const ticks = (lo, hi, step) => {
  const out = []
  for (let t = lo; t <= hi + 1e-9; t += step) out.push(+t.toFixed(6))
  return out
}
const yTicks = computed(() => ticks(ymin.value, ymax.value, +props.ymajor || 100))
const xTicks = computed(() => (props.type === 'xy' ? ticks(0, +props.xmax, +props.xmajor || 1) : []))
const num = t => String(+t.toFixed(4))
const legendX = i => {
  const widths = lines.value.map(s => 30 + s.name.length * 6.4)
  const total = widths.reduce((a, b) => a + b, 0)
  return (W.value - total) / 2 + widths.slice(0, i).reduce((a, b) => a + b, 0)
}
</script>

<style scoped>
.chart { position: absolute; overflow: visible; }
.c-title { font-family: Kanit, sans-serif; font-size: 13.9px; fill: #fff; }
.c-ytick { font-family: 'Open Sans', sans-serif; font-size: 11.1px; fill: #7E8089; }
.c-cat { font-family: 'Open Sans', sans-serif; font-size: 11.8px; fill: #B3B5BC; }
.c-label { font-family: 'Open Sans', sans-serif; font-size: 11.8px; fill: #fff; }
.c-axis { font-family: 'Open Sans', sans-serif; font-size: 11.1px; fill: #7E8089; }
.c-legend text { font-family: 'Open Sans', sans-serif; font-size: 11.8px; fill: #B3B5BC; }
</style>
