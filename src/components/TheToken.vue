<script setup>
defineProps({
  type: String,
  x: Number,
  y: Number,
})
</script>
<template>
  <circle v-if="type === 'circle'" :cx="x * 100 + 50" :cy="y * 100 + 50" r="30" class="token" />
  <g v-else>
    <line :x1="x * 100 + 20" :y1="y * 100 + 20" :x2="x * 100 + 80" :y2="y * 100 + 80" class="token" />
    <line :x1="x * 100 + 20" :y1="y * 100 + 80" :x2="x * 100 + 80" :y2="y * 100 + 20" class="token" />
  </g>
</template>
<style scoped>
.token {
  fill: transparent;
  stroke: black;
  stroke-width: 2px;
}
</style>
