<template>
  <section class="section" id="future">
    <h1 class="title">Future</h1>
    <div class="future">
      <h2>อนาคตอยากเป็นอะไร</h2>
      <transition-group name="fade" tag="div" class="future-text">
        <h4 v-for="(line, index) in futureLines" :key="index" :style="{ animationDelay: index * 0.5 + 's' }">
          {{ line }}
        </h4>
      </transition-group>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Future',
  data() {
    return {
      futureLines: [
        "ในอนาคต ดิฉันอยากทำอาชีพในสายงานของคอมพิวเตอร์",
        "ที่สามารถประยุกต์กับการแพทย์ได้ค่ะ",
        "",
        "อยากจะพัฒนาระบบ Software ที่ทำให้หมอและผู้เข้ารับการรักษา",
        "สื่อถึงกันได้ง่ายขึ้น หรือพัฒนาด้านอื่นๆ"
      ]
    };
  }
};
</script>

<style scoped>
.section {
  margin-top: 5%;
  text-align: center;
  padding: 5rem;
}

.title {
  margin-top: 10%;
  font-size: 4rem;
  margin-bottom: 2rem;
}

.future h2 {
  font-size: 2.5rem;
  margin-bottom: 3rem;
}

.future-text h4 {
  font-size: 2rem;
  margin-bottom: 1.5rem;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInUp 0.5s ease-in-out forwards;
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-enter-active {
  transition: opacity 0.5s, transform 0.5s;
}

.fade-enter {
  opacity: 0;
  transform: translateY(20px);
}
</style>
