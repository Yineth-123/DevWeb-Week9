<template>
  <div class="task-manager">
    <h1>Gestor de Tareas</h1>

    <div class="task-input">
      <input
        v-model="newTask"
        placeholder="Escribe aquí tu nueva tarea"
        @keyup.enter="addTask"
      />
      <button @click="addTask">Agregar</button>
    </div>

    <div v-if="hasTasks" class="task-sections">
      <TaskColumn
        title="To Do"
        color="#ffb347"
        :tasks="tasks.todo"
        @remove="removeTask('todo', $event)"
        @move="moveTask('todo', $event)"
      />
      <TaskColumn
        title="Doing"
        color="#ffcc33"
        :tasks="tasks.doing"
        @remove="removeTask('doing', $event)"
        @move="moveTask('doing', $event)"
      />
      <TaskColumn
        title="Done"
        color="#90ee90"
        :tasks="tasks.done"
        @move="moveTask('done', $event)"
      />
    </div>

    <p v-else class="no-tasks">No hay tareas registradas.</p>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import TaskColumn from './TaskColumn.vue'

const newTask = ref('')
const tasks = reactive({
  todo: [],
  doing: [],
  done: []
})

// ✅ Agregar tarea
const addTask = () => {
  const text = newTask.value.trim()
  if (text !== '') {
    tasks.todo.push(text)
    newTask.value = ''
  }
}

// ✅ Eliminar tarea
const removeTask = (column, index) => {
  tasks[column].splice(index, 1)
}

// ✅ Mover tarea entre columnas
const moveTask = (from, { index, direction }) => {
  const columns = ['todo', 'doing', 'done']
  const fromIndex = columns.indexOf(from)
  const toIndex = fromIndex + direction

  if (toIndex >= 0 && toIndex < columns.length) {
    const task = tasks[from][index]
    tasks[from].splice(index, 1)
    tasks[columns[toIndex]].push(task)
  }
}

// ✅ Mostrar secciones solo si hay tareas
const hasTasks = computed(() =>
  tasks.todo.length > 0 || tasks.doing.length > 0 || tasks.done.length > 0
)
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

.no-tasks {
  text-align: center;
  font-size: 1.3rem;
  color: #666;
  margin-top: 40px;
  font-style: italic;
}
</style>
