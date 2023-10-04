<script setup lang="ts">
import { computed, ref, type Ref } from 'vue'
import zoomIcon from '@/assets/zoom-in.svg'
import Modal from './Modal.vue'

export type Book = {
  title: string
  author: string
  img: string
  isFav: boolean
}

const url = 'https://www.google.com/'

const books: Ref<Book[]> = ref([
  {
    title: 'The Snows of Kilimanjaro',
    author: 'Ernest Hemingway',
    img: '../src/assets/snows.jpg',
    isFav: true
  },
  {
    title: 'Laughter in the Dark',
    author: 'Vladimir Nabokov',
    img: '../src/assets/laughter.jpg',
    isFav: false
  },
  {
    title: 'The Star Rover',
    author: 'Jack London',
    img: '../src/assets/star-rover.jpg',
    isFav: false
  }
])

const filteredBooks = computed(() =>
  books.value
    .filter((book) => book.isFav)
    .map((book) => book.title)
    .join(', ')
)

const expandedBook = ref<Book | null>(null)

const handleModalOpen = (book: Book) => {
  expandedBook.value = book
}

const handleModalClose = () => {
  expandedBook.value = null
}

const toggleFav = (book: Book) => {
  book.isFav = !book.isFav
}
</script>

<template>
  <div class="wrapper">
    <a :href="url">Link</a>
    <p v-show="filteredBooks">Favourite books: {{ filteredBooks }}</p>
    <ul class="list">
      <li
        v-for="book in books"
        :key="book.title"
        :class="{ book, fav: book.isFav }"
        @click="toggleFav(book)"
      >
        <img :src="book.img" :alt="book.title" class="img" draggable="false" />
        <div class="title">
          <h3>{{ book.title }}</h3>
          <p>{{ book.author }}</p>
        </div>
        <button class="expand-button" @click.stop="handleModalOpen(book)">
          <img :src="zoomIcon" alt="Zoom in" />
        </button>
      </li>
    </ul>
    <Modal v-if="expandedBook" :book="expandedBook" :onClose="handleModalClose">
      <img :src="expandedBook.img" :alt="expandedBook.title" class="large-img" draggable="false"
    /></Modal>
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 1000px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.list {
  width: 100%;
  max-width: 800px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 0;
}

.book {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px;
  background-color: #ececec;
  border-radius: 5px;

  cursor: pointer;
  transition: all 0.2s ease-in;
}

.img {
  object-fit: cover;
  width: 70px;
}

.large-img {
  height: 85vh;
}
.title {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.fav {
  background-color: rgb(255, 222, 228);
}

.expand-button {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: transparent;
  border: none;
  cursor: pointer;
  transition: all 0.1s ease-in;
  margin-left: auto;
}

.expand-button:hover {
  transform: scale(1.1);
}
</style>
