<!-- App.vue -->
<template>
  <div id="app">
    <Header />
    <Home />
    <AboutMe />
    <Achievements />
    <!-- <Education /> -->
    <Skill />
    <Contact />
    
  </div>
</template>

<script>
import Header from './components/HeaderNav.vue'  // Assuming this is the correct filename
import Home from './components/Home.vue'         // Fixed: Added complete path
import AboutMe from './components/AboutMe.vue'
import Achievements from './components/Achievements.vue'
import Skill from './components/Skill.vue'
import Contact from './components/Contact.vue'



export default {
  name: 'App',
  components: {
    Header,
    Home,
    AboutMe,
    Achievements,
    Skill,
    Contact,
    
  }
}
</script>
  
<style>
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@100;200;300;400;500;600;700&family=Mitr:wght@200;300;400;500;600;700&display=swap');
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    text-decoration: none;
    border: none;
    outline: none;
    font-family: 'IBM Plex Sans Thai',serif;
}

html {
    font-size: 62.5%;
    scroll-behavior: smooth;
}

body {
    background-color: #132743;
    color: rgb(223, 208, 184);
    line-height: 1.6;
    font-size: 1.6rem;
}

/* Header Section */
header.logohead {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: #142d4c;
    padding: 1rem 5%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 1000;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

header .logo {
    font-size: 3rem;
    font-weight: 700;
    color: rgb(253, 247, 244);
    transition: transform 0.3s ease;
}

header .logo:hover {
    transform: scale(1.1);
}



nav.navbar a {
    font-size: 1.8rem;
    color: rgb(148, 137, 121);
    margin-left: 3rem;
    transition: 0.3s ease;
}

nav.navbar a:hover,
nav.navbar a.active {
    color: rgb(223, 100, 77);
    border-bottom: 2px solid rgb(223, 100, 77);
}

/* Section Styling */
section {
    padding: 8rem 10%;
    min-height: 100vh;
    text-align: center;
}

section .title {
    font-size: 4rem;
    font-weight: 700;
    margin-bottom: 3rem;
    color: rgb(253, 247, 244);
    transition:cubic-bezier();
}

.Intro {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    gap: 4rem;
}

.Homepic img {
    width: 300px;
    height: 300px;
    border-radius: 20%;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);
    object-fit: cover;
    transition: transform 0.3s ease, box-shadow 0.3s ease, filter 0.3s ease;
}

.Homepic img:hover {
    transform: scale(1.1) rotate(3deg); /* ขยับได้ด้วย */
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.5); 
   
}




/* Future Section */
.future h2 {
    font-size: 2.5rem;
    color: rgb(253, 247, 244);
    margin-bottom: 2rem;
}

.future h4 {
    font-size: 2rem;
    color: rgb(223, 208, 184);
    max-width: 700px;
    margin: 0 auto;
}

/* Contact Section */
.My_contact {
    font-size: 1.8rem;
    color: rgb(223, 208, 184);
    line-height: 2;
}

.My_contact h4 {
    margin: 1rem 0;
}

/* Responsive Design */
@media (max-width: 768px) {
    html {
        font-size: 55%;
    }

    

    header.logohead {
        flex-direction: column;
        padding: 1.5rem;
    }

    nav.navbar {
        display: flex;
        flex-direction: column;
        margin-top: 1rem;
    }

    nav.navbar a {
        margin: 1rem 0;
    }

    .Intro {
        flex-direction: column;
        text-align: center;
    }

    .edu_01,
    .edu_02,
    .edu_03,
    .edu_04 {
        flex-direction: column;
        text-align: center;
    }
}

@media (max-width: 480px) {
    html {
        font-size: 50%;
    }

    header .logo {
        font-size: 2.5rem;
    }

    section .title {
        font-size: 3.2rem;
    }
}

</style>