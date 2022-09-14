<script setup>
import {ref, onMounted, computed, watch} from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) => {
  return a.createdAt - b.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null)
   return
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep:true})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos') || [])
})

</script>

<template>
  <main class="app">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320">
    <path fill="#0d2735" fill-opacity="1" d="M0,256L34.3,261.3C68.6,267,137,277,206,272C274.3,267,343,245,411,224C480,203,549,181,617,192C685.7,203,754,245,823,218.7C891.4,192,960,96,1029,74.7C1097.1,53,1166,107,1234,122.7C1302.9,139,1371,117,1406,106.7L1440,96L1440,0L1405.7,0C1371.4,0,1303,0,1234,0C1165.7,0,1097,0,1029,0C960,0,891,0,823,0C754.3,0,686,0,617,0C548.6,0,480,0,411,0C342.9,0,274,0,206,0C137.1,0,69,0,34,0L0,0Z"></path>
    </svg>
    <section class="greeting">
      <h2 class="title">
        what's up <input type="text" placeholder="Name here"
        v-model="name" />
      </h2>
      <section class="create-todo">
        <h3>CREATE A TODO</h3>
        <form @submit.prevent="addTodo">
          <h4>What's on your todo list</h4>
          <input type="text" placeholder="e.g. make a video" v-model="input_content"/>
          <h4>Pick a category</h4>
          <div class="options">
            <label>
              <input
                type="radio"
                name="category"
                id="category1"
                value="business"
                v-model="input_category"
              />
              <span class="bubble business"></span>
						<div>Business</div>
            </label>
            <label>
              <input
                type="radio"
                name="category"
                id="category1"
                value="personal"
                v-model="input_category"
              />
              <span class="bubble personal"></span>
						<div>Personal</div>
            </label>
          </div>
          <input type="submit" value="Add todo" />
        </form>
      </section>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todos_asc" v-bind:key="todo" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done"/>
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content"> 
            <input type="text" v-model="todo.content">
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </section>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320"><path fill="#0d2735" fill-opacity="1" d="M0,160L48,176C96,192,192,224,288,208C384,192,480,128,576,133.3C672,139,768,213,864,234.7C960,256,1056,224,1152,186.7C1248,149,1344,107,1392,85.3L1440,64L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"></path></svg>

  </main>
</template>