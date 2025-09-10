<template>
  <section class="c2-section" id="contact">
    <h1 class="c2-title" v-reveal>Contact</h1>

    <div class="c2-wrap">
      <main class="c2-right">
        <div class="c2-card" v-reveal style="--delay:140ms">
          <h4 class="c2-card-title">Reach me via</h4>
          <ul class="c2-list">
            <li
              v-for="(it, i) in primary"
              :key="'p' + i"
              class="c2-item"
            >
              <a
                class="c2-link"
                :href="it.href"
                target="_blank"
                rel="noreferrer"
              >
                <span class="c2-ico">{{ it.icon }}</span>
                <div class="c2-meta">
                  <span class="c2-label">{{ it.label }}</span>
                  <span class="c2-sub">{{ it.sub }}</span>
                </div>
              </a>

              <button
                v-if="it.copy"
                class="c2-copy"
                @click="copy(it.copy)"
                aria-label="Copy"
              >
                ⧉
              </button>
            </li>
          </ul>
        </div>


        <div class="c2-card" v-reveal style="--delay:200ms">
          <h4 class="c2-card-title">Socials</h4>
          <div class="c2-chips">
            <a
              v-for="(s, i) in socials"
              :key="'s' + i"
              :href="s.href"
              target="_blank"
              rel="noreferrer"
              class="c2-chip"
            >
              <span class="c2-ico">{{ s.icon }}</span>
              <span class="c2-chip-text">{{ s.label }}</span>
            </a>
          </div>
        </div>


        <transition name="c2-toast">
          <div v-if="toast" class="c2-toast">{{ toast }}</div>
        </transition>
      </main>


      <aside class="c2-left" v-reveal style="--delay:80ms">
        <div class="c2-visual">
          <div class="c2-blob"></div>
          <img :src="pic" alt="profile" />
        </div>

        <div class="c2-bio">
          <h3 class="c2-name">{{ profile.name }}</h3>
          <p class="c2-role">{{ profile.role }}</p>
        </div>
      </aside>
    </div>
  </section>
</template>

<script>
import pic from '@/assets/kanya_3.png'

export default {
  name: 'ContactShowcaseAlt',

  directives: {
    reveal: {
      mounted(el) {
        el.classList.add('reveal')
        const io = new IntersectionObserver(
          entries => {
            entries.forEach(e => {
              if (e.isIntersecting) e.target.classList.add('in-view')
              else e.target.classList.remove('in-view')
            })
          },
          { threshold: 0.12 }
        )
        el.__io = io
        io.observe(el)
      },
      unmounted(el) {
        el.__io?.disconnect()
      }
    }
  },

  data() {
    return {
      pic,
      toast: '',
      profile: {
        name: 'Kunyasiri P.',
        role: 'Computer Science • Student',
        email: 'you@example.com'
      },
      primary: [
        {
          icon: '📧',
          label: 'Email',
          sub: 'pksirichotkul@gmail.com',
          href: 'mailto:pksirichotkul@gmail.com',
          copy: 'pksirichotkul@gmail.com'
        },
        {
          icon: '💬',
          label: 'LINE',
          sub: '@kanyasiri_4869',
          href: 'https://line.me/R/ti/p/@kanyasiri_4869',
          copy: 'kanyasiri_4869'
        }
      ],
      socials: [
        { icon: '🐙', label: 'GitHub', href: 'https://github.com/SherlockPK010014' },
        { icon: '📸', label: 'Instagram', href: 'https://www.instagram.com/i3kxnyaz_?utm_source=ig_web_button_share_sheet&igsh=ZDNlZDc0MzIxNw==' },
        { icon: '🔷', label: 'FaceBook', href: 'https://www.facebook.com/share/16xLqbuJNz/?mibextid=wwXIfr' }
      ]
    }
  },

  methods: {
    copy(text) {
      navigator.clipboard.writeText(text).then(() => {
        this.toast = 'Copied to clipboard'
        setTimeout(() => (this.toast = ''), 1600)
      })
    }
  }
}
</script>

<style scoped>
.c2-section{
  margin-top:140px;
  padding:3rem 2rem;
  background:
    radial-gradient(1200px 600px at 10% 0%, rgba(56,189,248,.12), transparent),
    linear-gradient(135deg, #1e293b, #0f172a);
  color:#e2e8f0;
  border-radius:16px;
  max-width:1500px;
  min-height:600px;
  margin-left:auto;
  margin-right:auto;
  box-shadow:0 10px 30px rgba(0,0,0,.35);
}

.c2-title{
  font-size:3rem;
  font-weight:800;
  color:#38bdf8;
  margin-bottom:1.4rem;
}

.c2-wrap{
  display:grid;
  grid-template-columns: 1fr 380px;
  gap:1.6rem;
}
@media (max-width:1060px){
  .c2-wrap{ grid-template-columns: 1fr; }
}


.c2-left{
  display:flex;
  flex-direction:column;
  align-items:center;
  gap:1rem;
  background:rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.12);
  backdrop-filter:blur(8px);
  border-radius:16px;
  padding:1.2rem;
  box-shadow:0 14px 40px rgba(0,0,0,.25);
  max-height:500px;
  overflow:hidden;
}

.c2-visual{
  position:relative;
  width:100%;
  max-width:220px;
  aspect-ratio:3/4;
  overflow:visible;
  flex:0 0 auto;
}
.c2-visual img{
  width:100%;
  height:auto;
  object-fit:contain;
  transform:translateY(0);
  transition:transform .6s cubic-bezier(.22,.61,.36,1);
  will-change:transform;
}
.c2-visual:hover img{ transform:translateY(-6px); }

.c2-blob{
  position:absolute;
  inset:-10% -10% auto -10%;
  height:70%;
  filter:blur(26px);
  background:
    radial-gradient(60% 60% at 40% 40%, rgba(56,189,248,.45), transparent 60%),
    radial-gradient(50% 50% at 70% 70%, rgba(34,211,238,.35), transparent 60%);
  z-index:-1;
}

.c2-bio{
  width:100%;
  text-align:center;
  display:flex;
  flex-direction:column;
  gap:.5rem;
  flex:0 0 auto;
}
.c2-name{ font-size:1.6rem; font-weight:800; color:#e2e8f0; }
.c2-role{ color:#cbd5e1; }


.c2-right{
  display:flex;
  flex-direction:column;
  gap:1.4rem;
  min-height:400px;
}

.c2-card{
  background:rgba(255,255,255,.06);
  border:1px solid rgba(255,255,255,.12);
  border-radius:16px;
  padding:1.2rem 1.2rem 1.1rem;
  box-shadow:0 14px 40px rgba(0,0,0,.25);
  flex:1;
  min-height:240px;

  display:flex;
  flex-direction:column;
  justify-content:flex-start;
}

.c2-card-title{
  font-size:1.2rem;
  font-weight:800;
  color:#e2e8f0;
  margin:0 0 .8rem; 
}

.c2-list{
  list-style:none;
  padding:0;
  margin:0;
  display:grid;
  gap:.8rem;
}

.c2-item{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:.8rem;
  padding:.75rem .8rem;
  border-radius:12px;
  background:rgba(255,255,255,.04);
  border:1px solid rgba(255,255,255,.08);
  transition:
    transform .35s cubic-bezier(.68,-0.55,.27,1.55),
    box-shadow .35s ease,
    border-color .25s ease,
    background .25s ease;
}
.c2-item:hover{
  transform:scale(1.03);
  box-shadow:0 12px 28px rgba(0,0,0,.35);
  border-color:rgba(56,189,248,.28);
  background:rgba(56,189,248,.05);
}

.c2-link{
  display:flex;
  align-items:center;
  gap:.8rem;
  color:#e2e8f0;
  text-decoration:none;
}
.c2-ico{ font-size:1.4rem; }
.c2-meta{ display:flex; flex-direction:column; }
.c2-label{ font-weight:800; }
.c2-sub{ font-size:.95rem; color:#cbd5e1; }

.c2-copy{
  background:transparent;
  color:#9fb7d0;
  border:1px solid rgba(148,163,184,.3);
  padding:.4rem .6rem;
  border-radius:8px;
  cursor:pointer;
  transition: transform .2s ease, border-color .25s ease;
}
.c2-copy:hover{
  transform:translateY(-1px);
  border-color:rgba(56,189,248,.45);
}


.c2-chips{ display:flex; flex-wrap:wrap; gap:.6rem; }
.c2-chip{
  display:inline-flex; align-items:center; gap:.5rem;
  padding:.55rem .8rem;
  border-radius:999px;
  background:rgba(255,255,255,.05);
  border:1px solid rgba(255,255,255,.12);
  color:#e2e8f0;
  text-decoration:none;
  transition:
    transform .35s cubic-bezier(.68,-0.55,.27,1.55),
    box-shadow .35s ease,
    border-color .25s ease,
    background .25s ease;
}
.c2-chip:hover{
  transform:scale(1.06);
  box-shadow:0 10px 22px rgba(0,0,0,.35);
  border-color:rgba(56,189,248,.28);
  background:rgba(56,189,248,.06);
}
.c2-chip-text{ font-weight:700; }


.reveal{
  opacity:0;
  transform:translateY(16px);
  transition: opacity .7s ease, transform .7s ease;
  transition-delay: var(--delay, 0ms);
}
.reveal.in-view{
  opacity:1;
  transform:translateY(0);
}


.c2-toast{
  position:fixed;
  bottom:24px;
  left:50%;
  transform:translateX(-50%);
  background:#0b1220;
  color:#e2e8f0;
  border:1px solid rgba(255,255,255,.12);
  padding:.6rem 1rem;
  border-radius:10px;
  box-shadow:0 10px 24px rgba(0,0,0,.35);
}
.c2-toast-enter-from,
.c2-toast-leave-to{
  opacity:0;
  transform:translateX(-50%) translateY(8px);
}
.c2-toast-enter-active,
.c2-toast-leave-active{
  transition:all .25s ease;
}
</style>
