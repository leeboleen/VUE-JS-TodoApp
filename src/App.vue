<script setup>

// ref: handling state
// onMounted: First render of component -> gets local storage vals
// computed: processed in order, for computed/ordered list by date
// watch: watches for changes in refs, updates live vals from local storage

// import from Composition API, via Vue
import { ref, onMounted, computed, watch } from 'vue'

// define val for todos array
// define val for name ref
const todos = ref([])
const name = ref('')

// define val for content entered in input field
// define val for category, preset Null (business / personal category of to do item)
const input_content = ref('')
const input_category = ref(null)

// define val for todos items, in asc order -> sub-function to sort values
// sorts and processes b (vals) in order of createdAt
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

// create val for adding Todo item to list
const addTodo = () => {

  // if content is empty (trim() remove spaces) OR category is null
  if(input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  // pushes content inside
  // defines content, from input value, category from category value, aligns createdAt val with getTime() func to assign datetime created
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })

  // resets content/category vals on enter
  input_content.value = ''
  input_category.value = null

}

// create func for removeTodo
const removeTodo = () => {
  todos.value = todoa.value.filter(t => t !== todo)
}

// compares todos[] against local storage
watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
})

// watches local storage values, updates to 'name' if set in cache
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
}, { deep: true }) // 'deep' looks inside, catches ANY change granularly inside the method

// mount function to assign val in cache to live elem
onMounted(() => {
  // assign name value item 'name' from local storage || (else) '' blank
  name.value = localStorage.getItem('name') || ''
  // parses JSON via local storage, mounts by order from createdAt
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>

  <main class="app">

    <section class="greeting">
      
      <h2 class="title">Welcome, <input type="text" placeholder="Name Here" v-model="name" /></h2>

    </section>

    <section class="create-todo">

      <h3>ADD TO DO</h3>

      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input 
          type="text" 
          placeholder="e.g. hire this candidate" 
          v-model="input_content"/>

        <h4>Assign a category</h4>

        <div class="options">

          <label>
            <input type="radio" name="category" value="business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Work</div>
          </label>

          <label>
            <input type="radio" name="category" value="personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>

        </div>

        <input type="submit" value="Add todo" />
        
        <!-- Two-way bound 'input_content' output elem, from input (live type) 
        {{ input_content }} -->
      </form>

    </section>

    <section class="todo-list">
      <h3>MY LIST</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :key="`todo-item ${todo.done && 'done'}`">

          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
             <input type="text" v-model="todo.content" />
          </div>
          
          <div class="actions">
             <button class="delete" @click="removeTodo(todo)">Remove</button>
          </div>

        </div>
      </div>
    </section>

  </main>

</template>

