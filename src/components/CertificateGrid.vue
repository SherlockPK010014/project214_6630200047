<!-- components/CertificateGrid.vue -->
<template>
  <section class="cert-section" id="achievements">
    <h1 class="cert-title "v-reveal>Achievements</h1>

    <!-- FILTER TAGS -->
    <div class="filters" v-reveal>
      <button
    v-for="t in ['All', ...allTags]"
    :key="t"
    class="chip"
    :class="{ active: activeTag === t }"
    @click="activeTag = t"
  >
    {{ t }}
  </button>
    </div>

    <!-- GRID -->
    <div class="grid">
      <article v-for="(c, i) in filtered" :key="i" class="card" @click="open(c)"v-reveal
    :style="{ '--delay': (i * 80) + 'ms' }">
        <div class="thumb">
          <!-- ใช้ภาพหน้าปก: รูปแรกจาก images[] หรือ image เดี่ยว -->
          <img :src="c.images?.[0] ?? c.image" :alt="c.title" loading="lazy" />
          <span v-if="c.verified" class="ribbon">Verified</span>
          <span v-if="c.images?.length" class="multi-badge">{{ c.images.length }} images</span>
        </div>

        <div class="meta">
          <h3 class="ctitle">{{ c.title }}</h3>
          <p class="issuer">{{ c.issuer }}</p>

          <!-- ป้ายแท็ก -->
          <div class="cert-tags">
            <span class="cert-tag" v-for="t in c.tags" :key="t">{{ t }}</span>
          </div>

          <p class="date">{{ formatDate(c.date) }}</p>
        </div>
      </article>
    </div>

    <!-- LIGHTBOX -->
    <div v-if="active" class="lightbox" @click.self="close" v-reveal>
      <div class="lightbox-inner" v-reveal style="--delay:120ms">
        <!-- Viewer: รองรับหลายรูป -->
        <div class="viewer" v-reveal style="--delay:160ms">
          <button v-if="active.images?.length" class="nav prev" @click.stop="prev"
            aria-label="Previous image">&#10094;</button>

          <!-- แสดงรูปปัจจุบัน: ถ้ามี images[] ให้ใช้ตาม activeIndex, ไม่งั้นใช้ image เดี่ยว -->
          <img :src="active.images?.length ? active.images[activeIndex] : active.image" :alt="active.title" />

          <button v-if="active.images?.length" class="nav next" @click.stop="next"
            aria-label="Next image">&#10095;</button>

          <!-- Thumbnails -->
          <div v-if="active.images?.length" class="thumbs">
            <button v-for="(img, idx) in active.images" :key="idx" class="thumb-btn"
              :class="{ active: idx === activeIndex }" @click.stop="activeIndex = idx">
              <img :src="img" :alt="`thumb-${idx}`" />
            </button>
          </div>
        </div>

        <!-- ข้อมูลใบประกาศ/ผลงาน -->
        <!-- ข้อมูลใบประกาศ/ผลงาน -->
        <div class="lb-info" v-reveal style="--delay:200ms">
          <h3>{{ active.title }}</h3>
          <p class="issuer">{{ active.issuer }} • {{ formatDate(active.date) }}</p>
          <p class="desc" v-if="active.desc">{{ active.desc }}</p>

          <!-- ปุ่ม Close -->
          
        </div>
      <button class="btn ghost close-btn" @click="close">Close</button>
      </div>
      
    </div>
  </section>
</template>


<script>
import PK_1 from '@/assets/34277.jpg'
import mathLabCert from '@/assets/MathLABcertificate.jpg'
import SPANSDG001 from '@/assets/SPA&SDGs_page-0001.jpg'
import SPANSDG002 from '@/assets/SPA&SDGs_page-0002.jpg'
import FrontEnd from '@/assets/front-end.jpg'
import DSC1 from '@/assets/SDGs/DSC_1429.jpg'
import DSC2 from '@/assets/SDGs/DSC_1751.jpg'
import DSC3 from '@/assets/SDGs/DSC_1397.jpg'
import DSC4 from '@/assets/SDGs/DSC_1754.jpg'
import DSC5 from '@/assets/SDGs/DSC_1539.jpg'
import DSC6 from '@/assets/SDGs/DSC_1622.jpg'
import IOT1 from '@/assets/IOT/iot_1.jpg'
import IOT2 from '@/assets/IOT/iot_2.jpg'
import IOT3 from '@/assets/IOT/iot_3.jpg'
import IOT4 from '@/assets/IOT/iot_4.jpg'
import IOT5 from '@/assets/IOT/iot_5.jpg'
import IOT6 from '@/assets/IOT/iot_6.jpg'
import IOT7 from '@/assets/IOT/iot_7.jpg'
import Pro_1 from '@/assets/Promote/Pro_1.jpg'
import Pro_2 from '@/assets/Promote/Pro_2.jpg'
import Pro_3 from '@/assets/Promote/Pro_3.jpg'
import Pro_4 from '@/assets/Promote/Pro_4.jpg'
import Pro_5 from '@/assets/Promote/Pro_5.jpg'
import Pro_6 from '@/assets/Promote/Pro_6.jpg'
import Pro_7 from '@/assets/Promote/Pro_7.jpg'
import Pro_8 from '@/assets/Promote/Pro_8.jpg'
import Pro_9 from '@/assets/Promote/Pro_9.jpg'
import A1 from '@/assets/Anniversary/A1.jpg'
import A2 from '@/assets/Anniversary/A2.jpg'
import A3 from '@/assets/Anniversary/A3.jpg'
import G2 from '@/assets/GPT/G2.jpg'
import G3 from '@/assets/GPT/G3.jpg'
import O1 from '@/assets/OPHSC/O1.jpg'
import O2 from '@/assets/OPHSC/O2.jpg'
import O3 from '@/assets/OPHSC/O3.jpg'
import O4 from '@/assets/OPHSC/O4.jpg'
import O5 from '@/assets/OPHSC/O5.jpg'
import O6 from '@/assets/OPHSC/O6.jpg'
import O7 from '@/assets/OPHSC/O7.jpg'
import S1 from '@/assets/SWU/SWU1.jpg'
import S2 from '@/assets/SWU/SWU2.jpg'
import S3 from '@/assets/SWU/SWU3.jpg'
import S4 from '@/assets/SWU/SWU4.jpg' 
import S5 from '@/assets/SWU/SWU5.jpg'
import S6 from '@/assets/SWU/SWU6.jpg'
import S7 from '@/assets/SWU/SWU7.jpg'
import SBO1 from '@/assets/SBO/SBO1.jpg'
import SBO2 from '@/assets/SBO/SBO2.jpg'
import SBO3 from '@/assets/SBO/SBO3.jpg'
import SBO4 from '@/assets/SBO/SBO4.jpg'
import SBO5 from '@/assets/SBO/SBO5.jpg'
import SBO6 from '@/assets/SBO/SBO6.jpg'
import SBO7 from '@/assets/SBO/SBO7.jpg'
import SBO8 from '@/assets/SBO/SBO8.jpg'
import SBO9 from '@/assets/SBO/SBO9.jpg'
import SBO10 from '@/assets/SBO/SBO10.jpg'
import SEM1 from '@/assets/Seminar66/SEM1.jpg'
import SEM2 from '@/assets/Seminar66/SEM2.jpg'
import SEM3 from '@/assets/Seminar66/SEM3.jpg'
import SEM4 from '@/assets/Seminar66/SEM4.jpg'
import SEM5 from '@/assets/Seminar66/SEM5.jpg'
import SEM6 from '@/assets/Seminar66/SEM6.jpg'
import SEM7 from '@/assets/Seminar66/SEM7.jpg'
import SEM8 from '@/assets/Seminar66/SEM8.jpg'
import SE1 from '@/assets/Seminar67/SE1.jpg'
import SE2 from '@/assets/Seminar67/SE2.jpg'
import SE3 from '@/assets/Seminar67/SE3.jpg'
import SE4 from '@/assets/Seminar67/SE4.jpg'
import SE5 from '@/assets/Seminar67/SE5.jpg'
import SE6 from '@/assets/Seminar67/SE6.jpg'
import SE7 from '@/assets/Seminar67/SE7.jpg'
import F1 from '@/assets/figma/F1.png'
import F2 from '@/assets/figma/F2.png'
import F3 from '@/assets/figma/F3.png'
import F4 from '@/assets/figma/F4.png'
import F5 from '@/assets/figma/F5.png'
import F6 from '@/assets/figma/F6.png'
import F7 from '@/assets/figma/F7.png'
export default {
  directives: {
  reveal: {
    mounted(el) {
      // เริ่มซ่อน
      el.classList.add('reveal');
      const io = new IntersectionObserver(
        (entries) => {
          entries.forEach((e) => {
            if (e.isIntersecting) {
              e.target.classList.add('in-view');    // โผล่
            } else {
              e.target.classList.remove('in-view'); // ออกนอกจอ → กลับไปซ่อน (เล่นซ้ำได้)
            }
          });
        },
        { threshold: 0.15, rootMargin: '0px 0px -10% 0px' }
      );
      // เก็บไว้บน element เพื่อหยุดตอนถูกถอด
      el.__io = io;
      io.observe(el);
    },
    unmounted(el) {
      if (el.__io) {
        el.__io.disconnect();
        delete el.__io;
      }
    }
  }
},

  name: 'CertificateGrid',
  data() {
    
    return {
      activeTag: 'All',
      active: null,
      activeIndex: 0, // 👉 index รูปที่กำลังดูใน lightbox
      items: [
        {
          title: 'MathLab Certificate',
          issuer: 'MathLab Academy',
          date: '2024-03-08',
          tags: ['MathLab'],
          image: mathLabCert,
          file: null,
          verified: false,
          desc: 'Fundamentals of MATLAB scripting, matrices, and visualization.',
        },
        {
          title: 'President of the Student Constitution Committee',
          issuer: 'Spanisit • Kasetsart University',
          date: '2024-02-26',
          tags: ['KU', 'Student Council'],
          image: SPANSDG001,
          file: null,
          verified: false,
          desc: 'Recognized for serving as the President of the Student Constitution Committee under the Student Representative Council at Kasetsart University, Sriracha Campus, during the academic year 2024. This role highlights leadership in student governance and responsibility in upholding student rights and constitutional principles.',
        },
        {
          title: '2nd Runner-up Award – Sustainable Development Project (SDGs)',
          issuer: 'SDGs • Kasetsart University',
          date: '2025-02-10',
          tags: ['KU', 'SDGs'],
          image: SPANSDG002,
          file: null,
          verified: false,
          desc: 'Awarded 2nd Runner-up in the Sustainability Project Competition (SDGs) at Kasetsart University in 2024, under the theme Happy University @KU. The project, New-Council Activities Trainer Camp, promoted student leadership and sustainable engagement through innovative volunteer activities.',
        },
        {
          title: 'Front-end Web Development using React Certificate',
          issuer: 'Web Development • React',
          date: '2025-04-21',
          tags: ['WEB'],
          image: FrontEnd,
          file: null,
          verified: false,
          desc: 'Successfully completed the Front-end Web Development using React training program held on April 19–20, 2024, organized by Kasetsart University, Sriracha Campus. This certificate formally recognizes the achievement and proficiency in modern web development technologies with React.',
        },
        {
          title: 'KU Green Blood Camp',
          issuer: 'DSCs • Kasetsart University',
          date: '2024-05-07',
          tags: ['KU', 'SDGs'],
          images: [DSC1, DSC2, DSC3, DSC4, DSC5, DSC6],
          verified: false,
          desc: 'The KU Green Blood Camp 2024 is a sustainability-driven volunteer program that builds a network of SDGs ambassadors. Participants learn about the UN Sustainable Development Goals, practice calculating carbon footprints, and co-design activities that promote sustainability both within and beyond Kasetsart University.',
        },
        {
          title: 'Host & Staff – Robotics and IoT Training Workshop for Primary Students',
          issuer: 'IoT Training Workshop',
          date: '2025-08-19',
          tags: ['KU','IOT'],
          images: [IOT1, IOT2, IOT3, IOT4, IOT5, IOT6, IOT7],
          verified: false,
          desc: 'Served as a host and staff member in the Robotics and IoT Training Workshop for primary school students. The program aimed to inspire young learners through hands-on activities, introducing the fundamentals of robotics and IoT in an engaging and accessible way.',
        },
        {
          title: 'Host & Promotional Model – Faculty of Science, Kasetsart University Sriracha Campus',
          issuer: 'Promotional Faculty of Science ',
          date: '2024-02-19',
          tags: ['KU'],
          images: [Pro_1, Pro_2, Pro_3, Pro_4, Pro_5, Pro_6, Pro_7, Pro_8, Pro_9],
          verified: false,
          desc: 'Participated as a female host and promotional model in the publicity event for the Faculty of Science, Kasetsart University Sriracha Campus. Contributed to presenting the program, engaging the audience, and representing the faculty through professional photoshoots for promotional materials.',
        },
        {
          title: 'Kasetsart University Founding Anniversary',
          issuer: 'KU Anniversary Event • งานวันคล้ายวันสถาปนา',
          date: '2023-02-02',
          tags: ['KU'],
          images: [A1, A2, A3],
          verified: false,
          desc: 'I joined the Kasetsart University Founding Anniversary, an annual celebration honoring the university’s heritage and contributions to society. The event highlighted academic achievements and traditions, fostering a strong sense of community and pride.',
        },
        {
          title: 'Training Program – Learning Intelligent Technology: ChatGPT',
          issuer: 'ChatGPT',
          date: '2024-08-21',
          tags: ['KU'],
          images: [G2, G3],
          verified: false,
          desc: 'Completed a training program on intelligent technology with ChatGPT, focusing on practical applications of generative AI. The course introduced core concepts, usage techniques, and innovative ways to apply ChatGPT in academic and professional contexts.',
        },
        {
          title: 'The Student Constitution Open House',
          issuer: 'Student Constitution • โครงการเปิดบ้านธรรมนูญนิสิต',
          date: '2024-10-5',
          tags: ['KU', 'Student Council'],
          images: [O1, O2, O3, O4, O5, O6, O7],
          verified: false,
          desc: 'The Student Constitution Open House introduces students to their rights and responsibilities through interactive activities and fun experiences, fostering awareness and community spirit',
        },
        {
          title: 'Student Council Exchange Meeting',
          issuer: 'Kasetsart University Sriracha & Srinakharinwirot University',
          date: '2024-09-10',
          tags: ['KU', 'Student Council'],
          images: [S6, S2, S3, S4, S5, S1, S7],
          verified: false,
          desc: 'An academic exchange meeting between the Student Council of Kasetsart University, Sriracha Campus, and the Student Council of Srinakharinwirot University. The program provided a platform to share ideas, discuss student governance, and strengthen collaboration between the two institutions.',
        },
        {
          title: 'Seminar of Student Organizations—4 Campuses',
          issuer: 'สัมมนาองค์การนิสิต 4 วิทยาเขต',
          date: '2024-03-01',
          tags: ['KU', 'Student Council'],
          images: [SBO1, SBO2, SBO3, SBO4, SBO5, SBO6, SBO7, SBO8, SBO9, SBO10],
          verified: false,
          desc: 'The Seminar of Student Organizations—4 Campuses , 2024 is a collaborative program that brings together student leaders from four Kasetsart University campuses. The seminar aims to exchange ideas, share best practices in student governance, and foster unity and cooperation across campuses.',
        },
        {
          title: 'Seminar of Student Council—4 Campuses 2024',
          issuer: 'สัมมนาสภานิสิต 4 วิทยาเขต',
          date: '2024-02-23',
          tags: ['KU', 'Student Council'],
          images: [SEM1, SEM2, SEM3, SEM4, SEM5, SEM6, SEM7, SEM8],
          verified: false,
          desc: '',
        },
        {
          title: 'Seminar of Student Council—4 Campuses 2025',
          issuer: 'สัมมนาสภานิสิต 4 วิทยาเขต',
          date: '2025-02-22',
          tags: ['KU', 'Student Council'],
          images: [SE1, SE2, SE3, SE4, SE5, SE6, SE7],
          verified: false,
          desc: 'The Seminar of Student Organizations—4 Campuses FEBRUARY 22-23, 2025 is a collaborative program that brings together student leaders from four Kasetsart University campuses. The seminar aims to exchange ideas, share best practices in student governance, and foster unity and cooperation across campuses.',
        },
        {
          title: 'Figma Workshop',
          issuer: 'Figma Design – University Leave Request Form',
          date: '2025-07-10',
          tags: ['KU', 'Figma'],
          images: [F1, F2, F3, F4, F5, F6, F7],
          verified: false,
          desc: 'Designed a digital prototype of the official university leave request form using Figma. The project focused on creating a clear, user-friendly interface that simplifies the leave application process for students and ensures accessibility across devices.',
        }
      ],
    }
  },
  computed: {
    allTags() {
      const s = new Set()
      this.items.forEach(i => i.tags.forEach(t => s.add(t)))
      return Array.from(s)
    },
    filtered() {
      return this.activeTag === 'All'
        ? this.items
        : this.items.filter(i => i.tags.includes(this.activeTag))
    },
  },
  methods: {
    // รูปหน้าปก: ถ้ามี images[] ใช้อันแรก, ถ้าไม่มีก็ใช้ image ปกติ
    coverOf(item) {
      return item.images?.[0] ?? item.image
    },
    open(item) {
      this.active = item
      this.activeIndex = 0
      // ดักคีย์บอร์ดลูกศร
      window.addEventListener('keydown', this.onKey)
    },
    close() {
      this.active = null
      window.removeEventListener('keydown', this.onKey)
    },
    onKey(e) {
      if (!this.active) return
      if (e.key === 'Escape') this.close()
      if (e.key === 'ArrowRight') this.next()
      if (e.key === 'ArrowLeft') this.prev()
    },
    currentImage() {
      // คืน src รูปที่ต้องแสดงใน lightbox
      if (!this.active) return ''
      if (this.active.images?.length) return this.active.images[this.activeIndex]
      return this.active.image
    },
    next() {
      if (!this.active?.images?.length) return
      this.activeIndex = (this.activeIndex + 1) % this.active.images.length
    },
    prev() {
      if (!this.active?.images?.length) return
      this.activeIndex =
        (this.activeIndex - 1 + this.active.images.length) % this.active.images.length
    },
    go(i) {
      if (!this.active?.images?.length) return
      this.activeIndex = i
    },
    formatDate(iso) {
      try {
        const d = new Date(iso)
        return d.toLocaleDateString(undefined, { year: 'numeric', month: 'short', day: '2-digit' })
      } catch { return iso }
    }
  }
}
</script>

<style scoped>
/* ===== Lightbox Viewer (หลายรูป) ===== */
.viewer {
  position: relative;
  background: #0b1220;
  display: flex;
  flex-direction: column;
  gap: .6rem;
  padding: .6rem;
}

.viewer img {
  width: 100%;
  height: 100%;
  max-height: 65vh;
  object-fit: contain;
  background: #0b1220;
  border-radius: 8px;
}

/* ปุ่มเลื่อนซ้าย/ขวา */
.nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, .45);
  color: #fff;
  border: none;
  width: 42px;
  height: 42px;
  border-radius: 999px;
  cursor: pointer;
  display: grid;
  place-items: center;
  font-size: 1.4rem;
  transition: background .2s ease, transform .15s ease;
}

.nav:hover {
  background: rgba(0, 0, 0, .65);
  transform: translateY(-50%) scale(1.05);
}

.nav.prev {
  left: 20px;
}

.nav.next {
  right: 20px;
}

/* แถบ thumbnails */
.thumbs {
  display: flex;
  gap: .5rem;
  justify-content: center;
  flex-wrap: wrap;
  padding-top: .2rem;
}

.thumb-btn {
  width: 64px;
  height: 48px;
  border: none;
  padding: 0;
  cursor: pointer;
  background: transparent;
  border-radius: 6px;
  overflow: hidden;
  opacity: .7;
  transition: opacity .2s ease, transform .15s ease, box-shadow .2s ease;
}

.thumb-btn:hover {
  opacity: 1;
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, .25);
}

.thumb-btn.active {
  opacity: 1;
  outline: 2px solid #38bdf8;
  outline-offset: 2px;
}

.thumb-btn img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Badge บอกจำนวนรูปบนการ์ด */
.multi-badge {
  position: absolute;
  bottom: 8px;
  right: 8px;
  background: rgba(0, 0, 0, .55);
  color: #fff;
  font-size: .8rem;
  font-weight: 700;
  padding: .25rem .5rem;
  border-radius: 8px;
}

.cert-section {
  padding: 3rem 2rem;
  background: linear-gradient(135deg, #1e293b, #0f172a);
  color: #fff;
  border-radius: 16px;
  max-width: 1500px;
  margin: 2rem auto;
  margin-top: 500px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, .35);
}

.cert-title {
  font-weight: 700;
  font-size: 3.5rem;
  margin-bottom: 1.25rem;
  color: #38bdf8;
  letter-spacing: .5px;
}

/* Filter chips */
.filters {
  display: flex;
  gap: .6rem;
  flex-wrap: wrap;
  margin: .5rem 0 1.5rem;
}

.chip {
  background: rgba(255, 255, 255, .06);
  border: 1px solid rgba(255, 255, 255, .12);
  color: #e2e8f0;
  padding: .55rem 1rem;
  border-radius: 999px;
  cursor: pointer;
  transition: all .25s ease;
  font-size: 1rem;
  font-weight: 600;
}

.chip:hover {
  background: rgba(56, 189, 248, .18);
}

.chip.active {
  background: rgba(56, 189, 248, .24);
  border-color: rgba(56, 189, 248, .5);
  color: #e6f6ff;
}

/* Grid */
/* Grid */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
  /* เดิม 260px → ขยายเป็น 340px */
  gap: 1.8rem;
  /* เว้นระยะระหว่างใบมากขึ้นนิดหน่อย */
}

.card {
  background: rgba(255, 255, 255, .05);
  border: 1px solid rgba(255, 255, 255, .1);
  border-radius: 16px;
  /* มุมโค้งขึ้นอีกนิด */
  overflow: hidden;
  cursor: pointer;
  transition: transform .25s ease, box-shadow .25s ease, border-color .25s ease;
}

.thumb {
  position: relative;
  aspect-ratio: 4 / 3;
  /* อัตราส่วนรูป */
  background: #0b1220;
  overflow: hidden;
}

.thumb img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  /* ให้รูปโชว์เต็ม ไม่โดนตัด */
  background: #fff;
  /* เพิ่มพื้นหลังขาวรองรับใบประกาศ */
}



.thumb {
  position: relative;
  aspect-ratio: 4/3;
  background: #0b1220;
  overflow: hidden;
}

.thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.ribbon {
  position: absolute;
  top: 10px;
  left: -12px;
  background: #22c55e;
  color: #052e16;
  font-weight: 700;
  font-size: .8rem;
  padding: .3rem .8rem;
  transform: rotate(-12deg);
  border-radius: 4px;
}

/* Meta text sizes — ปรับให้ใหญ่ขึ้น */
.meta {
  padding: 1rem 1.1rem 1.2rem;
}

.ctitle {
  font-size: 1.5rem;
  margin: 0 0 .35rem;
  color: #e2e8f0;
  font-weight: 700;
}

.issuer {
  margin: 0 0 .6rem;
  color: #b6c2d3;
  font-size: 1.1rem;
  font-weight: 600;
}

.date {
  font-size: 1rem;
  color: #94a3b8;
}

/* NEW: Certificate tags (แทน .tags/.tag เดิมที่ชน) */
.cert-tags {
  display: flex;
  gap: .8rem;
  flex-wrap: wrap;
  margin: .4rem 0 1rem;
}

.cert-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 1.05rem;
  font-weight: 700;
  color: #1e293b;
  background: #38bdf8;
  padding: .45rem 1rem;
  border-radius: 999px;
  transition: transform .2s ease, box-shadow .2s ease;
}

.cert-tag:hover {
  transform: scale(1.06);
  box-shadow: 0 4px 12px rgba(56, 189, 248, .35);
}


.lightbox {
  position: fixed;
  inset: 0;
  z-index: 50;
  background: rgba(0,0,0,.65);
  display: grid;
  place-items: center;
  padding: 1rem;
  animation: fadeIn .25s ease;
}


@keyframes fadeIn {
  from {
    opacity: 0
  }

  to {
    opacity: 1
  }
}

.lightbox-inner{
  position: relative;            /* สำคัญมาก */
  display: grid;
  grid-template-columns: minmax(320px, 60vw) minmax(280px, 420px);
  gap: 1.2rem;
  background: #0b1220;
  border: 1px solid rgba(255,255,255,.12);
  border-radius: 16px;
  overflow: hidden;
  max-height: 85vh;
  box-shadow: 0 24px 60px rgba(0,0,0,.45);
}

/* ปุ่ม Close มุมขวาล่างของ lightbox-inner */
.close-btn{
  position: absolute;
  right: 14px;
  bottom: 14px;
  z-index: 10;
  background: #38bdf8;
  color: #031926;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 700;
  font-size: 1rem;
  transition: filter .2s ease, transform .15s ease;
}
.close-btn:hover{ filter:brightness(1.06); transform:translateY(-2px); }


.lightbox-inner img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background: #0b1220;
}

.lb-info {
  position: relative;
  /* ต้องใส่เพื่อให้ absolute ทำงานในกรอบนี้ */
  padding: 2rem 1.5rem 3rem;
  /* บวก padding บน/ซ้าย */
  margin-top: 50px;
  margin-right: 50px;
  display: flex;
  flex-direction: column;
  gap: .75rem;
}




.lb-info h3 {
  margin: .25rem 0;
  color: #e2e8f0;
  font-size: 3rem;
  /* เดิม ~1.4rem → ใหญ่ขึ้น */
  font-weight: 800;
  line-height: 1.25;
  letter-spacing: .2px;
}

.lb-info .issuer {
  color: #a5b4fc;
  font-size: 1.5rem;
  /* เดิม ~1.05rem → ใหญ่ขึ้น */
  font-weight: 600;
  line-height: 1.4;
  opacity: .98;
}

.lb-info .desc {
  margin: .5rem 0 0;
  color: #cbd5e1;
  font-size: 1.15rem;
  /* เดิม ~1rem → ใหญ่ขึ้น */
  line-height: 1.7;
}

/* ปุ่ม */
.actions {
  display: flex;
  gap: .7rem;
  margin-top: auto;
}

.btn {
  background: #38bdf8;
  color: #031926;
  border: none;
  padding: .7rem 1.1rem;
  border-radius: 12px;
  cursor: pointer;
  font-weight: 800;
  font-size: 1.5rem;
  transition: filter .2s ease, transform .15s ease;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.btn:hover {
  filter: brightness(1.06);
  transform: translateY(-1px);
}

.btn.outline {
  background: transparent;
  color: #93c5fd;
  border: 1px solid rgba(147, 197, 253, .55);
}

.btn.ghost {
  background: transparent;
  color: #cbd5e1;
  border: 1px solid rgba(148, 163, 184, .3);
}

/* Responsive */
@media (max-width: 900px) {
  .lightbox-inner {
    grid-template-columns: 1fr;
  }

  .lb-info h3 {
    font-size: 1.8rem;
  }

  .lb-info .issuer {
    font-size: 1.15rem;
  }

  .lb-info .desc {
    font-size: 1.05rem;
  }
}
.reveal {
  opacity: 0;
  transform: translateY(16px);
  transition:
    opacity .7s ease,
    transform .7s ease;
  transition-delay: var(--delay, 0ms); /* ใช้คู่กับ style="--delay:xxxms" */
  will-change: opacity, transform;
}
.reveal.in-view {
  opacity: 1;
  transform: translateY(0);
}
/* ====== Hover bounce on each certificate card ====== */

/* ช่วยให้แอนิเมชันลื่น */
.card{ will-change: transform, box-shadow; }

/* เด้งนิด ๆ พร้อมเงาชัดขึ้น */
.card:hover{
  animation: card-pop 260ms cubic-bezier(.22,1,.36,1) both;
  box-shadow: 0 18px 36px rgba(0,0,0,.40);
  border-color: rgba(56,189,248,.45);
}

/* เวลากดค้าง ให้กดลงเล็กน้อย (รู้สึกตอบสนอง) */
.card:active{
  transform: translateY(-2px) scale(.995);
}

/* เด้งนิดเดียว (micro-bounce) */
@keyframes card-pop{
  0%   { transform: translateY(0) scale(1); }
  60%  { transform: translateY(-6px) scale(1.018); }
  100% { transform: translateY(-4px) scale(1.012); }
}

/* รูปขยายจิ๊บ ๆ ตอนโฮเวอร์ */
.card .thumb img{
  transition: transform .25s ease;
}
.card:hover .thumb img{
  transform: scale(1.02);
}

/* โฟกัสด้วยคีย์บอร์ดก็ให้เอฟเฟกต์คล้าย ๆ กัน (a11y) */
.card:focus-visible{
  outline: 2px solid #38bdf8;
  outline-offset: 2px;
  animation: card-pop 260ms cubic-bezier(.22,1,.36,1) both;
}

</style>
