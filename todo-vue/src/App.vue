<template>
  <div class="app">
    <!-- Hero -->
    <section id="home" class="hero">
      <div class="hero-content">
        <h2>Organize Your Life</h2>
        <p>Stay productive and never forget a task with our intuitive todo app.</p>
        <button class="cta-btn" @click="scrollToTodo">Get Started</button>
      </div>
    </section>

    <!-- Features -->
    <section id="features" class="features">
      <h2>Why Choose TodoMaster?</h2>
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">✅</div>
          <h3>Easy to Use</h3>
          <p>Simple interface designed for productivity</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">⚡</div>
          <h3>Fast & Responsive</h3>
          <p>Works seamlessly on all devices</p>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🎨</div>
          <h3>Beautiful Design</h3>
          <p>Modern UI that looks great everywhere</p>
        </div>
      </div>
    </section>

    <!-- Todo -->
    <section id="todo" class="todo-section">
      <h2>Your Tasks</h2>

      <div class="todo-container">
          <div class="input-container">
            <input
              v-model="newTask"
              @keyup.enter="addTask"
              placeholder="What needs to be done?"
              aria-label="New task"
            />
            <button
              class="add-btn"
              @click="addTask"
              :disabled="!newTask.trim()"
            >
              ➕ Add Task
            </button>
          </div>

          <ul class="task-list">
            <li
              v-for="task in tasks"
              :key="task.id"
              class="task-item"
              :class="{ completed: task.done }"
            >
              <div class="task-content">
                <input
                  type="checkbox"
                  v-model="task.done"
                  class="task-checkbox"
                />
                <span class="task-text">{{ task.text }}</span>
              </div>

              <button
                class="delete-btn"
                @click="deleteTask(task.id)"
                aria-label="Delete task"
              >
                🗑️
              </button>
            </li>
          </ul>

          <div v-if="tasks.length === 0" class="empty-state">
            No tasks yet. Add one above!
          </div>

          <div class="stats">
            {{ completedTasks }} of {{ tasks.length }} tasks completed
          </div>
        </div>
    </section>

    <!-- About -->
    <section id="about" class="about">
      <h2>About TodoMaster</h2>
      <p>
        TodoMaster is a simple yet powerful task manager built with Vue 3.
        It helps you stay organized, focused, and productive every day.
      </p>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <p>© 2025 TodoMaster · Built with Vue 3</p>
    </footer>
  </div>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue'

export default {
  name: 'TodoMaster',
  setup() {
    const newTask = ref('')
    const tasks = ref([])

    // Load from localStorage
    onMounted(() => {
      const saved = localStorage.getItem('tasks')
      if (saved) tasks.value = JSON.parse(saved)
    })

    // Save to localStorage
    watch(
      tasks,
      (val) => localStorage.setItem('tasks', JSON.stringify(val)),
      { deep: true }
    )

    const addTask = () => {
      if (!newTask.value.trim()) return

      tasks.value.push({
        id: Date.now(),
        text: newTask.value.trim(),
        done: false
      })

      newTask.value = ''
    }

    const deleteTask = (id) => {
      tasks.value = tasks.value.filter(task => task.id !== id)
    }

    const completedTasks = computed(
      () => tasks.value.filter(task => task.done).length
    )

    const scrollToTodo = () => {
      document.getElementById('todo')?.scrollIntoView({ behavior: 'smooth' })
    }

    return {
      newTask,
      tasks,
      addTask,
      deleteTask,
      completedTasks,
      scrollToTodo
    }
  }
}
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}

.app {
  font-family: 'Inter', 'Segoe UI', sans-serif;
  color: #333;
  min-height: 100vh;
  background: linear-gradient(180deg, #f8f9fa 0%, #e9ecef 100%);
}

/* Layout */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Hero */
.hero {
  min-height: 100vh;
  padding-top: 0;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.hero-content h2 {
  font-size: 3rem;
}

.hero-content p {
  margin: 1rem 0 2rem;
  font-size: 1.2rem;
}

.cta-btn {
  padding: 14px 30px;
  border-radius: 30px;
  background: #48bb78;
  border: none;
  color: white;
  font-size: 1.1rem;
  cursor: pointer;
}

/* Features */
.features {
  padding: 80px 20px;
  background: #f8f9fa;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.feature-card {
  background: white;
  padding: 2rem;
  border-radius: 16px;
  text-align: center;
}

/* Todo */
.todo-section {
  padding: 80px 20px;
  background: white;
}

.todo-container {
  max-width: 600px;
  margin: auto;
  background: #fff;
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

.input-container {
  display: flex;
  margin-bottom: 2rem;
}

input {
  flex: 1;
  padding: 14px;
  border-radius: 10px 0 0 10px;
  border: 1px solid #ccc;
}

.add-btn {
  background: #667eea;
  color: white;
  border: none;
  padding: 14px 20px;
  border-radius: 0 10px 10px 0;
}

.add-btn:disabled {
  background: #cbd5e0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  padding: 12px;
  border-radius: 12px;
  margin-bottom: 10px;
  background: #f9f9f9;
}

.task-item.completed .task-text {
  text-decoration: line-through;
  color: #999;
}

/* About */
.about {
  padding: 80px 20px;
  background: #edf2f7;
  text-align: center;
}

/* Footer */
.footer {
  padding: 2rem 20px;
  text-align: center;
  background: #333;
  color: white;
}
</style>
