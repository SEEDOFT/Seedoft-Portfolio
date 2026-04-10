<script setup lang="ts">
import TerminalHero from './components/TerminalHero.vue';
import ProjectCard from './components/ProjectCard.vue';
import portfolioData from './data/portfolio.json';

// Explicitly type the imported JSON data
import type { Project } from './components/ProjectCard.vue';

interface SkillGroup {
  category: string;
  items: string[];
}

interface PortfolioInfo {
  name: string;
  tagline: string;
  location: string;
  email: string;
  github: string;
}

const projects: Project[] = portfolioData.projects;
const skills: SkillGroup[] = portfolioData.skills;
const info: PortfolioInfo = portfolioData.info;
</script>

<template>
  <!-- Navigation Bar -->
  <nav class="navbar fade-in">
    <div class="nav-content">
      <a href="#home" class="nav-logo">&gt; {{ info.name }}</a>
      <div class="nav-links">
        <a href="#home">Home</a>
        <a href="#profile">Profile</a>
        <a href="#skills">Skills</a>
        <a href="#projects">Projects</a>
      </div>
    </div>
  </nav>

  <div class="app-container">
    <header id="home" class="fade-in delay-1">
      <div class="header-spacing"></div>
      <TerminalHero />
    </header>

    <main>
      <!-- About / Info Section -->
      <section id="profile" class="info-section fade-in delay-2">
        <div class="info-content">
          <h1>{{ info.name }}</h1>
          <p class="tagline">{{ info.tagline }}</p>
          <div class="meta">
            <span>📍 {{ info.location }}</span>
            <span>📧 {{ info.email }}</span>
          </div>
        </div>
      </section>

      <!-- Skills Section -->
      <section id="skills" class="skills-section fade-in delay-3">
        <h2 class="section-title">Technical Arsenal</h2>
        <div class="skills-container">
          <div v-for="group in skills" :key="group.category" class="skill-group">
            <h3>{{ group.category }}</h3>
            <div class="skill-items">
              <span v-for="skill in group.items" :key="skill" class="skill-badge">
                {{ skill }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- Projects Section -->
      <section id="projects" class="projects-section fade-in delay-4">
        <h2 class="section-title">Selected Projects</h2>
        <div class="projects-grid">
          <ProjectCard 
            v-for="project in projects" 
            :key="project.id" 
            :project="project" 
          />
        </div>
      </section>
    </main>

    <footer class="fade-in delay-5">
      <p>&copy; 2026 {{ info.name }}.</p>
      <div class="footer-links">
        <a :href="info.github" target="_blank">GitHub</a>
        <a :href="'mailto:' + info.email">Contact</a>
      </div>
    </footer>
  </div>
</template>

<style>
/* CSS imports MUST be at the very top */
@import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;700&display=swap');

:root {
  --bg-color: #0d1117;
  --surface-color: #161b22;
  --border-color: #30363d;
  --text-color: #e6edf3;
  --text-muted: #8b949e;
  --accent-color: #42b883;
  --accent-secondary: #f55247;
  --nav-height: 70px;
}

html {
  scroll-behavior: smooth;
  scroll-padding-top: var(--nav-height); /* Ensure scroll stops right below navbar */
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
}

/* Navbar Styles */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: var(--nav-height);
  background-color: rgba(13, 17, 23, 0.85);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border-bottom: 1px solid var(--border-color);
  z-index: 1000;
  display: flex;
  align-items: center;
}

.nav-content {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-logo {
  font-family: 'Fira Code', monospace;
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--accent-color);
  text-decoration: none;
}

.nav-links {
  display: flex;
  gap: 1.5rem;
}

.nav-links a {
  color: var(--text-muted);
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  transition: color 0.2s ease;
}

.nav-links a:hover {
  color: var(--text-color);
}

.app-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 1.5rem;
}

.header-spacing {
  height: calc(var(--nav-height) + 2rem);
}

/* Animations */
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 0.8s ease forwards;
}

/* Navbar specific slide-down on initial load */
.navbar.fade-in {
  transform: translateY(-20px);
}

.delay-1 { animation-delay: 0.2s; }
.delay-2 { animation-delay: 0.4s; }
.delay-3 { animation-delay: 0.6s; }
.delay-4 { animation-delay: 0.8s; }
.delay-5 { animation-delay: 1.0s; }

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Section Styles */
section {
  padding: 4rem 0;
  border-bottom: 1px solid var(--border-color);
}

.section-title {
  font-family: 'Fira Code', monospace;
  font-size: 1.75rem;
  margin-bottom: 2.5rem;
  color: var(--accent-color);
  display: flex;
  align-items: center;
}

.section-title::before {
  content: "> ";
  opacity: 0.5;
  margin-right: 0.5rem;
}

/* Info Section */
.info-section {
  text-align: center;
}

.info-content h1 {
  font-size: 3rem;
  margin: 0;
  background: linear-gradient(120deg, var(--accent-color), #a5d6ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.tagline {
  font-family: 'Fira Code', monospace;
  font-size: 1.25rem;
  color: var(--text-muted);
  margin: 1rem 0;
}

.meta {
  display: flex;
  justify-content: center;
  gap: 2rem;
  color: var(--text-muted);
  font-size: 0.9rem;
}

/* Skills Section */
.skills-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.skill-group h3 {
  font-family: 'Fira Code', monospace;
  font-size: 1rem;
  margin-bottom: 1rem;
  color: var(--text-muted);
}

.skill-items {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.skill-badge {
  background-color: var(--surface-color);
  border: 1px solid var(--border-color);
  padding: 0.4rem 0.8rem;
  border-radius: 6px;
  font-size: 0.85rem;
  transition: all 0.2s ease;
}

.skill-badge:hover {
  border-color: var(--accent-color);
  color: var(--accent-color);
  transform: scale(1.05);
}

/* Projects Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

/* Footer */
footer {
  text-align: center;
  padding: 4rem 0;
  color: var(--text-muted);
}

.footer-links {
  margin-top: 1rem;
  display: flex;
  justify-content: center;
  gap: 1.5rem;
}

.footer-links a {
  color: var(--accent-color);
  text-decoration: none;
  font-size: 0.9rem;
}

.footer-links a:hover {
  text-decoration: underline;
}

/* Responsive adjustments for navbar */
@media (max-width: 600px) {
  .nav-links {
    gap: 1rem;
  }
  .nav-links a {
    font-size: 0.85rem;
  }
  .info-content h1 { font-size: 2rem; }
  .meta { flex-direction: column; gap: 0.5rem; }
}
</style>
