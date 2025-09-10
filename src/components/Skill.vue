
<template>
  <section class="skillga-section" id="skills">
    <h1 class="skillga-title" v-reveal>My Skills</h1>

    <div class="skillga-layout">
      <aside class="ga-left" v-reveal>
        <div class="ga-photo">
          <transition name="fade-swap" mode="out-in">
            <img 
              :key="hovering ? 'alt' : 'main'"
              :src="hovering ? picAlt : pic" 
              alt="profile"
              @mouseover="hovering = true"
              @mouseleave="hovering = false"
            />
          </transition>
        </div>
        <h3 class="ga-name">Kunyasiri P.</h3>
        <p class="ga-role">Computer Science • Student</p>
      </aside>



      <main class="ga-right">
        <div
          v-for="(group, gi) in groups"
          :key="group.category + gi"
          class="ga-acc ga-card"
          v-reveal
          :class="{ open: openIdx === gi }"
          :style="{ '--delay': (gi * 80) + 'ms' }"
        >
          <button class="ga-acc-head" @click="toggle(gi, $event)">
  <div class="ga-acc-title">
    <span class="dot"></span>
    <h4>{{ group.category }}</h4>
  </div>
  <span class="chev" :class="{ open: openIdx === gi }">▾</span>
</button>

          <transition name="ga-acc">
            <div v-show="openIdx === gi" class="ga-acc-body">
              <ul class="ga-bars">
                <li
                  v-for="(s, i) in group.items"
                  :key="s.name + i"
                  class="ga-bar"
                  :style="{ '--w': s.score + '%', '--item-delay': (i * 70) + 'ms', '--inner-delay': `calc(${i*70}ms + 120ms)` }"
                >
                  <div class="ga-bar-top">
                    <span class="label">{{ s.name }}</span>
                    <span class="percent">{{ s.score }}%</span>
                  </div>
                  <div class="ga-bar-outer">
                    <div class="ga-bar-inner" :data-w="s.score"></div>
                  </div>
                  <div class="ga-tags">
                    <span class="ga-tag" v-for="t in s.tags" :key="t">{{ t }}</span>
                  </div>
                  <p v-if="s.desc" class="ga-desc">{{ s.desc }}</p>
                </li>
              </ul>
            </div>
          </transition>
        </div>
      </main>
    </div>
  </section>
</template>

<script>
import pic from '@/assets/kanya_1.png'
import picAlt from '@/assets/kanya_hover.png'
export default {
  name: 'SkillGlassAccordion',
  directives: {
    reveal: {
      mounted(el) {
        el.classList.add('reveal')
        const io = new IntersectionObserver(
          (ents) => ents.forEach(e => {
            if (e.isIntersecting) e.target.classList.add('in-view')
            else e.target.classList.remove('in-view')
          }),
          { threshold: 0.12 }
        )
        el.__io = io; io.observe(el)
      },
      unmounted(el){ el.__io?.disconnect() }
    }
  },
  data() {
    return {
      pic,
      picAlt,
      hovering: false,  
      openIdx: 0,
      groups: [
        {
          category: 'Programming',
          items: [
            { name:'Python',       score: 50, },
            { name:'Java',     score: 30,          },
            { name:'JavaScript',  score: 30, tags:['Flex','Grid','RWD'] },
          ]
        },
        {
          category: 'Web Development',
          items: [
            { name:'Vue.js',   score: 60, tags:['Vite','Router'], desc:'Composition' },
            { name:'HTML/CSS',  score: 62, }
            
          ]
        },
        {
          category: ' AI & ML',
          items: [
            { name:'OpenCV',       score: 45, },
            { name:'DeepFace',     score: 40, },
            { name:'FER',     score: 20,}
          ]
        },
        {
          category: 'Tools',
          items: [
            { name:'Git',       score: 50,},
            { name:'VS Code',     score: 70,},
            { name:'NetBeans',     score: 19,},
            { name:'PowerBI',     score: 25,  },
            { name:'Figma',     score: 50, },
          ]
        },
        {
          category: 'Soft Skills',
          items: [
            { name:'Teamwork',        score: 85, },
            { name:'Critical Thinking', score: 75,},
            { name:'Time Management', score: 75,  },
            { name:'Project Coordinator', score: 80,  },
            { name:'Project Management', score: 85, },
          ]
        }
      ]
    }
  },
  methods: {
    toggle(i, evt){
    evt?.currentTarget?.blur?.()

    const prev = this.openIdx
    this.openIdx = this.openIdx === i ? -1 : i

    this.$nextTick(() => {
      if (prev !== -1) {
        const prevAcc = this.$el.querySelectorAll('.ga-acc')[prev]
        if (prevAcc) this.resetBars(prevAcc)
      }
      if (this.openIdx !== -1) {
        const acc = this.$el.querySelectorAll('.ga-acc')[this.openIdx]
        if (acc) this.playBars(acc)
      }
    })
  },
    resetBars(wrapper){
      wrapper.querySelectorAll('.ga-bar-inner').forEach(el=>{
        el.style.transition = 'none'
        el.style.width = '0px'
        el.style.opacity = '0'
        el.style.transform = 'translateY(6px)'
        void el.offsetWidth
        el.style.transition = ''
      })
      wrapper.querySelectorAll('.ga-bar').forEach(li=>{
        li.style.opacity = '0'
        li.style.transform = 'translateY(10px)'
      })
    },
    playBars(wrapper){
      const bars = [...wrapper.querySelectorAll('.ga-bar')]
      const inners = [...wrapper.querySelectorAll('.ga-bar-inner')]
      bars.forEach((li, idx)=>{
        const delay = idx * 70
        li.style.transition = `opacity .45s ease ${delay}ms, transform .45s ease ${delay}ms`
        li.style.opacity = '0'
        li.style.transform = 'translateY(10px)'
        requestAnimationFrame(()=>{
          li.style.opacity = '1'
          li.style.transform = 'translateY(0)'
        })
      })

      inners.forEach((el, idx)=>{
        const target = (el.dataset.w || '100') + '%'
        el.style.width = '0px'
        el.style.opacity = '0'
        el.style.transform = 'translateY(6px)'
        const delay = idx * 70 + 120
        void el.offsetWidth
        el.style.transition = `width .9s cubic-bezier(.22,.61,.36,1) .05s,
                               opacity .45s ease ${delay}ms,
                               transform .45s ease ${delay}ms`

        requestAnimationFrame(()=>{
          el.style.opacity = '1'
          el.style.transform = 'translateY(0)'
          el.style.width = target
        })
      })
    }
  },
  mounted(){
    this.$nextTick(()=>{
      const first = this.$el.querySelectorAll('.ga-acc')[this.openIdx]
      if (first) this.playBars(first)
    })
  }
}
</script>

<style scoped>

.skillga-section {
  margin-top: 120px; 
  padding: 3rem 2rem;
  background: radial-gradient(1200px 600px at 10% 0%, rgba(56,189,248,.12), transparent),
              linear-gradient(135deg, #1e293b, #0f172a);
  color: #fff;
  border-radius: 16px;
  max-width: 1500px;
  min-height: 400px;
  margin-left: auto;
  margin-right: auto;
  box-shadow: 0 10px 30px rgba(0,0,0,.35);
}
.skillga-title{ font-size:3rem; font-weight:800; color:#38bdf8; margin-bottom:1.4rem; }

.skillga-layout{
  display:grid; grid-template-columns: 360px 1fr; gap:1.4rem;
}
@media (max-width: 980px){ .skillga-layout{ grid-template-columns:1fr; } }


.ga-card{
  background: rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.12);
  backdrop-filter: blur(8px);
  border-radius:16px;
  padding:1.1rem 1.2rem;
  box-shadow: 0 14px 40px rgba(0,0,0,.25);
}


.ga-left .profile{ text-align:center; position: sticky; top: 90px; }
@media (max-width: 980px){ .ga-left .profile{ position: static; } }
.ga-photo{ aspect-ratio:1/1; width: 100%; max-width: 260px; margin: .3rem auto 1rem; overflow:hidden; border-radius:14px; background:#0b1220; }
.ga-photo img{ width:100%; height:100%; object-fit:cover; display:block; }
.ga-name{ margin:.25rem 0 .1rem; font-size:1.6rem; font-weight:800; color:#e2e8f0; }
.ga-role{ color:#cbd5e1; margin-bottom: .8rem; }
.ga-btn{ background:#38bdf8; color:#031926; padding:.55rem 1rem; border-radius:10px; font-weight:800; }
.ga-btn.ghost{ background:transparent; border:1px solid rgba(148,163,184,.3); color:#cbd5e1; }


.ga-acc{ padding:0; overflow:hidden; }
.ga-acc + .ga-acc{ margin-top: .9rem; }

.ga-acc-head{
  width:100%; background:transparent; color:#e2e8f0;
  padding:1rem 1.2rem; display:flex; align-items:center; justify-content:space-between;
  font-weight:800; cursor:pointer;
}
.ga-acc-title{ display:flex; align-items:center; gap:.6rem; }
.dot{ width:10px; height:10px; border-radius:50%; background:linear-gradient(45deg,#38bdf8,#22d3ee); box-shadow: 0 0 10px rgba(34,211,238,.5); }
.chev{ transition: transform .25s ease; }
.chev.open{ transform: rotate(180deg); }

.ga-acc-body{ padding: .6rem 1.2rem 1.2rem; }


.ga-bars{ list-style:none; padding:0; margin:0; display:grid; gap:1rem; }

.ga-bar{
  opacity: 0;
  transform: translateY(10px);
}

.ga-bar-top{ display:flex; align-items:center; justify-content:space-between; margin-bottom:.45rem; }
.label{ font-weight:700; color:#e2e8f0; }
.percent{ color:#a5b4fc; }

.ga-bar-outer{
  height:12px; border-radius:999px; background: rgba(255,255,255,.08);
  border:1px solid rgba(255,255,255,.12); overflow:hidden;
}
.ga-bar-inner{
  height:100%;
  width:0;
  opacity: 0;
  transform: translateY(6px);
  background: linear-gradient(90deg,#38bdf8,#22d3ee);
}


.ga-tags{ display:flex; gap:.35rem; flex-wrap:wrap; margin-top:.35rem; }
.ga-tag{ font-size:.8rem; padding:.18rem .5rem; border-radius:999px; background: rgba(148,163,184,.18); color:#cbd5e1; }
.ga-desc{ margin:.35rem 0 0; color:#cbd5e1; font-size:.95rem; }

.reveal{ opacity:0; transform: translateY(16px); transition: opacity .7s ease, transform .7s ease; transition-delay: var(--delay, 0ms); }
.reveal.in-view{ opacity:1; transform: translateY(0); }

.ga-acc-enter-from, .ga-acc-leave-to{ opacity:0; transform: translateY(-6px); }
.ga-acc-enter-active, .ga-acc-leave-active{ transition: all .25s ease; }

@media (prefers-reduced-motion: reduce){
  .ga-bar, .ga-bar-inner{ transition: none !important; transform:none !important; opacity:1 !important; }
  .reveal{ transition: none !important; transform:none !important; opacity:1 !important; }
  .ga-acc-enter-active, .ga-acc-leave-active{ transition: none !important; }
}
.ga-photo {
  width: 100%;
  max-width: 260px;
  margin: .3rem auto 1rem;
  overflow: visible;  
  border-radius: 0;   
  background: transparent; 
  box-shadow: none;  
}

.ga-photo img {
  width: 100%;
  height: auto;
  object-fit: contain; 
  display: block;
}
.ga-acc{
  position: relative;
  will-change: transform, box-shadow;
  transform: translateZ(0) scale(1);
  transition:
    transform 0.55s cubic-bezier(.68,-0.55,.27,1.55),
    box-shadow 0.4s ease;
}

.ga-acc{
  position: relative;
  will-change: transform, box-shadow;
  transform: translateZ(0) scale(1);
  transition:
    transform 0.55s cubic-bezier(.68,-0.55,.27,1.55),
    box-shadow 0.4s ease;
}

.ga-acc:hover{
  transform: scale(1.06);
  box-shadow: 0 18px 40px rgba(0,0,0,.45);
}

.ga-acc.open{
  transform: scale(1.03); 
  box-shadow: 0 18px 48px rgba(0,0,0,.40);
}

.ga-acc.open:hover{
  transform: scale(1.03);
  box-shadow: 0 18px 48px rgba(0,0,0,.40);
}

.ga-acc:not(:hover):not(.open){
  transform: scale(1);
  box-shadow: 0 14px 40px rgba(0,0,0,.25);
}

.ga-acc-head:focus{ outline: none; }

.fade-swap-enter-active,
.fade-swap-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.fade-swap-enter-from,
.fade-swap-leave-to {
  opacity: 0;
  transform: scale(0.98);
}
.fade-swap-enter-to,
.fade-swap-leave-from {
  opacity: 1;
  transform: scale(1);
}


</style>
