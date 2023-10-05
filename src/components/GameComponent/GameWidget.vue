<script setup lang="ts">
import { ref, type Ref } from 'vue'
import GameBlock from './GameBlock.vue'
import GameResults from './GameResults.vue'

interface GameState {
  isPlaying: boolean
  delay: number
  score: number | null
}

const gameState: Ref<GameState> = ref({
  isPlaying: false,
  delay: 0,
  score: null
})

const minDelay = 1000
const maxDelay = 5000

const startGame = () => {
  gameState.value.score = null
  gameState.value.isPlaying = true
  gameState.value.delay = minDelay + Math.random() * maxDelay
}

const endGame = (reactionTime: number) => {
  gameState.value.score = reactionTime
  gameState.value.isPlaying = false
}
</script>

<template>
  <div class="wrapper">
    <h2>Reaction timer game</h2>
    <button @click="startGame" class="play-button" :disabled="gameState.isPlaying">Play</button>
    <GameBlock v-if="gameState.isPlaying" :delay="gameState.delay" @end="endGame" />
    <GameResults :score="gameState.score" />
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 1000px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  color: #444;
}

.play-button {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px 15px;
  color: rgb(46, 191, 143);
  background-color: rgb(244, 244, 244);
  border: 3px solid rgb(46, 191, 143);
  border-radius: 10px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.1s ease-in;
}

.play-button:not(:disabled):hover {
  background-color: #fff;
}

.play-button:disabled {
  color: lightgray;
  border: 3px solid lightgray;
  cursor: not-allowed;
}
</style>
