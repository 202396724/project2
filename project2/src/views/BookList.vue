<template>
  <div class="container mt-4">
    <h2 class="mb-4">Liste de Livres</h2>

    <div class="mb-4">
      <input
        type="text"
        v-model="searchQuery"
        class="form-control"
        placeholder="Rechercher..."
      />
    </div>

    <div class="row">
      <div v-for="book in paginatedBooks" :key="book.codeBarre" class="col-md-12 mb-3">
        <div class="card" @click="goToBookDetail(book)" style="cursor: pointer">
          <div class="card-body">
            <h5 class="card-title">{{ book.title }}</h5>
            <p class="card-text">
              <strong>Author:</strong> {{ book.writer.fullname || 'Unknown' }}<br>
              <strong>Publisher:</strong> {{ book.editor || 'Unknown' }}<br>
              <strong>Year:</strong> {{ book.date }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- Pagination -->
    <div class="d-flex justify-content-between mt-4">
      <button
        class="btn btn-primary"
        @click="previousPage"
        :disabled="currentPage === 1"
      >
        Precedent
      </button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button
        class="btn btn-primary"
        @click="nextPage"
        :disabled="currentPage === totalPages"
      >
        Suivant
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { books } from '../data/books'

const router = useRouter()
const searchQuery = ref('')
const currentPage = ref(1)
const itemsPerPage = 5

const filteredBooks = computed(() => {
  return books.filter(book => 
    book.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    book.writer.fullname.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const totalPages = computed(() => 
  Math.ceil(filteredBooks.value.length / itemsPerPage)
)

const paginatedBooks = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return filteredBooks.value.slice(start, end)
})

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

const previousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

const goToBookDetail = (book: any) => {
  router.push(`/books/${book.codeBarre}`)
}
</script>