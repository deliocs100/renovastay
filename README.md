cat > /mnt/user-data/outputs/index.html << 'ENDOFFILE'
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Renova Stay — Gestion Locative Nouvelle Génération · Moselle</title>
<meta name="description" content="Renova Stay accompagne les propriétaires en Moselle dans l'optimisation, l'exploitation et la gestion quotidienne de leurs locations courte durée.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;0,700;1,300;1,400;1,500;1,600&family=DM+Sans:ital,opsz,wght@0,9..40,200;0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<style>
/* ═══════════════════════════════════════════════════════════════
   DESIGN TOKENS
═══════════════════════════════════════════════════════════════ */
:root {
  --ink:        #03060C;
  --deep:       #050A14;
  --navy:       #07101E;
  --navy2:      #0B1626;
  --navy3:      #0F1C30;
  --navy4:      #152338;
  --navy5:      #1A2A42;

  --gold:       #C8A84B;
  --gold2:      #DDB95E;
  --gold3:      #EDD080;
  --gold4:      #F7E9B0;
  --goldA6:     rgba(200,168,75,.06);
  --goldA10:    rgba(200,168,75,.10);
  --goldA15:    rgba(200,168,75,.15);
  --goldA25:    rgba(200,168,75,.25);
  --goldA40:    rgba(200,168,75,.40);
  --goldGlow:   rgba(200,168,75,.35);

  --w100: rgba(255,255,255,1.00);
  --w90:  rgba(255,255,255,0.90);
  --w70:  rgba(255,255,255,0.70);
  --w50:  rgba(255,255,255,0.50);
  --w30:  rgba(255,255,255,0.30);
  --w15:  rgba(255,255,255,0.15);
  --w08:  rgba(255,255,255,0.08);
  --w04:  rgba(255,255,255,0.04);
  --w02:  rgba(255,255,255,0.02);

  --border:   rgba(200,168,75,.10);
  --borderHi: rgba(200,168,75,.28);
  --borderXHi:rgba(200,168,75,.50);

  --r12: 12px;  --r16: 16px;  --r20: 20px;
  --r24: 24px;  --r28: 28px;  --r32: 32px;

  --ease-out: cubic-bezier(.16,1,.3,1);
  --ease-in-out: cubic-bezier(.76,0,.24,1);
}

/* ═══════════════════════════════════════════════════════════════
   RESET & BASE
═══════════════════════════════════════════════════════════════ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; font-size: 16px; }
body {
  font-family: 'DM Sans', sans-serif;
  background: var(--ink);
  color: var(--w70);
  overflow-x: hidden;
  cursor: none;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
::selection { background: var(--gold); color: var(--navy); }
::-webkit-scrollbar { width: 3px; }
::-webkit-scrollbar-track { background: var(--ink); }
::-webkit-scrollbar-thumb { background: linear-gradient(to bottom, var(--gold), var(--gold2)); border-radius: 2px; }
a, button { cursor: none; }

/* ═══════════════════════════════════════════════════════════════
   CUSTOM CURSOR — GOLD DOT + RING
═══════════════════════════════════════════════════════════════ */
#cursor-dot {
  position: fixed;
  top: 0; left: 0;
  width: 10px; height: 10px;
  border-radius: 50%;
  background: var(--gold);
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
  transition: width .2s var(--ease-out), height .2s var(--ease-out), background .2s, opacity .2s;
  will-change: transform;
  box-shadow: 0 0 8px var(--goldA40), 0 0 20px var(--goldA25);
}
#cursor-ring {
  position: fixed;
  top: 0; left: 0;
  width: 40px; height: 40px;
  border-radius: 50%;
  border: 1.5px solid var(--gold);
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%);
  transition: width .45s var(--ease-out), height .45s var(--ease-out), opacity .3s, border-color .3s;
  opacity: 0.45;
  will-change: transform;
}
#cursor-label {
  position: fixed;
  top: 0; left: 0;
  pointer-events: none;
  z-index: 9998;
  font-family: 'DM Sans', sans-serif;
  font-size: .6rem;
  font-weight: 700;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: var(--gold);
  opacity: 0;
  transform: translate(-50%, 26px);
  transition: opacity .25s;
  white-space: nowrap;
}
body.cursor-hover #cursor-dot   { width: 6px; height: 6px; }
body.cursor-hover #cursor-ring  { width: 56px; height: 56px; opacity: .2; }
body.cursor-cta   #cursor-dot   { width: 6px; height: 6px; background: var(--gold2); }
body.cursor-cta   #cursor-ring  { width: 72px; height: 72px; opacity: .15; border-color: var(--gold2); }
body.cursor-cta   #cursor-label { opacity: 1; }
body.cursor-click #cursor-dot   { transform: translate(-50%,-50%) scale(.5); }
body.cursor-video #cursor-ring  { width: 80px; height: 80px; border-width: 1px; }

/* ═══════════════════════════════════════════════════════════════
   PAGE LOADER
═══════════════════════════════════════════════════════════════ */
#page-loader {
  position: fixed; inset: 0; z-index: 9000;
  background: var(--ink);
  display: flex; flex-direction: column;
  align-items: center; justify-content: center; gap: 2.5rem;
  transition: opacity .9s var(--ease-out), visibility .9s;
}
#page-loader.loaded { opacity: 0; visibility: hidden; pointer-events: none; }

.loader-wordmark {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2rem; font-weight: 500;
  letter-spacing: .16em; color: var(--w30);
}
.loader-wordmark span { color: var(--gold); }

.loader-progress-wrap {
  width: 200px; height: 1px;
  background: var(--w04); border-radius: 1px; overflow: hidden;
}
.loader-progress-bar {
  height: 100%; width: 0;
  background: linear-gradient(90deg, var(--gold), var(--gold3));
  border-radius: 1px;
  transition: width .08s linear;
}
.loader-pct {
  font-size: .65rem; font-weight: 600;
  letter-spacing: .1em; color: var(--gold);
  font-variant-numeric: tabular-nums;
}

/* ═══════════════════════════════════════════════════════════════
   FLOATING PARTICLES CANVAS
═══════════════════════════════════════════════════════════════ */
#particles-canvas {
  position: fixed; inset: 0;
  pointer-events: none;
  z-index: 1;
  opacity: .35;
}

/* ═══════════════════════════════════════════════════════════════
   NAVIGATION
═══════════════════════════════════════════════════════════════ */
.nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 500;
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 5vw; height: 84px;
  transition: height .5s var(--ease-out), background .5s, box-shadow .5s, backdrop-filter .5s;
}
.nav.scrolled {
  height: 66px;
  background: rgba(3,6,12,.94);
  backdrop-filter: blur(32px) saturate(1.6);
  box-shadow: 0 1px 0 var(--border), 0 16px 48px rgba(0,0,0,.55);
}
.nav-logo {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem; font-weight: 600;
  letter-spacing: .1em; color: var(--w100);
  text-decoration: none; display: flex; align-items: center;
  transition: opacity .25s;
}
.nav-logo:hover { opacity: .75; }
.nav-logo-sep { color: var(--gold); margin: 0 2px; }

.nav-menu { display: flex; gap: 2.5rem; list-style: none; }
.nav-menu a {
  font-size: .72rem; font-weight: 500;
  letter-spacing: .13em; text-transform: uppercase;
  color: var(--w30); text-decoration: none;
  transition: color .25s; position: relative; padding-bottom: 2px;
}
.nav-menu a::after {
  content: ''; position: absolute; bottom: -3px; left: 0; right: 0;
  height: 1px; background: var(--gold);
  transform: scaleX(0); transform-origin: left;
  transition: transform .35s var(--ease-out);
}
.nav-menu a:hover { color: var(--gold2); }
.nav-menu a:hover::after { transform: scaleX(1); }

.nav-cta {
  position: relative; overflow: hidden;
  display: inline-flex; align-items: center; gap: .5rem;
  background: transparent; border: 1px solid var(--borderHi);
  color: var(--gold); padding: .65rem 1.5rem; border-radius: 100px;
  font-family: 'DM Sans', sans-serif;
  font-size: .72rem; font-weight: 600;
  letter-spacing: .1em; text-transform: uppercase;
  text-decoration: none;
  transition: color .35s, border-color .35s, box-shadow .35s;
}
.nav-cta::before {
  content: ''; position: absolute; inset: 0;
  background: var(--gold);
  transform: translateX(-101%);
  transition: transform .4s var(--ease-out);
  border-radius: 100px;
}
.nav-cta span { position: relative; z-index: 1; display: flex; align-items: center; gap: .5rem; }
.nav-cta:hover { color: var(--navy); border-color: var(--gold); box-shadow: 0 0 32px var(--goldA25); }
.nav-cta:hover::before { transform: translateX(0); }

/* ═══════════════════════════════════════════════════════════════
   HERO — VIDEO BG + TYPEWRITER + ROTATING WORDS
═══════════════════════════════════════════════════════════════ */
.hero {
  position: relative; min-height: 100vh;
  display: flex; align-items: center;
  overflow: hidden;
}

.hero-video-wrap {
  position: absolute; inset: 0; z-index: 0;
}
.hero-video-wrap::after {
  content: ''; position: absolute; inset: 0;
  background:
    linear-gradient(to right, rgba(3,6,12,.88) 35%, rgba(3,6,12,.45) 70%, rgba(3,6,12,.7) 100%),
    linear-gradient(to top, rgba(3,6,12,.8) 0%, transparent 40%);
}
.hero-video {
  width: 100%; height: 100%;
  object-fit: cover; object-position: center;
  filter: brightness(.5) saturate(.7);
  transition: opacity 1.2s ease;
}
/* Fallback mesh when no video */
.hero-mesh-fallback {
  position: absolute; inset: 0;
  background:
    radial-gradient(ellipse 80% 70% at 75% 40%, rgba(200,168,75,.055) 0%, transparent 60%),
    radial-gradient(ellipse 60% 80% at 10% 90%, rgba(7,16,30,.9) 0%, transparent 60%),
    linear-gradient(155deg, #03060C 0%, #07101E 55%, #03060C 100%);
}

.hero-grain {
  position: absolute; inset: 0; z-index: 1; opacity: .028; pointer-events: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.88' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 260px;
}

/* Decorative rings */
.hero-ring {
  position: absolute; border-radius: 50%;
  pointer-events: none; z-index: 2;
  animation: ringPulse 8s ease-in-out infinite;
}
.hero-ring-1 {
  width: 640px; height: 640px;
  top: 50%; right: 8%;
  transform: translateY(-50%);
  border: 1px solid rgba(200,168,75,.055);
  animation-delay: 0s;
}
.hero-ring-2 {
  width: 460px; height: 460px;
  top: 50%; right: 8%;
  transform: translateY(-50%);
  border: 1px solid rgba(200,168,75,.08);
  animation-delay: -2s;
}
.hero-ring-3 {
  width: 280px; height: 280px;
  top: 50%; right: 8%;
  transform: translateY(-50%);
  border: 1px solid rgba(200,168,75,.13);
  animation-delay: -4s;
}
@keyframes ringPulse {
  0%, 100% { opacity: .5; }
  50% { opacity: 1; }
}

.hero-content {
  position: relative; z-index: 3;
  max-width: 1240px; margin: 0 auto; width: 100%;
  padding: 140px 5vw 100px;
  display: grid; grid-template-columns: 1fr 420px;
  gap: 5vw; align-items: center;
}

/* Eyebrow */
.hero-eyebrow {
  display: inline-flex; align-items: center; gap: .85rem;
  font-size: .68rem; font-weight: 600;
  letter-spacing: .2em; text-transform: uppercase;
  color: var(--gold); margin-bottom: 2.5rem;
  opacity: 0; animation: animFadeUp .9s .15s var(--ease-out) both;
}
.hero-eyebrow-bar { width: 28px; height: 1px; background: var(--gold); flex-shrink: 0; }

/* Main heading */
.hero-h1 {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(3rem, 6vw, 5.8rem);
  font-weight: 300; line-height: 1.07;
  color: var(--w100); letter-spacing: -.01em;
  margin-bottom: 2rem;
  opacity: 0; animation: animFadeUp 1s .3s var(--ease-out) both;
}
.hero-h1-static { display: block; }
.hero-h1-accent {
  display: block; font-weight: 500; font-style: italic;
  background: linear-gradient(125deg, var(--gold4) 0%, var(--gold3) 30%, var(--gold) 70%, var(--gold2) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
  position: relative;
}

/* Rotating word */
.hero-rotate-wrap {
  display: inline-block; overflow: hidden;
  height: 1.1em; vertical-align: bottom;
}
.hero-rotate-words {
  display: flex; flex-direction: column;
  animation: rotateWords 6s ease-in-out infinite;
}
.hero-rotate-word {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(3rem, 6vw, 5.8rem);
  font-weight: 300; font-style: italic;
  line-height: 1.07; white-space: nowrap;
  background: linear-gradient(125deg, var(--gold4) 0%, var(--gold) 60%, var(--gold2) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
}
@keyframes rotateWords {
  0%,22%  { transform: translateY(0); }
  25%,47% { transform: translateY(-100%); }
  50%,72% { transform: translateY(-200%); }
  75%,97% { transform: translateY(-300%); }
  100%    { transform: translateY(-400%); }
}

/* Hero paragraph */
.hero-p {
  font-size: 1.02rem; font-weight: 300; color: var(--w50);
  line-height: 1.9; max-width: 480px; margin-bottom: 3rem;
  opacity: 0; animation: animFadeUp .9s .5s var(--ease-out) both;
}

/* Hero buttons */
.hero-btns {
  display: flex; gap: 1rem; flex-wrap: wrap;
  opacity: 0; animation: animFadeUp .9s .65s var(--ease-out) both;
}

/* Shared button styles */
.btn-primary {
  display: inline-flex; align-items: center; gap: .65rem;
  background: linear-gradient(135deg, var(--gold3), var(--gold));
  color: var(--navy); border: none; padding: 1.05rem 2.25rem;
  border-radius: 100px; font-family: 'DM Sans', sans-serif;
  font-size: .8rem; font-weight: 700;
  letter-spacing: .07em; text-transform: uppercase;
  text-decoration: none;
  box-shadow: 0 4px 28px rgba(200,168,75,.22), inset 0 1px 0 rgba(255,255,255,.15);
  transition: transform .28s var(--ease-out), box-shadow .28s, filter .28s;
  position: relative; overflow: hidden;
}
.btn-primary::before {
  content: ''; position: absolute; top: 0; left: -100%; width: 60%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,.25), transparent);
  transform: skewX(-20deg);
  transition: left .6s var(--ease-out);
}
.btn-primary:hover { transform: translateY(-3px); box-shadow: 0 14px 48px rgba(200,168,75,.4); filter: brightness(1.06); }
.btn-primary:hover::before { left: 160%; }

.btn-secondary {
  display: inline-flex; align-items: center; gap: .65rem;
  background: transparent; border: 1px solid var(--w15);
  color: var(--w50); padding: 1.05rem 2.25rem;
  border-radius: 100px; font-family: 'DM Sans', sans-serif;
  font-size: .8rem; font-weight: 400;
  letter-spacing: .05em; text-transform: uppercase;
  text-decoration: none;
  transition: border-color .3s, color .3s, background .3s, transform .25s;
}
.btn-secondary:hover { border-color: var(--borderHi); color: var(--gold2); background: var(--goldA6); transform: translateY(-2px); }

/* Hero stat cards */
.hero-stats {
  display: flex; flex-direction: column; gap: 1.1rem;
  opacity: 0; animation: animFadeUp .9s .5s var(--ease-out) both;
}
.hstat {
  background: rgba(255,255,255,.025);
  border: 1px solid var(--border);
  border-radius: var(--r20); padding: 1.4rem 1.75rem;
  display: flex; align-items: center; gap: 1.2rem;
  backdrop-filter: blur(20px);
  position: relative; overflow: hidden;
  transition: border-color .35s, background .35s, transform .35s var(--ease-out);
}
.hstat::before {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(135deg, var(--goldA6), transparent 60%);
  opacity: 0; transition: opacity .35s;
}
.hstat:hover { border-color: var(--borderHi); background: rgba(200,168,75,.04); transform: translateX(8px); }
.hstat:hover::before { opacity: 1; }
.hstat-icon {
  width: 46px; height: 46px; border-radius: var(--r12); flex-shrink: 0;
  background: var(--goldA10); border: 1px solid var(--goldA25);
  display: flex; align-items: center; justify-content: center; font-size: 1.2rem;
}
.hstat-title { font-size: .95rem; font-weight: 600; color: var(--w90); margin-bottom: .2rem; }
.hstat-sub { font-size: .78rem; font-weight: 300; color: var(--w50); line-height: 1.55; }
.hstat-live {
  position: absolute; top: 1.1rem; right: 1.25rem;
  display: flex; align-items: center; gap: .35rem;
  font-size: .6rem; font-weight: 700; letter-spacing: .1em;
  text-transform: uppercase; color: #4ade80;
}
.hstat-live-dot {
  width: 6px; height: 6px; border-radius: 50%;
  background: #4ade80; box-shadow: 0 0 8px #4ade80;
  animation: livePulse 2s ease infinite;
}
@keyframes livePulse { 0%,100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.4); opacity: .6; } }

/* Scroll indicator */
.hero-scroll-hint {
  position: absolute; bottom: 2.5rem; left: 50%; transform: translateX(-50%);
  z-index: 3; display: flex; flex-direction: column; align-items: center; gap: .6rem;
  opacity: 0; animation: animFadeIn 1s 1.8s both;
}
.hero-scroll-label {
  font-size: .6rem; font-weight: 700; letter-spacing: .2em;
  text-transform: uppercase; color: var(--w30);
}
.hero-scroll-line {
  width: 1px; height: 48px;
  background: linear-gradient(to bottom, var(--gold), transparent);
  animation: scrollDrop 2.2s ease infinite;
}
@keyframes scrollDrop {
  0% { transform: scaleY(0); transform-origin: top; opacity: 0; }
  40% { transform: scaleY(1); transform-origin: top; opacity: 1; }
  80% { transform: scaleY(0); transform-origin: bottom; opacity: 0; }
  100% { opacity: 0; }
}

/* ═══════════════════════════════════════════════════════════════
   MARQUEE BAND
═══════════════════════════════════════════════════════════════ */
.marquee-band {
  background: var(--gold); overflow: hidden;
  padding: .8rem 0; position: relative; z-index: 10;
  border-top: 1px solid rgba(0,0,0,.1);
}
.marquee-track {
  display: flex; width: max-content;
  animation: marqueeScroll 26s linear infinite;
}
.marquee-track:hover { animation-play-state: paused; }
.marquee-item {
  display: flex; align-items: center; gap: 2.5rem;
  padding: 0 2.5rem; white-space: nowrap;
  font-size: .7rem; font-weight: 700;
  letter-spacing: .16em; text-transform: uppercase;
  color: var(--navy);
}
.marquee-sep { width: 4px; height: 4px; border-radius: 50%; background: var(--navy); opacity: .35; }
@keyframes marqueeScroll { from { transform: translateX(0); } to { transform: translateX(-50%); } }

/* ═══════════════════════════════════════════════════════════════
   SECTION SYSTEM
═══════════════════════════════════════════════════════════════ */
.section { padding: 140px 5vw; position: relative; }
.bg-ink   { background: var(--ink); }
.bg-deep  { background: var(--deep); }
.bg-navy  { background: var(--navy); }
.bg-navy2 { background: var(--navy2); }
.bg-navy3 { background: var(--navy3); }

.container { max-width: 1200px; margin: 0 auto; }

.section-tag {
  display: inline-flex; align-items: center; gap: .75rem;
  font-size: .67rem; font-weight: 700;
  letter-spacing: .2em; text-transform: uppercase;
  color: var(--gold); margin-bottom: 1.5rem;
}
.section-tag-bar { width: 20px; height: 1px; background: var(--gold); flex-shrink: 0; }

h2.serif {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(2.1rem, 4.2vw, 3.5rem);
  font-weight: 400; color: var(--w100);
  line-height: 1.13; letter-spacing: -.01em;
  margin-bottom: 1.5rem;
}
h2.serif em { font-style: italic; font-weight: 300; color: var(--gold2); }

.section-lead {
  font-size: .98rem; font-weight: 300; color: var(--w50);
  line-height: 1.88; max-width: 530px;
}

/* Gold divider */
.gold-rule { height: 1px; background: linear-gradient(90deg, transparent, var(--gold), transparent); opacity: .18; margin: 0; }

/* ═══════════════════════════════════════════════════════════════
   REVEAL ANIMATIONS
═══════════════════════════════════════════════════════════════ */
.rev-up    { opacity: 0; transform: translateY(32px); transition: opacity .85s var(--ease-out), transform .85s var(--ease-out); }
.rev-left  { opacity: 0; transform: translateX(-32px); transition: opacity .85s var(--ease-out), transform .85s var(--ease-out); }
.rev-right { opacity: 0; transform: translateX(32px); transition: opacity .85s var(--ease-out), transform .85s var(--ease-out); }
.rev-scale { opacity: 0; transform: scale(.96); transition: opacity .85s var(--ease-out), transform .85s var(--ease-out); }
.revealed  { opacity: 1 !important; transform: none !important; }

/* Word-by-word reveal */
.word-reveal .word {
  display: inline-block; opacity: 0; transform: translateY(14px);
  transition: opacity .5s var(--ease-out), transform .5s var(--ease-out);
}
.word-reveal.revealed .word { opacity: 1; transform: translateY(0); }

/* ═══════════════════════════════════════════════════════════════
   SERVICES SECTION
═══════════════════════════════════════════════════════════════ */
.services-header {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 4rem; align-items: end; margin-bottom: 5rem;
}

.services-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 1.5px; background: var(--border);
  border: 1px solid var(--border); border-radius: var(--r28); overflow: hidden;
}
.service-card {
  background: var(--navy);
  padding: 3rem 2.75rem;
  position: relative; overflow: hidden;
  transition: background .4s;
}
.service-card::after {
  content: ''; position: absolute; bottom: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  transform: scaleX(0); transform-origin: left;
  transition: transform .5s var(--ease-out);
}
.service-card:hover { background: rgba(200,168,75,.04); }
.service-card:hover::after { transform: scaleX(1); }
.service-card:hover .service-num { color: rgba(200,168,75,.12); }

.service-num {
  font-family: 'Cormorant Garamond', serif;
  font-size: 4.5rem; font-weight: 600; line-height: 1;
  color: rgba(200,168,75,.05);
  position: absolute; top: 2rem; right: 2rem;
  transition: color .4s;
  pointer-events: none;
}
.service-chip {
  display: inline-block;
  background: var(--goldA10); border: 1px solid var(--goldA25);
  color: var(--gold2); border-radius: 100px;
  font-size: .64rem; font-weight: 700;
  letter-spacing: .1em; text-transform: uppercase;
  padding: .28rem .85rem; margin-bottom: 1.5rem;
  transition: background .3s;
}
.service-card:hover .service-chip { background: rgba(200,168,75,.2); }
.service-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem; font-weight: 500;
  color: var(--w100); margin-bottom: .85rem; line-height: 1.3;
}
.service-desc { font-size: .88rem; font-weight: 300; color: var(--w50); line-height: 1.82; max-width: 340px; }

/* Tilt effect applied via JS */
.tilt-card { transform-style: preserve-3d; }

/* ═══════════════════════════════════════════════════════════════
   PLATFORM STRIP
═══════════════════════════════════════════════════════════════ */
.platform-strip {
  background: var(--navy); padding: 4rem 5vw;
  border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
}
.platform-strip-inner {
  max-width: 1200px; margin: 0 auto;
  display: flex; align-items: center; gap: 4rem; flex-wrap: wrap;
}
.platform-label {
  font-size: .64rem; font-weight: 700;
  letter-spacing: .18em; text-transform: uppercase;
  color: var(--w30); white-space: nowrap; flex-shrink: 0;
}
.platform-list { display: flex; gap: 1rem; flex-wrap: wrap; }
.platform-pill {
  display: flex; align-items: center; gap: .6rem;
  background: var(--w02); border: 1px solid var(--border);
  border-radius: 100px; padding: .6rem 1.3rem;
  font-size: .8rem; font-weight: 500; color: var(--w50);
  transition: border-color .25s, background .25s, color .25s, transform .2s var(--ease-out);
}
.platform-pill:hover { border-color: var(--borderHi); background: var(--goldA10); color: var(--gold2); transform: translateY(-2px); }
.platform-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }

/* ═══════════════════════════════════════════════════════════════
   PILIERS / APPROCHE
═══════════════════════════════════════════════════════════════ */
.piliers-intro {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 4rem; align-items: end; margin-bottom: 5rem;
}
.piliers-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 1.75rem; }

.pilier-card {
  background: var(--navy2); border: 1px solid var(--border);
  border-radius: var(--r24); padding: 2.75rem 2.25rem;
  position: relative; overflow: hidden;
  transition: border-color .4s, transform .45s var(--ease-out), box-shadow .45s;
}
.pilier-card::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  transform: scaleX(0); transform-origin: center;
  transition: transform .55s var(--ease-out);
}
.pilier-card:hover { border-color: var(--borderHi); transform: translateY(-10px); box-shadow: 0 30px 72px rgba(0,0,0,.55), inset 0 0 0 1px var(--borderHi); }
.pilier-card:hover::before { transform: scaleX(1); }
.pilier-card:hover .pilier-num { color: rgba(200,168,75,.14); }

.pilier-icon {
  width: 46px; height: 46px; border-radius: var(--r12);
  background: var(--goldA10); border: 1px solid var(--goldA25);
  display: flex; align-items: center; justify-content: center;
  font-size: 1.15rem; margin-bottom: 1.75rem;
  transition: background .3s;
}
.pilier-card:hover .pilier-icon { background: rgba(200,168,75,.2); }
.pilier-num {
  font-family: 'Cormorant Garamond', serif;
  font-size: 5rem; font-weight: 600; line-height: 1;
  color: rgba(200,168,75,.055); display: block; margin-bottom: 1.25rem;
  transition: color .4s;
}
.pilier-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.45rem; font-weight: 500; color: var(--w100); margin-bottom: .75rem;
}
.pilier-desc { font-size: .875rem; font-weight: 300; color: var(--w50); line-height: 1.82; }

/* ═══════════════════════════════════════════════════════════════
   COMPARISON TABLE — Renova Stay vs Solo
═══════════════════════════════════════════════════════════════ */
.compare-section { }
.compare-intro { margin-bottom: 4rem; }
.compare-table { border: 1px solid var(--border); border-radius: var(--r24); overflow: hidden; }
.compare-head {
  display: grid; grid-template-columns: 1fr 1fr 1fr;
  background: var(--navy3); border-bottom: 1px solid var(--border);
}
.compare-head-cell {
  padding: 1.5rem 2rem; font-size: .78rem; font-weight: 600;
  letter-spacing: .05em; color: var(--w50);
  border-right: 1px solid var(--border);
}
.compare-head-cell:last-child { border-right: none; }
.compare-head-cell.highlight {
  color: var(--gold2);
  background: rgba(200,168,75,.05);
  font-weight: 700;
  display: flex; align-items: center; gap: .5rem;
}
.compare-head-cell.highlight::before {
  content: '★'; color: var(--gold); font-size: .85rem;
}
.compare-row {
  display: grid; grid-template-columns: 1fr 1fr 1fr;
  border-bottom: 1px solid var(--border);
  transition: background .25s;
}
.compare-row:last-child { border-bottom: none; }
.compare-row:hover { background: var(--goldA6); }
.compare-cell {
  padding: 1.35rem 2rem; font-size: .875rem; font-weight: 300;
  color: var(--w50); border-right: 1px solid var(--border);
  display: flex; align-items: center; gap: .65rem;
  line-height: 1.5;
}
.compare-cell:last-child { border-right: none; }
.compare-cell.feature { color: var(--w70); font-weight: 400; }
.compare-cell.rs { color: var(--w80); background: rgba(200,168,75,.025); }
.compare-icon-yes { color: #4ade80; font-size: 1rem; flex-shrink: 0; }
.compare-icon-no  { color: #f87171; font-size: 1rem; flex-shrink: 0; }
.compare-icon-meh { color: #fbbf24; font-size: .9rem; flex-shrink: 0; }

/* ═══════════════════════════════════════════════════════════════
   PROMISE SECTION
═══════════════════════════════════════════════════════════════ */
.promise-grid {
  display: grid; grid-template-columns: repeat(2,1fr);
  gap: 1.25rem; margin-top: 3.5rem;
}
.promise-card {
  background: var(--w02); border: 1px solid var(--border);
  border-radius: var(--r20); padding: 2rem 2.25rem;
  display: flex; gap: 1.25rem; align-items: flex-start;
  transition: border-color .3s, background .3s, transform .3s var(--ease-out);
}
.promise-card:hover { border-color: var(--borderHi); background: var(--goldA6); transform: translateY(-4px); }
.promise-icon {
  width: 44px; height: 44px; border-radius: var(--r12); flex-shrink: 0;
  background: var(--goldA10); border: 1px solid var(--goldA25);
  display: flex; align-items: center; justify-content: center; font-size: 1.1rem;
}
.promise-title { font-size: 1rem; font-weight: 600; color: var(--w90); margin-bottom: .4rem; }
.promise-desc { font-size: .85rem; font-weight: 300; color: var(--w50); line-height: 1.7; }

/* ═══════════════════════════════════════════════════════════════
   PROCESS TIMELINE
═══════════════════════════════════════════════════════════════ */
.process-layout { display: grid; grid-template-columns: 1fr 1fr; gap: 7rem; align-items: start; }
.timeline { position: relative; padding-left: 3.25rem; }
.timeline::before {
  content: ''; position: absolute; left: 9px; top: 10px; bottom: 10px; width: 1px;
  background: linear-gradient(to bottom, var(--gold) 0%, rgba(200,168,75,.1) 100%);
}
.timeline-step { position: relative; margin-bottom: 3.25rem; }
.timeline-step:last-child { margin-bottom: 0; }

.tl-dot {
  position: absolute; left: -3.25rem; top: 4px;
  width: 20px; height: 20px; border-radius: 50%;
  border: 1px solid var(--gold); background: var(--ink);
  display: flex; align-items: center; justify-content: center;
  transition: background .3s, box-shadow .3s;
}
.tl-dot::after {
  content: ''; width: 6px; height: 6px; border-radius: 50%;
  background: var(--gold); transition: transform .3s;
}
.timeline-step:hover .tl-dot { background: var(--goldA15); box-shadow: 0 0 18px var(--goldA40); }
.timeline-step:hover .tl-dot::after { transform: scale(1.6); }

.tl-step-num {
  font-size: .62rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--gold); margin-bottom: .55rem;
}
.tl-step-title { font-size: 1.05rem; font-weight: 600; color: var(--w90); margin-bottom: .5rem; }
.tl-step-desc { font-size: .875rem; font-weight: 300; color: var(--w50); line-height: 1.78; }

.process-panel {
  position: sticky; top: 120px;
  background: var(--navy3); border: 1px solid var(--border);
  border-radius: var(--r28); overflow: hidden;
}
.process-panel-head {
  padding: 2.5rem 2.5rem 2rem;
  background: linear-gradient(135deg, var(--goldA10), transparent);
  border-bottom: 1px solid var(--border);
}
.pp-label {
  font-size: .62rem; font-weight: 700; letter-spacing: .15em;
  text-transform: uppercase; color: var(--gold); margin-bottom: 1rem;
}
.pp-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.55rem; font-weight: 400; color: var(--w100); line-height: 1.35;
}
.process-panel-body { padding: 2rem 2.5rem; }
.pp-list { list-style: none; display: flex; flex-direction: column; gap: .85rem; }
.pp-item {
  display: flex; align-items: center; gap: 1rem;
  padding: .85rem 1.1rem; border: 1px solid var(--border); border-radius: var(--r12);
  font-size: .875rem; font-weight: 300; color: var(--w50);
  transition: border-color .25s, background .25s, color .25s, transform .22s var(--ease-out);
}
.pp-item:hover { border-color: var(--borderHi); background: var(--goldA10); color: var(--w80); transform: translateX(5px); }
.pp-item-icon {
  width: 32px; height: 32px; border-radius: 8px; flex-shrink: 0;
  background: var(--goldA10); display: flex; align-items: center; justify-content: center; font-size: .9rem;
}

/* ═══════════════════════════════════════════════════════════════
   FAQ SECTION
═══════════════════════════════════════════════════════════════ */
.faq-layout { display: grid; grid-template-columns: 1fr 1fr; gap: 7rem; align-items: start; }
.faq-list { display: flex; flex-direction: column; gap: .75rem; }
.faq-item {
  border: 1px solid var(--border); border-radius: var(--r16);
  overflow: hidden;
  transition: border-color .3s;
}
.faq-item.open { border-color: var(--borderHi); }
.faq-q {
  display: flex; align-items: center; justify-content: space-between; gap: 1rem;
  padding: 1.5rem 1.75rem; font-size: .95rem; font-weight: 500;
  color: var(--w80); background: transparent; border: none; width: 100%;
  text-align: left; transition: color .25s, background .25s;
}
.faq-q:hover { color: var(--w100); background: var(--w02); }
.faq-item.open .faq-q { color: var(--gold2); }
.faq-arrow {
  width: 28px; height: 28px; border-radius: 50%; flex-shrink: 0;
  border: 1px solid var(--border); background: var(--w02);
  display: flex; align-items: center; justify-content: center;
  transition: transform .35s var(--ease-out), border-color .3s, background .3s;
}
.faq-item.open .faq-arrow { transform: rotate(45deg); border-color: var(--gold); background: var(--goldA10); }
.faq-a {
  max-height: 0; overflow: hidden;
  transition: max-height .45s var(--ease-out), padding .35s;
  padding: 0 1.75rem;
  font-size: .875rem; font-weight: 300; color: var(--w50); line-height: 1.82;
}
.faq-item.open .faq-a { max-height: 300px; padding: 0 1.75rem 1.5rem; }

.faq-visual {
  background: var(--navy3); border: 1px solid var(--border);
  border-radius: var(--r28); overflow: hidden; position: sticky; top: 120px;
}
.faq-visual-top {
  padding: 2.5rem;
  background: linear-gradient(135deg, var(--goldA10), transparent);
  border-bottom: 1px solid var(--border);
}
.faq-visual-label {
  font-size: .62rem; font-weight: 700; letter-spacing: .15em;
  text-transform: uppercase; color: var(--gold); margin-bottom: 1rem;
}
.faq-visual-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem; font-weight: 400; color: var(--w100); line-height: 1.35;
}
.faq-visual-body { padding: 2rem 2.5rem; }
.faq-contact-links { display: flex; flex-direction: column; gap: 1rem; }
.faq-contact-link {
  display: flex; align-items: center; gap: 1.1rem;
  padding: 1.1rem 1.25rem; border: 1px solid var(--border); border-radius: var(--r12);
  text-decoration: none; color: var(--w50); font-size: .88rem; font-weight: 300;
  transition: border-color .25s, background .25s, color .25s, transform .22s var(--ease-out);
}
.faq-contact-link:hover { border-color: var(--borderHi); background: var(--goldA10); color: var(--w80); transform: translateX(5px); }
.faq-contact-icon {
  width: 38px; height: 38px; border-radius: 10px; flex-shrink: 0;
  background: var(--goldA10); border: 1px solid var(--goldA25);
  display: flex; align-items: center; justify-content: center; font-size: 1rem;
}
.faq-contact-text-label { font-size: .65rem; font-weight: 700; letter-spacing: .1em; text-transform: uppercase; color: var(--gold); margin-bottom: .2rem; }
.faq-contact-text-val { font-size: .88rem; font-weight: 400; color: var(--w80); }

/* ═══════════════════════════════════════════════════════════════
   MAP SECTION
═══════════════════════════════════════════════════════════════ */
.map-layout { display: grid; grid-template-columns: 1fr 1fr; min-height: 640px; }
.map-left { padding: 8rem 5vw; background: var(--navy2); display: flex; flex-direction: column; justify-content: center; }
.map-right { position: relative; overflow: hidden; }
#leaflet-map { width: 100%; height: 100%; min-height: 640px; }

.leaflet-container { background: var(--navy2) !important; }
.leaflet-tile { filter: invert(1) hue-rotate(195deg) brightness(.68) contrast(1.25) saturate(.45) !important; }
.leaflet-control-zoom { display: none !important; }
.leaflet-control-attribution { background: rgba(3,6,12,.75) !important; color: rgba(255,255,255,.22) !important; font-size: 9px !important; border-radius: 4px !important; }
.leaflet-control-attribution a { color: var(--gold) !important; }
.leaflet-popup-content-wrapper { background: rgba(3,6,12,.95) !important; border: 1px solid var(--borderHi) !important; border-radius: var(--r12) !important; color: var(--w80) !important; box-shadow: 0 8px 32px rgba(0,0,0,.6) !important; backdrop-filter: blur(16px) !important; }
.leaflet-popup-tip { background: rgba(3,6,12,.95) !important; }
.leaflet-popup-close-button { color: var(--w30) !important; }

.map-overlay {
  position: absolute; bottom: 2rem; left: 2rem; z-index: 10;
  background: rgba(3,6,12,.9); border: 1px solid var(--borderHi);
  border-radius: var(--r16); padding: 1.25rem 1.6rem;
  backdrop-filter: blur(20px);
}
.mo-title { font-size: .88rem; font-weight: 600; color: var(--w100); margin-bottom: .3rem; display: flex; align-items: center; gap: .5rem; }
.mo-live { width: 7px; height: 7px; border-radius: 50%; background: #4ade80; box-shadow: 0 0 8px #4ade80; animation: livePulse 2s infinite; }
.mo-sub { font-size: .75rem; font-weight: 300; color: var(--w50); }

.zone-list { display: flex; flex-direction: column; gap: .75rem; margin-top: 2.5rem; }
.zone-item {
  display: flex; align-items: center; gap: 1rem;
  padding: .85rem 1.1rem; border: 1px solid var(--border); border-radius: var(--r12);
  font-size: .875rem; font-weight: 300; color: var(--w50);
  transition: border-color .25s, color .25s, background .25s, transform .22s var(--ease-out);
}
.zone-item:hover { border-color: var(--gold); color: var(--w80); background: var(--goldA10); transform: translateX(6px); }
.zone-pin { color: var(--gold); font-size: .95rem; flex-shrink: 0; }

/* ═══════════════════════════════════════════════════════════════
   ABOUT / VISION
═══════════════════════════════════════════════════════════════ */
.about-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 7rem; align-items: center; }
.about-quote {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.65rem; font-style: italic; font-weight: 300;
  color: var(--w100); line-height: 1.5;
  border-left: 2px solid var(--gold); padding-left: 2rem;
  margin: 2.5rem 0;
}
.about-body { font-size: .95rem; font-weight: 300; color: var(--w50); line-height: 1.92; }
.about-signature { display: flex; align-items: center; gap: 1rem; margin-top: 2.5rem; }
.about-sig-line { width: 36px; height: 1px; background: var(--gold); }
.about-sig-text { font-size: .68rem; font-weight: 700; letter-spacing: .12em; text-transform: uppercase; color: var(--gold); }

.about-panel { background: var(--navy3); border: 1px solid var(--border); border-radius: var(--r28); overflow: hidden; }
.about-panel-head { padding: 2.25rem 2.5rem; border-bottom: 1px solid var(--border); background: linear-gradient(135deg, var(--goldA6), transparent); }
.about-panel-title { font-family: 'Cormorant Garamond', serif; font-size: 1.25rem; font-weight: 500; color: var(--w100); }
.about-panel-body { padding: 2rem 2.5rem; }
.about-list { list-style: none; display: flex; flex-direction: column; gap: .75rem; }
.about-item {
  display: flex; gap: 1rem; align-items: flex-start;
  padding: .85rem 1rem; border-radius: var(--r12);
  border: 1px solid transparent;
  font-size: .875rem; font-weight: 300; color: var(--w50); line-height: 1.72;
  transition: border-color .25s, background .25s, color .25s;
}
.about-item:hover { border-color: var(--border); background: var(--goldA6); color: var(--w70); }
.about-arrow { color: var(--gold); font-size: .8rem; margin-top: .22rem; flex-shrink: 0; font-weight: 600; }

/* ═══════════════════════════════════════════════════════════════
   AUDIT / CTA SECTION
═══════════════════════════════════════════════════════════════ */
.audit-section {
  background: linear-gradient(145deg, #050A14 0%, #03060C 50%, #070F1D 100%);
  position: relative; overflow: hidden;
}
.audit-glow {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);
  width: 900px; height: 900px; border-radius: 50%; pointer-events: none;
  background: radial-gradient(circle, rgba(200,168,75,.055) 0%, transparent 60%);
}
.audit-inner { max-width: 920px; margin: 0 auto; position: relative; z-index: 1; }
.audit-items-grid {
  display: grid; grid-template-columns: repeat(4,1fr);
  gap: 1px; background: var(--border); border: 1px solid var(--border);
  border-radius: var(--r20); overflow: hidden; margin-bottom: 3.5rem;
}
.audit-item {
  background: var(--navy); padding: 2rem 1.5rem; text-align: center;
  transition: background .3s;
}
.audit-item:hover { background: rgba(200,168,75,.06); }
.audit-item-icon { font-size: 1.5rem; display: block; margin-bottom: .85rem; }
.audit-item-label { font-size: .82rem; font-weight: 400; color: var(--w50); line-height: 1.5; transition: color .3s; }
.audit-item:hover .audit-item-label { color: var(--w80); }

/* Contact cards */
.contact-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1.25rem; margin-bottom: 2.5rem; }
.contact-card {
  display: flex; align-items: center; gap: 1.25rem;
  background: var(--w02); border: 1px solid var(--border);
  border-radius: var(--r20); padding: 1.75rem 2rem;
  text-decoration: none;
  transition: border-color .3s, background .3s, transform .3s var(--ease-out);
}
.contact-card:hover { border-color: var(--borderHi); background: var(--goldA10); transform: translateY(-4px); }
.contact-icon {
  width: 46px; height: 46px; border-radius: var(--r12); flex-shrink: 0;
  background: var(--goldA10); border: 1px solid var(--goldA25);
  display: flex; align-items: center; justify-content: center; font-size: 1.15rem;
}
.contact-label { font-size: .7rem; font-weight: 700; letter-spacing: .1em; text-transform: uppercase; color: var(--gold); margin-bottom: .3rem; }
.contact-value { font-size: .92rem; font-weight: 400; color: var(--w80); }

.audit-cta-row { display: flex; align-items: center; gap: 2.5rem; flex-wrap: wrap; }
.audit-note { font-size: .8rem; font-weight: 300; color: var(--w30); line-height: 1.72; }

/* ═══════════════════════════════════════════════════════════════
   FOOTER
═══════════════════════════════════════════════════════════════ */
footer { background: #02040A; border-top: 1px solid var(--border); padding: 5.5rem 5vw 2.5rem; }
.footer-grid {
  max-width: 1200px; margin: 0 auto;
  display: grid; grid-template-columns: 2fr 1fr 1fr 1.6fr;
  gap: 4rem; padding-bottom: 4rem;
  border-bottom: 1px solid rgba(255,255,255,.04); margin-bottom: 2rem;
}
.footer-brand {}
.footer-logo { font-family: 'Cormorant Garamond', serif; font-size: 1.6rem; font-weight: 600; color: var(--w100); margin-bottom: .9rem; }
.footer-logo span { color: var(--gold); }
.footer-tagline { font-size: .82rem; font-weight: 300; color: var(--w30); line-height: 1.72; max-width: 240px; }
.footer-col-title { font-size: .62rem; font-weight: 700; letter-spacing: .15em; text-transform: uppercase; color: var(--w30); margin-bottom: 1.3rem; }
.footer-links { display: flex; flex-direction: column; gap: .65rem; }
.footer-links a { font-size: .82rem; font-weight: 300; color: var(--w50); text-decoration: none; transition: color .2s; }
.footer-links a:hover { color: var(--gold2); }
.footer-contact-email { display: flex; align-items: center; gap: .55rem; font-size: .9rem; font-weight: 500; color: var(--gold); text-decoration: none; transition: opacity .2s; margin-bottom: .5rem; }
.footer-contact-email:hover { opacity: .7; }
.footer-contact-phone { display: flex; align-items: center; gap: .55rem; font-size: .88rem; font-weight: 400; color: var(--w50); text-decoration: none; transition: color .2s; }
.footer-contact-phone:hover { color: var(--gold2); }
.footer-zone { font-size: .75rem; font-weight: 300; color: var(--w30); margin-top: .85rem; display: flex; align-items: center; gap: .45rem; }
.footer-bottom { max-width: 1200px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 1rem; }
.footer-copy { font-size: .74rem; color: var(--w30); }
.footer-legal { display: flex; gap: 1.5rem; }
.footer-legal a { font-size: .74rem; color: var(--w30); text-decoration: none; transition: color .2s; }
.footer-legal a:hover { color: var(--gold); }

/* ═══════════════════════════════════════════════════════════════
   FLOATING BUTTONS
═══════════════════════════════════════════════════════════════ */
.float-btn-call {
  position: fixed; bottom: 2rem; right: 2rem; z-index: 400;
  display: flex; align-items: center; gap: .75rem;
  background: var(--gold);
  color: var(--navy); text-decoration: none;
  padding: .85rem 1.5rem; border-radius: 100px;
  font-family: 'DM Sans', sans-serif;
  font-size: .78rem; font-weight: 700; letter-spacing: .06em; text-transform: uppercase;
  box-shadow: 0 8px 32px rgba(200,168,75,.4);
  transition: transform .25s var(--ease-out), box-shadow .25s, filter .25s;
  animation: floatBtnIn .8s 2s both;
}
.float-btn-call:hover { transform: translateY(-3px); box-shadow: 0 16px 48px rgba(200,168,75,.55); filter: brightness(1.07); }
.float-btn-call-icon { font-size: 1rem; animation: callRing 3s 3s ease-in-out infinite; }
@keyframes callRing {
  0%,90%,100% { transform: rotate(0deg); }
  92% { transform: rotate(-15deg); }
  96% { transform: rotate(15deg); }
  98% { transform: rotate(-8deg); }
}
@keyframes floatBtnIn { from { opacity:0; transform: translateY(20px); } to { opacity:1; transform:translateY(0); } }

/* ═══════════════════════════════════════════════════════════════
   POPUP
═══════════════════════════════════════════════════════════════ */
#audit-popup {
  position: fixed; bottom: 2rem; left: 2rem; z-index: 400;
  background: rgba(3,6,12,.96); border: 1px solid var(--borderHi);
  border-radius: var(--r20); padding: 1.75rem 2rem;
  max-width: 320px; backdrop-filter: blur(24px);
  box-shadow: 0 16px 64px rgba(0,0,0,.6), 0 0 0 1px var(--border);
  transform: translateY(20px); opacity: 0;
  transition: transform .5s var(--ease-out), opacity .5s;
  pointer-events: none;
}
#audit-popup.show { transform: translateY(0); opacity: 1; pointer-events: all; }
.popup-close {
  position: absolute; top: .85rem; right: 1rem;
  background: none; border: none; color: var(--w30);
  font-size: 1.1rem; line-height: 1; transition: color .2s;
}
.popup-close:hover { color: var(--w80); }
.popup-tag {
  display: inline-flex; align-items: center; gap: .5rem;
  font-size: .6rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--gold); margin-bottom: .85rem;
}
.popup-tag-dot { width: 5px; height: 5px; border-radius: 50%; background: var(--gold); animation: livePulse 2s infinite; }
.popup-title { font-family: 'Cormorant Garamond', serif; font-size: 1.2rem; font-weight: 500; color: var(--w100); margin-bottom: .5rem; line-height: 1.3; }
.popup-sub { font-size: .8rem; font-weight: 300; color: var(--w50); margin-bottom: 1.25rem; line-height: 1.6; }
.popup-cta {
  display: flex; align-items: center; justify-content: center; gap: .6rem;
  background: linear-gradient(135deg, var(--gold3), var(--gold));
  color: var(--navy); text-decoration: none; border-radius: 100px;
  padding: .75rem 1.25rem; font-size: .75rem; font-weight: 700;
  letter-spacing: .07em; text-transform: uppercase;
  transition: filter .2s, transform .2s var(--ease-out);
  box-shadow: 0 4px 20px rgba(200,168,75,.3);
}
.popup-cta:hover { filter: brightness(1.08); transform: translateY(-2px); }

/* ═══════════════════════════════════════════════════════════════
   KEYFRAMES
═══════════════════════════════════════════════════════════════ */
@keyframes animFadeUp { from { opacity:0; transform:translateY(28px); } to { opacity:1; transform:translateY(0); } }
@keyframes animFadeIn { from { opacity:0; } to { opacity:1; } }

/* ═══════════════════════════════════════════════════════════════
   RESPONSIVE
═══════════════════════════════════════════════════════════════ */
@media (max-width: 1100px) {
  .hero-content { grid-template-columns: 1fr; }
  .hero-stats { display: none; }
  .services-header { grid-template-columns: 1fr; gap: 2rem; }
  .piliers-intro { grid-template-columns: 1fr; gap: 2rem; }
  .piliers-grid { grid-template-columns: 1fr 1fr; }
  .process-layout { grid-template-columns: 1fr; }
  .process-panel { position: static; }
  .faq-layout { grid-template-columns: 1fr; }
  .faq-visual { position: static; }
  .map-layout { grid-template-columns: 1fr; }
  .about-grid { grid-template-columns: 1fr; gap: 3rem; }
  .audit-items-grid { grid-template-columns: 1fr 1fr; }
  .footer-grid { grid-template-columns: 1fr 1fr; gap: 2.5rem; }
  .compare-table { overflow-x: auto; }
}
@media (max-width: 768px) {
  .section { padding: 100px 5vw; }
  .nav-menu { display: none; }
  .services-grid { grid-template-columns: 1fr; }
  .piliers-grid { grid-template-columns: 1fr; }
  .promise-grid { grid-template-columns: 1fr; }
  .contact-row { grid-template-columns: 1fr; }
  .compare-head, .compare-row { grid-template-columns: 1fr; }
  .compare-head-cell, .compare-cell { border-right: none; border-bottom: 1px solid var(--border); }
  .compare-head-cell:last-child, .compare-cell:last-child { border-bottom: none; }
  body { cursor: auto; }
  #cursor-dot, #cursor-ring, #cursor-label { display: none; }
  .float-btn-call { padding: .85rem 1.1rem; }
}
@media (max-width: 520px) {
  .hero-btns { flex-direction: column; }
  .audit-cta-row { flex-direction: column; align-items: flex-start; }
  .footer-grid { grid-template-columns: 1fr; }
  .audit-items-grid { grid-template-columns: 1fr 1fr; }
  #audit-popup { left: 1rem; right: 1rem; max-width: none; }
}
</style>
</head>
<body>

<!-- ═══ LOADER ═══ -->
<div id="page-loader">
  <div class="loader-wordmark">Renova<span>·</span>Stay</div>
  <div class="loader-progress-wrap"><div class="loader-progress-bar" id="loader-bar"></div></div>
  <div class="loader-pct" id="loader-pct">0%</div>
</div>

<!-- ═══ CURSOR ═══ -->
<div id="cursor-dot"></div>
<div id="cursor-ring"></div>
<div id="cursor-label">Explorer</div>

<!-- ═══ PARTICLES ═══ -->
<canvas id="particles-canvas"></canvas>

<!-- ═══ POPUP ═══ -->
<div id="audit-popup">
  <button class="popup-close" id="popup-close" aria-label="Fermer">✕</button>
  <div class="popup-tag"><span class="popup-tag-dot"></span>Offre gratuite</div>
  <div class="popup-title">Audit gratuit de votre annonce</div>
  <div class="popup-sub">Obtenez une analyse complète de votre bien en Moselle — sans engagement.</div>
  <a href="mailto:delio.casciu10@gmail.com?subject=Demande%20d%27audit%20gratuit%20-%20Renova%20Stay&body=Bonjour%2C%0A%0AJe%20souhaite%20recevoir%20un%20audit%20gratuit%20de%20mon%20annonce.%0A%0ALien%20%3A%20" class="popup-cta">
    Recevoir mon audit
    <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
  </a>
</div>

<!-- ═══ FLOATING CALL BUTTON ═══ -->
<a href="tel:+33783367640" class="float-btn-call" id="float-call">
  <span class="float-btn-call-icon">📞</span>
  Appeler maintenant
</a>

<!-- ═══ NAV ═══ -->
<nav class="nav" id="main-nav">
  <a href="#" class="nav-logo">Renova<span class="nav-logo-sep">·</span>Stay</a>
  <ul class="nav-menu">
    <li><a href="#services">Services</a></li>
    <li><a href="#approche">Approche</a></li>
    <li><a href="#pourquoi">Pourquoi nous</a></li>
    <li><a href="#processus">Méthode</a></li>
    <li><a href="#zone">Zone</a></li>
    <li><a href="#faq">FAQ</a></li>
  </ul>
  <a href="#contact" class="nav-cta"><span>
    <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
    Audit gratuit
  </span></a>
</nav>

<!-- ═══ HERO ═══ -->
<section class="hero">
  <div class="hero-video-wrap">
    <div class="hero-mesh-fallback"></div>
    <!-- Videos loop: Metz at night, luxury interior, Moselle nature -->
    <video class="hero-video" id="hero-video" autoplay muted loop playsinline poster="">
      <source src="https://assets.mixkit.co/videos/preview/mixkit-aerial-view-of-a-city-at-night-11-large.mp4" type="video/mp4">
      <source src="https://assets.mixkit.co/videos/preview/mixkit-luxury-living-room-41-large.mp4" type="video/mp4">
    </video>
  </div>
  <div class="hero-grain"></div>
  <div class="hero-ring hero-ring-1"></div>
  <div class="hero-ring hero-ring-2"></div>
  <div class="hero-ring hero-ring-3"></div>

  <div class="hero-content">
    <div class="hero-left">
      <div class="hero-eyebrow">
        <span class="hero-eyebrow-bar"></span>
        Gestion locative · Moselle
      </div>
      <h1 class="hero-h1">
        <span class="hero-h1-static">La gestion locative</span>
        <span class="hero-h1-accent">
          <span class="hero-rotate-wrap">
            <span class="hero-rotate-words">
              <span class="hero-rotate-word">nouvelle génération.</span>
              <span class="hero-rotate-word">qui performe.</span>
              <span class="hero-rotate-word">transparente.</span>
              <span class="hero-rotate-word">sur mesure.</span>
              <span class="hero-rotate-word">nouvelle génération.</span>
            </span>
          </span>
        </span>
      </h1>
      <p class="hero-p">Nous accompagnons les propriétaires en Moselle dans l'optimisation, l'exploitation et la gestion quotidienne de leurs biens — pour une rentabilité maximale et une expérience voyageur irréprochable.</p>
      <div class="hero-btns">
        <a href="#contact" class="btn-primary" data-magnetic data-cursor-label="Commencer">
          Demander un audit gratuit
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
        <a href="#services" class="btn-secondary" data-magnetic>Découvrir nos services</a>
      </div>
    </div>

    <div class="hero-stats">
      <div class="hstat" data-hover>
        <div class="hstat-icon">🗓️</div>
        <div>
          <div class="hstat-title">Réactivité 7j/7</div>
          <div class="hstat-sub">Disponibles chaque jour pour vous et vos voyageurs, sans exception.</div>
        </div>
        <div class="hstat-live"><span class="hstat-live-dot"></span>Active</div>
      </div>
      <div class="hstat" data-hover>
        <div class="hstat-icon">🤝</div>
        <div>
          <div class="hstat-title">Accompagnement personnalisé</div>
          <div class="hstat-sub">Un interlocuteur unique dédié à chaque propriétaire.</div>
        </div>
      </div>
      <div class="hstat" data-hover>
        <div class="hstat-icon">⚙️</div>
        <div>
          <div class="hstat-title">Service sur mesure</div>
          <div class="hstat-sub">Des solutions adaptées à chaque bien et chaque objectif.</div>
        </div>
      </div>
    </div>
  </div>

  <div class="hero-scroll-hint">
    <span class="hero-scroll-label">Défiler</span>
    <div class="hero-scroll-line"></div>
  </div>
</section>

<!-- ═══ MARQUEE ═══ -->
<div class="marquee-band">
  <div class="marquee-track">
    <div class="marquee-item">Gestion locative<span class="marquee-sep"></span></div>
    <div class="marquee-item">Moselle<span class="marquee-sep"></span></div>
    <div class="marquee-item">Optimisation d'annonces<span class="marquee-sep"></span></div>
    <div class="marquee-item">Renova Stay<span class="marquee-sep"></span></div>
    <div class="marquee-item">Courte durée<span class="marquee-sep"></span></div>
    <div class="marquee-item">Airbnb · Booking<span class="marquee-sep"></span></div>
    <div class="marquee-item">Expérience voyageur<span class="marquee-sep"></span></div>
    <div class="marquee-item">7j/7<span class="marquee-sep"></span></div>
    <div class="marquee-item">Gestion locative<span class="marquee-sep"></span></div>
    <div class="marquee-item">Moselle<span class="marquee-sep"></span></div>
    <div class="marquee-item">Optimisation d'annonces<span class="marquee-sep"></span></div>
    <div class="marquee-item">Renova Stay<span class="marquee-sep"></span></div>
    <div class="marquee-item">Courte durée<span class="marquee-sep"></span></div>
    <div class="marquee-item">Airbnb · Booking<span class="marquee-sep"></span></div>
    <div class="marquee-item">Expérience voyageur<span class="marquee-sep"></span></div>
    <div class="marquee-item">7j/7<span class="marquee-sep"></span></div>
  </div>
</div>

<!-- ═══ SERVICES ═══ -->
<section id="services" class="section bg-deep">
  <div class="container">
    <div class="services-header">
      <div>
        <div class="section-tag rev-up"><span class="section-tag-bar"></span>Services</div>
        <h2 class="serif word-reveal rev-up">Une prise en charge <em>complète</em></h2>
      </div>
      <p class="section-lead rev-up">De la communication voyageurs à la coordination terrain — nous gérons chaque aspect de votre activité locative sur toutes les plateformes.</p>
    </div>
    <div class="services-grid rev-up">
      <div class="service-card tilt-card">
        <div class="service-num">01</div>
        <div class="service-chip">Communication</div>
        <div class="service-title">Gestion voyageurs</div>
        <p class="service-desc">Nous assurons les échanges avec les voyageurs avant, pendant et après leur séjour pour une expérience irréprochable à chaque étape.</p>
      </div>
      <div class="service-card tilt-card">
        <div class="service-num">02</div>
        <div class="service-chip">Performance</div>
        <div class="service-title">Optimisation des annonces</div>
        <p class="service-desc">Analyse des performances, amélioration des descriptions, optimisation du positionnement et valorisation du bien sur toutes les plateformes.</p>
      </div>
      <div class="service-card tilt-card">
        <div class="service-num">03</div>
        <div class="service-chip">Opérationnel</div>
        <div class="service-title">Coordination opérationnelle</div>
        <p class="service-desc">Organisation des arrivées, départs et interventions nécessaires au bon fonctionnement du logement, sans que vous ayez à intervenir.</p>
      </div>
      <div class="service-card tilt-card">
        <div class="service-num">04</div>
        <div class="service-chip">Suivi</div>
        <div class="service-title">Accompagnement propriétaire</div>
        <p class="service-desc">Un interlocuteur unique et un suivi transparent de l'activité, avec des rapports réguliers et une communication directe en permanence.</p>
      </div>
    </div>
  </div>
</section>

<!-- ═══ PLATFORMS ═══ -->
<div class="platform-strip">
  <div class="platform-strip-inner">
    <span class="platform-label">Plateformes gérées</span>
    <div class="platform-list">
      <div class="platform-pill"><span class="platform-dot" style="background:#FF5A5F"></span>Airbnb</div>
      <div class="platform-pill"><span class="platform-dot" style="background:#003580"></span>Booking.com</div>
      <div class="platform-pill"><span class="platform-dot" style="background:#E7711B"></span>Abritel</div>
      <div class="platform-pill"><span class="platform-dot" style="background:#00A9CE"></span>Expedia</div>
      <div class="platform-pill"><span class="platform-dot" style="background:#C8A84B"></span>& autres</div>
    </div>
  </div>
</div>

<!-- ═══ APPROCHE / PILIERS ═══ -->
<section id="approche" class="section bg-navy">
  <div class="container">
    <div class="piliers-intro">
      <div>
        <div class="section-tag rev-up"><span class="section-tag-bar"></span>Notre approche</div>
        <h2 class="serif word-reveal rev-up">Plus qu'un simple <em>co-hôte</em></h2>
      </div>
      <p class="section-lead rev-up">Notre approche repose sur trois piliers fondamentaux qui font la différence sur le long terme pour chaque propriétaire que nous accompagnons.</p>
    </div>
    <div class="piliers-grid">
      <div class="pilier-card tilt-card rev-up">
        <div class="pilier-icon">📈</div>
        <span class="pilier-num">01</span>
        <div class="pilier-title">Performance</div>
        <p class="pilier-desc">Chaque logement possède un potentiel inexploité que nous aidons à révéler grâce à une analyse rigoureuse du marché et des données de performance sur les plateformes.</p>
      </div>
      <div class="pilier-card tilt-card rev-up">
        <div class="pilier-icon">⭐</div>
        <span class="pilier-num">02</span>
        <div class="pilier-title">Expérience voyageur</div>
        <p class="pilier-desc">Une expérience fluide et mémorable génère de meilleurs avis, favorise les réservations directes et fidélise les voyageurs sur la durée.</p>
      </div>
      <div class="pilier-card tilt-card rev-up">
        <div class="pilier-icon">🔍</div>
        <span class="pilier-num">03</span>
        <div class="pilier-title">Transparence</div>
        <p class="pilier-desc">Une communication claire, des comptes-rendus réguliers et un suivi détaillé de chaque action réalisée sur votre bien, sans zone d'ombre.</p>
      </div>
    </div>
  </div>
</section>

<!-- ═══ POURQUOI NOUS ═══ -->
<section id="pourquoi" class="section bg-ink">
  <div class="container">
    <!-- Comparison -->
    <div class="compare-intro">
      <div class="section-tag rev-up"><span class="section-tag-bar"></span>Pourquoi nous choisir</div>
      <h2 class="serif word-reveal rev-up">Renova Stay <em>vs</em> gestion en solo</h2>
      <p class="section-lead rev-up" style="margin-bottom:3rem">Comparez concrètement ce que nous apportons par rapport à une gestion autonome de votre bien.</p>
    </div>
    <div class="compare-table rev-up">
      <div class="compare-head">
        <div class="compare-head-cell">Critère</div>
        <div class="compare-head-cell highlight">Renova Stay</div>
        <div class="compare-head-cell">Gestion solo</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Disponibilité voyageurs</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>7j/7, réponse rapide garantie</div>
        <div class="compare-cell"><span class="compare-icon-meh">~</span>Variable, selon votre emploi du temps</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Optimisation des annonces</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Analyse continue & amélioration pro</div>
        <div class="compare-cell"><span class="compare-icon-no">✗</span>Rarement mis à jour</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Coordination arrivées/départs</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Entièrement prise en charge</div>
        <div class="compare-cell"><span class="compare-icon-no">✗</span>Vous devez être disponible</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Gestion des incidents</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Interlocuteur unique & réactif</div>
        <div class="compare-cell"><span class="compare-icon-meh">~</span>Vous gérez seul chaque problème</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Rapports de performance</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Suivi mensuel détaillé</div>
        <div class="compare-cell"><span class="compare-icon-no">✗</span>Aucun suivi structuré</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Multi-plateformes</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Toutes plateformes synchronisées</div>
        <div class="compare-cell"><span class="compare-icon-meh">~</span>Difficile à gérer seul</div>
      </div>
      <div class="compare-row">
        <div class="compare-cell feature">Tranquillité d'esprit</div>
        <div class="compare-cell rs"><span class="compare-icon-yes">✓</span>Totale — nous gérons tout</div>
        <div class="compare-cell"><span class="compare-icon-no">✗</span>Stress permanent</div>
      </div>
    </div>

    <!-- Promise cards -->
    <div style="margin-top:6rem">
      <div class="section-tag rev-up"><span class="section-tag-bar"></span>Notre promesse</div>
      <h2 class="serif word-reveal rev-up">Nos engagements <em>concrets</em></h2>
      <div class="promise-grid">
        <div class="promise-card rev-up">
          <div class="promise-icon">⚡</div>
          <div>
            <div class="promise-title">Réactivité sans compromis</div>
            <p class="promise-desc">Chaque message voyageur reçoit une réponse rapide, 7 jours sur 7. Vos voyageurs ne seront jamais laissés sans réponse.</p>
          </div>
        </div>
        <div class="promise-card rev-up">
          <div class="promise-icon">🔒</div>
          <div>
            <div class="promise-title">Transparence totale</div>
            <p class="promise-desc">Vous avez accès à tout : rapport d'activité, échanges, interventions. Rien ne se passe sans que vous le sachiez.</p>
          </div>
        </div>
        <div class="promise-card rev-up">
          <div class="promise-icon">🎯</div>
          <div>
            <div class="promise-title">Stratégie sur mesure</div>
            <p class="promise-desc">Pas de solution générique. Chaque bien est analysé et géré selon ses spécificités, sa localisation et ses objectifs propres.</p>
          </div>
        </div>
        <div class="promise-card rev-up">
          <div class="promise-icon">🤝</div>
          <div>
            <div class="promise-title">Un seul interlocuteur</div>
            <p class="promise-desc">Vous n'êtes jamais ballotté entre différents services. Un contact direct, disponible, qui connaît votre bien par cœur.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══ PROCESSUS ═══ -->
<section id="processus" class="section bg-navy2">
  <div class="container">
    <div class="section-tag rev-up" style="margin-bottom:1.5rem"><span class="section-tag-bar"></span>Méthode</div>
    <h2 class="serif word-reveal rev-up" style="margin-bottom:5rem">Comment nous <em>travaillons</em></h2>
    <div class="process-layout">
      <div class="timeline">
        <div class="timeline-step rev-up">
          <div class="tl-dot"></div>
          <div class="tl-step-num">Étape 01</div>
          <div class="tl-step-title">Analyse du logement</div>
          <p class="tl-step-desc">Étude approfondie de votre annonce, de votre positionnement et du marché local en Moselle pour établir un diagnostic précis et personnalisé.</p>
        </div>
        <div class="timeline-step rev-up">
          <div class="tl-dot"></div>
          <div class="tl-step-num">Étape 02</div>
          <div class="tl-step-title">Recommandations</div>
          <p class="tl-step-desc">Identification des opportunités d'amélioration concrètes, priorisées par impact potentiel sur votre rentabilité et votre note voyageur.</p>
        </div>
        <div class="timeline-step rev-up">
          <div class="tl-dot"></div>
          <div class="tl-step-num">Étape 03</div>
          <div class="tl-step-title">Mise en place</div>
          <p class="tl-step-desc">Déploiement méthodique des optimisations et organisation complète de la gestion quotidienne — sans perturber votre activité.</p>
        </div>
        <div class="timeline-step rev-up">
          <div class="tl-dot"></div>
          <div class="tl-step-num">Étape 04</div>
          <div class="tl-step-title">Suivi continu</div>
          <p class="tl-step-desc">Amélioration continue des performances et de l'expérience voyageur, semaine après semaine, avec des rapports transparents et réguliers.</p>
        </div>
      </div>
      <div class="process-panel rev-right">
        <div class="process-panel-head">
          <div class="pp-label">Approche Renova Stay</div>
          <div class="pp-title">Une gestion rigoureuse,<br>des résultats durables.</div>
        </div>
        <div class="process-panel-body">
          <ul class="pp-list">
            <li class="pp-item"><div class="pp-item-icon">🗓️</div>Disponibilité 7 jours sur 7</li>
            <li class="pp-item"><div class="pp-item-icon">📊</div>Rapports mensuels détaillés</li>
            <li class="pp-item"><div class="pp-item-icon">🤝</div>Interlocuteur unique dédié</li>
            <li class="pp-item"><div class="pp-item-icon">🔧</div>Coordination opérationnelle complète</li>
            <li class="pp-item"><div class="pp-item-icon">🌍</div>Toutes plateformes gérées</li>
            <li class="pp-item"><div class="pp-item-icon">📍</div>Expertise locale Moselle</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══ MAP — MOSELLE ═══ -->
<div id="zone" class="gold-rule"></div>
<div style="background:var(--navy3)">
  <div class="map-layout">
    <div class="map-left">
      <div class="section-tag rev-left"><span class="section-tag-bar"></span>Zone d'intervention</div>
      <h2 class="serif word-reveal rev-left">Basés en <em>Moselle</em>,<br>à votre service.</h2>
      <p class="section-lead rev-left" style="margin-bottom:2.5rem">Nous intervenons sur l'ensemble du département de la Moselle et ses alentours, avec une connaissance fine du marché local et des spécificités touristiques de la région.</p>
      <div class="zone-list">
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Metz &amp; agglomération</div>
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Thionville &amp; vallée de la Moselle</div>
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Sarreguemines &amp; Est mosellan</div>
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Forbach &amp; bassin houiller</div>
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Sarrebourg &amp; Pays de Phalsbourg</div>
        <div class="zone-item rev-left"><span class="zone-pin">📍</span>Ensemble du département 57</div>
      </div>
    </div>
    <div class="map-right">
      <div id="leaflet-map"></div>
      <div class="map-overlay">
        <div class="mo-title"><span class="mo-live"></span>Zone active</div>
        <div class="mo-sub">Moselle (57) · Grand Est · France</div>
      </div>
    </div>
  </div>
</div>

<!-- ═══ À PROPOS ═══ -->
<section id="propos" class="section bg-deep">
  <div class="container">
    <div class="about-grid">
      <div>
        <div class="section-tag rev-left"><span class="section-tag-bar"></span>À propos</div>
        <h2 class="serif word-reveal rev-left">Notre <em>vision</em></h2>
        <blockquote class="about-quote rev-left">"La gestion locative courte durée doit être plus simple, plus transparente et plus performante pour les propriétaires."</blockquote>
        <p class="about-body rev-left">Notre mission est d'apporter une approche moderne de l'exploitation des locations saisonnières en Moselle, grâce à une organisation rigoureuse et une attention particulière portée à l'expérience voyageur.<br><br>Nous croyons que chaque propriétaire mérite un accompagnement à la hauteur de son investissement — sans compromis sur la qualité ni sur la transparence.</p>
        <div class="about-signature rev-left">
          <div class="about-sig-line"></div>
          <div class="about-sig-text">Renova Stay · Moselle, France</div>
        </div>
      </div>
      <div class="about-panel rev-right">
        <div class="about-panel-head"><div class="about-panel-title">Ce qui nous distingue</div></div>
        <div class="about-panel-body">
          <ul class="about-list">
            <li class="about-item"><span class="about-arrow">→</span>Une gestion orientée performance, pas seulement réactive aux demandes entrantes.</li>
            <li class="about-item"><span class="about-arrow">→</span>Un suivi transparent avec des rapports réguliers sur l'activité de votre bien.</li>
            <li class="about-item"><span class="about-arrow">→</span>Une approche sur mesure adaptée à chaque logement et chaque propriétaire.</li>
            <li class="about-item"><span class="about-arrow">→</span>Une attention constante portée à la qualité de l'expérience voyageur sur toutes les plateformes.</li>
            <li class="about-item"><span class="about-arrow">→</span>Un interlocuteur unique, disponible 7j/7 et pleinement impliqué dans votre projet.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══ FAQ ═══ -->
<section id="faq" class="section bg-navy">
  <div class="container">
    <div class="section-tag rev-up" style="margin-bottom:1.5rem"><span class="section-tag-bar"></span>FAQ</div>
    <h2 class="serif word-reveal rev-up" style="margin-bottom:4rem">Questions <em>fréquentes</em></h2>
    <div class="faq-layout">
      <div class="faq-list rev-left">
        <div class="faq-item">
          <button class="faq-q">
            Combien coûte votre service de gestion ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Nos tarifs sont construits sur mesure selon le type de bien, le volume d'activité et les services souhaités. Contactez-nous pour obtenir une proposition personnalisée — l'audit initial est totalement gratuit et sans engagement.</div>
        </div>
        <div class="faq-item">
          <button class="faq-q">
            Je gère déjà mes annonces moi-même, pourquoi faire appel à vous ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Gérer soi-même demande une disponibilité permanente, une expertise en optimisation tarifaire, en rédaction d'annonces et en gestion des situations complexes. Nous libérons votre temps tout en maximisant les performances de votre bien grâce à notre expertise et à notre approche data.</div>
        </div>
        <div class="faq-item">
          <button class="faq-q">
            Quelles plateformes gérez-vous ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Nous gérons toutes les grandes plateformes : Airbnb, Booking.com, Abritel (VRBO), Expedia et d'autres selon votre positionnement. Nous synchronisons les calendriers pour éviter tout double booking.</div>
        </div>
        <div class="faq-item">
          <button class="faq-q">
            Restez-vous informé de tout ce qui se passe dans mon logement ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Absolument. Vous recevez des rapports réguliers et nous vous informons de tout événement notable. La transparence est au cœur de notre approche — vous gardez une visibilité totale sur votre bien.</div>
        </div>
        <div class="faq-item">
          <button class="faq-q">
            Comment se déroule l'audit gratuit ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Envoyez-nous le lien de votre annonce par email ou contactez-nous directement. Nous analysons le titre, les photos, la description et le positionnement tarifaire, puis vous remettons nos recommandations concrètes — le tout gratuitement et sous 48h.</div>
        </div>
        <div class="faq-item">
          <button class="faq-q">
            Intervenez-vous uniquement en Moselle ?
            <span class="faq-arrow"><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 5v14M5 12l7 7 7-7"/></svg></span>
          </button>
          <div class="faq-a">Nous sommes basés en Moselle et c'est notre zone d'expertise principale. Nous pouvons étudier des demandes dans les départements limitrophes au cas par cas. N'hésitez pas à nous contacter pour en discuter.</div>
        </div>
      </div>
      <div class="faq-visual rev-right">
        <div class="faq-visual-top">
          <div class="faq-visual-label">Contactez-nous directement</div>
          <div class="faq-visual-title">Une question ?<br>On vous répond vite.</div>
        </div>
        <div class="faq-visual-body">
          <div class="faq-contact-links">
            <a href="mailto:delio.casciu10@gmail.com" class="faq-contact-link">
              <div class="faq-contact-icon">✉️</div>
              <div>
                <div class="faq-contact-text-label">Email</div>
                <div class="faq-contact-text-val">delio.casciu10@gmail.com</div>
              </div>
            </a>
            <a href="tel:+33783367640" class="faq-contact-link">
              <div class="faq-contact-icon">📞</div>
              <div>
                <div class="faq-contact-text-label">Téléphone</div>
                <div class="faq-contact-text-val">+33 7 83 36 76 40</div>
              </div>
            </a>
            <a href="#contact" class="faq-contact-link">
              <div class="faq-contact-icon">📋</div>
              <div>
                <div class="faq-contact-text-label">Audit gratuit</div>
                <div class="faq-contact-text-val">Demander une analyse de mon bien</div>
              </div>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══ CONTACT / AUDIT CTA ═══ -->
<section id="contact" class="section audit-section">
  <div class="audit-glow"></div>
  <div class="audit-inner">
    <div class="section-tag rev-up"><span class="section-tag-bar"></span>Offre de
