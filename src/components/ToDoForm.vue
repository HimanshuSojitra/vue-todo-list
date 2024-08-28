<script setup>
import { ref, computed, watch, onMounted } from 'vue'

const props = defineProps({
  todo: Object
})

const emit = defineEmits(['addTodo'])

const newTodo = ref('');
const newTodoPriority = ref('2');

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

function addTodo() {
  emit('addTodo', {
    text: newTodo.value,
    priority: newTodoPriority.value
  });

  newTodo.value = '';
  newTodoPriority.value = '2'
}

watch(props, (newVal) => {
  console.log('prop ', props.todo);
  console.log('newVal ', newVal.todo);
  newTodo.value = newVal.todo?.text || ''
  newTodoPriority.value = newVal.todo?.priority || '2'
})

</script>

<template>
<div class="form-main">
  <h2>{{ todo ? 'Edit' : 'Add' }} to do</h2>
  <form @submit.prevent="addTodo">
    <input type="text" v-model="newTodo" placeholder="Enter what's on your mind" />
    <div class="priority-box">
      Select a priority:
      <label v-for="(priority, key) in priorities" :key="priority.value">
        <input v-model="newTodoPriority" type="radio" :value="key" name="todo-priority" />
        {{ priority.name }}
      </label>
      
    </div>
    <button>{{ todo ? 'Update' : 'Add' }}</button>
  </form>
</div>
</template>

<style scoped>
.form-main {
  border: 1px solid black;
  border-radius: 5px;
  padding: 10px;
  width: 30%;
}

label {
  display: block;
  font-weight: bold;
}

input[type="text"] {
  padding: 5px;
  border: 1px solid darkgrey;
  border-radius: 5px;
  font-size: 1.2rem;
}

.priority-box{
  margin-top: 5px;
}
</style>
