<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, computed, watch, onMounted } from 'vue'
import ToDoForm from './components/ToDoForm.vue'
import ToDoList from './components/ToDoList.vue'

const userName = ref('Guest');
const todos = ref([]);


const priorities = ref({
  1: {
    name: 'critical'
  },
  2: {
    name: 'moderate'
  },
  3: {
    name: 'optional'
  }
});
const id = ref(0);

const isSorted = ref(false);

function addTodo(todo) {
  console.log(todo);
  if (todo.text.trim() === '') {
    return
  }

  if (!todoToUpdate.value) {
    todos.value.push({
      text: todo.text,
      id: id.value++,
      priority: todo.priority
    })
  } else {
    todos.value = todos.value.map((currTodo) => {
      if (currTodo.id === todoToUpdate.value.id) {
        return todo
      }
      return currTodo
    })
  }
  todoToUpdate.value = null
}

function deleteTodo(id) {
  console.log('delete ', id);
  todos.value = todos.value.filter((todo) => todo.id !== id);
}

function handleEdit(todo) {
  console.log('handleEdit ', todo);
  todoToUpdate.value = todo;
}

function handlesort() {
  isSorted.value = !isSorted.value
}

const sortedTodos = computed(() => {
  const temp = [...todos.value];
  return isSorted.value ?
    temp.sort((a, b) => a.priority - b.priority)
    : todos.value;
})

function handleDeleteAll() {
  todos.value = [];
}

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

const todoToUpdate = ref(null);

</script>

<template>
  <h1>To Dos App</h1>

  <ToDoForm @addTodo="addTodo" :todo="todoToUpdate"/>
  
  <ToDoList :todos="sortedTodos" @deleteTodo="deleteTodo" @editTodo="handleEdit" @sortList="handlesort" @deleteAll="handleDeleteAll" />
</template>

<style scoped>

</style>
