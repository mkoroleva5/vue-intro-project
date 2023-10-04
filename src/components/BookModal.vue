<script setup lang="ts">
import type { Book } from './BooksWidget.vue'

defineProps<{
  book: Book | null
  onClose: () => void
}>()
</script>

<template>
  <div class="backdrop" @click="onClose()">
    <div class="modal" @click="(e) => e.stopPropagation()">
      <h3>{{ book?.title }}</h3>
      <p>{{ book?.author }}</p>
      <img :src="book?.img" :alt="book?.title" class="img" draggable="false" />
    </div>
  </div>
</template>

<style scoped>
.backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal {
  width: 50%;
  height: 80%;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #fff;
  border-radius: 10px;
  overflow: hidden;
}

.img {
  margin: 10px;
  object-fit: cover;
  height: 85%;
}
</style>
