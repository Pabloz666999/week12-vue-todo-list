<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

const toggleComplete = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed
}
</script>

<template>
  <div class="app-container">
    <div class="content-wrapper">
      <div class="header">
        <h1 class="title">✨ My Tasks</h1>
        <p class="subtitle">Organize your day, one task at a time</p>
      </div>

      <div class="main-card">
        <div class="form-section">
          <form @submit.prevent="addTask" class="task-form">
            <input
              v-model="newTask"
              type="text"
              placeholder="Add a new task..."
              class="task-input"
            />
            <button type="submit" class="add-button">
              <span class="plus-icon">+</span>
              <span class="button-text">Add</span>
            </button>
          </form>
        </div>

        <div class="tasks-section">
          <div v-if="tasks.length === 0" class="empty-state">
            <div class="empty-icon">✓</div>
            <p class="empty-title">Tidak ada tugas</p>
          </div>

          <div v-else class="tasks-list">
            <div
              v-for="(task, index) in tasks"
              :key="task.id"
              class="task-item"
              :style="{ animationDelay: `${index * 0.1}s` }"
            >
              <button @click="toggleComplete(index)" class="check-button">
                <span v-if="task.completed" class="check-icon completed">✓</span>
                <span v-else class="check-icon">○</span>
              </button>

              <span :class="['task-text', { completed: task.completed }]">
                {{ task.text }}
              </span>

              <button @click="deleteTask(index)" class="delete-button">
                <span class="trash-icon">🗑</span>
              </button>
            </div>
          </div>
        </div>

        <div v-if="tasks.length > 0" class="stats-footer">
          <div class="stat-box active">
            <span class="stat-text">
              {{ tasks.filter(t => !t.completed).length }} active
            </span>
          </div>
          <div class="stat-box completed">
            <span class="stat-text">
              {{ tasks.filter(t => t.completed).length }} completed
            </span>
          </div>
        </div>
      </div>

      <div class="footer">
        <p>Made by F1D02310144 using Vue.js</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #f1f1f1);
  padding: 3rem 1rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.content-wrapper {
  max-width: 700px;
  margin: 0 auto;
}

/* Header Styles */
.header {
  text-align: center;
  margin-bottom: 2rem;
}

.title {
  font-size: 3rem;
  font-weight: 800;
  background: linear-gradient(135deg, #ed3838, #ff7272, #ffc9c9);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.5rem;
}

.subtitle {
  color: #6b7280;
  font-size: 1rem;
}

/* Main Card */
.main-card {
  background: white;
  border-radius: 2rem;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
  overflow: hidden;
}

/* Form Section */
.form-section {
  background: linear-gradient(135deg,#ed3838, #ff7272, #ffc9c9);
  padding: 2rem;
}

.task-form {
  display: flex;
  gap: 0.75rem;
}

.task-input {
  flex: 1;
  padding: 1rem 1.5rem;
  border-radius: 1rem;
  border: none;
  font-size: 1.1rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.task-input:focus {
  outline: none;
  box-shadow: 0 0 0 4px rgba(255, 255, 255, 0.5);
}

.add-button {
  background: white;
  color: #ff0000;
  border: none;
  padding: 1rem 2rem;
  border-radius: 1rem;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.add-button:hover {
  transform: scale(1.05);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
}

.plus-icon {
  font-size: 1.5rem;
  line-height: 1;
}

.button-text {
  display: none;
}

@media (min-width: 640px) {
  .button-text {
    display: inline;
  }
}

/* Tasks Section */
.tasks-section {
  padding: 2rem;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 4rem 2rem;
}

.empty-icon {
  display: inline-block;
  width: 120px;
  height: 120px;
  line-height: 120px;
  font-size: 4rem;
  background: linear-gradient(135deg, #ffe7e7, #fce7f3);
  border-radius: 50%;
  margin-bottom: 1rem;
  color: #fc8484;
}

.empty-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #9ca3af;
  margin-bottom: 0.5rem;
}

.empty-text {
  color: #9ca3af;
}

/* Tasks List */
.tasks-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.25rem;
  background: linear-gradient(135deg, #ffffff, #f9fafb);
  border-radius: 1rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  animation: slideIn 0.3s ease-out both;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.task-item:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.check-button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.75rem;
  transition: transform 0.2s ease;
  color: #d1d5db;
}

.check-button:hover {
  transform: scale(1.1);
}

.check-icon.completed {
  color: #10b981;
}

.task-text {
  flex: 1;
  font-size: 1.1rem;
  color: #374151;
  transition: all 0.3s ease;
}

.task-text.completed {
  text-decoration: line-through;
  color: #9ca3af;
}

.delete-button {
  background: linear-gradient(135deg, #f87171, #ffffff);
  border: none;
  padding: 0.75rem;
  border-radius: 0.75rem;
  cursor: pointer;
  font-size: 1.25rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  opacity: 0;
  transition: all 0.3s ease;
}

.task-item:hover .delete-button {
  opacity: 1;
}

.delete-button:hover {
  transform: scale(1.1);
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
}

.trash-icon {
  display: block;
}

/* Stats Footer */
.stats-footer {
  padding: 0 2rem 2rem;
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  font-size: 0.875rem;
}

.stat-box {
  padding: 0.75rem 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.stat-box.active {
  background: linear-gradient(135deg, #ffe7e7, #fce7f3);
}

.stat-box.completed {
  background: linear-gradient(135deg, #d1fae5, #dbeafe);
}

.stat-text {
  font-weight: 700;
}

.stat-box.active .stat-text {
  color: #ea3333;
}

.stat-box.completed .stat-text {
  color: #10b981;
}

/* Footer */
.footer {
  text-align: center;
  margin-top: 2rem;
  color: #6b7280;
  font-size: 0.875rem;
}
</style>