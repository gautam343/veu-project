<template>
  <div class="app-container" :class="isDarkMode ? 'dark-theme' : 'light-theme'">
    <div class="stars" id="stars1"></div>
    <div class="stars" id="stars2"></div>
    <div class="stars" id="stars3"></div>

    <header class="header">
      <nav class="navbar">
        <a href="#home" class="nav-logo mono-font">{{ name }}</a>
        <div class="nav-controls">
          <ul class="nav-menu">
            <li><a href="#home" class="nav-link">/home</a></li>
            <li><a href="#experience" class="nav-link">/experience</a></li>
            <li><a href="#projects" class="nav-link">/projects</a></li>
            <li><a href="#skills" class="nav-link">/skills</a></li>
            <li><a href="#PoR" class="nav-link">/PoR</a></li>
            <li><a href="#certificates" class="nav-link">/certificates</a></li>
            <li><a href="#resume" class="nav-link">/resume</a></li>
            <li><a href="#contact" class="nav-link">/contact</a></li>
          </ul>
          <button @click="toggleTheme" class="theme-switcher" aria-label="Toggle Theme">
            <i class="icon icon-sun"></i>
            <i class="icon icon-moon"></i>
          </button>
        </div>
      </nav>
    </header>

    <main>
      <section id="home" class="hero-section">
        <div class="hero-main-content">
          <div class="hero-text-content" data-aos="fade-right">
            <p class="subtitle mono-font">Hi, my name is</p>
            <h1 class="main-title">{{ name }}</h1>
            <h2 class="subtitle-profession">{{ profession }}</h2>
            <p class="panel-body bio">{{ bio }}</p>
              <div class="social-links-hero">
                  <a v-for="link in socials" :key="link.name" :href="link.url" target="_blank" class="social-icon-link">
                    <i :class="'icon icon-' + link.name.toLowerCase()"></i>
                  </a>
              </div>
          </div>
          <div class="profile-picture-container" data-aos="fade-left">
            <img :src="profilePictureUrl" alt="Profile Picture" class="profile-picture" />
          </div>
        </div>
      </section>

      <section id="experience" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-briefcase"></i><span>Work Experience</span></h2>
        <div class="timeline">
            <div class="timeline-item" v-for="exp in experience" :key="exp.title">
                <div class="timeline-dot"></div>
                <h3>{{ exp.title }}</h3>
                <p class="timeline-meta">{{ exp.company }} | {{ exp.duration }}</p>
                <ul class="experience-list">
                    <li v-for="point in exp.points" :key="point">{{ point }}</li>
                </ul>
            </div>
        </div>
      </section>
      
      <section id="projects" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-grid"></i><span>Project Archives</span></h2>
        <div class="project-grid">
          <div v-for="project in projects" :key="project.id" class="project-card">
            <div class="project-image-container">
                <img :src="project.image" :alt="project.title + ' preview'" class="project-image"/>
            </div>
            <div class="project-info">
              <h3>{{ project.title }}</h3>
              <p>{{ project.description }}</p>
              <div class="project-tech-tags">
                <span v-for="tech in project.tech.split(', ')" :key="tech" class="tech-tag">{{ tech }}</span>
              </div>
            </div>
          </div>
        </div>
      </section>
      
      <section id="skills" class="content-panel" data-aos="fade-up">
          <h2 class="panel-title mono-font"><i class="icon icon-code"></i><span>Technical Skills</span></h2>
          <div class="skills-grid">
              <div class="skill-category">
                  <h4><i class="icon icon-speech-bubble"></i> Languages</h4>
                  <div class="skill-tabs">
                    <span v-for="skill in skills.languages" :key="skill" class="skill-tab">{{ skill }}</span>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-layers"></i> Frameworks & Libraries</h4>
                  <div class="skill-tabs">
                    <span v-for="skill in skills.frameworks" :key="skill" class="skill-tab">{{ skill }}</span>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-database"></i> Databases</h4>
                  <div class="skill-tabs">
                    <span v-for="skill in skills.databases" :key="skill" class="skill-tab">{{ skill }}</span>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-tool"></i> Tools & Platforms</h4>
                  <div class="skill-tabs">
                    <span v-for="skill in skills.tools_platforms" :key="skill" class="skill-tab">{{ skill }}</span>
                  </div>
              </div>
          </div>
      </section>

      <section id="PoR" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-users"></i><span>Positions of Responsibility</span></h2>
        <div class="timeline">
            <div v-for="pos in pOR" :key="pos.title" class="timeline-item">
                <div class="timeline-dot"></div>
                <h3>{{ pos.title }}</h3>
                <p class="timeline-meta">{{ pos.organization }}</p>
                <ul class="experience-list">
                    <li v-for="point in pos.points" :key="point">{{ point }}</li>
                </ul>
            </div>
        </div>
      </section>
      
      <section id="certificates" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-certificate"></i><span>Certificates</span></h2>
        <div class="certificate-list">
          <div v-for="cert in certifications" :key="cert.name" class="certificate-item">
            <div class="cert-header">
              <i class="icon icon-award"></i>
              <div class="cert-info">
                <h3>{{ cert.name }}</h3>
                <p>{{ cert.issuer }}</p>
              </div>
            </div>
            <a v-if="cert.url && cert.url !== '#'" :href="cert.url" target="_blank" class="cta-button" style="display: inline-block; margin-top: 1rem;">View Credential</a>
          </div>
        </div>
      </section>

      <section id="resume" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-book"></i><span>Resume</span></h2>
        <div class="certificate-item">
          <div class="cert-header">
            <i class="icon icon-book"></i>
            <div class="cert-info">
              <h3>My Professional Resume</h3>
              <p>{{ name }}</p>
            </div>
          </div>
          <a :href="resumeUrl" target="_blank" class="cta-button" style="display: inline-block; margin-top: 1rem;">View Resume</a>
        </div>
      </section>
      
      <section id="education" class="content-panel" data-aos="fade-up">
          <h2 class="panel-title mono-font"><i class="icon icon-book"></i><span>Education</span></h2>
          <div class="timeline">
            <div class="timeline-item" v-for="edu in education" :key="edu.degree">
                <div class="timeline-dot"></div>
                <h3>{{ edu.degree }}</h3>
                <p class="timeline-meta">{{ edu.institution }}</p>
                <p class="education-details" v-if="edu.details">{{ edu.details }}</p>
            </div>
          </div>
      </section>

      <section id="contact" class="content-panel text-center" data-aos="fade-up">
        <h2 class="panel-title mono-font justify-center"><span>What's Next?</span></h2>
        <h3>Get In Touch</h3>
        <p class="panel-body">Available for new opportunities and innovative challenges. Let's connect.</p>
        <div class="contact-links">
          <a :href="'mailto:' + email" class="cta-button">
            Say Hello
          </a>
        </div>
      </section>
    </main>

    <footer class="footer mono-font">
      <p>&copy; 2025 | Designed & Built by Gautam Sharma</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';

// --- THEME SWITCH LOGIC --- //
const isDarkMode = ref(true);
const toggleTheme = () => { isDarkMode.value = !isDarkMode.value; };

// --- YOUR PERSONALIZED DETAILS (MODIFIED) --- //
const name = ref('Gautam Sharma');
const profession = ref('Data Science Student | Machine Learning Enthusiast');
const profilePictureUrl = ref('/profile.jpg');
const resumeUrl = ref('Gautam-Sharma-Resume.pdf');
const bio = ref(`I am an ambitious and result-driven individual with practical experience leveraging predictive modeling and analytics to solve complex business problems. Aims to apply a deep understanding of machine learning algorithms to deliver impactful insights and support data-driven business growth.`);
const email = ref('gautam.sharma22@st.niituniversity.in');
const experience = ref([
    {
        title: 'Marketing Analytics Intern',
        company: 'Super Collections, Jhunjhunu/Remote',
        duration: 'June 2025 - Aug 2025',
        points: [
            'Worked on optimisation of online selling portals via data analytics to provide them with visualization dashboards.',
            'Managed the online presence of the company and produced dashboards for data visualization.'
        ]
    },
    {
        title: 'Learning Assistant, Digital Marketing and Storytelling Desk',
        company: 'Center of Excellence in Education Technology (COEET), NIIT University',
        duration: 'Aug 2024 - Dec 2024',
        points: [
            'Mentored and guided teams to include storytelling in their projects.',
            'Handled Instagram / LinkedIn and the organization website for all core operations.'
        ]
    }
]);
const projects = ref([
  { 
    id: 1, 
    title: 'Automated Credit Scoring System using ML',
    tech: 'MERN Stack, Jenkins, Docker',
    description: "Deployed a containerized MERN stack application to generate real-time credit scores using a Random Forest Regressor model. Integrated CI/CD pipelines with Jenkins for automated testing and seamless deployment.",
    image: '/KYCS.png'
  },
  {
    id: 2,
    title: 'ACO and Clustering',
    tech: 'Python',
    description: "Used networkx for graph simulation and implemented a custom Ant Colony Optimization (ACO) algorithm for the Traveling Salesperson Problem (TSP). Developed a clustering method based on analyzing graph edge structures derived from ACO results.",
    image: '/ACO.png'
  }
]);
const skills = ref({
    languages: ['Python', 'SQL', 'R', 'Java', 'JavaScript'],
    frameworks: ['NumPy', 'Pandas', 'Matplotlib', 'TensorFlow', 'Scikit-learn', 'Hadoop', 'Flask'],
    databases: ['MongoDB', 'MySQL'],
    tools_platforms: ['PowerBI', 'Docker', 'Jenkins', 'Canva', 'VS Code', 'GitHub', 'Kaggle', 'Google Colab', 'Asana', 'Jira', 'MS Office']
});
const pOR = ref([
    {
      title: 'CIIE (Centre of Incubation and Innovation) Lead',
      organization: 'NIIT University',
      points: [
        'Organized EdTech Growth Camp Event April 2025, managing and supporting tech startup founders.'
      ]
    }
]);
const education = ref([
    { degree: 'Bachelor of Technology in Computer Science', institution: 'NIIT University, Rajasthan, India', details: 'CGPA: 8.23' },
    { degree: 'XII (CBSE)', institution: 'S.S Mody Vidya Vihar, Jhunjhunu', details: '2021' },
    { degree: 'X (CBSE)', institution: 'S.S Mody Vidya Vihar, Jhunjhunu', details: '2019' }
]);
const certifications = ref([
    { name: 'Certificate of Appreciation - CIIE Volunteer', issuer: 'NIIT University', url: 'CIIE certificate .pdf' },
    { name: 'Certificate of Presentation - ICETESS-2025', issuer: 'Jaipur Engineering College and Research Centre', url: 'RnD Certificate .pdf' },
    { name: 'Certificate of Achievement - Learning Assistant', issuer: 'NIIT University', url: 'Coeet Certificate .pdf' },
    { name: 'Complete TensorFlow Course', issuer: 'GeeksforGeeks', url: 'Tensor.pdf' },
    { name: 'Mastering Generative AI and ChatGPT', issuer: 'GeeksforGeeks', url: 'GenAI.pdf' }
]);
const socials = ref([
    { name: 'LinkedIn', url: 'https://www.linkedin.com/in/gautam-sharma-87007029a/' }, 
    { name: 'GitHub', url: 'https://github.com/gautam343' }
]);

// --- SCROLL ANIMATION LOGIC --- //
onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
      }
    });
  }, { threshold: 0.1 });
  nextTick(() => {
    document.querySelectorAll('[data-aos]').forEach(el => {
      observer.observe(el);
    });
  });
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;700&display=swap');

/* === THEME & COLOR VARIABLES === */
:root {
  --main-font: 'Inter', sans-serif;
  --mono-font: 'JetBrains Mono', monospace;
  --transition-speed: 0.3s;
  --border-radius-sm: 8px;
  --border-radius-md: 16px;
}
.dark-theme {
  --bg-color: #0A0F1D;
  --panel-bg-color: #101626;
  --text-primary: #E2E8F0;
  --text-secondary: #94A3B8;
  --accent-primary: #38BDF8; /* Light Blue */
  --accent-secondary: #F472B6; /* Pink */
  --border-color: rgba(56, 189, 248, 0.15);
  --shadow-color: rgba(0, 0, 0, 0.2);
}
.light-theme {
  --bg-color: #F8F9FA;
  --panel-bg-color: #FFFFFF;
  --text-primary: #212529;
  --text-secondary: #6C757D;
  --accent-primary: #007BFF;
  --accent-secondary: #E83E8C;
  --border-color: #DEE2E6;
  --shadow-color: rgba(0, 0, 0, 0.08);
}

/* === DYNAMIC BACKGROUNDS === */
@keyframes move-stars { from { transform: translateY(0); } to { transform: translateY(-2000px); } }
.stars { position: fixed; top: 0; left: 0; right: 0; width: 1px; height: 2000px; background: transparent; box-shadow: 737px 1569px #FFF, 186px 1599px #FFF; animation: move-stars 150s linear infinite; z-index: -10;}
#stars2 { animation-duration: 100s; }
#stars3 { animation-duration: 50s; }
.light-theme .stars { display: none; }

/* === GLOBAL STYLES === */
.app-container {
  background-color: var(--bg-color); color: var(--text-primary);
  font-family: var(--main-font); min-height: 100vh;
  transition: background-color var(--transition-speed) ease, color var(--transition-speed) ease;
  overflow-x: hidden;
}
main { max-width: 900px; margin: 0 auto; padding: 0 1.5rem; }
h3 { color: var(--text-primary); font-weight: 600; font-size: 1.25rem; margin-bottom: 0.5rem;}

/* === HEADER & NAVIGATION === */
.header {
  position: sticky; top: 0; z-index: 100; padding: 1.5rem 5%;
  background-color: rgba(10, 15, 29, 0.5); backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--border-color);
  transition: all var(--transition-speed) ease;
}
.navbar { display: flex; justify-content: space-between; align-items: center; max-width: 1200px; margin: 0 auto; }
.nav-logo { color: var(--text-primary); text-decoration: none; font-size: 1.2rem; font-weight: 700; }
.nav-controls { display: flex; align-items: center; gap: 1.5rem; }
.nav-menu { display: none; }
@media (min-width: 1200px) { .nav-menu { display: flex; list-style: none; gap: 1.5rem;} }
.nav-link { color: var(--text-secondary); text-decoration: none; transition: color 0.3s ease; font-size: 0.95rem; }
.nav-link:hover { color: var(--accent-primary); }
.theme-switcher { background: transparent; border: 1px solid var(--border-color); width: 50px; height: 26px; border-radius: 13px; cursor: pointer; display: flex; align-items: center; position: relative; transition: all var(--transition-speed) ease;}
.icon-sun, .icon-moon { position: absolute; top: 3px; font-size: 16px; transition: all var(--transition-speed) ease; }
.icon-sun { left: 5px; opacity: 0; background-color: #f39c12; }
.icon-moon { left: 28px; opacity: 1; background-color: #f1c40f; }
.dark-theme .icon-sun { opacity: 1; }
.dark-theme .icon-moon { opacity: 0; }

/* === HERO SECTION === */
.hero-section { display: flex; flex-direction: column; justify-content: center; min-height: 90vh; padding: 6rem 0; }
.hero-main-content { display: flex; flex-direction: column-reverse; align-items: center; gap: 2rem; }
@media (min-width: 768px) { .hero-main-content { flex-direction: row; text-align: left; gap: 4rem; } }
.hero-text-content { flex: 1; }
.main-title {
  font-size: clamp(3rem, 8vw, 5rem); font-weight: 800; line-height: 1.1;
  background: linear-gradient(90deg, var(--accent-primary), var(--accent-secondary), var(--accent-primary));
  background-size: 200% auto; background-clip: text;
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  animation: gradient-flow 6s linear infinite;
}
@keyframes gradient-flow { to { background-position: 200% center; } }
.subtitle { color: var(--accent-primary); font-size: 1rem; margin-bottom: 1rem;}
.subtitle-profession { font-size: clamp(1.5rem, 4vw, 2rem); color: var(--text-secondary); font-weight: 600; margin-bottom: 1.5rem;}
.bio { max-width: 60ch; margin-bottom: 2rem; line-height: 1.7; }
.profile-picture-container { flex-shrink: 0; position: relative; }
.profile-picture {
    width: 240px; height: 240px; border-radius: 50%; object-fit: cover;
    border: 3px solid var(--border-color); padding: 6px; background-color: var(--bg-color);
    box-shadow: 0 10px 30px var(--shadow-color);
    transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.profile-picture:hover { transform: scale(1.05) rotate(2deg); }
.social-links-hero { display: flex; gap: 1.5rem; margin-top: 2rem;}
.social-icon-link { color: var(--text-secondary); transition: all 0.3s ease; }
.social-icon-link:hover { color: var(--accent-primary); transform: translateY(-3px); }

/* === CONTENT PANELS === */
.content-panel {
  padding: 3rem; margin: 8rem 0; background-color: var(--panel-bg-color);
  border: 1px solid var(--border-color); border-radius: var(--border-radius-md);
  box-shadow: 0 4px 20px var(--shadow-color); transition: all var(--transition-speed) ease;
  position: relative;
}
.content-panel.text-center { text-align: center; }
.panel-title { font-size: 1.1rem; font-weight: 500; color: var(--text-primary); margin-bottom: 2.5rem; display: flex; align-items: center; gap: 0.75rem; letter-spacing: 1px; }
.panel-title span { background: var(--panel-bg-color); padding-right: 1rem; }
.panel-title.justify-center { justify-content: center; }
.panel-body { color: var(--text-secondary); line-height: 1.8; font-size: 1.05rem; }

/* === TIMELINE STYLE === */
.timeline { position: relative; padding-left: 2rem; border-left: 2px solid var(--border-color); }
.timeline-item { position: relative; margin-bottom: 2.5rem; }
.timeline-item:last-child { margin-bottom: 0; }
.timeline-dot { position: absolute; left: -2.75rem; top: 5px; width: 16px; height: 16px; border-radius: 50%; background-color: var(--accent-primary); border: 4px solid var(--panel-bg-color); box-shadow: 0 0 10px var(--accent-primary);}
.timeline-meta { color: var(--text-secondary); font-size: 0.9rem; margin-bottom: 0.75rem; }
.experience-list { list-style-type: none; padding-left: 0; margin-top: 1rem; }
.experience-list li { color: var(--text-secondary); margin-bottom: 0.5rem; position: relative; padding-left: 1.5rem; }
.experience-list li::before { content: '›'; position: absolute; left: 0; color: var(--accent-primary); font-weight: 700; font-size: 1.2rem;}
.education-details { font-style: italic; font-size: 0.9rem; color: var(--text-secondary); margin-top: 0.5rem; }

/* === PROJECTS & SKILLS === */
.project-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1.5rem; }
.project-card {
  background-color: var(--bg-color); border: 1px solid var(--border-color);
  border-radius: var(--border-radius-md); overflow: hidden;
  transition: all 0.3s ease; display: flex; flex-direction: column;
}
.project-card:hover { transform: translateY(-8px); border-color: var(--accent-primary); box-shadow: 0 8px 30px var(--shadow-color);}
.project-card:hover .project-image { transform: scale(1.05); }
.project-image-container { width: 100%; height: 180px; overflow: hidden; }
.project-image { width: 100%; height: 100%; object-fit: cover; transition: transform 0.4s ease; }
.project-info { padding: 1.5rem; flex-grow: 1; display: flex; flex-direction: column;}
.project-info h3 { margin-bottom: 0.75rem; }
.project-info p { font-size: 0.95rem; color: var(--text-secondary); margin-bottom: 1rem; flex-grow: 1;}
.project-tech-tags { display: flex; flex-wrap: wrap; gap: 0.5rem; margin-top: auto; }
.tech-tag { background-color: rgba(56, 189, 248, 0.1); color: var(--accent-primary); padding: 0.25rem 0.75rem; border-radius: 999px; font-family: var(--mono-font); font-size: 0.75rem;}
.skills-grid { display: grid; grid-template-columns: 1fr; gap: 2.5rem; }
@media (min-width: 768px) { .skills-grid { grid-template-columns: repeat(2, 1fr); } }
.skill-category h4 { color: var(--text-primary); margin-bottom: 1rem; display: flex; align-items: center; gap: 0.5rem; font-weight: 500;}
.skill-tabs { display: flex; flex-wrap: wrap; gap: 0.75rem; }
.skill-tab { background-color: var(--panel-bg-color); border: 1px solid var(--border-color); color: var(--text-secondary); padding: 0.5rem 1rem; border-radius: var(--border-radius-sm); transition: all 0.3s ease; font-size: 0.9rem;}
.skill-tab:hover { color: var(--text-primary); border-color: var(--accent-primary); background-color: var(--bg-color);}

/* === CERTIFICATES & RESUME === */
.certificate-list { display: flex; flex-direction: column; gap: 3rem; }
.certificate-item { padding: 1.5rem; border: 1px solid var(--border-color); border-radius: var(--border-radius-md); background-color: var(--bg-color); transition: all 0.3s ease;}
.cert-header { display: flex; align-items: center; gap: 1rem; margin-bottom: 1.5rem; }
.cert-info h3 { font-size: 1.1rem; color: var(--text-primary); margin: 0; }
.cert-info p { font-size: 0.9rem; color: var(--text-secondary); margin: 0; margin-top: 0.25rem;}

/* === CONTACT SECTION === */
#contact h3 { font-size: 2rem; margin-bottom: 1rem;}
#contact .panel-body { max-width: 65ch; margin: 0 auto;}
.cta-button {
  background: var(--accent-primary); color: var(--bg-color);
  padding: 14px 32px; border: none; font-size: 1rem;
  border-radius: var(--border-radius-sm); text-decoration: none;
  font-weight: 600; transition: all 0.3s ease;
  box-shadow: 0 4px 15px var(--shadow-color), 0 0 20px rgba(56, 189, 248, 0.3);
}
.cta-button:hover { transform: translateY(-3px); box-shadow: 0 7px 20px rgba(56, 189, 248, 0.4), 0 0 30px rgba(56, 189, 248, 0.5); }
.contact-links { display: flex; justify-content: center; gap: 1rem; margin-top: 2.5rem;}

/* === FOOTER === */
.footer { text-align: center; padding: 4rem 1.5rem 2rem; color: var(--text-secondary); font-size: 0.9rem; }

/* === ANIMATIONS & ICONS === */
[data-aos] { opacity: 0; transition: transform 0.8s ease-out, opacity 0.8s ease-out; }
[data-aos="fade-up"].is-visible { opacity: 1; transform: translateY(0); }
[data-aos="fade-right"].is-visible { opacity: 1; transform: translateX(0); }
[data-aos="fade-left"].is-visible { opacity: 1; transform: translateX(0); }
[data-aos="fade-up"] { transform: translateY(40px); }
[data-aos="fade-right"] { transform: translateX(-40px); }
[data-aos="fade-left"] { transform: translateX(40px); }

.icon { display: inline-block; width: 20px; height: 20px; background-color: var(--text-secondary); mask-repeat: no-repeat; mask-size: contain; mask-position: center; transition: background-color var(--transition-speed) ease;}
.social-icon-link .icon { background-color: currentColor; }
.panel-title .icon { background-color: var(--accent-primary); }
.skill-category .icon, .cert-header .icon { color: var(--accent-primary); background-color: var(--accent-primary); }
.icon-sun { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M12 7c-2.76 0-5 2.24-5 5s2.24 5 5 5 5-2.24 5-5-2.24-5-5-5zM2 13h2c.55 0 1-.45 1-1s-.45-1-1-1H2c-.55 0-1 .45-1 1s.45 1 1 1zm18 0h2c.55 0 1-.45 1-1s-.45-1-1-1h-2c-.55 0-1 .45-1 1s.45 1 1 1zM11 2v2c0 .55.45 1 1 1s1-.45 1-1V2c0-.55-.45-1-1-1s-1 .45-1 1zm0 18v2c0 .55.45 1 1 1s1-.45 1-1v-2c0-.55-.45-1-1-1s-1 .45-1 1zM5.64 5.64c-.39-.39-1.02-.39-1.41 0-.39.39-.39 1.02 0 1.41l1.06 1.06c.39.39 1.02.39 1.41 0s.39-1.02 0-1.41L5.64 5.64zM18.36 18.36c-.39-.39-1.02-.39-1.41 0-.39.39-.39 1.02 0 1.41l1.06 1.06c.39.39 1.02.39 1.41 0 .39-.39.39-1.02 0-1.41l-1.06-1.06zM18.36 5.64l-1.06 1.06c-.39.39-.39 1.02 0 1.41s1.02.39 1.41 0l1.06-1.06c.39-.39.39-1.02 0-1.41s-1.02-.39-1.41 0zM5.64 18.36l-1.06 1.06c-.39.39-.39 1.02 0 1.41s1.02.39 1.41 0l1.06-1.06c.39-.39.39-1.02 0-1.41s-1.03-.39-1.42 0z"/></svg>');}
.icon-moon { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M10 2c-1.82 0-3.53.5-5 1.35C7.99 5.08 10 8.3 10 12s-2.01 6.92-5 8.65C6.47 21.5 8.18 22 10 22c5.52 0 10-4.48 10-10S15.52 2 10 2z"/></svg>');}
.icon-users { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>');}
.icon-briefcase { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M20 6h-4V4c0-1.11-.89-2-2-2h-4c-1.11 0-2 .89-2 2v2H4c-1.11 0-2 .89-2 2v11c0 1.11.89 2 2 2h16c1.1 0 2-.89 2-2V8c0-1.11-.9-2-2-2zM10 4h4v2h-4V4zm10 15H4V8h16v11z"/></svg>');}
.icon-grid { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M4 4h6v6H4zm8 0h6v6h-6zm-8 8h6v6H4zm8 8h6v6h-6zm8-8h-6v-6h6z"/></svg>'); }
.icon-code { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M9.4 16.6L4.8 12l4.6-4.6L8 6l-6 6 6 6 1.4-1.4zm5.2 0l4.6-4.6-4.6-4.6L16 6l6 6-6 6-1.4-1.4z"/></svg>');}
.icon-book { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M18 2H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zM6 4h5v8l-2.5-1.5L6 12V4z"/></svg>');}
.icon-certificate { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2zm0 2.23L10.06 8.5 4.5 9.27l3.99 3.87L7.47 18.5 12 15.77l4.53 2.73-1.02-5.36 3.99-3.87-5.56-.77L12 4.23z"/></svg>');}
.icon-award { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 15l-3.5-3.5 1.41-1.41L11 13.17l5.59-5.59L18 9l-7 7z"/></svg>');}
.icon-mail { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>'); }
.icon-speech-bubble { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>');}
.icon-layers { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"></polygon><polyline points="2 17 12 22 22 17"></polyline><polyline points="2 12 12 17 22 12"></polyline></svg>');}
.icon-database { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"></ellipse><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"></path><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"></path></svg>');}
.icon-tool { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg>');}
.icon-linkedin { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>');}
.icon-github { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path></svg>');}

</style>