<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  score: number | null
}>()
const score = computed(() => props.score)

const ranks = computed(() => {
  if (score.value) {
    if (score.value < 300) {
      return 'Ninja fingers'
    } else if (score.value < 500) {
      return 'Rapid reflexes'
    } else {
      return 'Snail pace...'
    }
  }
  return null
})
</script>

<template>
  <div v-if="score" class="wrapper">
    <h3 class="rank">{{ ranks }}</h3>
    <p>Reaction time: {{ score }}ms</p>
  </div>
</template>

<style scoped>
.rank {
  font-size: 24px;
  font-weight: bold;
  color: rgb(46, 191, 143);
}
</style>
