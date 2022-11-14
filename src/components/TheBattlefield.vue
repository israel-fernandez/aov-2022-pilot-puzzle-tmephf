<script setup>
import { ref, computed } from 'vue'
import TheToken from './TheToken.vue'

const getIndex = v => (v - (v % 100)) / 100

const turn = ref(0)
const winner = ref(null)

const reset = () => {
  turn.value = 0
  winner.value = false
  tokens.value = []
}

const onClick = event => {
  const x = getIndex(event.offsetX)
  const y = getIndex(event.offsetY)
  if (!checkAvailable(x, y)) return
  tokens.value.push({ x, y, type: turnType.value })
  winner.value = checkWinner(turnType.value) ?? turnType.value
  if (!winner.value) turn.value++
}

const turnType = computed(() => (turn.value % 2 ? 'cross' : 'circle'))
const tokens = ref([])

const checkAvailable = (x, y) => !winner.value && !tokens.value.filter(t => t.x === x && t.y === y).length

const checkWinner = type =>
  !!tokens.value
    .filter(t => t.type === type)
    .map(t => t.x + t.y * 3)
    .sort((a, b) => a - b)
    .join('')
    .match(/012|345|678|0.*3.*6|1.*4.*7|2.*5.*8|0.*4.*8|2.*4.*6/)
</script>
<template>
  <div>
    <svg
      width="300"
      height="300"
      viewBox="0 0 300 300"
      style="max-width: 100%; height: auto; height: intrinsic"
      @click="onClick"
    >
      <line x1="0" y1="100" x2="300" y2="100" class="line" />
      <line x1="0" y1="200" x2="300" y2="200" class="line" />
      <line x1="100" y1="0" x2="100" y2="300" class="line" />
      <line x1="200" y1="0" x2="200" y2="300" class="line" />

      <g v-for="token in tokens">
        <the-token :type="token.type" :x="token.x" :y="token.y" />
      </g>
    </svg>
    <div style="text-align: center" v-if="!winner && turn < 9">
      It's
      <div style="width: 30px; height: 30px; display: inline-block; vertical-align: middle">
        <svg width="100" height="100" viewBox="0 0 100 100" style="max-width: 100%; height: 100%">
          <the-token :type="turnType" :x="0" :y="0" />
        </svg>
      </div>
      's turn.
    </div>
    <div style="text-align: center" v-else>
      <div>
        <div v-if="winner" style="width: 30px; height: 30px; display: inline-block; vertical-align: middle">
          <svg width="100" height="100" viewBox="0 0 100 100" style="max-width: 100%; height: 100%">
            <the-token :type="turnType" :x="0" :y="0" />
          </svg>
        </div>
        <span v-if="winner">has won!</span>
        <span v-else>It's a draw!</span>
      </div>
      <button @click="reset">Play again</button>
    </div>
  </div>
</template>
<style scoped>
.line {
  stroke: black;
  stroke-width: 2;
}
button {
  color: rgb(15 23 42);
  background-color: rgb(66 184 131);
  border-radius: 5px;
  padding: 5px;
}
</style>
