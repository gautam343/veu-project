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
            <li><a href="#about" class="nav-link">/about</a></li>
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
          <div class="about-me-panel" data-aos="fade-right">
            <h2 class="panel-title mono-font"><span>Objective</span></h2>
            <p class="panel-body">{{ bio }}</p>
          </div>
          <div class="profile-picture-container" data-aos="fade-left">
            <img :src="profilePictureUrl" alt="Profile Picture" class="profile-picture" />
          </div>
        </div>
        <div class="hero-name-section" data-aos="fade-up">
            <h1 class="main-title">{{ name }}</h1>
            <p class="subtitle mono-font">{{ profession }}</p>
        </div>
      </section>

      <section id="experience" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-briefcase"></i><span>Work Experience</span></h2>
        <div class="experience-item">
            <h3>{{ experience.title }}</h3>
            <p class="experience-company">{{ experience.company }} | {{ experience.duration }}</p>
            <ul class="experience-list">
                <li v-for="point in experience.points" :key="point">{{ point }}</li>
            </ul>
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
              <p class="project-tech"><strong>Tech Used:</strong> {{ project.tech }}</p>
              <p>{{ project.description }}</p>
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
                    <button v-for="skill in skills.languages" :key="skill" @click="toggleSkill(skill)" :class="{ active: activeSkill === skill }" class="skill-tab">{{ skill }}</button>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-layers"></i> Frameworks & Libraries</h4>
                  <div class="skill-tabs">
                    <button v-for="skill in skills.frameworks" :key="skill" @click="toggleSkill(skill)" :class="{ active: activeSkill === skill }" class="skill-tab">{{ skill }}</button>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-database"></i> Databases</h4>
                  <div class="skill-tabs">
                    <button v-for="skill in skills.databases" :key="skill" @click="toggleSkill(skill)" :class="{ active: activeSkill === skill }" class="skill-tab">{{ skill }}</button>
                  </div>
              </div>
              <div class="skill-category">
                  <h4><i class="icon icon-tool"></i> Tools & Platforms</h4>
                  <div class="skill-tabs">
                    <button v-for="skill in skills.tools_platforms" :key="skill" @click="toggleSkill(skill)" :class="{ active: activeSkill === skill }" class="skill-tab">{{ skill }}</button>
                  </div>
              </div>
          </div>
      </section>

      <section id="PoR" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-users"></i><span>Positions of Responsibility</span></h2>
        <div v-for="pos in pOR" :key="pos.title" class="experience-item">
            <h3>{{ pos.title }}</h3>
            <p class="experience-company">{{ pos.organization }}</p>
            <ul class="experience-list">
                <li v-for="point in pos.points" :key="point">{{ point }}</li>
            </ul>
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
            <embed v-if="cert.url.endsWith('.pdf')" :src="cert.url" type="application/pdf" width="100%" height="700px" class="pdf-embed" />
            <a v-else-if="cert.url !== '#'" :href="cert.url" target="_blank" class="cert-link-button">View Credential</a>
          </div>
        </div>
      </section>

      <section id="resume" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-book"></i><span>Resume</span></h2>
        <div class="certificate-item">
          <div class="cert-header">
            <i class="icon icon-award"></i>
            <div class="cert-info">
              <h3>Resume</h3>
              <p>{{ name }}</p>
            </div>
          </div>
          <embed v-if="resumeUrl.endsWith('.pdf')" :src="resumeUrl" type="application/pdf" width="100%" height="700px" class="pdf-embed" />
          <a v-else :href="resumeUrl" target="_blank" class="cert-link-button">View Resume</a>
        </div>
      </section>
      
      <section id="education" class="content-panel" data-aos="fade-up">
          <h2 class="panel-title mono-font"><i class="icon icon-book"></i><span>Education</span></h2>
          <div class="education-item" v-for="edu in education" :key="edu.degree">
              <h3>{{ edu.degree }}</h3>
              <p>{{ edu.institution }}</p>
              <p class="education-details" v-if="edu.details">{{ edu.details }}</p>
          </div>
      </section>

      <section id="contact" class="content-panel" data-aos="fade-up">
        <h2 class="panel-title mono-font"><i class="icon icon-mail"></i><span>Contact</span></h2>
        <p class="panel-body">Available for new opportunities and innovative challenges. Let's connect.</p>
        <div class="contact-links">
          <a :href="'mailto:' + email" class="cta-button">Email Me</a>
        </div>
      </section>
    </main>

    <footer class="footer mono-font">
       <div class="social-links">
         <a v-for="link in socials" :key="link.name" :href="link.url" target="_blank">{{ link.name }}</a>
       </div>
      <p>&copy; 2025 | Designed by Gautam Sharma</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';

// --- THEME SWITCH LOGIC --- //
const isDarkMode = ref(true);
const toggleTheme = () => { isDarkMode.value = !isDarkMode.value; };

// --- INTERACTIVE SKILL LOGIC --- //
const activeSkill = ref(null);
const toggleSkill = (skill) => {
    if (activeSkill.value === skill) {
        activeSkill.value = null; // Toggle off if the same skill is clicked again
    } else {
        activeSkill.value = skill;
    }
};


// --- YOUR PERSONALIZED DETAILS FROM RESUME --- //

const name = ref('Gautam Shamra');
const profession = ref('Data Science Student | Machine Learning Enthusiast');
const profilePictureUrl = ref('/profile.jpg'); // Replace with your actual picture filename
const resumeUrl = ref('Gautam-Sharma-Resume.pdf');
const bio = ref(`I am an ambitious and result-driven individual with practical experience leveraging predictive modeling and analytics to solve complex business problems. Aims to apply a deep understanding of machine learning algorithms to deliver impactful insights and support data-driven business growth.`);
const email = ref('gautam.sharma22@st.niituniversity.in');

const experience = ref({
    title: 'Marketing Analytics Intern',
    company: 'Super Collections, Jhunjhunu/Remote',
    duration: 'June 2025 - Aug 2025',
    points: [
        'Worked on optimisation of online selling portals via data analytics to provide them with visualization dashboards.',
        'Managed the online presence of the company and produced dashboards for data visualization.'
    ]
});

const projects = ref([
  { 
    id: 1, 
    title: 'Automated Credit Scoring System using ML',
    tech: 'MERN Stack, Jenkins, Docker',
    description: "Deployed a containerized MERN stack application to generate real-time credit scores using a Random Forest Regressor model. Integrated CI/CD pipelines with Jenkins for automated testing and seamless deployment.",
    image: '/KYCS.png' // Updated to use your local image
  },
  {
    id: 2,
    title: 'ACO and Clustering',
    tech: 'Python',
    description: "Used networkx for graph simulation and implemented a custom Ant Colony Optimization (ACO) algorithm for the Traveling Salesperson Problem (TSP). Developed a clustering method based on analyzing graph edge structures derived from ACO results.",
    image: '/ACO.png' // Updated to use your local image
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
    },
    {
      title: 'Learning Assistant, Digital Marketing and Storytelling Desk',
      organization: 'Center of Excellence in Education Technology (COEET), NIIT University',
      points: [
        'Mentored and guided teams to include storytelling in their projects.',
        'Handled Instagram / LinkedIn and the organization website for all core operations.'
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
    { name: 'LinkedIn', url: 'https://www.linkedin.com/in/gautam-sharma-87007029a/' }, { name: 'GitHub', url: 'https://github.com/gautam343' }
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
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&family=Roboto+Mono:wght@400;700&display=swap');

:root {
  --main-font: 'Poppins', sans-serif;
  --mono-font: 'Roboto Mono', monospace;
  --transition-speed: 0.4s;
}

.dark-theme {
  --bg-color: #000000;
  --panel-bg-color: rgba(16, 18, 27, 0.7);
  --text-primary: #F0F0F5;
  --text-secondary: #A0A0B0;
  --accent-glow: #00BFFF;
  --accent-saber: #FF1E56;
  --border-color: rgba(0, 191, 255, 0.2);
}

.light-theme {
  --bg-color: #F0F2F5;
  --panel-bg-color: rgba(255, 255, 255, 0.8);
  --text-primary: #1C1E21;
  --text-secondary: #555;
  --accent-glow: #1877F2;
  --accent-saber: #1877F2;
  --border-color: rgba(24, 119, 242, 0.3);
}

@keyframes move-stars { from { transform: translateY(0); } to { transform: translateY(-2000px); } }
.stars {
  position: fixed; top: 0; left: 0; right: 0; width: 1px; height: 2000px;
  background: transparent;
  box-shadow: 737px 1569px #FFF, 186px 1599px #FFF, 1426px 129px #FFF, 1245px 632px #FFF, 1421px 129px #FFF, 859px 1195px #FFF, 479px 1032px #FFF, 137px 1139px #FFF, 1482px 1438px #FFF, 1334px 522px #FFF, 1269px 1324px #FFF, 1024px 338px #FFF, 1076px 1386px #FFF, 946px 1636px #FFF, 179px 1282px #FFF, 595px 147px #FFF, 32px 1492px #FFF, 103px 182px #FFF, 638px 101px #FFF, 1912px 1069px #FFF, 1279px 1024px #FFF, 25px 143px #FFF, 1349px 1338px #FFF, 1827px 1863px #FFF, 1759px 114px #FFF, 1238px 1003px #FFF, 1205px 1239px #FFF, 1446px 1252px #FFF, 1938px 107px #FFF, 1302px 1599px #FFF, 622px 1109px #FFF, 1789px 1690px #FFF, 1697px 199px #FFF, 497px 106px #FFF, 331px 1441px #FFF, 178px 103px #FFF, 320px 486px #FFF, 1862px 1058px #FFF, 130px 1019px #FFF, 1383px 1828px #FFF, 54px 1101px #FFF, 1177px 188px #FFF, 1805px 33px #FFF, 1375px 819px #FFF, 483px 185px #FFF, 1196px 1650px #FFF, 1491px 1353px #FFF, 848px 1172px #FFF, 1795px 691px #FFF, 689px 1233px #FFF, 1889px 608px #FFF, 831px 1119px #FFF, 127px 1235px #FFF, 1823px 1545px #FFF, 65px 1221px #FFF, 1335px 329px #FFF, 174px 1680px #FFF, 134px 1756px #FFF, 1813px 1475px #FFF, 1845px 233px #FFF;
  animation: move-stars 50s linear infinite;
  z-index: -1;
}
#stars2 { box-shadow: 1971px 162px #FFF, 113px 1581px #FFF, 1434px 186px #FFF, 188px 1184px #FFF, 109px 1515px #FFF, 1173px 1572px #FFF, 69px 748px #FFF, 1582px 1974px #FFF, 1007px 1709px #FFF, 394px 1968px #FFF, 167px 832px #FFF, 1783px 837px #FFF, 157px 1835px #FFF, 1022px 376px #FFF, 1891px 229px #FFF, 1490px 1888px #FFF, 1561px 1269px #FFF, 1883px 129px #FFF, 1837px 1564px #FFF, 1098px 1205px #FFF, 192px 100px #FFF, 1512px 1813px #FFF, 1391px 1593px #FFF, 1500px 197px #FFF, 1803px 43px #FFF, 1045px 1269px #FFF, 1481px 1265px #FFF, 1668px 1796px #FFF, 1894px 93px #FFF, 1599px 1629px #FFF, 1215px 1933px #FFF, 184px 1093px #FFF, 144px 796px #FFF, 1260px 1248px #FFF, 1134px 1916px #FFF, 197px 859px #FFF, 1656px 1173px #FFF, 191px 843px #FFF, 143px 1774px #FFF, 1604px 502px #FFF, 1885px 505px #FFF, 1120px 1109px #FFF, 1083px 1794px #FFF, 1317px 1299px #FFF, 1785px 1653px #FFF, 1717px 1813px #FFF, 187px 1181px #FFF; animation: move-stars 100s linear infinite; }
#stars3 { box-shadow: 1714px 1426px #FFF, 411px 1251px #FFF, 1516px 1198px #FFF, 178px 1099px #FFF, 139px 1618px #FFF, 1466px 1475px #FFF, 1633px 1644px #FFF, 1674px 1421px #FFF, 1494px 160px #FFF, 1439px 1845px #FFF, 1406px 1491px #FFF, 1105px 1419px #FFF, 34px 899px #FFF, 132px 1928px #FFF, 1750px 1133px #FFF, 185px 1113px #FFF, 1334px 1299px #FFF, 1246px 1432px #FFF, 1324px 1499px #FFF, 1925px 338px #FFF, 1666px 1388px #FFF, 194px 1636px #FFF, 180px 1282px #FFF, 1595px 147px #FFF, 34px 1492px #FFF, 109px 182px #FFF, 1642px 101px #FFF, 1912px 1073px #FFF, 1283px 1024px #FFF, 29px 143px #FFF, 1353px 1338px #FFF, 1831px 1863px #FFF, 1763px 114px #FFF, 1242px 1003px #FFF, 1209px 1239px #FFF, 1450px 1252px #FFF, 1942px 107px #FFF, 1306px 1599px #FFF, 1626px 1109px #FFF, 1793px 1690px #FFF, 1701px 199px #FFF, 501px 106px #FFF, 335px 1441px #FFF, 182px 103px #FFF, 324px 486px #FFF, 1866px 1058px #FFF, 134px 1019px #FFF, 1387px 1828px #FFF, 58px 1101px #FFF, 1181px 188px #FFF; animation: move-stars 150s linear infinite; }
.light-theme .stars { display: none; }

.app-container {
  background-color: var(--bg-color); color: var(--text-primary);
  font-family: var(--main-font); min-height: 100vh;
  transition: background-color var(--transition-speed) ease, color var(--transition-speed) ease;
  overflow-x: hidden;
}
.header {
  position: sticky; top: 0; z-index: 100; padding: 1rem 5%;
  background-color: rgba(10, 10, 16, 0.5); backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border-color);
  transition: border-color var(--transition-speed) ease;
}
.navbar { display: flex; justify-content: space-between; align-items: center; max-width: 1200px; margin: 0 auto; }
.nav-logo { color: var(--text-primary); text-decoration: none; font-size: 1.1rem; transition: color var(--transition-speed) ease; }
.nav-controls { display: flex; align-items: center; gap: 1.5rem; }
.nav-menu { display: none; }
@media (min-width: 1200px) { .nav-menu { display: flex; list-style: none; gap: 1rem; flex-wrap: wrap;} }
.nav-link { color: var(--text-secondary); text-decoration: none; transition: color 0.3s ease; font-size: 0.9rem;}
.nav-link:hover { color: var(--accent-glow); }
.theme-switcher {
  background: transparent; border: 1px solid var(--border-color);
  width: 50px; height: 26px; border-radius: 13px; cursor: pointer;
  display: flex; align-items: center; position: relative;
  transition: all var(--transition-speed) ease;
}
.icon-sun, .icon-moon { position: absolute; top: 3px; font-size: 16px; transition: all var(--transition-speed) ease; }
.icon-sun { left: 5px; opacity: 0; color: #f39c12; }
.icon-moon { left: 28px; opacity: 1; color: #f1c40f; }
.dark-theme .icon-sun { opacity: 1; }
.dark-theme .icon-moon { opacity: 0; }

.hero-section {
  display: flex; flex-direction: column; align-items: center;
  min-height: 90vh; padding: 4rem 2rem;
  width: 100%; max-width: 1200px; margin: 0 auto;
}
.hero-main-content {
  display: flex; flex-direction: column;
  align-items: center; gap: 2rem; width: 100%;
}
@media (min-width: 992px) { .hero-main-content { flex-direction: row; align-items: stretch; } }
.about-me-panel {
  flex: 1; padding: 2rem; background-color: var(--panel-bg-color);
  border: 1px solid var(--border-color); border-radius: 12px;
  backdrop-filter: blur(16px); text-align: left;
}
.profile-picture-container { flex-shrink: 0; display: flex; align-items: center; justify-content: center; }
.profile-picture {
    width: 200px; height: 200px; border-radius: 50%;
    border: 4px solid var(--accent-glow); object-fit: cover;
    box-shadow: 0 0 30px var(--accent-glow);
    transition: all var(--transition-speed) ease;
}
.hero-name-section { width: 100%; text-align: center; margin-top: 3rem; }
.main-title { font-size: clamp(3rem, 8vw, 4rem); font-weight: 700; color: var(--text-primary); transition: color var(--transition-speed) ease; line-height: 1.1; }
.subtitle { color: var(--accent-glow); margin-top: 0.5rem; letter-spacing: 1px; transition: color var(--transition-speed) ease; }

.cta-button {
  background: transparent; color: var(--accent-saber);
  padding: 12px 28px; border: 2px solid var(--accent-saber);
  border-radius: 8px; text-decoration: none; font-weight: 600;
  transition: all 0.3s ease;
}
.cta-button.secondary {
  color: var(--accent-glow);
  border-color: var(--accent-glow);
}
.cta-button:hover { background: var(--accent-saber); color: var(--bg-color); box-shadow: 0 0 25px var(--accent-saber); }
.cta-button.secondary:hover { background: var(--accent-glow); color: var(--bg-color); box-shadow: 0 0 25px var(--accent-glow); }
.contact-links { display: flex; flex-wrap: wrap; gap: 1rem; margin-top: 2rem;}

[data-aos] { opacity: 0; transition: transform 0.8s ease-out, opacity 0.8s ease-out; }
[data-aos="fade-up"].is-visible { opacity: 1; transform: translateY(0); }
[data-aos="fade-right"].is-visible { opacity: 1; transform: translateX(0); }
[data-aos="fade-left"].is-visible { opacity: 1; transform: translateX(0); }
[data-aos="fade-up"] { transform: translateY(50px); }
[data-aos="fade-right"] { transform: translateX(-50px); }
[data-aos="fade-left"] { transform: translateX(50px); }

.content-panel {
  padding: 2.5rem; max-width: 1000px; margin: 4rem auto;
  background-color: var(--panel-bg-color); backdrop-filter: blur(16px);
  border: 1px solid var(--border-color); border-radius: 12px;
  transition: all var(--transition-speed) ease;
}
.panel-title { font-size: 1.5rem; color: var(--text-primary); margin-bottom: 1.5rem; transition: color var(--transition-speed) ease; border-bottom: 1px solid var(--border-color); padding-bottom: 0.75rem; display: flex; align-items: center; gap: 0.75rem;}
.panel-body { color: var(--text-secondary); line-height: 1.7; transition: color var(--transition-speed) ease; }

.experience-item, .education-item, .achievement-item { margin-bottom: 1.5rem; }
.experience-item:last-child, .education-item:last-child, .achievement-item:last-child { margin-bottom: 0; }
h3 { color: var(--text-primary); margin-bottom: 0.25rem; }
.experience-company, .education-details { color: var(--text-secondary); font-size: 0.9rem; font-style: italic; margin-bottom: 0.75rem; }
.experience-list { list-style-type: none; padding-left: 0; }
.experience-list li { color: var(--text-secondary); margin-bottom: 0.5rem; position: relative; padding-left: 1.5rem; }
.experience-list li::before {
    content: '»'; position: absolute; left: 0;
    color: var(--accent-glow);
}

.skills-grid { display: grid; grid-template-columns: 1fr; gap: 2rem; }
@media (min-width: 768px) { .skills-grid { grid-template-columns: repeat(2, 1fr); } }
.skill-category h4 { color: var(--accent-glow); margin-bottom: 1rem; display: flex; align-items: center; gap: 0.5rem;}
.skill-tabs { display: flex; flex-wrap: wrap; gap: 0.75rem; }
.skill-tab {
    background-color: rgba(0,0,0,0.2);
    border: 1px solid var(--border-color);
    color: var(--text-secondary);
    padding: 0.5rem 1rem;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
}
.skill-tab::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0;
    height: 0;
    background-color: var(--accent-glow);
    border-radius: 50%;
    transform: translate(-50%, -50%);
    transition: width 0.4s ease, height 0.4s ease;
    z-index: -1;
    opacity: 0.3;
}
.skill-tab.active {
    color: var(--text-primary);
    border-color: var(--accent-glow);
}
.skill-tab.active::after {
    width: 200px;
    height: 200px;
}

.project-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1.5rem; }
.project-card {
  background-color: rgba(0,0,0,0.2); border: 1px solid var(--border-color);
  border-radius: 8px; overflow: hidden; transition: all 0.3s ease;
  display: flex; flex-direction: column;
}
.project-card:hover { transform: translateY(-5px); border-color: var(--accent-glow); box-shadow: 0 0 15px var(--accent-glow); }
.project-image-container { width: 100%; height: 180px; background-color: #000; }
.project-image { width: 100%; height: 100%; object-fit: cover; transition: transform 0.3s ease; }
.project-card:hover .project-image { transform: scale(1.05); }
.project-info { padding: 1.5rem; flex-grow: 1; }
.project-info h3 { font-weight: 600; margin-bottom: 0.5rem; }
.project-info p { font-size: 0.9rem; color: var(--text-secondary); margin-bottom: 1rem; }
.project-tech { font-size: 0.8rem; font-family: var(--mono-font); color: var(--accent-glow); margin-bottom: 1rem; }
.project-link { color: var(--accent-glow); text-decoration: none; font-weight: bold; }

/* === NEW CERTIFICATE STYLES === */
.certificate-list { 
  display: flex; 
  flex-direction: column; 
  gap: 3rem; 
}
.certificate-item {
  padding: 1.5rem;
  border: 1px solid var(--border-color);
  border-radius: 12px;
  background-color: rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}
.certificate-item:hover {
  border-color: var(--accent-glow);
}
.cert-header { 
  display: flex; 
  align-items: center; 
  gap: 1rem; 
  margin-bottom: 1.5rem; 
}
.cert-info h3 { 
  font-size: 1.1rem; 
  color: var(--text-primary); 
  margin: 0; 
}
.cert-info p { 
  font-size: 0.9rem; 
  color: var(--text-secondary); 
  margin: 0; 
  margin-top: 0.25rem;
}
.pdf-embed { 
  border-radius: 8px; 
  border: 1px solid var(--border-color); 
}
.cert-link-button {
  display: inline-block;
  background: transparent;
  color: var(--accent-glow);
  padding: 8px 16px;
  border: 1px solid var(--accent-glow);
  border-radius: 6px;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.3s ease;
}
.cert-link-button:hover { 
  background: var(--accent-glow); 
  color: var(--bg-color); 
}

.footer { text-align: center; padding: 2rem 5%; color: var(--text-secondary); font-size: 0.8rem; }
.social-links { margin-bottom: 1rem; display: flex; justify-content: center; gap: 1.5rem; }
.social-links a { color: var(--text-secondary); text-decoration: none; transition: color 0.3s ease; }
.social-links a:hover { color: var(--accent-glow); }

/* --- ICONS --- */
.icon {
  display: inline-block; width: 20px; height: 20px;
  background-color: var(--accent-glow);
  mask-repeat: no-repeat; mask-size: contain; mask-position: center;
  transition: background-color var(--transition-speed) ease;
}
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
.skill-category .icon { background-color: var(--text-secondary); }
.icon-speech-bubble { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>');}
.icon-layers { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"></polygon><polyline points="2 17 12 22 22 17"></polyline><polyline points="2 12 12 17 22 12"></polyline></svg>');}
.icon-database { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"></ellipse><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"></path><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"></path></svg>');}
.icon-tool { mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path></svg>');}

[data-aos] {
  opacity: 0;
  transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94), opacity 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
</style>