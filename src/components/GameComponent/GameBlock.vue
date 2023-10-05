<script setup lang="ts">
import { onMounted, ref, type Ref } from 'vue'

const props = defineProps({
  delay: Number
})
const { delay } = props

const emit = defineEmits<{
  (e: 'end', time: number): void
}>()

interface TimerState {
  showBlock: boolean
  timer: number | undefined
  reactionTime: number
}

const timerState: Ref<TimerState> = ref({
  showBlock: false,
  timer: undefined,
  reactionTime: 0
})

const startTimer = () => {
  timerState.value.timer = setInterval(() => {
    timerState.value.reactionTime += 10
  }, 10)
}

const stopTimer = () => {
  clearInterval(timerState.value.timer)
  emit('end', timerState.value.reactionTime)
}

onMounted(() => {
  setTimeout(() => {
    timerState.value.showBlock = true
    startTimer()
  }, delay)
})
</script>

<template>
  <div class="block" v-if="timerState.showBlock" @click="stopTimer">Click me</div>
</template>

<style scoped>
.block {
  width: 300px;
  height: 200px;
  margin: 20px auto;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  background-color: rgb(46, 191, 143);
}
</style>
