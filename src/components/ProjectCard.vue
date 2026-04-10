<script setup lang="ts">
// Define the structure of the data this component accepts
export interface Project {
  id: number;
  title: string;
  stack: string[];
  description: string;
  role: string;
  impact_snippet?: string;
  links: {
    github?: string;
    live?: string;
  };
}

// Receive the project data as a single prop
const props = defineProps<{
  project: Project;
}>();

// Simple function to dynamically color stack elements (Vue green, Laravel red, standard gray)
const getTagColor = (tech: string): string => {
  const t = tech.toLowerCase();
  if (t.includes('laravel')) return '#f55247'; // Red
  if (t.includes('vue')) return '#42b883';    // Vue Green
  if (t.includes('flutter')) return '#40d0fb'; // Flutter Blue
  return '#30363d'; // Default Surface Border Gray
};
</script>

<template>
  <div class="project-card">
    <div class="card-body">
      
      <div class="stack-container">
        <span 
          v-for="tech in project.stack" 
          :key="tech" 
          class="tech-pill"
          :style="{ borderColor: getTagColor(tech) }"
        >
          {{ tech }}
        </span>
      </div>

      <h3 class="title">{{ project.title }}</h3>
      
      <p class="description">{{ project.description }}</p>

      <div class="details">
        <p class="role"><strong>My Role:</strong> {{ project.role }}</p>
        
        <div v-if="project.impact_snippet" class="impact-snippet">
          <span class="prefix">API_LOG >></span>
          <span class="message">{{ project.impact_snippet }}</span>
        </div>
      </div>
    </div>

    <div v-if="project.links.github || project.links.live" class="card-footer">
      <a v-if="project.links.github" :href="project.links.github" target="_blank" class="link-btn github">
        View Code
      </a>
      <a v-if="project.links.live" :href="project.links.live" target="_blank" class="link-btn live">
        Live Demo
      </a>
    </div>
  </div>
</template>

<style scoped>
/* Fira Code or equivalent mono font required */
.project-card {
  font-family: 'Fira Code', 'JetBrains Mono', monospace;
  background-color: #161b22; /* Surface */
  border: 1px solid #30363d;
  border-radius: 8px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

/* Subtle interactiveness on hover */
.project-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.3);
  border-color: #42b883; /* Fade into Vue green on hover */
}

.card-body {
  padding: 1.5rem;
  flex-grow: 1;
}

/* Stack styling */
.stack-container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.tech-pill {
  font-size: 0.75rem;
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  background-color: #0d1117;
  color: #e6edf3;
  border-width: 1px;
  border-style: solid;
}

/* Title & Description */
.title {
  font-size: 1.25rem;
  color: #e6edf3;
  margin: 0 0 0.75rem 0;
  font-weight: bold;
}

.description {
  font-size: 0.9rem;
  color: #8b949e;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

/* Project Details & Log area */
.details {
  font-size: 0.85rem;
  border-top: 1px solid #30363d;
  padding-top: 1rem;
}

.role {
  color: #e6edf3;
  margin-bottom: 0.75rem;
}

.impact-snippet {
  background-color: #0d1117;
  padding: 0.75rem;
  border-radius: 4px;
  color: #a5d6ff; /* A slightly different mono blue */
  display: flex;
  gap: 0.5rem;
}

.impact-snippet .prefix {
  color: #42b883; /* Prefix in green */
  opacity: 0.8;
}

/* Footer / Links */
.card-footer {
  padding: 1rem 1.5rem;
  background-color: #0d1117;
  border-top: 1px solid #30363d;
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}

.link-btn {
  font-size: 0.8rem;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  text-decoration: none;
  font-weight: bold;
  text-align: center;
  flex: 1;
}

.link-btn.github {
  background-color: #21262d;
  color: #e6edf3;
  border: 1px solid #30363d;
}
.link-btn.github:hover {
  background-color: #30363d;
}

.link-btn.live {
  background-color: #f55247; /* Laravel Red for main action */
  color: #fff;
}
.link-btn.live:hover {
  filter: brightness(110%);
}
</style>
