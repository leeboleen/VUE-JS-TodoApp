<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - b.createdAt
}))
</script>

<template>

  <main class="app">

    <div class="greeting">
      
      <h2 class="title">Welcome, <input type="text" placeholder="Name Here" v-model="name" /></h2>

    </div>

  </main>

</template>

