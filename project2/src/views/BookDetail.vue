<template>
  <div class="container mt-4">
    <div v-if="book" class="card">
      <div class="card-body">
        <h2 class="card-title mb-4">{{ book.title }}</h2>
        <div class="row">
          <div class="col-md-6">
            <p><strong>Author:</strong> {{ book.writer.fullname || 'Unknown' }}</p>
            <p><strong>Publisher:</strong> {{ book.editor || 'Unknown' }}</p>
            <p><strong>Year:</strong> {{ book.date }}</p>
            <p><strong>Format:</strong> {{ book.format }}</p>
            <p><strong>Type:</strong> {{ book.type }}</p>
            <p><strong>Section:</strong> {{ book.section }}</p>
          </div>
          <div class="col-md-6">
            <p><strong>ISBN-10:</strong> {{ book.isbn?.isbn10?.id || 'N/A' }}</p>
            <p><strong>ISBN-13:</strong> {{ book.isbn?.isbn13?.id || 'N/A' }}</p>
            <p><strong>Code:</strong> {{ book.codeBarre }}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="mt-4">
      <router-link to="/books" class="btn btn-primary">Back to Book List</router-link>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { books } from '../data/books'

const route = useRoute()
const book = computed(() => 
  books.find(b => b.codeBarre === route.params.id)
)
</script>