<template>
  <div class="mw-container">
    <div class="scanline-effect"></div>
    <div class="hud-border"></div>
    <div class="vignette"></div>

    <header class="hud-header">
      <div class="header-left">
        <span class="logo-text">RECRUITMENT FILE // GS_25</span>
      </div>
      <div class="header-right">
        <span class="status-text">STATUS: <span class="status-active">ACTIVE</span></span>
        <span class="clock">{{ currentTime }}</span>
      </div>
    </header>

    <main class="dossier">
      <div class="operative-id-panel" data-aos="fade-right">
        <div class="operative-photo">
          <img :src="profilePictureUrl" alt="Operative Photo" class="operative-image">
        </div>
        <div class="operative-info">
          <h1 class="operative-name">{{ name }}</h1>
          <p class="operative-title">{{ profession }}</p>
        </div>
        <div class="operative-links">
          <a v-for="link in socials" :key="link.name" :href="link.url" target="_blank" class="link-button">
            {{ link.name }}
          </a>
        </div>
        <div class="contact-actions">
           <a :href="resumeUrl" target="_blank" class="action-button">DOWNLOAD DOSSIER</a>
        </div>
      </div>

      <div class="tabs-panel" data-aos="fade-left" data-aos-delay="200">
        <div class="tabs-nav">
          <button
            v-for="tab in tabs"
            :key="tab"
            @click="activeTab = tab"
            :class="{ 'active': activeTab === tab }"
            class="tab-button"
          >
            {{ tab }}
          </button>
        </div>

        <div class="tab-content">
          <div v-if="activeTab === 'Identification'" class="content-section">
            <h2 class="section-title">SUBJECT ANALYSIS</h2>
            <p class="bio">{{ bio }}</p>
          </div>

          <div v-if="activeTab === 'Skills'" class="content-section">
             <h2 class="section-title">SKILLS</h2>
             <div class="skill-category" v-for="(skillList, category) in skills" :key="category">
                <h3 class="skill-category-title">{{ formatSkillCategory(category) }}</h3>
                <div class="skill-grid">
                  <div v-for="skill in skillList" :key="skill" class="skill-tag">
                    {{ skill }}
                  </div>
                </div>
            </div>
          </div>

          <div v-if="activeTab === 'Operations'" class="content-section">
            <h2 class="section-title">OPERATIONS</h2>
            <div class="op-list">
              <div
                class="op-item"
                v-for="project in projects"
                :key="project.title"
                @click="toggleProject(project.title)"
                :class="{ 'expanded': expandedProject === project.title }"
              >
                <div class="op-header">
                  <h3 class="op-title">{{ project.title }}</h3>
                  <span class="op-status">COMPLETED</span>
                </div>
                <p class="op-desc">{{ project.description }}</p>
                <div class="op-tech">
                    <strong>TECH DEPLOYED:</strong> {{ project.tech }}
                </div>
                <transition name="expand">
                    <div v-if="expandedProject === project.title" class="op-details">
                        <img :src="project.image" alt="Operation Screenshot" class="op-image">
                    </div>
                </transition>
              </div>
            </div>
          </div>

          <div v-if="activeTab === 'Service Record'" class="content-section">
              <h2 class="section-title">SERVICE RECORD</h2>
              <div class="record-list">
                  <div class="record-item" v-for="exp in experience" :key="exp.title">
                      <div class="record-info">
                          <h3 class="record-title">{{ exp.title }}</h3>
                          <p class="record-meta">{{ exp.company }}</p>
                      </div>
                      <div class="record-duration">{{ exp.duration }}</div>
                  </div>
              </div>
          </div>

          <div v-if="activeTab === 'Certifications'" class="content-section">
              <h2 class="section-title">AWARDS & COMMENDATIONS</h2>
              <div class="cert-list">
                  <div class="cert-item" v-for="cert in certifications" :key="cert.name">
                      <div class="cert-info">
                          <h3 class="cert-title">{{ cert.name }}</h3>
                          <p class="cert-issuer">{{ cert.issuer }}</p>
                      </div>
                      <a :href="cert.url" target="_blank" class="action-button cert-button">VIEW CREDENTIAL</a>
                  </div>
              </div>
          </div>


          <div v-if="activeTab === 'Comms'" class="content-section">
                <h2 class="section-title">ESTABLISH COMMS</h2>
                <form @submit.prevent="handleFormSubmit" class="contact-form">
                    <div class="form-group">
                        <label for="callsign">YOUR CALLSIGN (NAME)</label>
                        <input type="text" id="callsign" v-model="formData.name" required class="form-input">
                    </div>
                     <div class="form-group">
                        <label for="email">YOUR EMAIL</label>
                        <input type="email" id="email" v-model="formData.email" required class="form-input">
                    </div>
                     <div class="form-group">
                        <label for="message">MESSAGE</label>
                        <textarea id="message" v-model="formData.message" required rows="5" class="form-textarea"></textarea>
                    </div>
                    <div class="form-footer">
                        <button type="submit" class="action-button primary" :disabled="formStatus.isSending">
                            {{ formStatus.isSending ? 'TRANSMITTING...' : 'SEND MESSAGE' }}
                        </button>
                        <p v-if="formStatus.message" class="form-status" :class="{ 'success': formStatus.isSuccess, 'error': !formStatus.isSuccess }">
                            {{ formStatus.message }}
                        </p>
                    </div>
                </form>
           </div>

        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue';

// --- Data Store ---
const name = ref('Gautam Sharma');
const profession = ref('Data Science Operative | ML Specialist');
const profilePictureUrl = ref('/profile.jpg');
const bio = ref(`A highly motivated and analytical operative with field experience in leveraging predictive modeling and advanced analytics to resolve complex strategic challenges. Specializes in the application of machine learning algorithms to extract actionable intelligence and drive mission success.`);
const resumeUrl = ref('Gautam-Sharma-Resume.pdf');
const projects = ref([
  {
    title: 'Operation Credit Score',
    tech: 'MERN Stack, Jenkins, Docker',
    description: "Deployed a containerized MERN stack application to generate real-time credit scores using a Random Forest Regressor model with CI/CD pipelines.",
    image: '/KYCS.png'
  },
  {
    title: 'Operation Ant Colony',
    tech: 'Python, networkx',
    description: "Executed a simulation of the Ant Colony Optimization (ACO) algorithm for the Traveling Salesperson Problem (TSP), developing a novel clustering method from the results.",
    image: '/ACO.png'
  }
]);
const skills = ref({
    'Primary Weapons': ['Python', 'SQL', 'R'],
    'Tactical Gear': ['NumPy', 'Pandas', 'TensorFlow', 'Scikit-learn', 'Flask'],
    'Intel Platforms': ['PowerBI', 'Docker', 'Jenkins', 'GitHub', 'Kaggle']
});
const experience = ref([
    {
        title: 'Marketing Analytics Intern',
        company: 'Super Collections',
        duration: 'June 2025 - Aug 2025',
    },
    {
        title: 'Learning Assistant, Digital Marketing',
        company: 'COEET, NIIT University',
        duration: 'Aug 2024 - Dec 2024',
    }
]);
const socials = ref([
    { name: 'Intel Profile (LinkedIn)', url: 'https://www.linkedin.com/in/gautam-sharma-87007029a/' },
    { name: 'Code Repository (GitHub)', url: 'https://github.com/gautam343' }
]);
const certifications = ref([
    { name: 'CIIE Volunteer', issuer: 'NIIT University', url: 'CIIE certificate .pdf' },
    { name: 'Presentation at ICETESS-2025', issuer: 'Jaipur Engineering College', url: 'RnD Certificate .pdf' },
    { name: 'Learning Assistant', issuer: 'NIIT University', url: 'Coeet Certificate .pdf' },
    { name: 'Complete TensorFlow Course', issuer: 'GeeksforGeeks', url: 'Tensor.pdf' },
    { name: 'Mastering Generative AI', issuer: 'GeeksforGeeks', url: 'GenAI.pdf' }
]);


// --- UI Logic ---
const currentTime = ref('');
const activeTab = ref('Identification');
const tabs = ['Identification', 'Skills', 'Operations', 'Service Record', 'Certifications', 'Comms'];
const expandedProject = ref(null);

const updateTime = () => {
    const now = new Date();
    currentTime.value = now.toLocaleTimeString('en-US', { hour12: false, hour: '2-digit', minute: '2-digit', second: '2-digit' });
};

const formatSkillCategory = (category) => {
  return category.replace(/_/g, ' ').toUpperCase();
};

const toggleProject = (projectTitle) => {
    if (expandedProject.value === projectTitle) {
        expandedProject.value = null;
    } else {
        expandedProject.value = projectTitle;
    }
};

// --- Form Submission Logic ---
const formspreeEndpoint = 'https://formspree.io/f/xnnzyvpo';

const formData = reactive({ name: '', email: '', message: '' });
const formStatus = reactive({ isSending: false, isSuccess: false, message: '' });

async function handleFormSubmit() {
    formStatus.isSending = true;
    formStatus.message = '';
    try {
        const response = await fetch(formspreeEndpoint, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(formData)
        });
        if (response.ok) {
            formStatus.isSuccess = true;
            formStatus.message = 'TRANSMISSION SUCCESSFUL.';
            formData.name = ''; formData.email = ''; formData.message = '';
        } else { throw new Error('Network response was not ok.'); }
    } catch (error) {
        formStatus.isSuccess = false;
        formStatus.message = 'TRANSMISSION FAILED. TRY AGAIN.';
    } finally {
        formStatus.isSending = false;
        setTimeout(() => { formStatus.message = '' }, 5000);
    }
}


onMounted(() => {
  updateTime();
  setInterval(updateTime, 1000);
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) { entry.target.classList.add('is-visible'); }
    });
  }, { threshold: 0.1 });
  document.querySelectorAll('[data-aos]').forEach(el => { observer.observe(el); });
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@400;700&family=Teko:wght@600&display=swap');

.mw-container {
  --bg-color: #01020a;
  --primary-accent: #00FF41;
  --text-primary: #EAEAEA;
  --text-secondary: #8A9A9A;
  --border-color: rgba(0, 255, 65, 0.2);
  --panel-bg: rgba(1, 2, 10, 0.7);
  --font-heading: 'Teko', sans-serif;
  --font-body: 'Roboto Mono', monospace;

  background-color: var(--bg-color);
  color: var(--text-primary);
  font-family: var(--font-body);
  min-height: 100vh;
  overflow-x: hidden;
  position: relative;
  padding: 2rem;
  box-sizing: border-box;
}

@keyframes scanline { 0% { transform: translateY(0); } 100% { transform: translateY(100vh); } }
.scanline-effect {
  position: fixed; top: -10vh; left: 0; width: 100%; height: 10vh;
  background: linear-gradient(to bottom, rgba(0,255,65,0) 0%, rgba(0,255,65,0.05) 50%, rgba(0,255,65,0) 100%);
  animation: scanline 6s linear infinite; z-index: 1; pointer-events: none;
}
.hud-border { position: fixed; top: 1rem; right: 1rem; bottom: 1rem; left: 1rem; border: 1px solid var(--border-color); pointer-events: none; z-index: 10; }
.vignette { position: fixed; top: 0; left: 0; width: 100%; height: 100%; box-shadow: inset 0 0 150px rgba(0,0,0,0.8); pointer-events: none; z-index: 2; }
.hud-header { display: flex; justify-content: space-between; align-items: center; position: absolute; top: 1rem; left: 1rem; right: 1rem; padding: 0.5rem 1rem; font-family: var(--font-heading); letter-spacing: 2px; font-size: 1.2rem; z-index: 20; }
.status-active { color: var(--primary-accent); }
.header-right { display: flex; gap: 2rem; }
.dossier { display: grid; grid-template-columns: 1fr; gap: 2rem; margin-top: 5rem; position: relative; z-index: 5; }
@media (min-width: 1024px) { .dossier { grid-template-columns: 400px 1fr; } }

.operative-id-panel { background: var(--panel-bg); backdrop-filter: blur(10px); border: 1px solid var(--border-color); padding: 2rem; text-align: center; }
.operative-photo {
  width: 120px; height: 120px; margin: 0 auto 1.5rem auto; border: 2px solid var(--primary-accent);
  background: rgba(0, 255, 65, 0.1); padding: 5px; box-sizing: border-box;
}
.operative-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.operative-name { font-family: var(--font-heading); font-size: 3rem; letter-spacing: 2px; margin: 0; }
.operative-title { color: var(--text-secondary); font-size: 1rem; margin-bottom: 2rem; }
.operative-links { display: flex; flex-direction: column; gap: 1rem; margin-bottom: 2rem; }
.link-button { border: 1px solid var(--border-color); padding: 0.75rem; color: var(--text-secondary); text-decoration: none; transition: all 0.3s ease; }
.link-button:hover { background-color: var(--primary-accent); color: var(--bg-color); border-color: var(--primary-accent); }
.contact-actions { display: flex; flex-direction: column; gap: 1rem; }
.action-button { padding: 1rem; font-family: var(--font-heading); font-size: 1.2rem; letter-spacing: 1px; text-decoration: none; background: transparent; border: 1px solid var(--primary-accent); color: var(--primary-accent); transition: all 0.3s ease; cursor: pointer; }
.action-button.primary { background: var(--primary-accent); color: var(--bg-color); }
.action-button:hover { filter: brightness(1.2); }
.action-button:disabled { filter: grayscale(1); cursor: not-allowed; background-color: var(--text-secondary); border-color: var(--text-secondary); color: var(--bg-color); }

.tabs-panel { background: var(--panel-bg); backdrop-filter: blur(10px); border: 1px solid var(--border-color); }
.tabs-nav { display: flex; border-bottom: 1px solid var(--border-color); flex-wrap: wrap; }
.tab-button { flex: 1 1 auto; padding: 1rem; background: none; border: none; border-right: 1px solid var(--border-color); color: var(--text-secondary); font-family: var(--font-heading); font-size: 1.2rem; letter-spacing: 2px; cursor: pointer; transition: all 0.3s ease; }
.tab-button:last-child { border-right: none; }
.tab-button:hover, .tab-button.active { background-color: var(--primary-accent); color: var(--bg-color); }
.tab-content { padding: 2rem; }
.section-title { font-family: var(--font-heading); font-size: 1.8rem; letter-spacing: 1px; color: var(--primary-accent); margin-bottom: 1.5rem; }
.bio { line-height: 1.8; color: var(--text-secondary); }

.skill-category { margin-bottom: 2rem; }
.skill-category-title { font-family: var(--font-heading); font-size: 1.2rem; letter-spacing: 1px; border-bottom: 1px solid var(--border-color); padding-bottom: 0.5rem; margin-bottom: 1rem; }
.skill-grid { display: flex; flex-wrap: wrap; gap: 0.75rem; }
.skill-tag { background-color: rgba(0, 255, 65, 0.1); border: 1px solid var(--border-color); padding: 0.5rem 1rem; }

.op-list { display: flex; flex-direction: column; gap: 1rem; }
.op-item { border: 1px solid var(--border-color); padding: 1.5rem; cursor: pointer; transition: background-color 0.3s ease; }
.op-item.expanded, .op-item:hover { background-color: rgba(0, 255, 65, 0.05); }
.op-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 0.5rem; }
.op-title { font-size: 1.2rem; margin: 0; font-weight: 700; color: var(--text-primary); }
.op-status { background-color: var(--primary-accent); color: var(--bg-color); font-size: 0.8rem; padding: 0.25rem 0.5rem; }
.op-desc { color: var(--text-secondary); margin-bottom: 1rem; }
.op-tech { font-size: 0.9rem; }

.op-details { margin-top: 1.5rem; padding-top: 1.5rem; border-top: 1px solid var(--border-color); }
.op-image { width: 100%; display: block; border: 1px solid var(--border-color); }
.expand-enter-active, .expand-leave-active { transition: all 0.5s ease-in-out; max-height: 500px; opacity: 1; }
.expand-enter-from, .expand-leave-to { max-height: 0; opacity: 0; margin-top: 0; padding-top: 0; overflow: hidden; }

.record-list { display: flex; flex-direction: column; }
.record-item { display: flex; justify-content: space-between; align-items: center; padding: 1rem; border-bottom: 1px solid var(--border-color); }
.record-item:last-child { border-bottom: none; }
.record-title { font-size: 1.1rem; font-weight: 700; margin: 0; }
.record-meta { color: var(--text-secondary); }
.record-duration { font-family: var(--font-heading); font-size: 1.5rem; color: var(--primary-accent); }

.cert-list { display: flex; flex-direction: column; gap: 1.5rem; }
.cert-item { display: flex; flex-direction: column; align-items: flex-start; gap: 1rem; padding: 1rem; border: 1px solid var(--border-color); }
@media(min-width: 768px) { .cert-item { flex-direction: row; justify-content: space-between; align-items: center; } }
.cert-title { font-size: 1.1rem; font-weight: 700; margin: 0; }
.cert-issuer { color: var(--text-secondary); margin: 0; }
.cert-button { padding: 0.5rem 1rem; font-size: 1rem; }

.contact-form { display: flex; flex-direction: column; gap: 1.5rem; }
.form-group { display: flex; flex-direction: column; }
.form-group label { font-family: var(--font-heading); font-size: 1rem; letter-spacing: 1px; color: var(--text-secondary); margin-bottom: 0.5rem; }
.form-input, .form-textarea { background-color: rgba(0, 255, 65, 0.05); border: 1px solid var(--border-color); color: var(--text-primary); padding: 0.75rem; font-family: var(--font-body); font-size: 1rem; }
.form-input:focus, .form-textarea:focus { outline: none; border-color: var(--primary-accent); }
.form-footer { display: flex; align-items: center; gap: 1.5rem; margin-top: 1rem; }
.form-status { font-size: 0.9rem; font-weight: 700; }
.form-status.success { color: var(--primary-accent); }
.form-status.error { color: #f7768e; }

[data-aos] { opacity: 0; transition: transform 0.8s ease-out, opacity 0.8s ease-out; }
[data-aos="fade-right"] { transform: translateX(-50px); }
[data-aos="fade-left"] { transform: translateX(50px); }
[data-aos].is-visible { opacity: 1; transform: translateX(0); }
</style>