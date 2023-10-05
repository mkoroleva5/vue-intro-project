<script setup lang="ts">
import { computed, ref, type Ref } from 'vue'
import zoomIcon from '@/assets/zoom-in.svg'
import Modal from '@/components/shared/Modal.vue'

export type Book = {
  title: string
  author: string
  img: string
  isFav: boolean
}

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
const showButton = ref<Book | null>(null)

const toggleShowButton = (book: Book | null) => {
  showButton.value = book
}

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
    <div class="books-list">
      <div
        v-for="book in books"
        :key="book.title"
        :class="{ book, fav: book.isFav }"
        @click="toggleFav(book)"
      >
        <button
          class="img-button"
          @click.stop="handleModalOpen(book)"
          @mouseenter="toggleShowButton(book)"
          @mouseleave="toggleShowButton(null)"
        >
          <img
            :src="book.img"
            :alt="book.title"
            class="image"
            :class="{ fade: showButton === book }"
            draggable="false"
          />
          <img
            :src="zoomIcon"
            alt="Zoom in"
            class="expand-icon"
            :class="{ show: showButton === book }"
          />
        </button>
        <div class="title">
          <h3>{{ book.title }}</h3>
          <p>{{ book.author }}</p>
        </div>
      </div>
    </div>
    <p v-show="filteredBooks">Favourite books: {{ filteredBooks }}</p>
    <Modal v-if="expandedBook" :onClose="handleModalClose">
      <img :src="expandedBook.img" :alt="expandedBook.title" class="large-image" draggable="false"
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
  gap: 20px;
}

.books-list {
  width: 100%;
  max-width: 800px;
  display: flex;
  gap: 20px;
  margin: 20px;
  padding: 0;
}

.book {
  min-width: 250px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background-color: #f6f6f6;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.2s ease-in;
  box-shadow: 2px 2px 10px #ccc;
}

.img-button {
  position: relative;
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.image {
  object-fit: cover;
  height: 200px;
  transition: all 0.2s ease-in;
}

.large-image {
  height: 85vh;
}
.title {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.fav {
  background-color: rgb(255, 225, 233);
}

.expand-icon {
  width: 30%;
  height: 30%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.2s ease-in;
  opacity: 0;
}

.show {
  opacity: 1;
}

.fade {
  filter: brightness(70%);
}

.expand-button:hover {
  transform: scale(1.1);
}
</style>
