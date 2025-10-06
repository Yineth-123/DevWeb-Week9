<template>
  <div class="task-manager">
    <h1>Gestor de Tareas</h1>

    <div class="task-input">
      <input
        v-model="newTask"
        placeholder="Escribe una nueva tarea"
        @keyup.enter="addTask"
      />
      <button @click="addTask">Agregar</button>
    </div>

    <div class="task-sections">
      <TaskColumn
        title="To Do"
        color="#ff0707"
        :tasks="tasks.todo"
        @remove="removeTask('todo', $event)"
        @move="moveTask('todo', $event)"
      />
      <TaskColumn
        title="Doing"
        color="#ff0707"
        :tasks="tasks.doing"
        @remove="removeTask('doing', $event)"
        @move="moveTask('doing', $event)"
      />
      <TaskColumn
        title="Done ✅"
        color="#ff0707"
        :tasks="tasks.done"
        @remove="removeTask('done', $event)"
        @move="moveTask('done', $event)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TaskColumn from './TaskColumn.vue'

const newTask = ref('')
const tasks = ref({
  todo: [],
  doing: [],
  done: []
})

const addTask = () => {
  const text = newTask.value.trim()
  if (text !== '') {
    tasks.value.todo.push(text)
    newTask.value = ''
  }
}

const removeTask = (column, index) => {
  tasks.value[column].splice(index, 1)
}

const moveTask = (from, { index, direction }) => {
  const columns = ['todo', 'doing', 'done']
  const fromIndex = columns.indexOf(from)
  const toIndex = fromIndex + direction

  if (toIndex >= 0 && toIndex < columns.length) {
    const task = tasks.value[from][index]
    tasks.value[from].splice(index, 1)
    tasks.value[columns[toIndex]].push(task)
  }
}
</script>

<style scoped>
.task-manager {
  max-width: 1000px;
  margin: 0 auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

h1 {
  color: rgba(110, 53, 6, 0.979);
  font-size: 2.5rem;
  margin: 20px 0;
}

.task-input {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.task-input input {
  padding: 10px;
  width: 60%;
  border: 2px solid #d17245;
  border-radius: 20px 0 0 20px;
  outline: none;
}

.task-input button {
  background: #c07333;
  color: rgb(245, 232, 223);
  border: none;
  padding: 10px 20px;
  border-radius: 0 20px 20px 0;
  cursor: pointer;
}

.task-sections {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}
</style>
