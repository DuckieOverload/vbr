<template>
  <svg class="circuits" viewBox="0 0 3000 1688" preserveAspectRatio="none" aria-hidden="true">
    <g fill="none" stroke-width="4" stroke-linejoin="round">
      <g v-for="(t, i) in traces" :key="i" :stroke="t.color">
        <polyline :points="t.points.map(p => p.join(',')).join(' ')" />
        <circle :cx="t.points[3][0]" :cy="t.points[3][1]" r="9" />
      </g>
    </g>
  </svg>
</template>

<script setup>
// Faint circuit traces at the slide edges (the deck's "Circuit traces" layout picture).
const W = 3000
const RED = 'rgba(224, 67, 90, 0.22)'
const GREY = 'rgba(150, 152, 160, 0.16)'
const left = [[260, 160, 120, RED], [330, 110, 150, GREY], [1180, 190, -140, RED], [1250, 130, -90, GREY], [1320, 230, -40, RED]]
const right = [[380, 170, 130, RED], [450, 120, 170, GREY], [1360, 200, -120, RED], [1430, 140, -70, GREY]]
const traces = [
  ...left.map(([y, dx, dy, color]) => ({
    color, points: [[0, y], [dx, y], [dx + Math.abs(dy), y + dy], [dx + Math.abs(dy) + 60, y + dy]],
  })),
  ...right.map(([y, dx, dy, color]) => ({
    color, points: [[W, y], [W - dx, y], [W - dx - Math.abs(dy), y + dy], [W - dx - Math.abs(dy) - 60, y + dy]],
  })),
]
</script>

<style scoped>
.circuits { position: absolute; inset: 0; width: 100%; height: 100%; pointer-events: none; }
</style>
