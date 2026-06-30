# CLAUDE.md — Yash Agarwal Portfolio Project (v2 — Updated)

## What this file is
This is the authoritative spec for building `index.html` — a single-file Spider-Verse themed portfolio for Yash Agarwal (B.Tech CSE + AI/ML, VIT Vellore, Jio Platforms intern). Every decision in this file has been approved by Yash after multiple design iteration rounds. Do not deviate from the design system below without explicit instruction.

---

## INCLUDED SKILL: Frontend Design

Approach this as the design lead at a small studio known for giving every client a visual identity that could not be mistaken for anyone else's.

### Ground it in the subject
The subject is Spider-Man: Into the Spider-Verse. Every visual decision must be traceable to the film's aesthetic: halftone dots, comic-book offset shadows, chromatic aberration glitch, Bangers display font, hot pink + electric purple + yellow palette, urban energy. Do not let it drift into generic dark-mode SaaS.

### Design principles
- The hero is a thesis. The cursor-spotlight reveal of the name IS the first impression. Protect it.
- Typography carries personality. Bangers for all display/headline text. Space Mono for all body/meta. No substitutions.
- Structure is information. Each section has a visually distinct layout — never repeat the same card treatment across sections.
- Motion is deliberate. Every animation serves the Spider-Verse brief: snappy frame-skip glitch (the "swish"), not smooth/eased fades.
- Restraint on effects: the signature is the glitch swish + cursor spotlight. Everything else supports these two, never competes.
- **Text must be readable.** Minimum body text: 13px. Minimum label text: 11px. Section title: 56px minimum. Hero name: 110px. Never go below these.

### Process rule
Build section by section. After each section, verify it against the approved demo before moving on. Do not build the whole file at once and then fix.

### CSS specificity warning
Watch for selector conflicts especially between `.section` padding and individual section overrides. Use section IDs (`#about`, `#projects`) for layout, classes for components.

---

## INCLUDED SKILL: Animation Engineering

### The "Swish" — frame-skip glitch (APPROVED DESIGN ELEMENT)
This is the signature animation. It MUST be aggressive and visible. The previous implementation was too weak. Follow this exactly:

**AGGRESSIVE swish keyframes — do not soften these values:**
```css
.swish { position: relative; display: inline-block; }
.swish::before, .swish::after {
  content: attr(data-t);
  position: absolute;
  top: 0; left: 0;
  font-family: 'Bangers', cursive;
  font-size: inherit;
  letter-spacing: inherit;
  line-height: inherit;
  pointer-events: none;
}
.swish::before { color: #00d4ff; animation: sw-a var(--sd, 4s) var(--sd2, 0s) infinite; }
.swish::after  { color: #ff2d78; animation: sw-b var(--sd, 4s) var(--sd2, 0s) infinite; }

@keyframes sw-a {
  0%,78%  { transform:translate(0);      opacity:0;   clip-path:inset(0 0 100% 0); }
  79%     { transform:translate(-8px,0);  opacity:0.9; clip-path:inset(15% 0 60% 0); }
  81%     { transform:translate(7px,-2px);opacity:1;   clip-path:inset(55% 0 10% 0); }
  83%     { transform:translate(-6px,1px);opacity:0.9; clip-path:inset(35% 0 40% 0); }
  85%     { transform:translate(5px,0);   opacity:1;   clip-path:inset(70% 0 5% 0); }
  87%,100%{ transform:translate(0);      opacity:0;   clip-path:inset(0 0 100% 0); }
}
@keyframes sw-b {
  0%,79%  { transform:translate(0);      opacity:0;   clip-path:inset(0 0 100% 0); }
  80%     { transform:translate(8px,2px); opacity:0.9; clip-path:inset(50% 0 25% 0); }
  82%     { transform:translate(-7px,0);  opacity:1;   clip-path:inset(5% 0 70% 0); }
  84%     { transform:translate(6px,-1px);opacity:0.9; clip-path:inset(65% 0 8% 0); }
  86%     { transform:translate(-4px,0);  opacity:1;   clip-path:inset(30% 0 45% 0); }
  88%,100%{ transform:translate(0);      opacity:0;   clip-path:inset(0 0 100% 0); }
}
```

Each headline MUST use different timing so they never fire at the same time:
- WHO AM I: `--sd:4.2s; --sd2:0.1s`
- PROJECTS: `--sd:5.8s; --sd2:1.4s`
- SKILLS: `--sd:3.9s; --sd2:2.6s`
- EXPERIENCE: `--sd:6.1s; --sd2:0.7s`
- CONTACT: `--sd:4.7s; --sd2:3.2s`

**Hero name swish — MUST be MORE aggressive than section headlines:**
```css
.name-swish { position: relative; display: inline-block; }
.name-swish::before, .name-swish::after {
  content: attr(data-t);
  position: absolute;
  top: 0; left: 0;
  font-family: 'Bangers', cursive;
  font-size: inherit;
  letter-spacing: inherit;
  line-height: inherit;
  pointer-events: none;
}
/* Active glitch — plays when cursor is FAR from name */
.name-swish.glitching::before {
  color: #00d4ff;
  animation: name-sw-a 2s 0s infinite;
}
.name-swish.glitching::after {
  color: #ff2d78;
  animation: name-sw-b 2s 0.15s infinite;
}
/* Calm — plays when cursor is NEAR name */
.name-swish.calm::before,
.name-swish.calm::after {
  animation: none;
  opacity: 0;
}

@keyframes name-sw-a {
  0%,60%  { transform:translate(0);       opacity:0;   clip-path:inset(0 0 100% 0); }
  62%     { transform:translate(-12px,0);  opacity:1;   clip-path:inset(10% 0 65% 0); }
  64%     { transform:translate(10px,-3px);opacity:1;   clip-path:inset(60% 0 8% 0);  }
  66%     { transform:translate(-8px,2px); opacity:1;   clip-path:inset(30% 0 45% 0); }
  68%     { transform:translate(6px,0);    opacity:0.8; clip-path:inset(75% 0 3% 0);  }
  70%,100%{ transform:translate(0);       opacity:0;   clip-path:inset(0 0 100% 0); }
}
@keyframes name-sw-b {
  0%,61%  { transform:translate(0);       opacity:0;   clip-path:inset(0 0 100% 0); }
  63%     { transform:translate(12px,3px); opacity:1;   clip-path:inset(55% 0 18% 0); }
  65%     { transform:translate(-10px,0);  opacity:1;   clip-path:inset(8% 0 72% 0);  }
  67%     { transform:translate(8px,-2px); opacity:1;   clip-path:inset(70% 0 5% 0);  }
  69%     { transform:translate(-5px,1px); opacity:0.8; clip-path:inset(25% 0 50% 0); }
  71%,100%{ transform:translate(0);       opacity:0;   clip-path:inset(0 0 100% 0); }
}
```

**Cursor proximity logic for hero name:**
```js
hero.addEventListener('mousemove', function(e) {
  const r = hero.getBoundingClientRect();
  const hx = e.clientX - r.left, hy = e.clientY - r.top;
  const cx = r.width / 2, cy = r.height * 0.44;
  const dist = Math.hypot(hx - cx, hy - cy);
  const near = dist < 150; // slightly larger radius than before
  [ns1, ns2].forEach(n => {
    n.classList.toggle('glitching', !near);
    n.classList.toggle('calm', near);
  });
});
hero.addEventListener('mouseleave', function() {
  [ns1, ns2].forEach(n => { n.classList.add('glitching'); n.classList.remove('calm'); });
});
```

### Lenis Smooth Scroll
```js
// CDN: https://cdn.jsdelivr.net/npm/lenis@1.1.14/dist/lenis.min.js
const lenis = new Lenis({ duration: 1.2, easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)) });
function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
```

### GSAP ScrollTrigger — section reveals
```js
// GSAP CDN: https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js
// ScrollTrigger: https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollTrigger.min.js
gsap.registerPlugin(ScrollTrigger);
gsap.fromTo('.reveal-up',
  { clipPath: 'inset(0 0 100% 0)', y: 30 },
  { clipPath: 'inset(0 0 0% 0)', y: 0, duration: 0.7, stagger: 0.08,
    scrollTrigger: { trigger: el, start: 'top 80%' } }
);
```

### Skill bar animation
IntersectionObserver, bars start 0%, animate to `data-w` on enter, stagger 55ms.

### Cursor
Ring `#sv-cur`: 28px, `border: 2px solid #ff2d78`, `border-radius: 50%`, lerp 0.13.
Dot `#sv-dot`: 5px, `background: #ff2d78`, direct mx/my.
On project card hover: ring scales 1.6x.

### Magnetic buttons
```js
// mousemove within 80px of button center:
gsap.to(btn, { x: dx * 0.38, y: dy * 0.38, duration: 0.3, ease: 'power2.out' });
// mouseleave:
gsap.to(btn, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.4)' });
```

---

## INCLUDED SKILL: Single-File HTML Architecture

### File constraints
- ONE `index.html` file. No build tools, no npm, no separate CSS/JS files.
- All external resources via CDN only.
- Vanilla JS only. No React, no Vue, no jQuery.
- Must run via `file://` protocol in Chrome without a server.
- Deploy target: GitHub Pages at `Yash-Agarwal3125.github.io`

### File structure order
```
1. <!DOCTYPE html> <html> <head>
   - meta charset, viewport, title="Yash Agarwal | Portfolio"
   - Google Fonts (Bangers + Space Mono)
2. <style> block — ALL CSS
   - CSS custom properties first
   - Reset / base
   - Global: cursor, bg canvas, halftone, nav
   - Each section
   - Keyframes last
3. <body>
   - #sv-cur + #sv-dot (cursor)
   - #sv-bub (speech bubble)
   - #bg-canvas (fixed global bg)
   - <nav>
   - <main>
     - #hero
     - #about
     - #projects
     - #skills
     - #experience
     - #contact
4. CDN scripts: Lenis, GSAP, ScrollTrigger
5. <script> block — ALL JS in this order:
   - Lenis init
   - GSAP + ScrollTrigger register
   - Cursor logic
   - Hero canvas (web nodes + spotlight)
   - Bg canvas (Hyd skyline + particles)
   - Name swish proximity toggle
   - Project card speech bubble + reveal
   - Skill bars IntersectionObserver
   - Magnetic buttons
   - Scroll reveals (GSAP)
   - Nav smooth scroll
   - Easter egg: YA logo click
   - Easter egg: hidden konami/secret trigger
```

---

## DESIGN SYSTEM (LOCKED)

### Color tokens
```css
:root {
  --bg:     #0a0008;
  --bg-2:   #08000f;
  --bg-3:   #040010;
  --bg-4:   #050008;
  --bg-5:   #06000c;
  --pink:   #ff2d78;
  --purple: #7b2fff;
  --yellow: #f0e000;
  --cyan:   #00d4ff;
  --white:  #ffffff;
  --muted:  #c4a8e0;
  --dim:    #2a0a3a;
}
```

### Typography (MINIMUM SIZES — NEVER GO BELOW)
- Display / headlines: `'Bangers', cursive`
- Body / meta: `'Space Mono', monospace`
- Hero name YASH: **110px**
- Hero name AGARWAL: **58px**
- Section titles: **56px minimum**
- Body text / descriptions: **13px minimum**
- Label / eyebrow text: **11px minimum**
- Stat numbers: **60px**

### Text shadow system
- Hero name: `6px 6px 0 var(--pink), 12px 12px 0 var(--purple), -3px -3px 0 var(--cyan)`
- Section titles odd: `3px 3px 0 var(--purple)`
- Section titles even: `3px 3px 0 var(--pink)`
- Stat numbers: `3px 3px 0 var(--pink)`

### Comic offset shadow on cards
```css
.card { position: relative; border: 2px solid var(--purple); }
.card::after {
  content: ''; position: absolute;
  bottom: -5px; right: -5px;
  width: 100%; height: 100%;
  border: 2px solid var(--pink);
  z-index: -1;
}
```

### Halftone pattern
```css
section::before {
  content: '';
  position: absolute; inset: 0;
  background-image: radial-gradient(circle, #1a0830 1.2px, transparent 1.2px);
  background-size: 20px 20px;
  opacity: 0.45;
  pointer-events: none;
}
```

---

## SECTION SPECS (APPROVED LAYOUTS)

### Hero (`#hero`)
- `min-height: 100vh`
- Canvas fills full section: web nodes + cursor spotlight
- Corner bangs: TL="VIT · 2026", TR="CGPA 8.78", BL="JIO PLATFORMS", BR="AI / ML"
- Note: CGPA is 8.78 per resume, not 8.89
- Name YASH: 110px Bangers, yellow + full triple shadow
- Name AGARWAL: 58px Bangers, white + pink shadow
- Both name elements use `.name-swish.glitching` by default
- Role: "DATA ENGINEER · DISTRIBUTED SYSTEMS · BUILDER" (11px Space Mono, pink, letter-spacing 5px)
- CTAs: "VIEW MY WORK →" (filled pink) + "RESUME ↗" (outline yellow) — both magnetic
- Scroll cue: "SCROLL ↓" bouncing

### About (`#about`)
- Eyebrow: "ORIGIN STORY"
- Title: "WHO AM I?" — swish `--sd:4.2s --sd2:0.1s`
- 2-column stat cards:
  - Card 1: stat "8.78", label "CGPA · VIT VELLORE", bio: "3rd year B.Tech CSE + AI/ML. Building production systems — Kafka pipelines at 50K events/sec, distributed inference platforms, DQN agents. I ship things that work."
  - Card 2: stat "5+", label "PRODUCTION PROJECTS", bio: "Interning at Jio Platforms, Mumbai — engineered Kafka streaming pipelines processing 50,000+ events/sec. Reduced replication lag by 30%. Open to SDE / MLE roles, placement 2026."

### Projects (`#projects`)
- Eyebrow: "THE MULTIVERSE OF WORK"
- Title: "PROJECTS" — swish `--sd:5.8s --sd2:1.4s`
- Horizontal scroll row, 5 cards
- See exact project content in CONTENT section below
- Speech bubble on hover, reveal strip from bottom, translate(-3,-3) on hover

### Skills (`#skills`)
- Eyebrow: "POWER SET"
- Title: "SKILLS" — swish `--sd:3.9s --sd2:2.6s`
- Bento grid `repeat(4,1fr)`, Python spans 2 cols
- See skill list + percentages in CONTENT section

### Experience (`#experience`)
- Eyebrow: "THE JOURNEY"
- Title: "EXPERIENCE" — swish `--sd:6.1s --sd2:0.7s`
- Vertical dot timeline, 3-col grid (date | dot+stem | content)
- **3 entries — see exact content in CONTENT section**
- Dot: 11px, pink bg, yellow border. Stem: 2px dim.

### Contact (`#contact`)
- Eyebrow: "INTO THE NEXT DIMENSION"
- Title: "CONTACT" — swish `--sd:4.7s --sd2:3.2s`
- Comic-border frame with inner border
- Corner tags: "OPEN TO HIRE" / "2026" / "SDE / MLE" / "PLACEMENT"
- Green pulse dot + "AVAILABLE FOR OPPORTUNITIES"
- Big text: "LET'S BUILD." (68px yellow, full shadow)
- 4 CTAs: RESUME (filled pink), GITHUB (outline yellow), LINKEDIN (outline purple), EMAIL (outline yellow)
- Footer: "© 2026 YASH AGARWAL · BUILT WITH CHAOS AND CAFFEINE · HYDERABAD → VIT → MUMBAI"

---

## BACKGROUND SYSTEM

### Global background canvas (`#bg-canvas`)
- `position: fixed; top:0; left:0; width:100vw; height:100vh; z-index:0; pointer-events:none;`
- Redraws every frame. Parallax: skyline drawn at `baseY - scrollY * 0.15`

**Hyderabad skyline (drawn once per frame at parallax offset):**
- Charminar: rgba(255,45,120,0.09) — 4 minarets + arch + pointed tops
- Hussain Sagar obelisk: rgba(240,224,0,0.05)
- IT tower cluster: rgba(123,47,255,0.07) — 4 towers + window dot grid
- Golconda fort: rgba(255,45,120,0.07) — squat fortress + towers
- Far-right modern tower: rgba(123,47,255,0.06)

**Floating particles (every frame):**
- 55 particles, vy: -0.1 to -0.4, wrap top→bottom
- Colors: rgba(123,47,255,0.08–0.18) and rgba(255,45,120,0.05–0.15)
- Radius: 0.5–2px

### Hero canvas (`#hero-canvas`)
- `position:absolute` inside hero only
- 20 web nodes + connecting purple lines (opacity by distance)
- Pink radial spotlight follows cursor

---

## CONTENT (EXACT — USE VERBATIM)

### Personal info
- Name: Yash Agarwal
- Email: yashagarwal3125@gmail.com
- Phone: +91 96187 69926
- GitHub: github.com/Yash-Agarwal3125
- LinkedIn: linkedin.com/in/yash-agarwal-3125 (use this placeholder if exact URL unknown)
- CGPA: 8.78
- Institution: VIT Vellore, B.Tech CSE + AI/ML Specialization, 2023–Present
- Deploy: Yash-Agarwal3125.github.io

### Projects (USE EXACTLY THESE 5 — sourced from audit, these are the strongest)

**Card 1 — Distributed AI Inference Platform**
- Tag: DISTRIBUTED SYSTEMS
- Name: AI INFERENCE PLATFORM
- Desc: 12 Dockerized microservices with async request queuing via FastAPI + Redis. Kafka-based task routing hitting 50 req/sec at 50ms p99. Adaptive Q-Learning scheduler for dynamic batch optimization.
- Stack chips: FASTAPI, KAFKA, REDIS, DOCKER, PROMETHEUS
- Speech bubble: "INFRA!"

**Card 2 — IPL-Bhai-Log**
- Tag: FULLSTACK · BACKEND
- Name: IPL-BHAI-LOG
- Desc: Full-stack sports prediction platform. SQLAlchemy row-level locking prevents race conditions on concurrent bets. JWT auth, bcrypt, rate limiting, Alembic migrations. ACID-compliant transaction engine.
- Stack chips: FASTAPI, REACT, MYSQL, JWT, SQLALCHEMY
- Speech bubble: "FULLSTACK!"

**Card 3 — Neural Networks Car Project**
- Tag: REINFORCEMENT LEARNING
- Name: SELF-DRIVING DQN
- Desc: Deep Q-Network agent for autonomous navigation using LiDAR sensor data in a Pygame simulation. Complex reward function for lane-keeping + collision avoidance. 50 commits of iterative RL research.
- Stack chips: TENSORFLOW, PYGAME, DQN, KERAS, COLAB
- Speech bubble: "RL AGENT!"

**Card 4 — Placement Mail Tracker**
- Tag: AUTOMATION · AI
- Name: PLACEMENT TRACKER
- Desc: Modular Gmail → Gemini AI → Google Sheets placement pipeline. pytest test suite, GitHub Actions CI, ruff linting. Most production-grade repo: tests + CI + docs + separation of concerns.
- Stack chips: PYTHON, GEMINI, GMAIL API, PYTEST, CI/CD
- Speech bubble: "AUTOMATION!"

**Card 5 — Flight Price Predicter**
- Tag: MACHINE LEARNING
- Name: FLIGHT PREDICTOR
- Desc: From-scratch gradient descent implementation (no sklearn for core model). EDA-heavy preprocessing pipeline, deployed on Render. Built to understand ML fundamentals, not just use them.
- Stack chips: PYTHON, NUMPY, FLASK, RENDER, FROM-SCRATCH
- Speech bubble: "ML!"

### Skills
- Python: 95%
- FastAPI: 88%
- Kafka: 85%
- Docker: 80%
- TensorFlow/Keras: 82%
- React.js: 78%
- SQL/PostgreSQL: 80%
- Redis: 75%
- Gemini API: 76%
- C++ / Java: 72%

### Experience (3 entries — USE EXACTLY THESE)

**Entry 1:**
- Date: MAY–JUL 2026
- Title: DATA ENGINEERING INTERN
- Company: JIO PLATFORMS · MUMBAI
- Desc: Engineered Kafka-based event streaming pipelines processing 50,000+ events/sec across 5 topics. Reduced replication lag and stale-read rate by 30% via consumer group tuning. Instrumented observability with Prometheus + Grafana dashboards, cutting mean detection time for throughput degradation by 25%.

**Entry 2:**
- Date: JUN–JUL 2025
- Title: SOFTWARE ENGINEERING INTERN
- Company: JALA ACADEMY · REMOTE
- Desc: Summer internship focused on software engineering fundamentals and applied development. Built and shipped features in a structured team environment during second year of B.Tech.

**Entry 3:**
- Date: 2023–PRESENT
- Title: B.TECH CSE + AI/ML
- Company: VIT VELLORE · CGPA 8.78
- Desc: Specialization in AI & Machine Learning. Core coursework in distributed systems, computer architecture, neural networks, and image processing. 5+ production projects across ML, data engineering, and full-stack domains.

---

## EASTER EGGS (IMPLEMENT BOTH EXACTLY AS DESCRIBED)

### Easter Egg 1 — "YA." logo click (REVEALED)
When the user clicks the "YA." nav logo, trigger a comic-panel style overlay popup.

**Implementation:**
```js
document.getElementById('sv-logo').addEventListener('click', function() {
  showEasterEgg1();
});

function showEasterEgg1() {
  // Create overlay
  const overlay = document.createElement('div');
  overlay.id = 'ee1-overlay';
  overlay.style.cssText = `
    position:fixed; inset:0; background:rgba(10,0,8,0.92);
    z-index:9999; display:flex; align-items:center; justify-content:center;
    cursor:none; font-family:'Bangers',cursive;
  `;

  // Comic panel content
  overlay.innerHTML = `
    <div style="
      background:#0a0008; border:4px solid #ff2d78; padding:36px 42px;
      max-width:460px; position:relative; text-align:center;
    ">
      <!-- inner border -->
      <div style="position:absolute;inset:8px;border:1px solid #7b2fff;pointer-events:none;"></div>
      <!-- corner tags -->
      <div style="position:absolute;top:-2px;left:20px;background:#f0e000;color:#0a0008;font-size:10px;letter-spacing:2px;padding:3px 10px;border:2px solid #0a0008;">SECRET UNLOCKED</div>
      <div style="position:absolute;bottom:-2px;right:20px;background:#ff2d78;color:#0a0008;font-size:10px;letter-spacing:2px;padding:3px 10px;border:2px solid #0a0008;">CLICK ANYWHERE TO CLOSE</div>

      <!-- speech bubble style heading -->
      <div style="font-size:38px;color:#f0e000;text-shadow:3px 3px 0 #ff2d78;margin-bottom:6px;letter-spacing:3px;">YASH'S FUEL</div>
      <div style="font-size:13px;color:#ff2d78;letter-spacing:4px;margin-bottom:24px;font-family:'Space Mono',monospace;">CLASSIFIED INFORMATION</div>

      <!-- the secret -->
      <div style="font-size:20px;color:#ffffff;letter-spacing:2px;line-height:1.5;margin-bottom:20px;">
        Cold brew coffee,<br>
        dark chocolate brownies,<br>
        and a terminal that<br>
        <span style="color:#f0e000;">never stops compiling.</span>
      </div>

      <div style="font-size:11px;color:#7b2fff;letter-spacing:3px;font-family:'Space Mono',monospace;">
        — the stack behind every project above
      </div>

      <!-- halftone dots decoration -->
      <div style="position:absolute;bottom:20px;left:20px;font-size:28px;color:#7b2fff;opacity:0.3;">☕</div>
      <div style="position:absolute;top:20px;right:20px;font-size:28px;color:#ff2d78;opacity:0.3;">🍫</div>
    </div>
  `;

  document.body.appendChild(overlay);
  overlay.addEventListener('click', () => overlay.remove());
}
```

### Easter Egg 2 — KONAMI CODE (HIDDEN — DO NOT TELL YASH WHAT IT IS)
Trigger: user types the classic Konami code on keyboard: ↑ ↑ ↓ ↓ ← → ← → B A

When triggered, show a fullscreen comic panel with an ASCII / CSS-drawn Spider-Man meme. The meme is the classic "Spider-Man pointing at Spider-Man" but done in pure CSS/text art, with a technical/developer twist: both Spider-Men are labelled — one says "my resume" and the other says "my GitHub README". Caption below: "THEY'RE THE SAME PERSON."

**Implementation:**
```js
const KONAMI = ['ArrowUp','ArrowUp','ArrowDown','ArrowDown','ArrowLeft','ArrowRight','ArrowLeft','ArrowRight','b','a'];
let konamiProgress = 0;

document.addEventListener('keydown', function(e) {
  if (e.key === KONAMI[konamiProgress]) {
    konamiProgress++;
    if (konamiProgress === KONAMI.length) {
      konamiProgress = 0;
      showEasterEgg2();
    }
  } else {
    konamiProgress = 0;
  }
});

function showEasterEgg2() {
  const overlay = document.createElement('div');
  overlay.style.cssText = `
    position:fixed; inset:0; background:rgba(4,0,16,0.97);
    z-index:9999; display:flex; flex-direction:column;
    align-items:center; justify-content:center;
    cursor:none; font-family:'Bangers',cursive;
  `;

  overlay.innerHTML = `
    <div style="text-align:center;max-width:600px;padding:20px;">
      <div style="font-size:14px;color:#ff2d78;letter-spacing:4px;margin-bottom:8px;font-family:'Space Mono',monospace;">↑↑↓↓←→←→BA — YOU FOUND IT.</div>
      <div style="font-size:42px;color:#f0e000;text-shadow:4px 4px 0 #ff2d78;margin-bottom:24px;letter-spacing:4px;">NICE ONE.</div>

      <!-- CSS Spider-Man pointing meme -->
      <div style="display:flex;gap:0;align-items:flex-end;justify-content:center;margin-bottom:16px;">

        <!-- Left Spider-Man figure -->
        <div style="display:flex;flex-direction:column;align-items:center;gap:0;">
          <div style="background:#ff2d78;color:#0a0008;font-size:9px;letter-spacing:1px;padding:4px 10px;border:2px solid #f0e000;margin-bottom:6px;font-family:'Space Mono',monospace;">MY RESUME</div>
          <!-- head -->
          <div style="width:36px;height:36px;background:#ff2d78;border-radius:50%;border:3px solid #f0e000;display:flex;align-items:center;justify-content:center;">
            <div style="font-size:16px;">🕷</div>
          </div>
          <!-- body -->
          <div style="width:28px;height:44px;background:#ff2d78;border:2px solid #f0e000;margin-top:2px;position:relative;display:flex;align-items:center;justify-content:center;">
            <div style="font-size:9px;color:#0a0008;font-family:'Space Mono',monospace;font-weight:700;">SWE</div>
          </div>
          <!-- arm pointing right -->
          <div style="position:relative;margin-top:-30px;margin-left:32px;">
            <div style="width:50px;height:8px;background:#ff2d78;border:2px solid #f0e000;border-radius:4px;transform:rotate(-20deg);"></div>
          </div>
          <!-- legs -->
          <div style="display:flex;gap:4px;margin-top:0;">
            <div style="width:10px;height:28px;background:#ff2d78;border:1px solid #f0e000;"></div>
            <div style="width:10px;height:28px;background:#ff2d78;border:1px solid #f0e000;"></div>
          </div>
        </div>

        <!-- vs / pointing label -->
        <div style="font-size:28px;color:#7b2fff;padding:0 16px;padding-bottom:28px;">👉👈</div>

        <!-- Right Spider-Man figure -->
        <div style="display:flex;flex-direction:column;align-items:center;gap:0;">
          <div style="background:#7b2fff;color:#ffffff;font-size:9px;letter-spacing:1px;padding:4px 10px;border:2px solid #00d4ff;margin-bottom:6px;font-family:'Space Mono',monospace;">MY GITHUB README</div>
          <div style="width:36px;height:36px;background:#7b2fff;border-radius:50%;border:3px solid #00d4ff;display:flex;align-items:center;justify-content:center;">
            <div style="font-size:16px;">🕷</div>
          </div>
          <div style="width:28px;height:44px;background:#7b2fff;border:2px solid #00d4ff;margin-top:2px;display:flex;align-items:center;justify-content:center;">
            <div style="font-size:9px;color:#ffffff;font-family:'Space Mono',monospace;font-weight:700;">SWE</div>
          </div>
          <div style="position:relative;margin-top:-30px;margin-right:32px;">
            <div style="width:50px;height:8px;background:#7b2fff;border:2px solid #00d4ff;border-radius:4px;transform:rotate(200deg);"></div>
          </div>
          <div style="display:flex;gap:4px;margin-top:0;">
            <div style="width:10px;height:28px;background:#7b2fff;border:1px solid #00d4ff;"></div>
            <div style="width:10px;height:28px;background:#7b2fff;border:1px solid #00d4ff;"></div>
          </div>
        </div>
      </div>

      <div style="font-size:26px;color:#ffffff;letter-spacing:3px;margin-bottom:8px;">THEY'RE THE SAME PERSON.</div>
      <div style="font-size:12px;color:#7b2fff;letter-spacing:3px;font-family:'Space Mono',monospace;margin-bottom:20px;">— every developer ever</div>
      <div style="font-size:10px;color:#2a0a3a;letter-spacing:3px;font-family:'Space Mono',monospace;">CLICK ANYWHERE TO CLOSE · YOU'RE CLEARLY A PERSON OF CULTURE</div>
    </div>
  `;

  document.body.appendChild(overlay);
  overlay.addEventListener('click', () => overlay.remove());
}
```

---

## CDN LINKS (USE EXACTLY THESE)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bangers&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/lenis@1.1.14/dist/lenis.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollTrigger.min.js"></script>
```

---

## PERFORMANCE RULES
- All animation via `requestAnimationFrame` — never `setInterval`
- `will-change: transform` on cursor elements only
- `@media (prefers-reduced-motion: reduce)`: disable all keyframe animations, stop canvas particle animation, keep static layout
- Zero image requests — all visuals CSS + canvas
- Target: Lighthouse > 85

---

## WHAT DONE LOOKS LIKE

- [ ] Hero loads with cursor spotlight and web nodes active
- [ ] YASH/AGARWAL name glitches aggressively (4 frames of chromatic shift visible clearly)
- [ ] Moving cursor within 150px of name — glitch stops, name settles cleanly
- [ ] Moving cursor away — glitch restarts immediately
- [ ] All 5 section headlines (WHO AM I / PROJECTS / SKILLS / EXPERIENCE / CONTACT) swish autonomously at different times, never simultaneously
- [ ] Section swish is clearly visible — cyan and pink channels shift 6–12px, 5 keyframe steps
- [ ] Hyderabad skyline subtle in bg, parallax on scroll
- [ ] Particles drift upward slowly
- [ ] Projects horizontal scroll works, 5 cards with correct content from audit
- [ ] Speech bubbles pop on project card hover
- [ ] Skill bars animate in on scroll with correct % values
- [ ] Experience has 3 entries: Jio Platforms, Jala Academy, VIT education
- [ ] Lenis smooth scroll active
- [ ] GSAP reveals on section enter
- [ ] Magnetic buttons on all CTAs
- [ ] Clicking "YA." shows cold coffee brownie easter egg panel
- [ ] Konami code (↑↑↓↓←→←→BA) triggers Spider-Man pointing meme
- [ ] All text ≥13px body, ≥11px labels, hero name ≥110px
- [ ] `prefers-reduced-motion` gracefully disables animation
- [ ] Works offline from `file://`
- [ ] CGPA shown as 8.78 everywhere (not 8.89)