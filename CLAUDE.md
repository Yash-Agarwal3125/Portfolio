# CLAUDE.md — Yash Agarwal Portfolio Project

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

### Process rule
Build section by section. After each section, verify it against the approved demo before moving on. Do not build the whole file at once and then fix.

### CSS specificity warning
Watch for selector conflicts especially between `.section` padding and individual section overrides. Use section IDs (`#about`, `#projects`) for layout, classes for components.

---

## INCLUDED SKILL: Animation Engineering

### The "Swish" — frame-skip glitch (APPROVED DESIGN ELEMENT)
This is the signature animation. It must be implemented exactly as follows:

**On all section headlines (autonomous, not cursor-triggered):**
```css
.swish::before { color: #00d4ff; animation: sw-a var(--sd) var(--sd2) infinite; }
.swish::after  { color: #ff2d78; animation: sw-b var(--sd) var(--sd2) infinite; }

@keyframes sw-a {
  0%,82%  { transform:translate(0);     opacity:0; clip-path:inset(0 0 100% 0); }
  83%     { transform:translate(-5px,0); opacity:1; clip-path:inset(20% 0 55% 0); }
  85%     { transform:translate(4px,0);  opacity:1; clip-path:inset(60% 0 15% 0); }
  87%     { transform:translate(-3px,0); opacity:1; clip-path:inset(40% 0 30% 0); }
  89%,100%{ transform:translate(0);     opacity:0; clip-path:inset(0 0 100% 0); }
}
@keyframes sw-b {
  0%,83%  { transform:translate(0);    opacity:0; clip-path:inset(0 0 100% 0); }
  84%     { transform:translate(5px,1px);opacity:1; clip-path:inset(55% 0 20% 0); }
  86%     { transform:translate(-4px,0); opacity:1; clip-path:inset(10% 0 65% 0); }
  88%     { transform:translate(2px,-1px);opacity:1; clip-path:inset(75% 0 5% 0); }
  90%,100%{ transform:translate(0);    opacity:0; clip-path:inset(0 0 100% 0); }
}
```
Each headline gets different `--sd` (4s–6.5s) and `--sd2` (0.1s–2.5s) so they never fire simultaneously.

**On the hero name (YASH / AGARWAL) — INVERSE cursor logic:**
- When cursor is FAR from name (>130px from center): `.glitching` class active → swish plays
- When cursor is NEAR name (<130px): `.calm` class active → swish stops, name settles
- On `mouseleave` from hero: revert to `.glitching`

### Lenis Smooth Scroll
```js
// Load from CDN: https://cdn.jsdelivr.net/npm/lenis@1.1.14/dist/lenis.min.js
const lenis = new Lenis({ duration: 1.2, easing: t => Math.min(1, 1.001 - Math.pow(2, -10 * t)) });
function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
```

### GSAP ScrollTrigger — section reveals
```js
// Load GSAP from: https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js
// Load ScrollTrigger from: https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollTrigger.min.js
gsap.registerPlugin(ScrollTrigger);

// Clip-path reveal for section content
gsap.fromTo('.reveal-up', 
  { clipPath: 'inset(0 0 100% 0)', y: 30 },
  { clipPath: 'inset(0 0 0% 0)', y: 0, duration: 0.7, stagger: 0.08,
    scrollTrigger: { trigger: el, start: 'top 80%' }
  }
);
```

### Skill bar animation
Trigger on IntersectionObserver. Bars start at `width: 0%`, animate to `data-w` value on enter. Stagger 55ms per bar.

### Cursor trail
Custom ring cursor `#sv-cur` (28px, `border: 2px solid #ff2d78`, `border-radius: 50%`) lerped at 0.13 speed.
Cursor dot `#sv-dot` (5px, `background: #ff2d78`) follows mx/my directly (no lerp).
On hover over project cards: cursor ring scales to 1.6x.

### Magnetic buttons
On `mousemove` within 80px of button center:
```js
const dx = mx - btnCenterX, dy = my - btnCenterY;
gsap.to(btn, { x: dx * 0.38, y: dy * 0.38, duration: 0.3, ease: 'power2.out' });
// On mouseleave:
gsap.to(btn, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.4)' });
```

---

## INCLUDED SKILL: Single-File HTML Architecture

### File constraints
- Output: one single `index.html` file. No build tools, no npm, no separate CSS/JS files.
- All external resources via CDN only (fonts.googleapis.com, cdn.jsdelivr.net, cdnjs.cloudflare.com).
- No frameworks (no React, no Vue). Vanilla JS only.
- Must run by opening the file directly in a browser (file:// protocol). No server required.
- Target deployment: GitHub Pages at `Yash-Agarwal3125.github.io`

### File structure order
```
1. <!DOCTYPE html> + <html> + <head>
   - meta charset, viewport, title
   - Google Fonts: Bangers + Space Mono
   - Lenis CSS (if any)
2. <style> block — ALL CSS
   - CSS custom properties / design tokens first
   - Reset / base
   - Global elements (cursor, bg, halftone, nav)
   - Section by section
   - Animations / keyframes last
3. <body>
   - #cursor + #cursor-dot
   - #speech-bubble
   - Canvas #bg-canvas (global bg)
   - <nav>
   - <main>
     - #hero
     - #about
     - #projects
     - #skills
     - #experience
     - #contact
4. CDN <script> tags (Lenis, GSAP, ScrollTrigger)
5. <script> block — ALL JS
   - Init Lenis
   - Init GSAP + ScrollTrigger
   - Cursor logic
   - Hero canvas (web nodes + spotlight)
   - Background canvas (Hyd skyline + particles)
   - Name swish toggle (cursor proximity)
   - Project card speech bubble
   - Skill bars IntersectionObserver
   - Magnetic buttons
   - Scroll reveals (GSAP)
   - Nav smooth scroll
```

---

## DESIGN SYSTEM (LOCKED — DO NOT CHANGE)

### Color tokens
```css
:root {
  --bg:        #0a0008;   /* near-black base */
  --bg-2:      #08000f;   /* about section */
  --bg-3:      #040010;   /* projects section */
  --bg-4:      #050008;   /* skills + contact */
  --bg-5:      #06000c;   /* experience */
  --pink:      #ff2d78;   /* primary accent — hot pink */
  --purple:    #7b2fff;   /* secondary accent — electric purple */
  --yellow:    #f0e000;   /* tertiary accent — Spider-Verse yellow */
  --cyan:      #00d4ff;   /* glitch channel colour */
  --white:     #ffffff;
  --muted:     #a78bc4;   /* body text on dark */
  --dim:       #2a0a3a;   /* borders, dividers */
}
```

### Typography
- **Display / headlines:** `'Bangers', cursive` — all section titles, name, nav logo, stat numbers
- **Body / meta:** `'Space Mono', monospace` — descriptions, eyebrows, labels, role text
- **Hero name size:** 100px (YASH), 52px (AGARWAL)
- **Section title size:** 52px
- **Body text size:** 10–11px
- **Eyebrow size:** 9px, letter-spacing: 4px, color: var(--pink)

### Text shadow system
- Hero name: `5px 5px 0 var(--pink), 10px 10px 0 var(--purple), -2px -2px 0 var(--cyan)`
- Section titles alternating: odd → `3px 3px 0 var(--purple)`, even → `3px 3px 0 var(--pink)`
- Stat numbers: `3px 3px 0 var(--pink)`

### Comic offset shadow (cards)
```css
.card::after {
  content: '';
  position: absolute;
  bottom: -4px; right: -4px;
  width: 100%; height: 100%;
  border: 2px solid var(--pink);
  z-index: -1;
}
```
Primary border: `2px solid var(--purple)` on the card itself.

### Halftone dot pattern
```css
.halftone {
  background-image: radial-gradient(circle, #1a0830 1.2px, transparent 1.2px);
  background-size: 20px 20px;
  opacity: 0.45;
}
```
Applied as `::before` pseudo on each section and as a global overlay.

---

## SECTION SPECS (APPROVED LAYOUTS — DO NOT CHANGE STRUCTURE)

### Hero (`#hero`)
- Min-height: 100vh
- `<canvas id="hero-canvas">` fills full section (web nodes + cursor spotlight)
- Corner "bang" labels: top-left "VIT · 2026", top-right "CGPA 8.89", bottom-left "JIO PLATFORMS", bottom-right "AI / ML"
- Name: YASH (100px Bangers, yellow + pink/purple/cyan shadow) + AGARWAL (52px, white + pink shadow)
- Swish: inverse cursor logic (glitch when far, calm when near)
- Role line: "DATA ENGINEER · ML RESEARCHER · BUILDER" (9px Space Mono, pink, letter-spacing 5px)
- CTAs: "VIEW MY WORK →" (filled pink btn) + "RESUME ↗" (outline yellow btn)
- Bottom: "SCROLL ↓" bouncing cue

### About (`#about`)
- Eyebrow: "ORIGIN STORY"
- Title: "WHO AM I?" with swish (`--sd: 5s`, `--sd2: 0.1s`)
- Layout: 2-column grid of stat cards
- Card 1: stat "8.89", label "CGPA · VIT VELLORE", bio text
- Card 2: stat "6+", label "SHIPPED PROJECTS", bio text
- Each card: `border: 2px solid var(--purple)` + pink offset shadow

### Projects (`#projects`)
- Eyebrow: "THE MULTIVERSE OF WORK"
- Title: "PROJECTS" with swish (`--sd: 6s`, `--sd2: 1.2s`)
- Layout: horizontal scroll row of project cards (flex, overflow-x: auto)
- 5 cards: BiLSTM Attention, IPL-Bets, Kafka Banking, YOLO v8→v11, Project Ascension
- Each card: `border: 2px solid var(--pink)` + purple offset shadow
- Hover: card shifts `translate(-3px, -3px)`, pink reveal strip slides up from bottom
- Speech bubble pops on hover with category label

### Skills (`#skills`)
- Eyebrow: "POWER SET"
- Title: "SKILLS" with swish (`--sd: 4.5s`, `--sd2: 2.1s`)
- Layout: bento grid `repeat(4, 1fr)` with Python spanning 2 columns
- 10 skills: Python, PyTorch, Kafka, Spark, FastAPI, YOLO/CV, React, SQL, C/C++, Gemini
- Skill bar: 2px height, `linear-gradient(90deg, var(--pink), var(--purple))`
- Bars animate from 0% on IntersectionObserver

### Experience (`#experience`)
- Eyebrow: "THE JOURNEY"
- Title: "EXPERIENCE" with swish (`--sd: 5.5s`, `--sd2: 0.8s`)
- Layout: vertical dot timeline — 3-column grid (date | dot+stem | content)
- 3 items: Jio Platforms intern, ML Researcher, Senior Mentor
- Dot: 11px circle, `background: var(--pink)`, `border: 2px solid var(--yellow)`
- Stem: 2px wide, `background: var(--dim)`

### Contact (`#contact`)
- Eyebrow: "INTO THE NEXT DIMENSION"
- Title: "CONTACT" with swish (`--sd: 4s`, `--sd2: 1.6s`)
- Layout: full comic-border frame with inner border
- Corner tags: "OPEN TO HIRE", "2026", "SDE / MLE", "PLACEMENT"
- Availability pulse: green dot (`#00ff88`) + "AVAILABLE FOR OPPORTUNITIES"
- Big headline: "LET'S BUILD." (68px, yellow, full shadow)
- 4 CTAs: RESUME (filled pink), GITHUB (outline yellow), LINKEDIN (outline purple), EMAIL (outline yellow)
- Footer: "© 2026 YASH AGARWAL · BUILT WITH CHAOS AND CAFFEINE · VIT VELLORE"

---

## BACKGROUND SYSTEM

### Global background canvas (`#bg-canvas`)
- `position: fixed`, `top: 0`, `left: 0`, `width: 100vw`, `height: 100vh`, `z-index: 0`
- Redraws every frame, parallax offset = `window.scrollY * 0.25`
- Contains two layers:

**Layer 1 — Hyderabad skyline silhouette (static, drawn once):**
Opacity: very subtle (fillStyle rgba values ~0.06–0.09)
Buildings from left to right:
1. Charminar: 4 minarets (8px wide, 120px tall) + central arch rect + pointed tops
2. Hussain Sagar obelisk: thin column + triangle peak
3. IT tower cluster: 4 rectangles varying heights + tiny window dot grid
4. Golconda fort: squat fortress shape + two towers + crenellations hinted
5. Far-right modern tower: slim rectangle + triangle spire
Colors: Charminar in `rgba(255,45,120,0.09)`, IT towers in `rgba(123,47,255,0.07)`, Golconda in `rgba(255,45,120,0.07)`, obelisk in `rgba(240,224,0,0.05)`

**Layer 2 — Floating particles (animated every frame):**
- 55 particles, random x/y, slow upward drift (`vy: -0.1 to -0.4`)
- Colors: alternating `rgba(123,47,255, 0.05–0.2)` and `rgba(255,45,120, 0.05–0.15)`
- Radius: 0.5–2px
- Wrap: when particle exits top, reset to bottom

### Hero canvas (`#hero-canvas`)
- `position: absolute` inside hero section only
- 20 web nodes with connecting lines (purple, opacity proportional to distance)
- Cursor spotlight: radial gradient `rgba(255,45,120,0.14)` centred on cursor
- Nodes near cursor (<160px) turn pink and enlarge to 2.5px

---

## PERFORMANCE RULES

- All canvas drawing uses `requestAnimationFrame` — never `setInterval`
- Particles and web nodes use simple Euler integration — no physics engine
- GSAP ScrollTrigger only on elements that need it — don't over-trigger
- `will-change: transform` on cursor elements only
- `@media (prefers-reduced-motion: reduce)` block: disable all keyframe animations, disable canvas particle animation, keep static layout intact
- Images: none. All visuals are CSS + canvas. Zero image requests.
- Target: Lighthouse performance score > 85 on GitHub Pages

---

## CONTENT (EXACT COPY — USE AS-IS)

### Personal info
- Name: Yash Agarwal
- Role: Data Engineer · ML Researcher · Builder
- Institution: VIT Vellore, B.Tech CSE + AI/ML, 3rd year
- CGPA: 8.89
- Current: Data Engineering Intern, Jio Platforms, Mumbai (May–July 2026)
- Email: [your email]
- GitHub: github.com/Yash-Agarwal3125
- LinkedIn: [your LinkedIn]
- Deploy URL: Yash-Agarwal3125.github.io

### Projects
1. **BiLSTM Attention** — Crowd congestion prediction using physics-based synthetic training data. BiLSTM + Attention mechanism. Research paper with ablation studies and baseline comparisons. Stack: PyTorch, BiLSTM, NumPy
2. **IPL-Bets** — End-to-end sports betting platform with AI prop bet generation. FastAPI + React + Supabase + Gemini 2.0 Flash. 22 prompts, shipped. Stack: FastAPI, React, Gemini, Supabase
3. **Kafka Banking Simulation** — Real-time banking simulation with fraud detection. Kafka producer/consumer pipeline + Spark processing. Stack: Kafka, Spark, Python
4. **YOLO v8→v11** — Object detection model migration. Performance benchmarking, mAP improvements, real-world deployment testing. Stack: YOLO, OpenCV, PyTorch
5. **Project Ascension** — 3000-line idle/management game hidden inside a VS Code shell. Research trees, skill trees, prestige loops, AI narrative, True Ending. Stack: HTML, CSS, JS

### Skills with proficiency %
- Python: 95%
- PyTorch: 88%
- Kafka: 80%
- Spark: 75%
- FastAPI: 85%
- YOLO / CV: 82%
- React: 78%
- SQL / PostgreSQL: 80%
- C / C++: 70%
- Gemini API: 76%

### Experience
1. **Data Engineering Intern** — Jio Platforms, Mumbai · May–Jul 2026 · Real-time data pipelines at scale. Kafka, Spark, distributed systems. Infrastructure serving millions of users daily.
2. **ML Researcher** — VIT Vellore, Self-directed · 2023–Present · BiLSTM attention model for crowd prediction. Physics-based synthetic data generation. Published research with ablation studies.
3. **Senior Mentor** — VIT CS Community · Ongoing · Mentoring junior CS students on tech stacks, projects, and careers. Evaluated agentic coding platforms — recommended Windsurf + Gemini 2.5 Flash.

---

## CDN LINKS (USE EXACTLY THESE — DO NOT CHANGE VERSIONS)

```html
<!-- Google Fonts -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bangers&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">

<!-- Lenis smooth scroll -->
<script src="https://cdn.jsdelivr.net/npm/lenis@1.1.14/dist/lenis.min.js"></script>

<!-- GSAP -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollTrigger.min.js"></script>
```

---

## WHAT DONE LOOKS LIKE

The file is complete when:
- [ ] Opening `index.html` in Chrome shows the hero with cursor spotlight working
- [ ] Moving cursor to name area stops the glitch, moving away restarts it
- [ ] All 5 section headlines swish autonomously at different intervals
- [ ] Hyderabad skyline is visible as a very subtle bg silhouette
- [ ] Particles float upward slowly in the background
- [ ] Project cards show speech bubble on hover and reveal strip on hover
- [ ] Skill bars animate in when skills section scrolls into view
- [ ] Scroll is smooth (Lenis working)
- [ ] GSAP clip-path reveals fire on each section enter
- [ ] Nav links smooth-scroll to correct sections
- [ ] Magnetic effect on CTA buttons
- [ ] `prefers-reduced-motion` disables all animations gracefully
- [ ] File works offline (no server needed beyond CDN fonts/scripts)
- [ ] Deployed to `Yash-Agarwal3125.github.io` and loads correctly