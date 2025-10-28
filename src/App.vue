
<template>
  <div class="app" :class="{ 'reduce-motion': prefersReduced }">
    <!-- Top Bar -->
    <header class="topbar">
      <div class="brand" role="banner" aria-label="한가위 기념 웹페이지">
        <span class="brand-emblem" aria-hidden="true">🌕</span>
        <span class="brand-text">한가위 복福</span>
      </div>
      <nav class="nav" aria-label="빠른 메뉴">
        <a href="#wish" class="nav-link">소원</a>
        <a href="#info" class="nav-link">한가위</a>
        <a href="#games" class="nav-link">놀이</a>
      </nav>
    </header>

    <!-- Hero Section -->
    <section class="hero" aria-labelledby="hero-title">
      <!-- decorative clouds -->
      <div class="cloud cloud-1" aria-hidden="true"></div>
      <div class="cloud cloud-2" aria-hidden="true"></div>

      <div class="moon">
        <img :src="moonImg" alt="보름달 이미지" class="moon-image" />
      </div>


      <!-- Floating Lanterns -->
      <ul class="lanterns" aria-hidden="true">
        <li v-for="l in lanterns" :key="l.id" class="lantern" :style="lanternStyle(l)">
          <svg viewBox="0 0 72 120" class="lantern-svg" aria-hidden="true">
            <defs>
              <linearGradient :id="`lanternBody-${l.id}`" x1="0" y1="0" x2="0" y2="1">
                <stop offset="0%" stop-color="#FFB74D"/>
                <stop offset="100%" stop-color="#F57C00"/>
              </linearGradient>
              <linearGradient :id="`lanternGlow-${l.id}`" x1="0" y1="0" x2="0" y2="1">
                <stop offset="0%" stop-color="#ffdca5"/>
                <stop offset="100%" stop-color="#ffab40"/>
              </linearGradient>
            </defs>
            <rect x="10" y="20" width="52" height="60" rx="18" :fill="`url(#lanternBody-${l.id})`" />
            <rect x="18" y="15" width="36" height="10" rx="4" fill="#5d4037"/>
            <rect x="18" y="82" width="36" height="10" rx="4" fill="#5d4037"/>
            <circle cx="36" cy="60" r="22" :fill="`url(#lanternGlow-${l.id})`" opacity="0.75"/>
            <line x1="36" y1="0" x2="36" y2="15" stroke="#8d6e63" stroke-width="2" />
            <path d="M32 94 h8 l-4 10 z" fill="#5d4037"/>
          </svg>
        </li>
      </ul>

      <div class="hero-inner">
        <h1 id="hero-title">넉넉한 보름달처럼<br/>마음이 가득해지는 한가위</h1>
        <p class="subtitle">가족의 안부를 묻고, 소원을 빌고, 따뜻함을 나눠요.</p>
        <div class="cta-row">
          <button class="btn primary" @click="openWish">소원 빌기</button>
          <button class="btn ghost" @click="addSampleWishes">송편 만들기(샘플)</button>
        </div>
      </div>

      <!-- wave divider -->
      <svg class="wave" viewBox="0 0 1440 140" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,80 C240,10 480,10 720,80 C960,150 1200,150 1440,80 L1440,140 L0,140Z"></path>
      </svg>
    </section>

    <!-- Wishes -->
    <section id="wish" class="section container">
      <header class="section-head">
        <h2>한가위 소원함</h2>
        <p class="section-desc">올해 소원, 다 같이 보름달에 띄워요. (내 브라우저에만 저장돼요)</p>
      </header>

      <form class="wish-form" @submit.prevent="submitWish" aria-label="소원 입력 폼">
        <input
            v-model="wishInput"
            type="text"
            placeholder="예) 가족 모두 건강하게 해주세요"
            class="input"
            aria-label="소원 입력"
        />
        <button class="btn primary" :disabled="!wishInput.trim()">올리기</button>
      </form>

      <ul class="wish-list" role="list">
        <li v-for="w in wishes" :key="w.id" class="wish-card">
          <div class="wish-text">{{ w.text }}</div>
          <div class="wish-meta">
            <time :datetime="w.date">{{ formatDate(w.date) }}</time>
            <button class="btn tiny" @click="removeWish(w.id)" aria-label="소원 삭제">삭제</button>
          </div>
        </li>
        <li v-if="wishes.length === 0" class="wish-empty" aria-live="polite">
          아직 소원이 없어요. 첫 소원을 빌어보세요 🌝
        </li>
      </ul>
    </section>

    <!-- Info -->
    <section id="info" class="section container">
      <header class="section-head">
        <h2>한가위 한눈에 보기</h2>
        <p class="section-desc">풍요와 감사의 명절, 추석을 더 즐겁게!</p>
      </header>
      <div class="grid">
        <article class="card">
          <h3>보름달 관측 팁</h3>
          <ul class="list">
            <li>도심 불빛 적은 곳에서 하늘을 넓게 바라보기</li>
            <li>구름 많은 날엔 서쪽 하늘까지 시선 넓히기</li>
            <li>따뜻한 겉옷과 간단한 간식 준비</li>
          </ul>
        </article>
        <article class="card">
          <h3>차례 음식 아이디어</h3>
          <ul class="list">
            <li>송편(깨·콩·밤) & 전(동그랑땡, 동태)</li>
            <li>나물 3종(시금치·고사리·도라지)</li>
            <li>과일(배·사과) & 식혜</li>
          </ul>
        </article>
        <article class="card">
          <h3>놀이 & 나눔</h3>
          <ul class="list">
            <li>윷놀이, 강강술래, 연 만들기</li>
            <li>이웃과 송편 나누기</li>
            <li>감사 편지 쓰기</li>
          </ul>
        </article>
      </div>
    </section>

    <!-- Games -->
    <section id="games" class="section container">
      <header class="section-head">
        <h2>가벼운 한가위 미니게임</h2>
        <p class="section-desc">보름달 아래 등(燈)을 띄워 점수 얻기!</p>
      </header>

      <div class="game">
        <button class="btn ghost" @click="shootLantern">등 띄우기</button>
        <div class="score">점수: {{ score }}</div>
        <div class="game-sky" aria-label="하늘">
          <div
              class="game-lantern"
              v-for="i in gameLanterns"
              :key="i.key"
              :style="{ left: i.x + '%', animationDelay: i.delay + 'ms' }"
              aria-hidden="true"
          ></div>
        </div>
      </div>
    </section>

    <footer class="footer">
      <p>풍성한 한가위 보내세요 • © {{ new Date().getFullYear() }}</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue'
import moonImg from './assets/moon.jpg'

type Lantern = { id: number; x: number; delay: number; duration: number; scale: number }

const prefersReduced = computed(() =>
    window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
)

// Decorative lanterns
const lanterns = ref<Lantern[]>(
    Array.from({ length: 8 }).map((_, idx) => ({
      id: idx + 1,
      x: 5 + Math.random() * 90,
      delay: Math.floor(Math.random() * 3000),
      duration: 9000 + Math.floor(Math.random() * 5000),
      scale: 0.7 + Math.random() * 0.8
    }))
)

const lanternStyle = (l: Lantern) => ({
  left: `${l.x}vw`,
  animationDelay: `${l.delay}ms`,
  animationDuration: `${l.duration}ms`,
  transform: `scale(${l.scale})`
})

// Wishes (persist in localStorage)
type Wish = { id: string; text: string; date: string }
const LS_KEY = 'hangawi_wishes_v1'
const wishes = ref<Wish[]>([])
const wishInput = ref('')

onMounted(() => {
  try {
    const raw = localStorage.getItem(LS_KEY)
    if (raw) wishes.value = JSON.parse(raw)
  } catch {}
})

watch(wishes, v => localStorage.setItem(LS_KEY, JSON.stringify(v)), { deep: true })

const formatDate = (iso: string) => {
  const d = new Date(iso)
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`
}

const submitWish = () => {
  const text = wishInput.value.trim()
  if (!text) return
  wishes.value.unshift({ id: crypto.randomUUID(), text, date: new Date().toISOString() })
  wishInput.value = ''
}

const removeWish = (id: string) => {
  wishes.value = wishes.value.filter(w => w.id !== id)
}

const openWish = () => {
  const t = prompt('보름달에게 빌 소원을 적어주세요 ✨')
  if (t && t.trim()) {
    wishes.value.unshift({ id: crypto.randomUUID(), text: t.trim(), date: new Date().toISOString() })
  }
}

const addSampleWishes = () => {
  const samples = ['가족 모두 건강하게', '학업과 프로젝트 대박', '마음이 평안하기를']
  samples.forEach(s =>
      wishes.value.push({ id: crypto.randomUUID(), text: s, date: new Date().toISOString() })
  )
}

// Mini game
const score = ref(0)
const gameLanterns = ref<{ key: string; x: number; delay: number }[]>([])
const shootLantern = () => {
  const x = Math.random() * 90 + 5
  const delay = Math.floor(Math.random() * 200)
  const key = crypto.randomUUID()
  gameLanterns.value.push({ key, x, delay })
  score.value += 1
  // remove after animation
  setTimeout(() => {
    gameLanterns.value = gameLanterns.value.filter(l => l.key !== key)
  }, 3000)
}
</script>

<style scoped>
/* CSS Variables */
:root {
  --bg: #0b1022;
  --bg-2: #11183a;
  --text: #eef2ff;
  --muted: #c7d2fe;
  --accent: #ffcf6e;
  --accent-2: #ff9f40;
  --card: #1b234a;
  --card-2: #222c5f;
  --border: #2e3973;
  --shadow: 0 10px 30px rgba(0,0,0,.35);
}

.app {
  min-height: 100dvh;
  color: var(--text);
  background: radial-gradient(1200px 600px at 70% -10%, #19214a 0%, #0b1022 60%) no-repeat,
  linear-gradient(180deg, var(--bg-2), var(--bg));
  font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "Apple SD Gothic Neo", "Noto Sans KR", "Segoe UI Emoji", Arial, sans-serif;
  line-height: 1.6;
  overflow-x: hidden;
}

/* Topbar */
.topbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: clamp(12px, 2vw, 20px) clamp(16px, 3vw, 28px);
  position: sticky;
  top: 0;
  z-index: 20;
  background: linear-gradient(180deg, rgba(11,16,34,0.9), rgba(11,16,34,0.6) 70%, transparent);
  backdrop-filter: saturate(120%) blur(6px);
}
.brand {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: 800;
  letter-spacing: 0.3px;
}
.brand-emblem { font-size: 22px }
.brand-text { font-size: clamp(16px, 2.2vw, 20px) }

.nav {
  display: flex;
  gap: clamp(10px, 2vw, 20px);
}
.nav-link {
  color: var(--muted);
  text-decoration: none;
  font-weight: 600;
  font-size: 14px;
  border: 1px solid transparent;
  padding: 6px 10px;
  border-radius: 999px;
}
.nav-link:hover { border-color: var(--border); color: var(--text) }

/* Hero */
.hero {
  position: relative;
  padding: clamp(70px, 12vw, 140px) 20px 0;
  min-height: 68dvh;
  display: grid;
  place-items: center;
  text-align: center;
}

.moon {
  position: absolute;
  top: clamp(40px, 6vw, 70px);
  right: clamp(10px, 6vw, 70px);
  width: min(46vw, 420px);
  filter: drop-shadow(0 30px 50px rgba(255, 221, 122, .2));
  z-index: 1;
}
.moon-svg { width: 100%; height: auto }

.cloud {
  position: absolute;
  background: radial-gradient(50% 50% at 50% 50%, rgba(255,255,255,.18) 0%, rgba(255,255,255,0) 70%);
  filter: blur(6px);
  width: 50vw; height: 20vw; border-radius: 50%;
  z-index: 0;
  pointer-events: none;
  animation: drift 30s linear infinite;
}
.cloud-1 { top: 12%; left: -10%; animation-delay: -8s }
.cloud-2 { top: 28%; right: -15%; animation-delay: -18s }

@keyframes drift {
  0% { transform: translateX(0) }
  50% { transform: translateX(6vw) }
  100% { transform: translateX(0) }
}

.hero-inner {
  position: relative;
  z-index: 2;
  max-width: 900px;
  padding: 0 12px 60px;
}
.hero h1 {
  font-size: clamp(28px, 5.2vw, 56px);
  line-height: 1.15;
  text-shadow: 0 2px 0 rgba(0,0,0,.2);
}
.subtitle {
  color: var(--muted);
  margin-top: 10px;
  font-size: clamp(14px, 2.4vw, 18px);
}
.cta-row { display: flex; gap: 12px; justify-content: center; margin-top: 22px }

/* Lanterns */
.lanterns {
  list-style: none;
  margin: 0; padding: 0;
  position: absolute; inset: 0;
  pointer-events: none;
}
.lantern {
  position: absolute;
  bottom: -120px;
  width: clamp(26px, 4.5vw, 54px);
  animation: floatUp var(--float-dur, 10s) ease-in infinite;
  opacity: .85;
  will-change: transform;
  transform-origin: 50% 100%;
}
.lantern-svg { width: 100%; height: auto; display: block }
.lantern { filter: drop-shadow(0 8px 14px rgba(0,0,0,.35)) }
.lantern::after {
  content: '';
  position: absolute; bottom: -8px; left: 50%; transform: translateX(-50%);
  width: 2px; height: 10px; background: rgba(255, 200, 120, .8);
  border-radius: 1px; filter: blur(1px);
}

@keyframes floatUp {
  0%   { transform: translateY(0) rotate(0deg) }
  50%  { transform: translateY(-50vh) rotate(2.5deg) }
  100% { transform: translateY(-100vh) rotate(-1.5deg) }
}

.wave {
  width: 100%; height: 100px; display: block; margin-top: 30px;
}
.wave path { fill: var(--bg) }

/* Sections */
.section {
  padding: 56px 0;
}
.container {
  width: min(1100px, 92%);
  margin: 0 auto;
}
.section-head {
  text-align: center;
  margin-bottom: 22px;
}
.section-head h2 {
  font-size: clamp(22px, 3.6vw, 34px);
}
.section-desc {
  color: var(--muted);
  margin-top: 6px;
}

/* Wish form & list */
.wish-form {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 10px;
  margin: 14px 0 22px;
}
.input {
  background: var(--card);
  color: var(--text);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 12px 14px;
  outline: none;
  transition: border .2s ease;
}
.input:focus { border-color: var(--accent) }

.wish-list { display: grid; gap: 12px }
.wish-card {
  background: linear-gradient(180deg, var(--card), var(--card-2));
  border: 1px solid var(--border);
  border-radius: 16px;
  box-shadow: var(--shadow);
  padding: 14px 16px;
}
.wish-text { font-weight: 600 }
.wish-meta {
  display: flex; gap: 10px; justify-content: space-between; align-items: center;
  color: var(--muted); margin-top: 6px; font-size: 13px;
}
.wish-empty {
  text-align: center; color: var(--muted); padding: 24px 10px; border: 1px dashed var(--border);
  border-radius: 14px; background: rgba(255,255,255,0.02);
}

/* Info grid */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}
.card {
  background: linear-gradient(180deg, var(--card), var(--card-2));
  border: 1px solid var(--border);
  border-radius: 16px;
  box-shadow: var(--shadow);
  padding: 16px;
}
.card h3 { margin-bottom: 8px }
.list { padding-left: 18px; margin: 0 }

/* Game */
.game {
  background: linear-gradient(180deg, #151c3d, #0e1430);
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 16px;
  box-shadow: var(--shadow);
}
.score { margin: 8px 0 12px; color: var(--muted) }
.game-sky {
  position: relative;
  height: 180px;
  overflow: hidden;
  border-radius: 12px;
  border: 1px solid var(--border);
  background:
      radial-gradient(800px 260px at 80% -20%, rgba(255,230,150,.15), transparent 60%) no-repeat,
      linear-gradient(180deg, #0c1130 0%, #060a22 100%);
}
.game-lantern {
  position: absolute; bottom: -20px;
  width: 16px; height: 24px; border-radius: 6px;
  background: linear-gradient(180deg, #ffb74d, #f57c00);
  box-shadow: 0 0 12px rgba(255, 180, 80, .8);
  animation: game-float 3s ease-in forwards;
}
@keyframes game-float {
  0%   { transform: translateY(0) }
  70%  { transform: translateY(-140px) }
  100% { transform: translateY(-170px); opacity: 0 }
}

/* Buttons */
.btn {
  cursor: pointer;
  border: 1px solid var(--border);
  background: var(--card);
  color: var(--text);
  padding: 10px 14px;
  border-radius: 12px;
  font-weight: 700;
  transition: transform .06s ease, border-color .2s ease, background .2s ease;
  box-shadow: var(--shadow);
}
.btn:hover { border-color: var(--accent); transform: translateY(-1px) }
.btn:active { transform: translateY(0) }
.btn.primary {
  background: linear-gradient(180deg, var(--accent), var(--accent-2));
  border-color: transparent;
  color: #391c00;
}
.btn.ghost {
  background: transparent;
  border-color: var(--border);
}
.btn.tiny { padding: 6px 10px; font-size: 12px }

/* Footer */
.footer {
  border-top: 1px solid var(--border);
  padding: 26px 12px 46px;
  text-align: center;
  color: var(--muted);
}

/* Responsive */
@media (max-width: 900px) {
  .grid { grid-template-columns: 1fr; }
  .moon { width: min(58vw, 420px); opacity: .95 }
  .nav { gap: 10px }
}

/* Reduced motion */
.reduce-motion .lantern,
.reduce-motion .cloud,
.reduce-motion .game-lantern {
  animation: none !important;
}
.moon-image {
  width: 100%;
  height: auto;
  border-radius: 50%;
  box-shadow: 0 0 40px rgba(255, 225, 150, 0.6);
  filter: brightness(1.05) contrast(1.1);
}

</style>
