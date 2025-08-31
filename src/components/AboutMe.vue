<template>
  <section class="section" id="about-me">
    <div class="about-me-container">
      <div class="Pic-ABU reveal" v-reveal style="--delay:0ms">
        <div class="card">
          <div class="card-face front">
            <img src="@/assets/34285.jpg" alt="abu-pic-front" />
          </div>
          <div class="card-face back">
            <img src="@/assets/34287.jpg" alt="abu-pic-back" />
          </div>
        </div>
      </div>

      <div class="profile-content">
        <br />
        <h1 class="title reveal" v-reveal style="--delay:120ms">About Me</h1>
        <br />
        <div class="profile-inner">
          <h4 class="reveal" v-reveal style="--delay:200ms">
            <span class="label">ชื่อ - นามสกุล : </span>
            <span class="value">{{ profile.fullNameTH }}</span>
          </h4>
          <br />
          <h4 class="reveal" v-reveal style="--delay:280ms">
            <span class="label">Name : </span>
            <span class="value">{{ profile.fullNameEN }}</span>
          </h4>
          <br />
          <h4 class="reveal" v-reveal style="--delay:360ms">
            <span class="label">Nickname : </span>
            <span class="value">{{ profile.nickname }}</span>
          </h4>
          <br />
          <h4 class="reveal" v-reveal style="--delay:440ms">
            <span class="label">Student ID : </span>
            <span class="value">{{ profile.studentID }}</span>
          </h4>
          <br />
          <h4 class="reveal" v-reveal style="--delay:520ms">
            <span class="label">Major : </span>
            <span class="value">{{profile.major}}</span>
          </h4>
          <br />
          <h4 class="reveal" v-reveal style="--delay:600ms">
            <span class="label">University : </span>
            <span class="value">{{profile.university}}</span>
          </h4>
          <br />
        </div>
      </div>
    </div>
  </section>
</template>


<script>
import profileData from '@/data/db.json'

const revealDirective = {
  mounted(el) {
    el.classList.add('reveal')

    const io = new IntersectionObserver((entries) => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('in-view')
        } else {
          // พอเลื่อนออกจากจอ ให้กลับไปซ่อน
          e.target.classList.remove('in-view')
        }
      })
    }, { threshold: 0.15, rootMargin: '0px 0px -10% 0px' })

    io.observe(el)
  }
}


export default {
  name: 'AboutMe',
  directives: { reveal: revealDirective },
  data() {
    return { profile: null, originalProfile: null }
  },
  created() {
    const payload = profileData
    this.profile = { ...payload }
    this.originalProfile = { ...payload }
  }
}
</script>



<style scoped>
.section {
  padding: 3rem 2rem;
  background: linear-gradient(135deg, #1e293b, #0f172a);
  color: #fff;
  border-radius: 16px;
  max-width: 1500px;
  min-height: 400px; 
  margin: 0 auto;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
  transition: transform 0.3s ease;
}

.section:hover {
  transform: translateY(-3px);
}

.about-me-container {
  display: flex;
  align-items: flex-start;
  gap: 2rem;
  flex-wrap: wrap; /* responsive */
}

.Pic-ABU {
  
  margin: 1rem;
  margin-left: 5rem;
  flex: 1;
  min-width: 350px;
  max-width: 400px;
  height: 460px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  overflow: hidden;
  perspective: 1000px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
}
.Pic-ABU img {
  width: 80%;
  height: 80%;
  margin-top: 12%;
  object-fit: cover;
  border-radius: 10px;
  margin-left: 1%;
}
.card {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  animation: flip 8s infinite;
}

.card-face {
  position: absolute;
  inset: 0;
  backface-visibility: hidden;
}

.card-face.front { transform: rotateY(0deg); }
.card-face.back { transform: rotateY(180deg); }




.profile-content {
  flex: 2;
  text-align: left;
  padding: rem;
}

.title {
  margin-left: 1rem;
  font-size: 3.5rem;
  margin-bottom: 1rem;
  color: #38bdf8; 
  font-weight: 700;
  letter-spacing: 1px;
}

.profile-inner h4 {
  margin-left: 1rem;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.label {
  font-size: 2.5rem;
  font-weight: 600;
  color: #fbbf24; 
}

.value {
  font-size: 2rem;
  color: #e2e8f0;
  font-weight: 400;
  padding: 0.25rem 0.5rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 6px;
  transition: background 0.3s ease;
}

.value:hover {
  background: rgba(56, 189, 248, 0.15);
}

/* Animation */
@keyframes flip {
  0%   { transform: rotateY(0deg); }
  45%  { transform: rotateY(180deg); }
  50%  { transform: rotateY(180deg); }
  95%  { transform: rotateY(0deg); }
  100% { transform: rotateY(0deg); }
}

/* Responsive */
@media (max-width: 900px) {
  .about-me-container { flex-direction: column; align-items: center; }
  .title { text-align: center; }
  .profile-content { text-align: center; }
  .profile-inner h4 { justify-content: center; }
}
/* --- Scroll reveal (fade + slide-up) --- */
.reveal{
  opacity: 0;
  transform: translateY(16px);
  transition:
    opacity .7s ease,
    transform .7s ease;
  transition-delay: var(--delay, 0ms);
  will-change: opacity, transform;
}

.reveal.in-view{
  opacity: 1;
  transform: none;
}

@media (prefers-reduced-motion: reduce){
  .reveal,
  .reveal.in-view{
    opacity:1 !important;
    transform:none !important;
    transition:none !important;
  }
}

</style>