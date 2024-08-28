<script setup>
import { ref, computed, watch, onMounted } from 'vue'
const emit = defineEmits(['deleteTodo', 'editTodo', 'sort'])
const props = defineProps(['todos'])
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

const todoList = ref([])

watch(props, (newVal) => {
  console.log('prop ', props.todos)
  console.log('newVal ', newVal.todos)
  todoList.value = newVal.todos || []
})

function deleteTodo(id) {
  emit('deleteTodo', id);
}

function editTodo(todo) {
  emit('editTodo', todo);
}

function sortList() {
  emit('sortList');
}

function deleteAll() {
  emit('deleteAll');
}



</script>

<template>
  <div v-if="todoList.length > 0" class="todo-list-main">
    <button @click="sortList()">Sort by priority</button>
    <button @click="deleteAll()">Delete all</button>
    <!-- <button @click="isSorted = !isSorted">Sort by priority</button> -->
    <ul>
      <li v-for="todo in todoList" key="todo.id">
        <span>{{todo.text}}</span>
        <span class="priority">{{priorities[todo.priority].name}}</span>
        <button @click="editTodo(todo)">Edit</button>
        <button @click="deleteTodo(todo.id)">x</button>
      </li>
    </ul>
  </div>
  
  <p v-else>You have not added any To Dos yet.</p>

</template>

<style scoped>
ul {
  
  padding-inline-start: 0;
}
li {
  display: flex;
  width: 100%;
  justify-content: space-between;
  border-bottom: 1px solid black;
  padding: 5px 0;
  font-size: 1.3rem;
}
.todo-list-main {
  margin-top: 20px;
  width: 30%;
}
</style>
