<template>
  <section class="section" id="home">
    <h1 class="title">HELLO, GET TO KNOW ME...</h1>
    <h2 class="typing">Welcome to my profile</h2>
    <br>
    <div :class="['Homepic-container', { expanded: isExpanded }]" @mouseover="expandImages">
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
    };
  },
  methods: {
    expandImages() {
      this.isExpanded = true;
    }
  }
};
</script>

<style scoped>
.title{
  margin-top: 3%;
}
.typing{
  margin-top: 5%;
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
</style>



