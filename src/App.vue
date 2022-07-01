<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Привет, <input type="text" placeholder="Ваше имя" v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h2>Добавить новое</h2>
      <form action="#" @submit.prevent="addTodo">
        <h4>Введите название</h4>
        <input type="text" placeholder="Название" v-model="input_content">
        <h4>Выберите категорию</h4>
        <div class="options">
          <label>
            <input
                type="radio"
                name="category"
                value="business"
                v-model="input_category"
            >
            <span class="bubble bisiness"></span>
            <div>Бизнес</div>
          </label>
          <label>
            <input
                type="radio"
                name="category"
                value="personal"
                v-model="input_category"
            >
            <span class="bubble personal"></span>
            <div>Личное</div>
          </label>
        </div>
        <input type="submit" value="Добавить">
      </form>
    </section>
    <section class="todo-list">
      <h2>Список дел</h2>
      <div
          class="list"
          v-for="todo in todos_asc"
          :key="todo.createdAt"
          v-if="todos_asc.length>0"
      >
        <div class="todo-item" :class="{done: todo.done}">
          <label>
            <input type="checkbox" v-model="todo.done"/>
            <span
                class="bubble"
                :class="{business : todo.category === 'business', personal: todo.category === 'personal'}"
            ></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content"/>
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Удалить</button>
          </div>
        </div>
      </div>
      <h4 v-else>Список пуст</h4>
    </section>
  </main>
</template>

<script setup>
import {ref, onMounted, computed, watch} from 'vue';

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref('')

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  })
  input_content.value = '';
  input_category.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t=>t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep: true})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<style>

</style>
