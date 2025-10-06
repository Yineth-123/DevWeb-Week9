<template>
  <div class="section">
    <h2 :style="{ backgroundColor: background }">{{ title }}</h2>
    <ul>
      <li v-if="tasks.length === 0" class="empty">Sin tareas pendientes</li>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task }}
        <div class="actions">
          <button @click="$emit('move', { index, direction: -1 })">⬅</button>
          <button @click="$emit('move', { index, direction: 1 })">➡</button>
          <button @click="$emit('remove', index)">✖</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  color: String,
  tasks: Array
})

defineEmits(['remove', 'move'])
</script>

<style scoped>
.section {
  flex: 1;
  background-color: #ffd57c;
  border-radius: 12px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(0, 255, 128, 0.9);
}

.section h2 {
  color: rgb(253, 242, 242);
  background: rgb(255, 208, 0);
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: rgb(168, 235, 207);
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li.empty {
  text-align: center;
  color: gray;
  font-style: italic;
}

.actions button {
  margin-left: 5px;
  cursor: pointer;
  border: none;
  background: rgb(22, 163, 149);
  font-size: 1rem;
}
</style>
