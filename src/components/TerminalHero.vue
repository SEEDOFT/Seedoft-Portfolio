<script setup lang="ts">
import { ref, onMounted } from 'vue';

// Customize your personal data here
const username = ref("SEEDOFT");
const university = ref("National University of Management, Cambodia");
const graduating = ref("Year 4th Student, Expected Graduation: 2026");
const currentRole = ref("Backend Developer (Laravel)");

// Simple typing effect management
const isTyping = ref(true);
const visibleLines = ref<number>(0);

const commands = [
  "sudo whoami",
  `$ whoami: ${currentRole.value}`,
  "ls ~/education",
  `$ ~education: ['${university.value}', 'Graduating: ${graduating.value}']`,
  "history | grep 'projects' | head -n 3",
  "1: Laravel_API_Performance_tuning.log",
  "2: Flutter_ERP_Integration.md",
  "3: WinForms_Migration_Tool.exe"
];

// Simple line-by-line reveal
onMounted(() => {
  let line = 0;
  const interval = setInterval(() => {
    visibleLines.value++;
    line++;
    if (line >= commands.length) {
      isTyping.value = false;
      clearInterval(interval);
    }
  }, 1000); // Reveal one line per second
});

</script>

<template>
  <div class="terminal-container">
    <div class="terminal-window">
      <div class="terminal-header">
        <div class="dots">
          <span class="dot red"></span>
          <span class="dot yellow"></span>
          <span class="dot green"></span>
        </div>
        <div class="terminal-title">{{ username }}@portfolio: ~/intro</div>
      </div>
      
      <div class="terminal-content">
        <div v-for="(cmd, index) in commands.slice(0, visibleLines)" :key="index" class="line">
          <span class="prompt">{{ username }}@portfolio:~$</span>
          <span :class="{ 'command': index % 2 === 0, 'response': index % 2 !== 0 }">
            {{ cmd }}
          </span>
        </div>
        
        <div class="cursor-line">
          <span class="prompt">{{ username }}@portfolio:~$</span>
          <span class="cursor">_</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Core container for positioning */
.terminal-container {
  font-family: 'Fira Code', 'JetBrains Mono', 'Courier New', monospace;
  width: 100%;
  padding: 2rem 0;
  display: flex;
  justify-content: center;
}

/* Styling the fake window */
.terminal-window {
  background-color: #161b22; /* Surface */
  border: 1px solid #30363d;
  border-radius: 8px;
  width: 90%;
  max-width: 800px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.5);
  overflow: hidden;
}

/* Header bar styling */
.terminal-header {
  background-color: #0d1117; /* Background */
  padding: 0.5rem 1rem;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #30363d;
}

.dots {
  display: flex;
  gap: 0.5rem;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.dot.red { background-color: #ff5f56; }
.dot.yellow { background-color: #ffbd2e; }
.dot.green { background-color: #27c93f; }

.terminal-title {
  color: #8b949e;
  font-size: 0.8rem;
  margin-left: auto;
  margin-right: auto; /* Simple center align */
}

/* Content area */
.terminal-content {
  padding: 1rem;
  font-size: 0.95rem;
  line-height: 1.6;
  color: #e6edf3;
}

.prompt {
  color: #42b883; /* Vue Green */
  font-weight: bold;
  margin-right: 0.75rem;
}

.command {
  color: #e6edf3;
}

.response {
  color: #8b949e;
}

/* The prompt line waiting for input */
.cursor-line {
  display: flex;
  align-items: center;
}

/* Custom cursor animation */
.cursor {
  background-color: #42b883;
  color: #42b883;
  animation: blink 1s infinite steps(1);
}

@keyframes blink {
  50% { opacity: 0; }
}
</style>
