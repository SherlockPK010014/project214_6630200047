<template>
  <section class="section" id="home">
    <h1 class="title animated-title" v-reveal>
  <span
    v-for="(ch, i) in letters"
    :key="i"
    :style="{ animationDelay: (i * 0.05) + 's' }"
  >
    {{ ch === ' ' ? '\u00A0' : ch }}
  </span>
</h1>
    <h2 class="typing" v-reveal>
  <span
    v-for="(ch, i) in lettersH2"
    :key="i"
    :style="{ animationDelay: (i * 0.15) + 's' }"
  >
    {{ ch === ' ' ? '\u00A0' : ch }}
  </span>
</h2>
    <br>
    <div :class="['Homepic-container', { expanded: isExpanded }]"
  @mouseover="expandImages"
  v-reveal>
      <div class="Homepic">
        <img src="@/assets/34264.jpg" alt="MainProfile-pic" class="main-pic" />
        <img src="@/assets/34276.jpg" alt="Extra Pic1" class="extra-1" />
        <img src="@/assets/34278.jpg" alt="Extra Pic2" class="extra-2" />
        <img src="@/assets/34277.jpg" alt="Extra Pic3" class="extra-3" />
        <img src="@/assets/34323.jpg" alt="Extra Pic4" class="extra-4" />
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      isExpanded: false,
      text: "HELLO, GET TO KNOW ME...",
      text2: "Welcome to my profile",
    };
  },
  computed: {
    letters() {
      return this.text.split('');
    },
    lettersH2() {
      return this.text2.split("");
    }
  },
  methods: {
    expandImages() {
      this.isExpanded = true;
    }
  },
  // ⬇️ เพิ่มส่วนนี้
  directives: {
    reveal: {
      mounted(el) {
        // เริ่มจากสถานะซ่อน
        el.classList.add('reveal');

        const io = new IntersectionObserver(
          (entries) => {
            entries.forEach((e) => {
              if (e.isIntersecting) {
                e.target.classList.add('in-view');  
              } else {
                e.target.classList.remove('in-view');
              }
            });
          },
          {
            threshold: 0.15,
            rootMargin: '0px 0px -10% 0px',
          }
        );

        io.observe(el);
      },
    },
  },
};

</script>

<style scoped>
.title{
  margin-top: 3%;
}

.animated-title {
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
}

.animated-title span {
  display: inline-block;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 1.2s ease forwards;
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.typing span {
  display: inline-block;
  opacity: 0;
  animation: blinkIn 0.6s forwards;
}

@keyframes blinkIn {
  0%   { opacity: 0; }
  50%  { opacity: 1; }
  100% { opacity: 1; }
}
.Homepic-container {
  position: relative;
  width: 200px;
  height: 200px;
  margin: 0 auto;
  transition: width 0.3s ease; 
}

.Homepic-container.expanded {
  width: 1080px; 
}

.Homepic {
  margin-top: 5%;
  position: relative;
  width: 100%;
  height: 100%;
}

.main-pic {
  width: 200px;
  height: 200px;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
  transition: all 0.3s ease;
}

.extra-1,
.extra-2,
.extra-3,
.extra-4 {
  position: absolute;
  width: 200px;
  height: 200px;
  object-fit: cover;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 1;
  top: 0;
}

.extra-1,
.extra-2,
.extra-3,
.extra-4 {
  left: 50%;
  transform: translateX(-50%);
}

.expanded .extra-1,
.expanded .extra-2,
.expanded .extra-3,
.expanded .extra-4 {
  opacity: 1;
}

.expanded .extra-1 {
  left: 0;
  transform: translateX(0);
}

.expanded .extra-2 {
  left: 220px;
  transform: translateX(0);
}

.expanded .main-pic {
  left: 440px;
  transform: translateX(0);
}

.expanded .extra-3 {
  left: 660px;
  transform: translateX(0);
}

.expanded .extra-4 {
  left: 880px;
  transform: translateX(0);
}

.reveal {
  opacity: 0;
  transform: translateY(16px);
  transition:
    opacity .7s ease,
    transform .7s ease;
  transition-delay: var(--delay, 0ms);
  will-change: opacity, transform;
}

.reveal.in-view {
  opacity: 1;
  transform: translateY(0);
}

</style>



