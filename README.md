# renovastay
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Renova Stay — Gestion Locative Nouvelle Génération · Moselle</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500&family=Jost:wght@200;300;400;500;600&display=swap" rel="stylesheet">
<style>
/* ══════════════════════════════════════════════
   ROOT & RESET
══════════════════════════════════════════════ */
:root {
  --ink:        #05090F;
  --navy:       #080E1C;
  --navy-2:     #0D1628;
  --navy-3:     #121E36;
  --navy-4:     #1A2A4A;
  --gold:       #C8A84B;
  --gold-2:     #DFC068;
  --gold-3:     #F0D98A;
  --gold-dim:   rgba(200,168,75,.15);
  --gold-glow:  rgba(200,168,75,.35);
  --white:      #FFFFFF;
  --off:        #F4F0E8;
  --text:       rgba(255,255,255,.85);
  --text-mid:   rgba(255,255,255,.5);
  --text-low:   rgba(255,255,255,.25);
  --border:     rgba(200,168,75,.12);
  --border-hi:  rgba(200,168,75,.3);
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; font-size: 16px; }
body {
  font-family: 'Jost', sans-serif;
  background: var(--ink);
  color: var(--text);
  overflow-x: hidden;
  cursor: none;
}
::selection { background: var(--gold); color: var(--navy); }
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-track { background: var(--navy); }
::-webkit-scrollbar-thumb { background: var(--gold); border-radius: 2px; }

/* ══════════════════════════════════════════════
   CUSTOM CURSOR
══════════════════════════════════════════════ */
#cursor {
  position: fixed; z-index: 9999;
  width: 10px; height: 10px;
  background: var(--gold);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%,-50%);
  transition: transform .1s, width .3s, height .3s, background .3s;
  mix-blend-mode: difference;
}
#cursor-ring {
  position: fixed; z-index: 9998;
  width: 38px; height: 38px;
  border: 1px solid var(--gold);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%,-50%);
  transition: transform .18s cubic-bezier(.16,1,.3,1), width .3s, height .3s, opacity .3s;
  opacity: .6;
}
body.hovering #cursor { width: 6px; height: 6px; }
body.hovering #cursor-ring { width: 56px; height: 56px; opacity: .3; }
body.clicking #cursor { transform: translate(-50%,-50%) scale(.6); }

/* ══════════════════════════════════════════════
   CANVAS PARTICLES (hero background)
══════════════════════════════════════════════ */
#particles-canvas {
  position: fixed; inset: 0;
  pointer-events: none; z-index: 0;
  opacity: .55;
}

/* ══════════════════════════════════════════════
   NAV
══════════════════════════════════════════════ */
nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 500;
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 5vw; height: 82px;
  transition: all .5s cubic-bezier(.16,1,.3,1);
}
nav::after {
  content: '';
  position: absolute; inset: 0;
  background: transparent;
  transition: background .5s, backdrop-filter .5s;
  z-index: -1;
}
nav.scrolled::after {
  background: rgba(5,9,15,.9);
  backdrop-filter: blur(24px);
  box-shadow: 0 1px 0 var(--border), 0 16px 48px rgba(0,0,0,.4);
}

.nav-logo {
  font-family: 'Playfair Display', serif;
  font-size: 1.4rem; font-weight: 600;
  color: var(--white); letter-spacing: .06em;
  text-decoration: none;
  display: flex; align-items: center; gap: 2px;
}
.nav-logo-accent { color: var(--gold); }

.nav-center { display: flex; gap: 2.5rem; list-style: none; }
.nav-center a {
  font-size: .72rem; font-weight: 500; letter-spacing: .14em;
  text-transform: uppercase; color: var(--text-mid);
  text-decoration: none; transition: color .25s;
  position: relative;
}
.nav-center a::after {
  content: ''; position: absolute; bottom: -4px; left: 0; right: 0;
  height: 1px; background: var(--gold);
  transform: scaleX(0); transform-origin: left;
  transition: transform .3s cubic-bezier(.16,1,.3,1);
}
.nav-center a:hover { color: var(--gold-2); }
.nav-center a:hover::after { transform: scaleX(1); }

.nav-btn {
  display: flex; align-items: center; gap: .5rem;
  background: var(--gold); color: var(--navy);
  border: none; cursor: none; padding: .65rem 1.5rem;
  border-radius: 100px;
  font-family: 'Jost', sans-serif;
  font-size: .72rem; font-weight: 700; letter-spacing: .1em; text-transform: uppercase;
  text-decoration: none;
  transition: transform .2s, box-shadow .2s, filter .2s;
  box-shadow: 0 0 0 0 var(--gold-glow);
}
.nav-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 32px var(--gold-glow);
  filter: brightness(1.08);
}

/* ══════════════════════════════════════════════
   HERO
══════════════════════════════════════════════ */
.hero {
  min-height: 100vh;
  display: flex; align-items: center;
  position: relative; overflow: hidden;
  padding: 0 5vw;
}

/* Animated mesh gradient */
.hero-mesh {
  position: absolute; inset: 0; z-index: 0;
  background:
    radial-gradient(ellipse 80% 60% at 70% 40%, rgba(200,168,75,.07) 0%, transparent 60%),
    radial-gradient(ellipse 50% 80% at 10% 80%, rgba(13,22,40,.8) 0%, transparent 60%),
    linear-gradient(160deg, #05090F 0%, #0D1628 50%, #05090F 100%);
}
.hero-noise {
  position: absolute; inset: 0; z-index: 1; opacity: .03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  background-size: 200px;
}

/* Large decorative text */
.hero-bg-text {
  position: absolute; right: -2vw; top: 50%;
  transform: translateY(-50%);
  font-family: 'Playfair Display', serif;
  font-size: clamp(120px, 18vw, 220px);
  font-weight: 700; letter-spacing: -.02em;
  color: transparent;
  -webkit-text-stroke: 1px rgba(200,168,75,.06);
  pointer-events: none; z-index: 1;
  white-space: nowrap; line-height: 1;
  user-select: none;
}

.hero-content {
  position: relative; z-index: 2;
  max-width: 1200px; margin: 0 auto; width: 100%;
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 5vw; align-items: center;
  padding: 100px 0;
}

.hero-left {}

.hero-tag {
  display: inline-flex; align-items: center; gap: .7rem;
  margin-bottom: 2.5rem;
  font-size: .68rem; font-weight: 600; letter-spacing: .18em; text-transform: uppercase;
  color: var(--gold);
  opacity: 0; animation: fadeSlideUp .8s .2s ease forwards;
}
.hero-tag-line { width: 28px; height: 1px; background: var(--gold); }

.hero-h1 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2.8rem, 5.5vw, 5.2rem);
  font-weight: 400; line-height: 1.08;
  color: var(--white); letter-spacing: -.01em;
  margin-bottom: 2rem;
  opacity: 0; animation: fadeSlideUp .9s .35s ease forwards;
}
.hero-h1 em {
  font-style: italic; font-weight: 400;
  background: linear-gradient(135deg, var(--gold-3) 0%, var(--gold) 50%, var(--gold-2) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-sub {
  font-size: 1rem; font-weight: 300; color: var(--text-mid);
  line-height: 1.85; max-width: 460px; margin-bottom: 3rem;
  opacity: 0; animation: fadeSlideUp .9s .5s ease forwards;
}

.hero-btns {
  display: flex; gap: 1rem; flex-wrap: wrap;
  opacity: 0; animation: fadeSlideUp .9s .65s ease forwards;
}

.btn-primary {
  display: inline-flex; align-items: center; gap: .65rem;
  background: linear-gradient(135deg, var(--gold-2), var(--gold));
  color: var(--navy); border: none; cursor: none;
  padding: 1rem 2.1rem; border-radius: 100px;
  font-family: 'Jost', sans-serif;
  font-size: .8rem; font-weight: 700; letter-spacing: .08em; text-transform: uppercase;
  text-decoration: none;
  box-shadow: 0 4px 28px rgba(200,168,75,.25);
  transition: transform .25s, box-shadow .25s, filter .25s;
}
.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 48px rgba(200,168,75,.4);
  filter: brightness(1.07);
}
.btn-secondary {
  display: inline-flex; align-items: center; gap: .65rem;
  background: transparent;
  color: var(--text-mid); border: 1px solid var(--border-hi); cursor: none;
  padding: 1rem 2.1rem; border-radius: 100px;
  font-family: 'Jost', sans-serif;
  font-size: .8rem; font-weight: 500; letter-spacing: .06em; text-transform: uppercase;
  text-decoration: none;
  transition: border-color .25s, color .25s, background .25s, transform .25s;
}
.btn-secondary:hover {
  border-color: var(--gold);
  color: var(--gold-2);
  background: var(--gold-dim);
  transform: translateY(-2px);
}

/* Hero right — glass cards */
.hero-right {
  opacity: 0; animation: fadeSlideUp .9s .5s ease forwards;
}

.hero-cards { display: flex; flex-direction: column; gap: 1.25rem; }

.hcard {
  background: rgba(255,255,255,.03);
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 1.5rem 1.75rem;
  display: flex; align-items: center; gap: 1.25rem;
  backdrop-filter: blur(12px);
  position: relative; overflow: hidden;
  transition: border-color .3s, background .3s, transform .3s;
}
.hcard::before {
  content: '';
  position: absolute; inset: 0;
  background: linear-gradient(135deg, rgba(200,168,75,.06) 0%, transparent 60%);
  opacity: 0; transition: opacity .3s;
}
.hcard:hover {
  border-color: var(--border-hi);
  background: rgba(200,168,75,.05);
  transform: translateX(6px);
}
.hcard:hover::before { opacity: 1; }

.hcard-icon {
  width: 46px; height: 46px; border-radius: 12px; flex-shrink: 0;
  background: rgba(200,168,75,.1);
  border: 1px solid rgba(200,168,75,.2);
  display: flex; align-items: center; justify-content: center;
  font-size: 1.15rem;
}
.hcard-title { font-size: .95rem; font-weight: 600; color: var(--white); margin-bottom: .2rem; }
.hcard-sub { font-size: .8rem; font-weight: 300; color: var(--text-mid); line-height: 1.5; }

.hcard-badge {
  position: absolute; top: 1.25rem; right: 1.25rem;
  background: rgba(200,168,75,.12); border: 1px solid rgba(200,168,75,.2);
  border-radius: 100px; padding: .25rem .7rem;
  font-size: .65rem; font-weight: 700; letter-spacing: .1em;
  text-transform: uppercase; color: var(--gold-2);
}

/* Scroll indicator */
.hero-scroll {
  position: absolute; bottom: 2.5rem; left: 50%; transform: translateX(-50%);
  z-index: 2; display: flex; flex-direction: column; align-items: center; gap: .6rem;
  opacity: 0; animation: fadeIn 1s 1.4s ease forwards;
  cursor: none;
}
.hero-scroll-text {
  font-size: .63rem; font-weight: 600; letter-spacing: .18em;
  text-transform: uppercase; color: var(--text-low);
}
.hero-scroll-line {
  width: 1px; height: 48px;
  background: linear-gradient(to bottom, var(--gold), transparent);
  animation: scrollPulse 2s infinite;
}
@keyframes scrollPulse {
  0%,100% { opacity: .3; transform: scaleY(.6); transform-origin: top; }
  50% { opacity: 1; transform: scaleY(1); }
}

/* ══════════════════════════════════════════════
   MARQUEE BAND
══════════════════════════════════════════════ */
.marquee-band {
  background: var(--gold);
  padding: .8rem 0; overflow: hidden;
  position: relative; z-index: 10;
}
.marquee-track {
  display: flex; gap: 0;
  animation: marquee 20s linear infinite;
  width: max-content;
}
.marquee-item {
  display: flex; align-items: center; gap: 2.5rem;
  padding: 0 2.5rem; white-space: nowrap;
  font-size: .72rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--navy);
}
.marquee-dot { width: 4px; height: 4px; border-radius: 50%; background: var(--navy); opacity: .4; }
@keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }

/* ══════════════════════════════════════════════
   SECTIONS — SHARED
══════════════════════════════════════════════ */
.section { padding: 140px 5vw; position: relative; }
.s-navy { background: var(--navy-2); }
.s-dark { background: var(--ink); }
.s-mid  { background: var(--navy-3); }

.container { max-width: 1200px; margin: 0 auto; }

.s-tag {
  display: inline-flex; align-items: center; gap: .7rem;
  font-size: .68rem; font-weight: 700; letter-spacing: .16em;
  text-transform: uppercase; color: var(--gold);
  margin-bottom: 1.5rem;
}
.s-tag-bar { width: 22px; height: 1px; background: var(--gold); }

h2 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2rem, 4vw, 3.2rem);
  font-weight: 400; color: var(--white);
  line-height: 1.15; letter-spacing: -.01em;
  margin-bottom: 1.5rem;
}
h2 em { font-style: italic; color: var(--gold-2); }

.s-desc {
  font-size: .97rem; font-weight: 300; color: var(--text-mid);
  line-height: 1.85; max-width: 520px;
}

/* Reveal */
.rv {
  opacity: 0; transform: translateY(32px);
  transition: opacity .8s cubic-bezier(.16,1,.3,1), transform .8s cubic-bezier(.16,1,.3,1);
}
.rv.on { opacity: 1; transform: translateY(0); }
.rv-left { opacity: 0; transform: translateX(-32px); transition: opacity .8s cubic-bezier(.16,1,.3,1), transform .8s cubic-bezier(.16,1,.3,1); }
.rv-left.on { opacity: 1; transform: translateX(0); }
.rv-right { opacity: 0; transform: translateX(32px); transition: opacity .8s cubic-bezier(.16,1,.3,1), transform .8s cubic-bezier(.16,1,.3,1); }
.rv-right.on { opacity: 1; transform: translateX(0); }

/* ══════════════════════════════════════════════
   SERVICES
══════════════════════════════════════════════ */
.services-top {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 4rem; align-items: end; margin-bottom: 5rem;
}

.services-grid {
  display: grid; grid-template-columns: repeat(2,1fr);
  gap: 1px; background: var(--border);
  border: 1px solid var(--border); border-radius: 24px; overflow: hidden;
}

.svc {
  background: var(--navy-2);
  padding: 3rem 2.5rem; position: relative; overflow: hidden;
  cursor: none;
  transition: background .4s;
}
.svc::after {
  content: '';
  position: absolute; bottom: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  transform: scaleX(0); transition: transform .5s cubic-bezier(.16,1,.3,1);
}
.svc:hover { background: rgba(200,168,75,.05); }
.svc:hover::after { transform: scaleX(1); }

.svc-num {
  font-family: 'Playfair Display', serif;
  font-size: 4.5rem; font-weight: 700; line-height: 1;
  color: rgba(200,168,75,.06);
  position: absolute; top: 2rem; right: 2rem;
  transition: color .4s;
}
.svc:hover .svc-num { color: rgba(200,168,75,.12); }

.svc-chip {
  display: inline-block;
  background: var(--gold-dim); border: 1px solid var(--border-hi);
  color: var(--gold-2); border-radius: 100px;
  font-size: .65rem; font-weight: 700; letter-spacing: .1em; text-transform: uppercase;
  padding: .3rem .85rem; margin-bottom: 1.5rem;
  transition: background .3s;
}
.svc:hover .svc-chip { background: rgba(200,168,75,.2); }

.svc-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.4rem; font-weight: 500; color: var(--white);
  margin-bottom: .85rem; line-height: 1.3;
}
.svc-desc {
  font-size: .875rem; font-weight: 300; color: var(--text-mid); line-height: 1.8;
}

/* ══════════════════════════════════════════════
   PILIERS
══════════════════════════════════════════════ */
.piliers-wrap {
  display: grid; grid-template-columns: 1fr 1fr 1fr;
  gap: 2rem;
}

.pilier {
  border: 1px solid var(--border);
  border-radius: 20px; padding: 2.75rem 2.25rem;
  position: relative; overflow: hidden; cursor: none;
  background: var(--navy-2);
  transition: border-color .35s, transform .35s, box-shadow .35s;
}
.pilier::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  transform: scaleX(0); transform-origin: center;
  transition: transform .5s cubic-bezier(.16,1,.3,1);
}
.pilier:hover {
  border-color: var(--border-hi);
  transform: translateY(-8px);
  box-shadow: 0 24px 64px rgba(0,0,0,.4), 0 0 0 1px var(--border-hi);
}
.pilier:hover::before { transform: scaleX(1); }

.pilier-n {
  font-family: 'Playfair Display', serif;
  font-size: 5rem; font-weight: 700; color: rgba(200,168,75,.07);
  line-height: 1; margin-bottom: 1.5rem; display: block;
  transition: color .4s;
}
.pilier:hover .pilier-n { color: rgba(200,168,75,.14); }

.pilier-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.35rem; font-weight: 500; color: var(--white); margin-bottom: .75rem;
}
.pilier-desc { font-size: .875rem; font-weight: 300; color: var(--text-mid); line-height: 1.8; }

.pilier-icon-wrap {
  width: 44px; height: 44px; border-radius: 12px;
  background: var(--gold-dim); border: 1px solid var(--border-hi);
  display: flex; align-items: center; justify-content: center;
  font-size: 1.1rem; margin-bottom: 1.5rem;
  transition: background .3s;
}
.pilier:hover .pilier-icon-wrap { background: rgba(200,168,75,.2); }

/* ══════════════════════════════════════════════
   PROCESSUS — TIMELINE
══════════════════════════════════════════════ */
.process-layout {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 7rem; align-items: start;
}

.timeline { position: relative; padding-left: 3rem; }
.timeline::before {
  content: '';
  position: absolute; left: 9px; top: 8px; bottom: 8px; width: 1px;
  background: linear-gradient(to bottom, var(--gold), rgba(200,168,75,.1));
}

.tl-step {
  position: relative; margin-bottom: 3rem; cursor: none;
}
.tl-step:last-child { margin-bottom: 0; }

.tl-dot {
  position: absolute; left: -3rem; top: 4px;
  width: 20px; height: 20px; border-radius: 50%;
  border: 1px solid var(--gold);
  background: var(--ink);
  display: flex; align-items: center; justify-content: center;
  transition: background .3s, box-shadow .3s;
}
.tl-dot::after {
  content: ''; width: 6px; height: 6px; border-radius: 50%;
  background: var(--gold);
  transition: transform .3s;
}
.tl-step:hover .tl-dot {
  background: var(--gold-dim);
  box-shadow: 0 0 16px var(--gold-glow);
}
.tl-step:hover .tl-dot::after { transform: scale(1.5); }

.tl-n {
  font-size: .65rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--gold); margin-bottom: .6rem;
}
.tl-title { font-size: 1.05rem; font-weight: 600; color: var(--white); margin-bottom: .5rem; }
.tl-desc { font-size: .875rem; font-weight: 300; color: var(--text-mid); line-height: 1.75; }

/* Process right — visual card */
.process-visual {
  position: sticky; top: 120px;
  background: var(--navy-3);
  border: 1px solid var(--border);
  border-radius: 24px; overflow: hidden;
}
.process-visual-top {
  padding: 2.5rem;
  background: linear-gradient(135deg, rgba(200,168,75,.08), transparent);
  border-bottom: 1px solid var(--border);
}
.pv-label {
  font-size: .65rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--gold); margin-bottom: 1rem;
}
.pv-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem; font-weight: 400; color: var(--white); line-height: 1.35;
}
.process-visual-body { padding: 2rem 2.5rem; }
.pv-list { list-style: none; display: flex; flex-direction: column; gap: 1rem; }
.pv-item {
  display: flex; align-items: center; gap: 1rem;
  font-size: .875rem; font-weight: 300; color: var(--text-mid);
  padding: .85rem 1.1rem;
  border: 1px solid var(--border);
  border-radius: 12px;
  transition: border-color .25s, background .25s;
  cursor: none;
}
.pv-item:hover {
  border-color: var(--border-hi);
  background: var(--gold-dim);
  color: var(--text);
}
.pv-item-icon {
  width: 32px; height: 32px; border-radius: 8px;
  background: var(--gold-dim); flex-shrink: 0;
  display: flex; align-items: center; justify-content: center; font-size: .9rem;
}

/* ══════════════════════════════════════════════
   MAP SECTION
══════════════════════════════════════════════ */
.map-section { padding: 0; }
.map-layout {
  display: grid; grid-template-columns: 1fr 1fr;
  min-height: 600px;
}
.map-left {
  padding: 8rem 5vw 8rem 5vw;
  background: var(--navy-2);
  display: flex; flex-direction: column; justify-content: center;
}
.map-right { position: relative; overflow: hidden; }
#map { width: 100%; height: 100%; min-height: 600px; }

.map-overlay-card {
  position: absolute; bottom: 2rem; left: 2rem;
  background: rgba(5,9,15,.88);
  border: 1px solid var(--border-hi);
  border-radius: 16px; padding: 1.25rem 1.5rem;
  backdrop-filter: blur(16px);
  z-index: 10;
}
.moc-title { font-size: .85rem; font-weight: 600; color: var(--white); margin-bottom: .3rem; }
.moc-sub { font-size: .75rem; font-weight: 300; color: var(--text-mid); }
.moc-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: #4ade80; display: inline-block; margin-right: .4rem;
  box-shadow: 0 0 8px #4ade80;
}

.zone-list { display: flex; flex-direction: column; gap: .75rem; margin-top: 2.5rem; }
.zone-item {
  display: flex; align-items: center; gap: 1rem;
  font-size: .875rem; font-weight: 400; color: var(--text-mid);
  padding: .85rem 1.1rem;
  border: 1px solid var(--border);
  border-radius: 12px;
  transition: border-color .25s, color .25s, background .25s;
  cursor: none;
}
.zone-item:hover {
  border-color: var(--gold);
  color: var(--text);
  background: var(--gold-dim);
}
.zone-pin { color: var(--gold); font-size: 1rem; flex-shrink: 0; }

/* Leaflet custom overrides */
.leaflet-container { background: #0D1628 !important; }
.leaflet-tile { filter: invert(1) hue-rotate(180deg) brightness(.8) contrast(1.2) saturate(.6); }
.leaflet-control-zoom { display: none; }
.leaflet-control-attribution { background: rgba(5,9,15,.7) !important; color: rgba(255,255,255,.3) !important; font-size: 9px !important; }
.leaflet-control-attribution a { color: var(--gold) !important; }

/* ══════════════════════════════════════════════
   À PROPOS
══════════════════════════════════════════════ */
.apropos-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 7rem; align-items: center;
}
.apropos-quote {
  font-family: 'Playfair Display', serif;
  font-size: 1.55rem; font-style: italic; font-weight: 400;
  color: var(--white); line-height: 1.5;
  border-left: 2px solid var(--gold);
  padding-left: 2rem; margin: 2.5rem 0;
}
.apropos-body {
  font-size: .95rem; font-weight: 300; color: var(--text-mid); line-height: 1.9;
}
.apropos-sig {
  display: flex; align-items: center; gap: 1rem; margin-top: 2.5rem;
}
.apropos-sig-bar { width: 36px; height: 1px; background: var(--gold); }
.apropos-sig-text {
  font-size: .7rem; font-weight: 700; letter-spacing: .12em;
  text-transform: uppercase; color: var(--gold);
}

.apropos-panel {
  background: var(--navy-3); border: 1px solid var(--border);
  border-radius: 24px; overflow: hidden;
}
.ap-header {
  padding: 2.25rem 2.5rem; border-bottom: 1px solid var(--border);
  background: linear-gradient(135deg, rgba(200,168,75,.06), transparent);
}
.ap-header-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.2rem; font-weight: 500; color: var(--white);
}
.ap-body { padding: 2rem 2.5rem; }
.ap-list { list-style: none; display: flex; flex-direction: column; gap: .85rem; }
.ap-item {
  display: flex; gap: 1rem; align-items: flex-start;
  font-size: .875rem; font-weight: 300; color: var(--text-mid); line-height: 1.7;
  padding: .85rem 1rem; border-radius: 10px;
  border: 1px solid transparent;
  transition: border-color .25s, background .25s, color .25s;
  cursor: none;
}
.ap-item:hover {
  border-color: var(--border);
  background: var(--gold-dim);
  color: var(--text);
}
.ap-arr { color: var(--gold); font-size: .8rem; margin-top: .22rem; flex-shrink: 0; font-weight: 600; }

/* ══════════════════════════════════════════════
   PLATEFORMES
══════════════════════════════════════════════ */
.platforms-section {
  padding: 5rem 5vw;
  background: var(--ink);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
}
.platforms-inner {
  max-width: 1200px; margin: 0 auto;
  display: flex; align-items: center; gap: 4rem; flex-wrap: wrap;
}
.platforms-label {
  font-size: .65rem; font-weight: 700; letter-spacing: .16em;
  text-transform: uppercase; color: var(--text-low); white-space: nowrap;
  flex-shrink: 0;
}
.platforms-list { display: flex; gap: 1rem; flex-wrap: wrap; }
.p-pill {
  display: flex; align-items: center; gap: .6rem;
  background: rgba(255,255,255,.03); border: 1px solid var(--border);
  border-radius: 100px; padding: .6rem 1.25rem;
  font-size: .8rem; font-weight: 500; color: var(--text-mid);
  transition: border-color .25s, background .25s, color .25s, transform .2s;
  cursor: none;
}
.p-pill:hover {
  border-color: var(--border-hi);
  background: var(--gold-dim);
  color: var(--gold-2);
  transform: translateY(-2px);
}
.p-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }

/* ══════════════════════════════════════════════
   AUDIT / CTA
══════════════════════════════════════════════ */
.audit-section {
  padding: 140px 5vw;
  background: linear-gradient(135deg, #0D1628 0%, #05090F 50%, #0D1628 100%);
  position: relative; overflow: hidden;
}
.audit-section::before {
  content: '';
  position: absolute; top: 50%; left: 50%; transform: translate(-50%,-50%);
  width: 800px; height: 800px;
  background: radial-gradient(circle, rgba(200,168,75,.07) 0%, transparent 65%);
  pointer-events: none;
}
.audit-inner { max-width: 900px; margin: 0 auto; position: relative; z-index: 1; }
.audit-header { margin-bottom: 4rem; }

.audit-grid {
  display: grid; grid-template-columns: repeat(4,1fr);
  gap: 1px; background: var(--border);
  border: 1px solid var(--border); border-radius: 20px; overflow: hidden;
  margin-bottom: 3.5rem;
}
.a-item {
  background: var(--navy-2);
  padding: 2rem 1.5rem; text-align: center;
  transition: background .3s; cursor: none;
}
.a-item:hover { background: rgba(200,168,75,.07); }
.a-icon { font-size: 1.5rem; display: block; margin-bottom: .85rem; }
.a-label { font-size: .82rem; font-weight: 400; color: var(--text-mid); line-height: 1.5; }
.a-item:hover .a-label { color: var(--text); }

.audit-foot {
  display: flex; align-items: center; gap: 2.5rem; flex-wrap: wrap;
}
.audit-note {
  font-size: .8rem; font-weight: 300; color: var(--text-low); line-height: 1.7;
}

/* ══════════════════════════════════════════════
   FOOTER
══════════════════════════════════════════════ */
footer {
  background: #030609;
  border-top: 1px solid var(--border);
  padding: 5rem 5vw 2.5rem;
}
.footer-grid {
  max-width: 1200px; margin: 0 auto;
  display: grid; grid-template-columns: 2fr 1fr 1fr 1.5fr;
  gap: 4rem; padding-bottom: 4rem;
  border-bottom: 1px solid rgba(255,255,255,.05);
  margin-bottom: 2rem;
}
.footer-logo {
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem; font-weight: 600; color: var(--white);
  margin-bottom: .85rem;
}
.footer-logo span { color: var(--gold); }
.footer-tagline {
  font-size: .82rem; font-weight: 300; color: var(--text-low);
  line-height: 1.7; max-width: 240px;
}
.footer-col-title {
  font-size: .65rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--text-low);
  margin-bottom: 1.25rem;
}
.footer-col-links { display: flex; flex-direction: column; gap: .65rem; }
.footer-col-links a {
  font-size: .82rem; font-weight: 300; color: var(--text-mid);
  text-decoration: none; transition: color .2s;
  cursor: none;
}
.footer-col-links a:hover { color: var(--gold-2); }
.footer-email-link {
  display: flex; align-items: center; gap: .6rem;
  font-size: .88rem; font-weight: 500; color: var(--gold);
  text-decoration: none; transition: opacity .2s; cursor: none;
}
.footer-email-link:hover { opacity: .7; }
.footer-zone-text {
  font-size: .78rem; font-weight: 300; color: var(--text-low);
  margin-top: .75rem; display: flex; align-items: center; gap: .45rem;
}
.footer-bottom {
  max-width: 1200px; margin: 0 auto;
  display: flex; justify-content: space-between; align-items: center;
  flex-wrap: wrap; gap: 1rem;
}
.footer-copy { font-size: .74rem; color: var(--text-low); }
.footer-legal { display: flex; gap: 1.5rem; }
.footer-legal a {
  font-size: .74rem; color: var(--text-low);
  text-decoration: none; transition: color .2s; cursor: none;
}
.footer-legal a:hover { color: var(--gold); }

/* Gold divider line */
.gold-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
  opacity: .3; margin: 0;
}

/* ══════════════════════════════════════════════
   KEYFRAMES
══════════════════════════════════════════════ */
@keyframes fadeSlideUp {
  from { opacity: 0; transform: translateY(28px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

/* ══════════════════════════════════════════════
   RESPONSIVE
══════════════════════════════════════════════ */
@media (max-width: 1024px) {
  .hero-content { grid-template-columns: 1fr; }
  .hero-right { display: none; }
  .hero-bg-text { display: none; }
  .services-top { grid-template-columns: 1fr; gap: 2rem; }
  .piliers-wrap { grid-template-columns: 1fr 1fr; }
  .process-layout { grid-template-columns: 1fr; }
  .process-visual { position: static; }
  .map-layout { grid-template-columns: 1fr; }
  .apropos-grid { grid-template-columns: 1fr; gap: 3rem; }
  .audit-grid { grid-template-columns: 1fr 1fr; }
  .footer-grid { grid-template-columns: 1fr 1fr; gap: 2.5rem; }
}
@media (max-width: 768px) {
  .section { padding: 100px 5vw; }
  .services-grid { grid-template-columns: 1fr; }
  .piliers-wrap { grid-template-columns: 1fr; }
  .nav-center { display: none; }
  .audit-grid { grid-template-columns: 1fr 1fr; }
  #map { min-height: 400px; }
}
@media (max-width: 500px) {
  .audit-grid { grid-template-columns: 1fr 1fr; }
  .hero-btns { flex-direction: column; }
  .footer-grid { grid-template-columns: 1fr; }
  body { cursor: auto; }
  #cursor, #cursor-ring { display: none; }
}
</style>
<!-- Leaflet CSS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
</head>
<body>

<!-- Custom Cursor -->
<div id="cursor"></div>
<div id="cursor-ring"></div>

<!-- Particles Canvas -->
<canvas id="particles-canvas"></canvas>

<!-- ─── NAV ─── -->
<nav id="nav">
  <a href="#" class="nav-logo">Renova<span class="nav-logo-accent">·</span>Stay</a>
  <ul class="nav-center">
    <li><a href="#services">Services</a></li>
    <li><a href="#valeur">Approche</a></li>
    <li><a href="#processus">Méthode</a></li>
    <li><a href="#zone">Zone</a></li>
    <li><a href="#propos">À propos</a></li>
  </ul>
  <a href="#audit" class="nav-btn">
    <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
    Audit gratuit
  </a>
</nav>

<!-- ─── HERO ─── -->
<section class="hero">
  <div class="hero-mesh"></div>
  <div class="hero-noise"></div>
  <div class="hero-bg-text">RENOVA</div>

  <div class="hero-content">
    <div class="hero-left">
      <div class="hero-tag">
        <span class="hero-tag-line"></span>
        Gestion locative · Moselle
      </div>
      <h1 class="hero-h1">
        La gestion locative<br>
        <em>nouvelle génération</em><br>
        pour la courte durée.
      </h1>
      <p class="hero-sub">Nous accompagnons les propriétaires dans l'optimisation, l'exploitation et la gestion quotidienne de leurs biens afin d'améliorer leur rentabilité et l'expérience des voyageurs.</p>
      <div class="hero-btns">
        <a href="#audit" class="btn-primary">
          Demander un audit gratuit
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
        <a href="#services" class="btn-secondary">Découvrir nos services</a>
      </div>
    </div>

    <div class="hero-right">
      <div class="hero-cards">
        <div class="hcard">
          <div class="hcard-icon">🗓️</div>
          <div>
            <div class="hcard-title">Réactivité 7j/7</div>
            <div class="hcard-sub">Disponibles chaque jour pour vous et vos voyageurs.</div>
          </div>
          <div class="hcard-badge">Active</div>
        </div>
        <div class="hcard">
          <div class="hcard-icon">🤝</div>
          <div>
            <div class="hcard-title">Accompagnement personnalisé</div>
            <div class="hcard-sub">Un interlocuteur dédié à chaque propriétaire.</div>
          </div>
        </div>
        <div class="hcard">
          <div class="hcard-icon">⚙️</div>
          <div>
            <div class="hcard-title">Service sur mesure</div>
            <div class="hcard-sub">Des solutions adaptées à chaque bien et objectif.</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="hero-scroll">
    <span class="hero-scroll-text">Défiler</span>
    <div class="hero-scroll-line"></div>
  </div>
</section>

<!-- ─── MARQUEE ─── -->
<div class="marquee-band">
  <div class="marquee-track" id="marquee-track">
    <div class="marquee-item">Gestion locative<span class="marquee-dot"></span></div>
    <div class="marquee-item">Moselle<span class="marquee-dot"></span></div>
    <div class="marquee-item">Optimisation d'annonces<span class="marquee-dot"></span></div>
    <div class="marquee-item">Renova Stay<span class="marquee-dot"></span></div>
    <div class="marquee-item">Courte durée<span class="marquee-dot"></span></div>
    <div class="marquee-item">Airbnb · Booking<span class="marquee-dot"></span></div>
    <div class="marquee-item">Expérience voyageur<span class="marquee-dot"></span></div>
    <div class="marquee-item">7j/7<span class="marquee-dot"></span></div>
    <div class="marquee-item">Gestion locative<span class="marquee-dot"></span></div>
    <div class="marquee-item">Moselle<span class="marquee-dot"></span></div>
    <div class="marquee-item">Optimisation d'annonces<span class="marquee-dot"></span></div>
    <div class="marquee-item">Renova Stay<span class="marquee-dot"></span></div>
    <div class="marquee-item">Courte durée<span class="marquee-dot"></span></div>
    <div class="marquee-item">Airbnb · Booking<span class="marquee-dot"></span></div>
    <div class="marquee-item">Expérience voyageur<span class="marquee-dot"></span></div>
    <div class="marquee-item">7j/7<span class="marquee-dot"></span></div>
  </div>
</div>

<!-- ─── SERVICES ─── -->
<section id="services" class="section s-navy">
  <div class="container">
    <div class="services-top">
      <div>
        <div class="s-tag rv"><span class="s-tag-bar"></span>Services</div>
        <h2 class="rv">Une prise en charge <em>complète</em></h2>
      </div>
      <p class="s-desc rv">De la communication voyageurs à la coordination terrain — nous gérons chaque aspect de votre activité locative sur toutes les plateformes.</p>
    </div>

    <div class="services-grid rv">
      <div class="svc">
        <div class="svc-num">01</div>
        <div class="svc-chip">Communication</div>
        <div class="svc-title">Gestion voyageurs</div>
        <p class="svc-desc">Nous assurons les échanges avec les voyageurs avant, pendant et après leur séjour pour une expérience irréprochable à chaque étape.</p>
      </div>
      <div class="svc">
        <div class="svc-num">02</div>
        <div class="svc-chip">Performance</div>
        <div class="svc-title">Optimisation des annonces</div>
        <p class="svc-desc">Analyse des performances, amélioration des descriptions, optimisation du positionnement et valorisation du bien sur toutes les plateformes.</p>
      </div>
      <div class="svc">
        <div class="svc-num">03</div>
        <div class="svc-chip">Opérationnel</div>
        <div class="svc-title">Coordination opérationnelle</div>
        <p class="svc-desc">Organisation des arrivées, départs et interventions nécessaires au bon fonctionnement du logement, sans que vous ayez à intervenir.</p>
      </div>
      <div class="svc">
        <div class="svc-num">04</div>
        <div class="svc-chip">Suivi</div>
        <div class="svc-title">Accompagnement propriétaire</div>
        <p class="svc-desc">Un interlocuteur unique et un suivi transparent de l'activité, avec des rapports réguliers et une communication directe en permanence.</p>
      </div>
    </div>
  </div>
</section>

<!-- ─── PLATEFORMES ─── -->
<div class="platforms-section">
  <div class="platforms-inner">
    <span class="platforms-label">Plateformes gérées</span>
    <div class="platforms-list">
      <div class="p-pill"><span class="p-dot" style="background:#FF5A5F"></span>Airbnb</div>
      <div class="p-pill"><span class="p-dot" style="background:#003580"></span>Booking.com</div>
      <div class="p-pill"><span class="p-dot" style="background:#E7711B"></span>Abritel</div>
      <div class="p-pill"><span class="p-dot" style="background:#00A9CE"></span>Expedia</div>
      <div class="p-pill"><span class="p-dot" style="background:#C9A84C"></span>& autres</div>
    </div>
  </div>
</div>

<!-- ─── VALEUR ─── -->
<section id="valeur" class="section s-dark">
  <div class="container">
    <div style="margin-bottom:5rem;">
      <div class="s-tag rv"><span class="s-tag-bar"></span>Notre approche</div>
      <h2 class="rv">Plus qu'un simple <em>co-hôte</em></h2>
      <p class="s-desc rv">Notre approche repose sur trois piliers fondamentaux qui font la différence sur le long terme.</p>
    </div>
    <div class="piliers-wrap">
      <div class="pilier rv">
        <div class="pilier-icon-wrap">📈</div>
        <span class="pilier-n">01</span>
        <div class="pilier-title">Performance</div>
        <p class="pilier-desc">Chaque logement possède un potentiel inexploité que nous aidons à révéler grâce à une analyse rigoureuse du marché et des données de performance.</p>
      </div>
      <div class="pilier rv">
        <div class="pilier-icon-wrap">⭐</div>
        <span class="pilier-n">02</span>
        <div class="pilier-title">Expérience voyageur</div>
        <p class="pilier-desc">Une expérience fluide et mémorable génère de meilleurs avis, favorise les réservations et fidélise les voyageurs sur la durée.</p>
      </div>
      <div class="pilier rv">
        <div class="pilier-icon-wrap">🔍</div>
        <span class="pilier-n">03</span>
        <div class="pilier-title">Transparence</div>
        <p class="pilier-desc">Une communication claire, des comptes-rendus réguliers et un suivi détaillé de chaque action réalisée sur votre bien, sans zone d'ombre.</p>
      </div>
    </div>
  </div>
</section>

<!-- ─── PROCESSUS ─── -->
<section id="processus" class="section s-mid">
  <div class="container">
    <div class="s-tag rv" style="margin-bottom:1.5rem;"><span class="s-tag-bar"></span>Méthode</div>
    <h2 class="rv" style="margin-bottom:5rem;">Comment nous <em>travaillons</em></h2>
    <div class="process-layout">
      <div class="timeline">
        <div class="tl-step rv">
          <div class="tl-dot"></div>
          <div class="tl-n">Étape 01</div>
          <div class="tl-title">Analyse du logement</div>
          <p class="tl-desc">Étude approfondie de votre annonce, de votre positionnement et de votre marché local en Moselle pour établir un diagnostic précis.</p>
        </div>
        <div class="tl-step rv">
          <div class="tl-dot"></div>
          <div class="tl-n">Étape 02</div>
          <div class="tl-title">Recommandations</div>
          <p class="tl-desc">Identification des opportunités d'amélioration concrètes, priorisées par impact potentiel sur votre rentabilité.</p>
        </div>
        <div class="tl-step rv">
          <div class="tl-dot"></div>
          <div class="tl-n">Étape 03</div>
          <div class="tl-title">Mise en place</div>
          <p class="tl-desc">Déploiement des optimisations et organisation structurée de la gestion quotidienne sans perturber votre activité.</p>
        </div>
        <div class="tl-step rv">
          <div class="tl-dot"></div>
          <div class="tl-n">Étape 04</div>
          <div class="tl-title">Suivi continu</div>
          <p class="tl-desc">Amélioration continue des performances et de l'expérience voyageur, semaine après semaine, avec des rapports transparents.</p>
        </div>
      </div>

      <div class="process-visual rv-right">
        <div class="process-visual-top">
          <div class="pv-label">Approche Renova Stay</div>
          <div class="pv-title">Une gestion rigoureuse,<br>des résultats durables.</div>
        </div>
        <div class="process-visual-body">
          <ul class="pv-list">
            <li class="pv-item"><div class="pv-item-icon">🗓️</div>Disponibilité 7 jours sur 7</li>
            <li class="pv-item"><div class="pv-item-icon">📊</div>Rapports mensuels détaillés</li>
            <li class="pv-item"><div class="pv-item-icon">🤝</div>Interlocuteur unique dédié</li>
            <li class="pv-item"><div class="pv-item-icon">🔧</div>Coordination opérationnelle complète</li>
            <li class="pv-item"><div class="pv-item-icon">🌍</div>Toutes plateformes gérées</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ─── CARTE MOSELLE ─── -->
<section id="zone" class="map-section">
  <div class="map-layout">
    <div class="map-left">
      <div class="s-tag rv-left"><span class="s-tag-bar"></span>Zone d'intervention</div>
      <h2 class="rv-left">Basés en <em>Moselle</em>,<br>à votre service.</h2>
      <p class="s-desc rv-left" style="margin-bottom:2.5rem;">Nous intervenons sur l'ensemble du département de la Moselle et ses alentours, avec une connaissance fine du marché local.</p>
      <div class="zone-list">
        <div class="zone-item rv-left">
          <span class="zone-pin">📍</span>
          Metz & agglomération
        </div>
        <div class="zone-item rv-left">
          <span class="zone-pin">📍</span>
          Thionville & vallée de la Moselle
        </div>
        <div class="zone-item rv-left">
          <span class="zone-pin">📍</span>
          Sarreguemines & Est mosellan
        </div>
        <div class="zone-item rv-left">
          <span class="zone-pin">📍</span>
          Forbach & bassin houiller
        </div>
        <div class="zone-item rv-left">
          <span class="zone-pin">📍</span>
          Ensemble du département 57
        </div>
      </div>
    </div>
    <div class="map-right">
      <div id="map"></div>
      <div class="map-overlay-card">
        <div class="moc-title"><span class="moc-dot"></span>Zone active</div>
        <div class="moc-sub">Moselle (57) · Grand Est</div>
      </div>
    </div>
  </div>
</section>

<!-- ─── À PROPOS ─── -->
<section id="propos" class="section s-navy">
  <div class="container">
    <div class="apropos-grid">
      <div>
        <div class="s-tag rv-left"><span class="s-tag-bar"></span>À propos</div>
        <h2 class="rv-left">Notre <em>vision</em></h2>
        <blockquote class="apropos-quote rv-left">
          "La gestion locative courte durée doit être plus simple, plus transparente et plus performante pour les propriétaires."
        </blockquote>
        <p class="apropos-body rv-left">Notre mission est d'apporter une approche moderne de l'exploitation des locations saisonnières en Moselle, grâce à une organisation rigoureuse et une attention particulière portée à l'expérience voyageur.<br><br>Nous croyons que chaque propriétaire mérite un accompagnement à la hauteur de son investissement — sans compromis sur la qualité ni sur la transparence.</p>
        <div class="apropos-sig rv-left">
          <div class="apropos-sig-bar"></div>
          <div class="apropos-sig-text">Renova Stay · Moselle, France</div>
        </div>
      </div>
      <div class="apropos-panel rv-right">
        <div class="ap-header">
          <div class="ap-header-title">Ce qui nous distingue</div>
        </div>
        <div class="ap-body">
          <ul class="ap-list">
            <li class="ap-item"><span class="ap-arr">→</span>Une gestion orientée performance, pas seulement réactive aux demandes entrantes.</li>
            <li class="ap-item"><span class="ap-arr">→</span>Un suivi transparent avec des rapports réguliers sur l'activité de votre bien.</li>
            <li class="ap-item"><span class="ap-arr">→</span>Une approche sur mesure adaptée à chaque logement et chaque propriétaire.</li>
            <li class="ap-item"><span class="ap-arr">→</span>Une attention constante portée à la qualité de l'expérience voyageur sur toutes les plateformes.</li>
            <li class="ap-item"><span class="ap-arr">→</span>Un interlocuteur unique, disponible 7j/7 et pleinement impliqué dans votre projet.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ─── AUDIT ─── -->
<section id="audit" class="audit-section">
  <div class="audit-inner">
    <div class="audit-header">
      <div class="s-tag rv"><span class="s-tag-bar"></span>Offre de bienvenue</div>
      <h2 class="rv">Recevez une analyse <em>gratuite</em><br>de votre annonce</h2>
      <p class="s-desc rv">Un audit complet pour identifier les axes d'amélioration et révéler le véritable potentiel de votre logement en Moselle.</p>
    </div>
    <div class="audit-grid rv">
      <div class="a-item">
        <span class="a-icon">📝</span>
        <span class="a-label">Titre de l'annonce</span>
      </div>
      <div class="a-item">
        <span class="a-icon">📸</span>
        <span class="a-label">Qualité des photos</span>
      </div>
      <div class="a-item">
        <span class="a-icon">✍️</span>
        <span class="a-label">Description du bien</span>
      </div>
      <div class="a-item">
        <span class="a-icon">💡</span>
        <span class="a-label">Points d'amélioration</span>
      </div>
    </div>
    <div class="audit-foot rv">
      <a href="mailto:delio.casciu10@gmail.com?subject=Demande%20d'audit%20gratuit%20-%20Renova%20Stay&body=Bonjour%2C%0A%0AJe%20souhaite%20recevoir%20un%20audit%20gratuit%20de%20mon%20annonce.%0A%0ALien%20de%20mon%20annonce%20%3A%20%0A%0AMerci." class="btn-primary">
        Recevoir mon audit gratuit
        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
      </a>
      <p class="audit-note">Gratuit &amp; sans engagement.<br>Réponse sous 48h.</p>
    </div>
  </div>
</section>

<!-- ─── FOOTER ─── -->
<div class="gold-divider"></div>
<footer>
  <div class="footer-grid">
    <div>
      <div class="footer-logo">Renova<span>·</span>Stay</div>
      <p class="footer-tagline">La gestion locative nouvelle génération pour les locations courte durée en Moselle.</p>
    </div>
    <div>
      <div class="footer-col-title">Navigation</div>
      <div class="footer-col-links">
        <a href="#services">Services</a>
        <a href="#valeur">Notre approche</a>
        <a href="#processus">Méthode</a>
        <a href="#zone">Zone d'intervention</a>
        <a href="#propos">À propos</a>
      </div>
    </div>
    <div>
      <div class="footer-col-title">Légal</div>
      <div class="footer-col-links">
        <a href="#">Mentions légales</a>
        <a href="#">Politique de confidentialité</a>
      </div>
    </div>
    <div>
      <div class="footer-col-title">Contact</div>
      <a href="mailto:delio.casciu10@gmail.com" class="footer-email-link">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
        delio.casciu10@gmail.com
      </a>
      <div class="footer-zone-text">
        <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
        Moselle (57) · Grand Est · France
      </div>
    </div>
  </div>
  <div class="footer-bottom">
    <div class="footer-copy">© 2025 Renova Stay. Tous droits réservés.</div>
    <div class="footer-legal">
      <a href="#">Mentions légales</a>
      <a href="#">Confidentialité</a>
    </div>
  </div>
</footer>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
/* ══════════════════════════════════════
   CURSOR
══════════════════════════════════════ */
const cur = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  cur.style.left = mx + 'px';
  cur.style.top  = my + 'px';
});

function animRing() {
  rx += (mx - rx) * .14;
  ry += (my - ry) * .14;
  ring.style.left = rx + 'px';
  ring.style.top  = ry + 'px';
  requestAnimationFrame(animRing);
}
animRing();

document.querySelectorAll('a, button, .hcard, .svc, .pilier, .pv-item, .a-item, .zone-item, .ap-item, .p-pill').forEach(el => {
  el.addEventListener('mouseenter', () => document.body.classList.add('hovering'));
  el.addEventListener('mouseleave', () => document.body.classList.remove('hovering'));
});
document.addEventListener('mousedown', () => document.body.classList.add('clicking'));
document.addEventListener('mouseup', () => document.body.classList.remove('clicking'));

/* ══════════════════════════════════════
   PARTICLES
══════════════════════════════════════ */
const canvas = document.getElementById('particles-canvas');
const ctx = canvas.getContext('2d');
let W, H, particles = [];

function resize() {
  W = canvas.width  = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);

function Particle() {
  this.x = Math.random() * W;
  this.y = Math.random() * H;
  this.r = Math.random() * 1.5 + .3;
  this.vx = (Math.random() - .5) * .3;
  this.vy = (Math.random() - .5) * .3;
  this.alpha = Math.random() * .4 + .1;
}
Particle.prototype.update = function() {
  this.x += this.vx; this.y += this.vy;
  if (this.x < 0) this.x = W;
  if (this.x > W) this.x = 0;
  if (this.y < 0) this.y = H;
  if (this.y > H) this.y = 0;
};

for (let i = 0; i < 80; i++) particles.push(new Particle());

function drawParticles() {
  ctx.clearRect(0, 0, W, H);
  particles.forEach(p => {
    p.update();
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(200,168,75,${p.alpha})`;
    ctx.fill();
  });
  // Draw lines between close particles
  for (let i = 0; i < particles.length; i++) {
    for (let j = i+1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x;
      const dy = particles[i].y - particles[j].y;
      const d = Math.sqrt(dx*dx + dy*dy);
      if (d < 120) {
        ctx.beginPath();
        ctx.moveTo(particles[i].x, particles[i].y);
        ctx.lineTo(particles[j].x, particles[j].y);
        ctx.strokeStyle = `rgba(200,168,75,${.06 * (1 - d/120)})`;
        ctx.lineWidth = .5;
        ctx.stroke();
      }
    }
  }
  requestAnimationFrame(drawParticles);
}
drawParticles();

/* ══════════════════════════════════════
   NAVBAR
══════════════════════════════════════ */
const nav = document.getElementById('nav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 60);
}, { passive: true });

/* ══════════════════════════════════════
   SCROLL REVEAL
══════════════════════════════════════ */
const rvEls = document.querySelectorAll('.rv, .rv-left, .rv-right');
const rvObs = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      // Stagger siblings of same class inside same parent
      const parent = entry.target.parentElement;
      const siblings = [...parent.querySelectorAll('.rv, .rv-left, .rv-right')].filter(el => !el.classList.contains('on'));
      const idx = [...parent.querySelectorAll('.rv, .rv-left, .rv-right')].indexOf(entry.target);
      setTimeout(() => {
        entry.target.classList.add('on');
      }, (idx % 6) * 100);
      rvObs.unobserve(entry.target);
    }
  });
}, { threshold: 0.08, rootMargin: '0px 0px -40px 0px' });
rvEls.forEach(el => rvObs.observe(el));

/* ══════════════════════════════════════
   LEAFLET MAP — MOSELLE
══════════════════════════════════════ */
const map = L.map('map', {
  center: [49.12, 6.18],
  zoom: 9,
  zoomControl: false,
  scrollWheelZoom: false,
  dragging: true,
  attributionControl: true
});

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap'
}).addTo(map);

// Custom gold marker
const goldIcon = L.divIcon({
  html: `<div style="
    width:14px; height:14px; border-radius:50%;
    background: #C8A84B;
    border: 2px solid rgba(200,168,75,.4);
    box-shadow: 0 0 12px rgba(200,168,75,.6), 0 0 24px rgba(200,168,75,.3);
  "></div>`,
  className: '',
  iconSize: [14, 14],
  iconAnchor: [7, 7]
});

const cities = [
  { name: 'Metz', lat: 49.1193, lng: 6.1757 },
  { name: 'Thionville', lat: 49.3584, lng: 6.1680 },
  { name: 'Forbach', lat: 49.1877, lng: 6.9027 },
  { name: 'Sarreguemines', lat: 49.1128, lng: 7.0674 },
  { name: 'Saint-Avold', lat: 49.1016, lng: 6.7063 },
  { name: 'Sarrebourg', lat: 48.7344, lng: 7.0538 },
];

cities.forEach(c => {
  L.marker([c.lat, c.lng], { icon: goldIcon })
    .addTo(map)
    .bindPopup(`<b style="color:#C8A84B">${c.name}</b><br><small style="color:#888">Zone couverte par Renova Stay</small>`, {
      className: 'custom-popup'
    });
});

// Moselle department outline (approximate bounding box highlight)
L.rectangle([[48.6, 5.9], [49.5, 7.3]], {
  color: 'rgba(200,168,75,.35)',
  weight: 1.5,
  fill: true,
  fillColor: 'rgba(200,168,75,.04)',
  dashArray: '4,6'
}).addTo(map);

/* ══════════════════════════════════════
   MAGNETIC BUTTONS
══════════════════════════════════════ */
document.querySelectorAll('.btn-primary, .btn-secondary, .nav-btn').forEach(btn => {
  btn.addEventListener('mousemove', e => {
    const r = btn.getBoundingClientRect();
    const x = e.clientX - r.left - r.width/2;
    const y = e.clientY - r.top  - r.height/2;
    btn.style.transform = `translate(${x*.18}px, ${y*.18}px)`;
  });
  btn.addEventListener('mouseleave', () => {
    btn.style.transform = '';
  });
});

/* ══════════════════════════════════════
   PARALLAX HERO BG TEXT
══════════════════════════════════════ */
const bgText = document.querySelector('.hero-bg-text');
if (bgText) {
  window.addEventListener('scroll', () => {
    bgText.style.transform = `translateY(calc(-50% + ${window.scrollY * .25}px))`;
  }, { passive: true });
}
</script>
</body>
</html>
