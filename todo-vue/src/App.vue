<template>
  <div class="app">
    <!-- Header -->
    <header class="header">
      <nav class="nav">
        <div class="nav-brand">
          <h1>📝 TodoMaster</h1>
        </div>
        <ul class="nav-links">
          <li><a href="#home">Home</a></li>
          <li><a href="#features">Features</a></li>
          <li><a href="#about">About</a></li>
        </ul>
      </nav>
    </header>

    <!-- Hero Section -->
    <section id="home" class="hero">
      <div class="hero-content">
        <h2>Organize Your Life</h2>
        <p>Stay productive and never forget a task with our intuitive todo app.</p>
        <button class="cta-btn" @click="scrollToTodo">Get Started</button>
      </div>
    </section>

    <!-- Features Section -->
    <section id="features" class="features">
      <div class="container">
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
      </div>
    </section>

    <!-- Todo Section -->
    <section id="todo" class="todo-section">
      <div class="container">
        <h2>Your Tasks</h2>
        <div class="todo-container">
          <div class="input-container">
            <input v-model="newTask" @keyup.enter="addTask" placeholder="What needs to be done?" />
            <button @click="addTask" class="add-btn">➕ Add Task</button>
          </div>
          <ul class="task-list">
            <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.done }" class="task-item">
              <div class="task-content">
                <input type="checkbox" :checked="task.done" @change="toggleDone(index)" class="task-checkbox" />
                <span class="task-text">{{ task.text }}</span>
              </div>
              <button @click="deleteTask(index)" class="delete-btn">🗑️</button>
            </li>
          </ul>
          <div v-if="tasks.length === 0" class="empty-state">
            <p>No tasks yet. Add one above!</p>
          </div>
          <div class="stats">
            <p>{{ completedTasks }} of {{ tasks.length }} tasks completed</p>
          </div>
        </div>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="about">
      <div class="container">
        <h2>About TodoMaster</h2>
        <p>TodoMaster is your ultimate productivity companion. Built with Vue.js and modern web technologies, it helps you stay organized and focused on what matters most.</p>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <p>&copy; 2025 TodoMaster. Built with Vue.js</p>
      </div>
    </footer>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
export default {
  setup() {
    const newTask = ref('')
    const tasks = ref([])

    const addTask = () => {
      if (newTask.value.trim() !== '') {
        tasks.value.push({ text: newTask.value, done: false })
        newTask.value = ''
      }
    }
    const toggleDone = (index) => { tasks.value[index].done = !tasks.value[index].done }
    const deleteTask = (index) => { tasks.value.splice(index, 1) }

    const completedTasks = computed(() => tasks.value.filter(task => task.done).length)

    const scrollToTodo = () => {
      document.getElementById('todo').scrollIntoView({ behavior: 'smooth' })
    }

    return { newTask, tasks, addTask, toggleDone, deleteTask, completedTasks, scrollToTodo }
  }
}
</script>

<style scoped>
/* Global Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

.app {
  font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  color: #333;
  background: linear-gradient(180deg, #f8f9fa 0%, #e9ecef 100%);
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Header */
.header {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 20px rgba(0,0,0,0.1);
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 1000;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0;
  max-width: 1200px;
  margin: 0 auto;
}

.nav-brand h1 {
  color: #667eea;
  font-size: 1.8rem;
  margin: 0;
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
  transition: color 0.3s;
}

.nav-links a:hover {
  color: #667eea;
}

/* Hero Section */
.hero {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
  color: white;
  padding: 120px 0 80px;
  text-align: center;
  min-height: 100vh;
  display: flex;
  align-items: center;
  position: relative;
  overflow: hidden;
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="80" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="60" r="1" fill="rgba(255,255,255,0.1)"/></svg>');
  animation: float 20s infinite linear;
}

@keyframes float {
  0% { transform: translateY(0px); }
  100% { transform: translateY(-100px); }
}

.hero-content h2 {
  font-size: 3.5rem;
  margin-bottom: 1rem;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.hero-content p {
  font-size: 1.3rem;
  margin-bottom: 2rem;
  opacity: 0.9;
}

.cta-btn {
  background: linear-gradient(135deg, #48bb78 0%, #38a169 100%);
  color: white;
  border: none;
  padding: 15px 30px;
  font-size: 1.1rem;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(72, 187, 120, 0.3);
  position: relative;
  overflow: hidden;
}

.cta-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transition: left 0.5s;
}

.cta-btn:hover::before {
  left: 100%;
}

.cta-btn:hover {
  background: linear-gradient(135deg, #38a169 0%, #2f855a 100%);
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(72, 187, 120, 0.4);
}

/* Features Section */
.features {
  padding: 80px 0;
  background: #f8f9fa;
}

.features h2 {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 3rem;
  color: #333;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.feature-card {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 8px 32px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.feature-card:hover {
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 15px 40px rgba(0,0,0,0.2);
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.feature-card h3 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #667eea;
}

/* Todo Section */
.todo-section {
  padding: 80px 0;
  background: white;
}

.todo-section h2 {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 3rem;
  color: #333;
}

.todo-container {
  max-width: 600px;
  margin: 0 auto;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.input-container {
  display: flex;
  margin-bottom: 2rem;
}

input {
  flex: 1;
  padding: 15px;
  border: 2px solid #e1e5e9;
  border-radius: 10px 0 0 10px;
  font-size: 16px;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #667eea;
}

.add-btn {
  background: #667eea;
  color: white;
  border: none;
  padding: 15px 20px;
  border-radius: 0 10px 10px 0;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

.add-btn:hover {
  background: #5a67d8;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0 0 2rem 0;
}

.task-item {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  margin-bottom: 10px;
  padding: 15px;
  border-radius: 15px;
  transition: all 0.3s ease;
  border-left: 5px solid #667eea;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.task-item:hover {
  transform: translateY(-3px) scale(1.01);
  box-shadow: 0 8px 25px rgba(0,0,0,0.15);
}

.task-item.completed {
  background: #e8f5e8;
  border-left-color: #48bb78;
}

.task-content {
  flex: 1;
  display: flex;
  align-items: center;
}

.task-checkbox {
  margin-right: 15px;
  transform: scale(1.2);
  accent-color: #667eea;
}

.task-text {
  font-size: 16px;
  transition: all 0.3s;
}

.task-item.completed .task-text {
  text-decoration: line-through;
  color: #a0aec0;
}

.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: #e53e3e;
  transition: transform 0.2s;
  margin-left: 10px;
}

.delete-btn:hover {
  transform: scale(1.2);
}

.empty-state {
  text-align: center;
  color: #a0aec0;
  font-style: italic;
  margin: 2rem 0;
}

.stats {
  text-align: center;
  color: #667eea;
  font-weight: 500;
}

/* About Section */
.about {
  padding: 80px 0;
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.about::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><polygon points="50,0 61,35 98,35 68,57 79,91 50,70 21,91 32,57 2,35 39,35" fill="rgba(255,255,255,0.05)"/></svg>');
  animation: rotate 30s infinite linear;
}

@keyframes rotate {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.about h2 {
  font-size: 2.5rem;
  margin-bottom: 2rem;
  color: #333;
}

.about p {
  font-size: 1.2rem;
  max-width: 800px;
  margin: 0 auto;
  color: #666;
}

/* Footer */
.footer {
  background: rgba(51, 51, 51, 0.9);
  backdrop-filter: blur(10px);
  color: white;
  padding: 2rem 0;
  text-align: center;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer p {
  margin: 0;
  opacity: 0.8;
}

/* Responsive */
@media (max-width: 768px) {
  .nav {
    flex-direction: column;
    gap: 1rem;
  }

  .nav-links {
    gap: 1rem;
  }

  .hero-content h2 {
    font-size: 2.5rem;
  }

  .hero-content p {
    font-size: 1.1rem;
  }

  .features-grid {
    grid-template-columns: 1fr;
  }

  .todo-container {
    margin: 0 20px;
  }
}
</style>
