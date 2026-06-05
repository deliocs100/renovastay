# renovastay
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Renova Stay — Gestion Locative Courte Durée en Moselle</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;0,700;1,300;1,400;1,500&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
/* ════════════════════════════════════════
   TOKENS & RESET
════════════════════════════════════════ */
:root {
  --navy:        #0B1628;
  --navy-mid:    #132040;
  --navy-soft:   #1A2D55;
  --navy-muted:  #243660;
  --gold:        #C9A84C;
  --gold-light:  #E2C97E;
  --gold-pale:   #F5EDD6;
  --gold-ultra:  #FDF8EE;
  --cream:       #F9F6F0;
  --cream-dark:  #EDE8DF;
  --white:       #FFFFFF;
  --text:        #0B1628;
  --text-mid:    #2A3A5C;
  --text-muted:  #5A6A8A;
  --text-light:  #9AAABF;
  --border:      #E4DDD0;
  --border-dark: #C9A84C33;
  --radius:      20px;
  --radius-sm:   12px;
  --radius-xs:   8px;
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: 'Outfit', sans-serif;
  background: var(--cream);
  color: var(--text);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
  overflow-x: hidden;
}

/* ════════════════════════════════════════
   SCROLLBAR
════════════════════════════════════════ */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--cream); }
::-webkit-scrollbar-thumb { background: var(--gold); border-radius: 3px; }

/* ════════════════════════════════════════
   NAV
════════════════════════════════════════ */
nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 200;
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 6vw; height: 80px;
  transition: all .4s cubic-bezier(.16,1,.3,1);
}
nav.scrolled {
  background: rgba(11,22,40,.96);
  backdrop-filter: blur(20px);
  height: 68px;
  box-shadow: 0 1px 0 rgba(201,168,76,.15), 0 8px 40px rgba(0,0,0,.25);
}

.nav-logo {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem; font-weight: 600;
  color: var(--white); letter-spacing: .04em;
  text-decoration: none; display: flex; align-items: center; gap: .35rem;
}
.nav-logo-dot { color: var(--gold); font-size: 2rem; line-height: 0; position: relative; top: 4px; }

.nav-links { display: flex; gap: 2.5rem; list-style: none; }
.nav-links a {
  font-size: .82rem; font-weight: 500; letter-spacing: .06em;
  text-transform: uppercase;
  color: rgba(255,255,255,.55);
  text-decoration: none;
  transition: color .2s;
}
.nav-links a:hover { color: var(--gold-light); }

.nav-cta {
  display: flex; align-items: center; gap: .5rem;
  background: transparent;
  border: 1px solid var(--gold);
  color: var(--gold); cursor: pointer;
  padding: .6rem 1.4rem; border-radius: 100px;
  font-family: 'Outfit', sans-serif;
  font-size: .8rem; font-weight: 600; letter-spacing: .05em; text-transform: uppercase;
  text-decoration: none;
  transition: background .25s, color .25s, transform .15s, box-shadow .25s;
}
.nav-cta:hover {
  background: var(--gold); color: var(--navy);
  transform: translateY(-1px);
  box-shadow: 0 4px 20px rgba(201,168,76,.35);
}

/* ════════════════════════════════════════
   HERO
════════════════════════════════════════ */
.hero {
  min-height: 100vh;
  background: var(--navy);
  display: flex; flex-direction: column;
  justify-content: center;
  position: relative; overflow: hidden;
  padding: 120px 6vw 100px;
}

/* Decorative background elements */
.hero-bg-circle {
  position: absolute; border-radius: 50%;
  pointer-events: none;
}
.hero-bg-circle-1 {
  width: 700px; height: 700px;
  top: -200px; right: -150px;
  background: radial-gradient(circle, rgba(201,168,76,.08) 0%, transparent 70%);
}
.hero-bg-circle-2 {
  width: 400px; height: 400px;
  bottom: -100px; left: -100px;
  background: radial-gradient(circle, rgba(201,168,76,.05) 0%, transparent 70%);
}
.hero-grid-line {
  position: absolute; pointer-events: none;
  border: 1px solid rgba(201,168,76,.04);
}
.hero-grid-line-1 { inset: 80px; border-radius: 40px; }
.hero-grid-line-2 { inset: 40px; border-radius: 50px; opacity: .5; }

/* Gold divider line */
.hero-line {
  width: 60px; height: 1px; background: var(--gold);
  margin-bottom: 2rem; opacity: .8;
}

.hero-inner {
  position: relative; z-index: 2;
  max-width: 1200px; margin: 0 auto; width: 100%;
  display: grid; grid-template-columns: 1fr 420px;
  gap: 80px; align-items: center;
}

.hero-eyebrow {
  display: inline-flex; align-items: center; gap: .75rem;
  margin-bottom: 1.75rem;
  font-size: .72rem; font-weight: 600;
  letter-spacing: .14em; text-transform: uppercase;
  color: var(--gold);
}
.hero-eyebrow-bar {
  width: 32px; height: 1px; background: var(--gold); opacity: .7;
}

h1 {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(3rem, 5.5vw, 5rem);
  font-weight: 300; line-height: 1.08;
  color: var(--white); letter-spacing: -.01em;
  margin-bottom: 1.75rem;
}
h1 strong {
  font-weight: 600;
  background: linear-gradient(135deg, var(--gold-light) 0%, var(--gold) 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  background-clip: text;
  display: block;
}

.hero-sub {
  font-size: 1.05rem; font-weight: 300;
  color: rgba(255,255,255,.5);
  line-height: 1.8; max-width: 500px;
  margin-bottom: 3rem;
}

.hero-actions { display: flex; gap: 1rem; flex-wrap: wrap; }

.btn-gold {
  display: inline-flex; align-items: center; gap: .6rem;
  background: linear-gradient(135deg, var(--gold-light) 0%, var(--gold) 100%);
  color: var(--navy); border: none; cursor: pointer;
  padding: .95rem 2rem; border-radius: 100px;
  font-family: 'Outfit', sans-serif;
  font-size: .85rem; font-weight: 700; letter-spacing: .04em;
  text-decoration: none;
  transition: transform .2s, box-shadow .2s, filter .2s;
  box-shadow: 0 4px 24px rgba(201,168,76,.3);
}
.btn-gold:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 40px rgba(201,168,76,.45);
  filter: brightness(1.05);
}
.btn-gold svg { transition: transform .2s; }
.btn-gold:hover svg { transform: translateX(3px); }

.btn-outline {
  display: inline-flex; align-items: center; gap: .6rem;
  background: transparent;
  color: rgba(255,255,255,.7); border: 1px solid rgba(255,255,255,.15); cursor: pointer;
  padding: .95rem 2rem; border-radius: 100px;
  font-family: 'Outfit', sans-serif;
  font-size: .85rem; font-weight: 500; letter-spacing: .03em;
  text-decoration: none;
  transition: border-color .2s, color .2s, background .2s, transform .2s;
}
.btn-outline:hover {
  border-color: rgba(255,255,255,.35);
  color: var(--white);
  background: rgba(255,255,255,.05);
  transform: translateY(-2px);
}

/* Hero right — stat panel */
.hero-panel {
  background: rgba(255,255,255,.03);
  border: 1px solid rgba(201,168,76,.15);
  border-radius: 28px;
  padding: 2.5rem;
  backdrop-filter: blur(10px);
  position: relative;
}
.hero-panel::before {
  content: '';
  position: absolute; top: -1px; left: 40px; right: 40px; height: 1px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
}

.hero-panel-label {
  font-size: .7rem; font-weight: 600; letter-spacing: .12em;
  text-transform: uppercase; color: var(--gold);
  margin-bottom: 1.75rem;
  display: flex; align-items: center; gap: .75rem;
}
.hero-panel-label::after {
  content: ''; flex: 1; height: 1px; background: rgba(201,168,76,.2);
}

.stat-row {
  display: flex; flex-direction: column; gap: 1.25rem;
}

.stat-item {
  display: flex; align-items: center; gap: 1.25rem;
  padding: 1.25rem 1.25rem;
  border-radius: var(--radius-sm);
  background: rgba(255,255,255,.035);
  border: 1px solid rgba(255,255,255,.06);
  transition: background .2s, border-color .2s, transform .2s;
  cursor: default;
}
.stat-item:hover {
  background: rgba(201,168,76,.07);
  border-color: rgba(201,168,76,.18);
  transform: translateX(4px);
}
.stat-icon {
  width: 44px; height: 44px; border-radius: 10px;
  background: rgba(201,168,76,.1);
  border: 1px solid rgba(201,168,76,.2);
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0; font-size: 1.1rem;
}
.stat-text-label {
  font-size: .9rem; font-weight: 600; color: var(--white);
  margin-bottom: .2rem;
}
.stat-text-sub { font-size: .78rem; color: rgba(255,255,255,.4); line-height: 1.5; }

.hero-panel-footer {
  margin-top: 1.75rem; padding-top: 1.5rem;
  border-top: 1px solid rgba(255,255,255,.06);
  font-size: .78rem; color: rgba(255,255,255,.3);
  display: flex; align-items: center; gap: .5rem;
}
.hero-panel-footer::before {
  content: '';
  width: 6px; height: 6px; border-radius: 50%;
  background: #4ade80; flex-shrink: 0;
  box-shadow: 0 0 6px #4ade80;
}

/* ════════════════════════════════════════
   SECTION BASE
════════════════════════════════════════ */
.section { padding: 140px 6vw; }
.section-cream { background: var(--cream); }
.section-white { background: var(--white); }
.section-navy { background: var(--navy); }

.container { max-width: 1200px; margin: 0 auto; }

.section-tag {
  display: inline-flex; align-items: center; gap: .75rem;
  font-size: .7rem; font-weight: 700; letter-spacing: .14em;
  text-transform: uppercase; color: var(--gold);
  margin-bottom: 1.25rem;
}
.section-tag-bar { width: 24px; height: 1px; background: var(--gold); }

h2 {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(2.2rem, 4vw, 3.4rem);
  font-weight: 400; line-height: 1.15;
  color: var(--navy); letter-spacing: -.01em;
  margin-bottom: 1.25rem;
}
h2 em { font-style: italic; font-weight: 300; }
h2.light { color: var(--white); }

.section-desc {
  font-size: 1rem; font-weight: 300; color: var(--text-muted);
  line-height: 1.8; max-width: 540px;
}
.section-desc.light { color: rgba(255,255,255,.45); }

/* ════════════════════════════════════════
   INTRO BAND (après hero)
════════════════════════════════════════ */
.intro-band {
  background: var(--white);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 2rem 6vw;
}
.intro-band-inner {
  max-width: 1200px; margin: 0 auto;
  display: flex; align-items: center; justify-content: center;
  gap: 3rem; flex-wrap: wrap;
}
.intro-band-item {
  display: flex; align-items: center; gap: .75rem;
  font-size: .8rem; font-weight: 500; color: var(--text-muted);
  letter-spacing: .03em;
}
.intro-band-item svg { color: var(--gold); flex-shrink: 0; }
.intro-band-sep {
  width: 1px; height: 20px; background: var(--border);
}

/* ════════════════════════════════════════
   SERVICES
════════════════════════════════════════ */
.services-header {
  display: flex; justify-content: space-between; align-items: flex-end;
  margin-bottom: 5rem; gap: 3rem; flex-wrap: wrap;
}
.services-header-right { max-width: 420px; }

.services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, auto);
  gap: 1.5px;
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  background: var(--border);
}

.service-card {
  background: var(--white);
  padding: 3rem 2.75rem;
  position: relative; overflow: hidden;
  transition: background .3s;
  cursor: default;
}
.service-card:hover { background: var(--gold-ultra); }

.service-card-inner { position: relative; z-index: 1; }

.service-num {
  font-family: 'Cormorant Garamond', serif;
  font-size: 3.5rem; font-weight: 300;
  color: var(--cream-dark); line-height: 1;
  position: absolute; top: 2.75rem; right: 2.75rem;
  transition: color .3s;
}
.service-card:hover .service-num { color: var(--gold-pale); }

.service-tag {
  display: inline-block;
  background: var(--gold-pale); color: var(--gold);
  font-size: .68rem; font-weight: 700; letter-spacing: .1em;
  text-transform: uppercase; padding: .3rem .8rem;
  border-radius: 100px; margin-bottom: 1.5rem;
  border: 1px solid var(--gold-pale);
}
.service-card:hover .service-tag {
  background: var(--gold); color: var(--navy);
}

.service-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.6rem; font-weight: 500; color: var(--navy);
  margin-bottom: .9rem; line-height: 1.25;
}
.service-desc {
  font-size: .9rem; font-weight: 300;
  color: var(--text-muted); line-height: 1.75;
  max-width: 360px;
}

/* ════════════════════════════════════════
   VALEUR (3 piliers)
════════════════════════════════════════ */
.valeur-top {
  display: flex; justify-content: space-between; align-items: flex-end;
  margin-bottom: 5rem; gap: 3rem; flex-wrap: wrap;
}

.piliers-grid {
  display: grid; grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}

.pilier-card {
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 2.75rem 2.25rem;
  position: relative; overflow: hidden;
  transition: border-color .3s, box-shadow .3s, transform .3s;
  background: var(--white);
}
.pilier-card:hover {
  border-color: var(--gold);
  box-shadow: 0 16px 60px rgba(201,168,76,.1);
  transform: translateY(-6px);
}
.pilier-card::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, var(--gold), var(--gold-light));
  transform: scaleX(0); transform-origin: left;
  transition: transform .4s cubic-bezier(.16,1,.3,1);
}
.pilier-card:hover::before { transform: scaleX(1); }

.pilier-num {
  font-family: 'Cormorant Garamond', serif;
  font-size: 5rem; font-weight: 300; line-height: 1;
  color: var(--cream-dark);
  margin-bottom: 1.5rem; display: block;
  transition: color .3s;
}
.pilier-card:hover .pilier-num { color: var(--gold-pale); }

.pilier-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem; font-weight: 500;
  color: var(--navy); margin-bottom: .75rem;
}
.pilier-desc { font-size: .875rem; font-weight: 300; color: var(--text-muted); line-height: 1.75; }

/* ════════════════════════════════════════
   PROCESSUS
════════════════════════════════════════ */
.process-wrap {
  display: grid; grid-template-columns: 360px 1fr;
  gap: 8rem; align-items: start;
}

.process-steps { display: flex; flex-direction: column; gap: 0; }

.process-step {
  display: flex; gap: 2rem;
  padding: 2rem 0;
  border-bottom: 1px solid var(--border);
  position: relative;
  cursor: default;
  transition: padding .2s;
}
.process-step:first-child { padding-top: 0; }
.process-step:last-child { border-bottom: none; }
.process-step:hover { padding-left: 8px; }

.step-num {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.1rem; font-weight: 600;
  color: var(--gold); min-width: 32px;
  padding-top: .15rem;
}
.step-title {
  font-size: 1rem; font-weight: 600;
  color: var(--navy); margin-bottom: .45rem;
}
.step-desc { font-size: .875rem; font-weight: 300; color: var(--text-muted); line-height: 1.7; }

.process-right { padding-top: 0; }

.process-visual {
  background: var(--navy);
  border-radius: var(--radius);
  padding: 3rem;
  position: relative; overflow: hidden;
  height: 100%;
  min-height: 420px;
  display: flex; flex-direction: column; justify-content: flex-end;
}
.process-visual::before {
  content: '';
  position: absolute; top: -100px; right: -100px;
  width: 350px; height: 350px;
  background: radial-gradient(circle, rgba(201,168,76,.12) 0%, transparent 65%);
  pointer-events: none;
}
.process-visual-badge {
  position: absolute; top: 2.5rem; left: 2.5rem;
  background: rgba(201,168,76,.12);
  border: 1px solid rgba(201,168,76,.25);
  border-radius: 100px;
  padding: .45rem 1.1rem;
  font-size: .72rem; font-weight: 600; letter-spacing: .1em;
  text-transform: uppercase; color: var(--gold-light);
}
.process-visual-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2rem; font-weight: 400;
  color: var(--white); line-height: 1.3;
  margin-bottom: .75rem; position: relative; z-index: 1;
}
.process-visual-sub {
  font-size: .85rem; font-weight: 300;
  color: rgba(255,255,255,.4); line-height: 1.7;
  position: relative; z-index: 1;
}

/* ════════════════════════════════════════
   A PROPOS
════════════════════════════════════════ */
.apropos-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 7rem; align-items: center;
}
.apropos-quote {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.75rem; font-style: italic; font-weight: 300;
  color: var(--navy); line-height: 1.5;
  padding-left: 2rem;
  border-left: 2px solid var(--gold);
  margin: 2.5rem 0;
}
.apropos-body {
  font-size: .95rem; font-weight: 300; color: var(--text-muted); line-height: 1.85;
}
.apropos-signature {
  margin-top: 2.5rem;
  display: flex; align-items: center; gap: 1rem;
}
.apropos-sig-line {
  width: 40px; height: 1px; background: var(--gold);
}
.apropos-sig-text {
  font-size: .78rem; font-weight: 600; letter-spacing: .08em;
  text-transform: uppercase; color: var(--gold);
}

.apropos-panel {
  background: var(--navy);
  border-radius: var(--radius);
  padding: 3.5rem;
  position: relative; overflow: hidden;
}
.apropos-panel::after {
  content: '';
  position: absolute; bottom: -60px; right: -60px;
  width: 250px; height: 250px;
  background: radial-gradient(circle, rgba(201,168,76,.08) 0%, transparent 65%);
  pointer-events: none;
}
.apropos-panel-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.35rem; font-weight: 400;
  color: var(--white); margin-bottom: 2.25rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid rgba(255,255,255,.08);
}
.apropos-list { list-style: none; display: flex; flex-direction: column; gap: 1.25rem; }
.apropos-item {
  display: flex; gap: 1rem; align-items: flex-start;
  font-size: .875rem; font-weight: 300; color: rgba(255,255,255,.6);
  line-height: 1.7;
}
.apropos-arrow {
  color: var(--gold); font-size: .8rem; flex-shrink: 0;
  margin-top: .25rem; font-weight: 600;
}

/* ════════════════════════════════════════
   PLATEFORMES BAND
════════════════════════════════════════ */
.platforms-band { background: var(--cream-dark); padding: 4rem 6vw; }
.platforms-inner {
  max-width: 1200px; margin: 0 auto;
  display: flex; align-items: center; justify-content: space-between;
  gap: 2rem; flex-wrap: wrap;
}
.platforms-label {
  font-size: .72rem; font-weight: 700; letter-spacing: .1em;
  text-transform: uppercase; color: var(--text-light);
  white-space: nowrap;
}
.platforms-list { display: flex; gap: 2.5rem; align-items: center; flex-wrap: wrap; }
.platform-pill {
  display: flex; align-items: center; gap: .6rem;
  background: var(--white); border: 1px solid var(--border);
  border-radius: 100px; padding: .55rem 1.25rem;
  font-size: .82rem; font-weight: 600; color: var(--text-mid);
  transition: border-color .2s, box-shadow .2s;
}
.platform-pill:hover {
  border-color: var(--gold);
  box-shadow: 0 4px 16px rgba(201,168,76,.12);
}
.platform-dot {
  width: 7px; height: 7px; border-radius: 50%;
}

/* ════════════════════════════════════════
   AUDIT CTA
════════════════════════════════════════ */
.audit-section { background: var(--navy); padding: 140px 6vw; }
.audit-inner {
  max-width: 920px; margin: 0 auto;
}
.audit-header { margin-bottom: 4rem; }
.audit-header .section-tag { margin-bottom: 1.5rem; }
.audit-header h2 { color: var(--white); margin-bottom: 1rem; }
.audit-header .section-desc { color: rgba(255,255,255,.4); }

.audit-grid {
  display: grid; grid-template-columns: repeat(4, 1fr);
  gap: 1px;
  border: 1px solid rgba(201,168,76,.12);
  border-radius: var(--radius);
  overflow: hidden;
  background: rgba(201,168,76,.08);
  margin-bottom: 3.5rem;
}

.audit-item {
  background: rgba(255,255,255,.025);
  padding: 2rem 1.5rem;
  text-align: center;
  transition: background .2s;
}
.audit-item:hover { background: rgba(201,168,76,.07); }
.audit-icon { font-size: 1.5rem; margin-bottom: .9rem; display: block; }
.audit-label {
  font-size: .82rem; font-weight: 500; color: rgba(255,255,255,.6); line-height: 1.5;
}

.audit-cta-row {
  display: flex; align-items: center; gap: 2rem; flex-wrap: wrap;
}
.audit-note {
  font-size: .8rem; font-weight: 400; color: rgba(255,255,255,.25);
  line-height: 1.65;
}

/* ════════════════════════════════════════
   FOOTER
════════════════════════════════════════ */
footer {
  background: #070E1C;
  padding: 4rem 6vw 2.5rem;
  border-top: 1px solid rgba(201,168,76,.1);
}
.footer-top {
  max-width: 1200px; margin: 0 auto;
  display: flex; justify-content: space-between; align-items: flex-start;
  gap: 3rem; flex-wrap: wrap;
  padding-bottom: 3rem;
  border-bottom: 1px solid rgba(255,255,255,.05);
  margin-bottom: 2rem;
}
.footer-brand-block {}
.footer-logo {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.6rem; font-weight: 600; color: var(--white);
  margin-bottom: .75rem;
}
.footer-logo span { color: var(--gold); }
.footer-tagline {
  font-size: .8rem; font-weight: 300; color: rgba(255,255,255,.3);
  max-width: 220px; line-height: 1.65;
}
.footer-nav { display: flex; flex-direction: column; gap: .75rem; }
.footer-nav-title {
  font-size: .68rem; font-weight: 700; letter-spacing: .12em;
  text-transform: uppercase; color: rgba(255,255,255,.25);
  margin-bottom: .25rem;
}
.footer-nav a {
  font-size: .82rem; font-weight: 400; color: rgba(255,255,255,.4);
  text-decoration: none; transition: color .2s;
}
.footer-nav a:hover { color: var(--gold-light); }
.footer-contact-block {}
.footer-contact-label {
  font-size: .68rem; font-weight: 700; letter-spacing: .12em;
  text-transform: uppercase; color: rgba(255,255,255,.25);
  margin-bottom: .75rem;
}
.footer-email {
  font-size: .9rem; font-weight: 500; color: var(--gold);
  text-decoration: none; display: flex; align-items: center; gap: .5rem;
  transition: opacity .2s;
}
.footer-email:hover { opacity: .7; }
.footer-zone {
  margin-top: .75rem; font-size: .78rem; font-weight: 300;
  color: rgba(255,255,255,.2); display: flex; align-items: center; gap: .4rem;
}

.footer-bottom {
  max-width: 1200px; margin: 0 auto;
  display: flex; justify-content: space-between; align-items: center;
  flex-wrap: wrap; gap: 1rem;
}
.footer-copy { font-size: .75rem; color: rgba(255,255,255,.2); }
.footer-legal { display: flex; gap: 1.5rem; }
.footer-legal a {
  font-size: .75rem; color: rgba(255,255,255,.2);
  text-decoration: none; transition: color .2s;
}
.footer-legal a:hover { color: rgba(255,255,255,.4); }

/* ════════════════════════════════════════
   ANIMATIONS
════════════════════════════════════════ */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(30px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes fadeIn {
  from { opacity: 0; } to { opacity: 1; }
}
@keyframes slideRight {
  from { transform: scaleX(0); } to { transform: scaleX(1); }
}

.hero-eyebrow { animation: fadeUp .8s ease both; }
h1            { animation: fadeUp .8s .15s ease both; }
.hero-sub     { animation: fadeUp .8s .3s ease both; }
.hero-actions { animation: fadeUp .8s .45s ease both; }
.hero-panel   { animation: fadeUp .8s .3s ease both; }

.reveal {
  opacity: 0; transform: translateY(24px);
  transition: opacity .7s ease, transform .7s ease;
}
.reveal.visible { opacity: 1; transform: translateY(0); }

/* ════════════════════════════════════════
   RESPONSIVE
════════════════════════════════════════ */
@media (max-width: 1024px) {
  .hero-inner { grid-template-columns: 1fr; }
  .hero-panel { display: none; }
  .services-grid { grid-template-columns: 1fr; }
  .piliers-grid { grid-template-columns: 1fr 1fr; }
  .process-wrap { grid-template-columns: 1fr; gap: 3rem; }
  .process-visual { min-height: 260px; }
  .apropos-grid { grid-template-columns: 1fr; gap: 3rem; }
  .audit-grid { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 768px) {
  .section { padding: 100px 5vw; }
  .nav-links { display: none; }
  .services-grid { grid-template-columns: 1fr; gap: 0; }
  .piliers-grid { grid-template-columns: 1fr; }
  .audit-grid { grid-template-columns: 1fr 1fr; }
  .footer-top { flex-direction: column; }
  h1 { font-size: 2.8rem; }
  h2 { font-size: 2rem; }
  .intro-band-sep { display: none; }
}
@media (max-width: 500px) {
  .audit-grid { grid-template-columns: 1fr; }
  .hero-actions { flex-direction: column; }
  .hero-actions a { text-align: center; justify-content: center; }
}
</style>
</head>
<body>

<!-- ─── NAV ─── -->
<nav id="navbar">
  <a href="#" class="nav-logo">Renova<span class="nav-logo-dot">·</span>Stay</a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#valeur">Approche</a></li>
    <li><a href="#processus">Méthode</a></li>
    <li><a href="#propos">À propos</a></li>
  </ul>
  <a href="#audit" class="nav-cta">
    <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
    Audit gratuit
  </a>
</nav>

<!-- ─── HERO ─── -->
<section class="hero">
  <div class="hero-bg-circle hero-bg-circle-1"></div>
  <div class="hero-bg-circle hero-bg-circle-2"></div>
  <div class="hero-grid-line hero-grid-line-1"></div>

  <div class="hero-inner">
    <div>
      <div class="hero-eyebrow">
        <span class="hero-eyebrow-bar"></span>
        Gestion locative · Moselle
      </div>
      <h1>
        La gestion locative<br>
        <strong>nouvelle génération</strong>
        pour les locations courte durée.
      </h1>
      <p class="hero-sub">Nous accompagnons les propriétaires dans l'optimisation, l'exploitation et la gestion quotidienne de leurs biens afin d'améliorer leur rentabilité et l'expérience des voyageurs.</p>
      <div class="hero-actions">
        <a href="#audit" class="btn-gold">
          Demander un audit gratuit
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
        <a href="#services" class="btn-outline">
          Découvrir nos services
        </a>
      </div>
    </div>

    <div class="hero-panel">
      <div class="hero-panel-label">Nos engagements</div>
      <div class="stat-row">
        <div class="stat-item">
          <div class="stat-icon">🗓️</div>
          <div>
            <div class="stat-text-label">Réactivité 7j/7</div>
            <div class="stat-text-sub">Disponibles chaque jour pour vous et vos voyageurs.</div>
          </div>
        </div>
        <div class="stat-item">
          <div class="stat-icon">🤝</div>
          <div>
            <div class="stat-text-label">Accompagnement personnalisé</div>
            <div class="stat-text-sub">Un interlocuteur dédié à chaque propriétaire.</div>
          </div>
        </div>
        <div class="stat-item">
          <div class="stat-icon">⚙️</div>
          <div>
            <div class="stat-text-label">Service sur mesure</div>
            <div class="stat-text-sub">Des solutions adaptées à chaque bien et objectif.</div>
          </div>
        </div>
      </div>
      <div class="hero-panel-footer">
        Opérationnel en Moselle &amp; alentours
      </div>
    </div>
  </div>
</section>

<!-- ─── INTRO BAND ─── -->
<div class="intro-band">
  <div class="intro-band-inner">
    <div class="intro-band-item">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
      Toutes plateformes
    </div>
    <div class="intro-band-sep"></div>
    <div class="intro-band-item">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 8v4l3 3"/></svg>
      Réactivité 7j/7
    </div>
    <div class="intro-band-sep"></div>
    <div class="intro-band-item">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>
      Accompagnement personnalisé
    </div>
    <div class="intro-band-sep"></div>
    <div class="intro-band-item">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
      Moselle &amp; alentours
    </div>
    <div class="intro-band-sep"></div>
    <div class="intro-band-item">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>
      Optimisation continue
    </div>
  </div>
</div>

<!-- ─── SERVICES ─── -->
<section id="services" class="section section-white">
  <div class="container">
    <div class="services-header">
      <div>
        <div class="section-tag reveal"><span class="section-tag-bar"></span>Services</div>
        <h2 class="reveal">Une prise en charge <em>complète</em></h2>
      </div>
      <p class="section-desc reveal">De la communication voyageurs à la coordination terrain — nous gérons chaque aspect de votre activité locative sur toutes les plateformes.</p>
    </div>

    <div class="services-grid reveal">
      <div class="service-card">
        <div class="service-card-inner">
          <span class="service-tag">Communication</span>
          <div class="service-title">Gestion voyageurs</div>
          <p class="service-desc">Nous assurons les échanges avec les voyageurs avant, pendant et après leur séjour pour une expérience irréprochable à chaque étape.</p>
        </div>
        <div class="service-num">01</div>
      </div>
      <div class="service-card">
        <div class="service-card-inner">
          <span class="service-tag">Performance</span>
          <div class="service-title">Optimisation des annonces</div>
          <p class="service-desc">Analyse des performances, amélioration des descriptions, optimisation du positionnement et valorisation du bien sur toutes les plateformes.</p>
        </div>
        <div class="service-num">02</div>
      </div>
      <div class="service-card">
        <div class="service-card-inner">
          <span class="service-tag">Opérationnel</span>
          <div class="service-title">Coordination opérationnelle</div>
          <p class="service-desc">Organisation des arrivées, départs et interventions nécessaires au bon fonctionnement du logement, sans que vous ayez à intervenir.</p>
        </div>
        <div class="service-num">03</div>
      </div>
      <div class="service-card">
        <div class="service-card-inner">
          <span class="service-tag">Suivi</span>
          <div class="service-title">Accompagnement propriétaire</div>
          <p class="service-desc">Un interlocuteur unique et un suivi transparent de l'activité du logement, avec des rapports réguliers et une communication directe.</p>
        </div>
        <div class="service-num">04</div>
      </div>
    </div>
  </div>
</section>

<!-- ─── PLATEFORMES ─── -->
<div class="platforms-band">
  <div class="platforms-inner">
    <span class="platforms-label">Plateformes gérées</span>
    <div class="platforms-list">
      <div class="platform-pill">
        <span class="platform-dot" style="background:#FF5A5F;"></span>
        Airbnb
      </div>
      <div class="platform-pill">
        <span class="platform-dot" style="background:#003580;"></span>
        Booking.com
      </div>
      <div class="platform-pill">
        <span class="platform-dot" style="background:#E7711B;"></span>
        Abritel
      </div>
      <div class="platform-pill">
        <span class="platform-dot" style="background:#00AF87;"></span>
        Expedia
      </div>
      <div class="platform-pill">
        <span class="platform-dot" style="background:#6B7280;"></span>
        &amp; autres
      </div>
    </div>
  </div>
</div>

<!-- ─── VALEUR ─── -->
<section id="valeur" class="section section-cream">
  <div class="container">
    <div class="valeur-top">
      <div>
        <div class="section-tag reveal"><span class="section-tag-bar"></span>Notre approche</div>
        <h2 class="reveal">Plus qu'un simple <em>co-hôte</em></h2>
      </div>
      <p class="section-desc reveal">Notre approche repose sur trois piliers fondamentaux qui font la différence sur le long terme.</p>
    </div>
    <div class="piliers-grid">
      <div class="pilier-card reveal">
        <span class="pilier-num">01</span>
        <div class="pilier-title">Performance</div>
        <p class="pilier-desc">Chaque logement possède un potentiel inexploité que nous aidons à révéler grâce à une analyse rigoureuse du marché et des données.</p>
      </div>
      <div class="pilier-card reveal">
        <span class="pilier-num">02</span>
        <div class="pilier-title">Expérience voyageur</div>
        <p class="pilier-desc">Une expérience fluide et mémorable génère de meilleurs avis, favorise les réservations et fidélise les voyageurs sur la durée.</p>
      </div>
      <div class="pilier-card reveal">
        <span class="pilier-num">03</span>
        <div class="pilier-title">Transparence</div>
        <p class="pilier-desc">Une communication claire, des comptes-rendus réguliers et un suivi détaillé de chaque action réalisée sur votre bien.</p>
      </div>
    </div>
  </div>
</section>

<!-- ─── PROCESSUS ─── -->
<section id="processus" class="section section-white">
  <div class="container">
    <div class="section-tag reveal" style="margin-bottom:1.25rem;"><span class="section-tag-bar"></span>Méthode</div>
    <h2 style="margin-bottom:4rem;" class="reveal">Comment nous <em>travaillons</em></h2>

    <div class="process-wrap">
      <div class="process-steps">
        <div class="process-step reveal">
          <div class="step-num">01</div>
          <div>
            <div class="step-title">Analyse du logement</div>
            <p class="step-desc">Étude approfondie de votre annonce, de votre positionnement et de votre marché local en Moselle.</p>
          </div>
        </div>
        <div class="process-step reveal">
          <div class="step-num">02</div>
          <div>
            <div class="step-title">Recommandations</div>
            <p class="step-desc">Identification des opportunités d'amélioration concrètes, priorisées par impact potentiel.</p>
          </div>
        </div>
        <div class="process-step reveal">
          <div class="step-num">03</div>
          <div>
            <div class="step-title">Mise en place</div>
            <p class="step-desc">Déploiement des optimisations et organisation structurée de la gestion quotidienne.</p>
          </div>
        </div>
        <div class="process-step reveal">
          <div class="step-num">04</div>
          <div>
            <div class="step-title">Suivi continu</div>
            <p class="step-desc">Amélioration continue des performances et de l'expérience voyageur, semaine après semaine.</p>
          </div>
        </div>
      </div>

      <div class="process-visual reveal">
        <div class="process-visual-badge">Approche Renova Stay</div>
        <h3 class="process-visual-title">Une gestion rigoureuse,<br>des résultats durables.</h3>
        <p class="process-visual-sub">Chaque étape est conçue pour maximiser le potentiel de votre bien tout en vous libérant des contraintes opérationnelles du quotidien.</p>
      </div>
    </div>
  </div>
</section>

<!-- ─── À PROPOS ─── -->
<section id="propos" class="section section-cream">
  <div class="container">
    <div class="apropos-grid">
      <div>
        <div class="section-tag reveal"><span class="section-tag-bar"></span>À propos</div>
        <h2 class="reveal">Notre <em>vision</em></h2>
        <blockquote class="apropos-quote reveal">
          "La gestion locative courte durée doit être plus simple, plus transparente et plus performante pour les propriétaires."
        </blockquote>
        <p class="apropos-body reveal">
          Notre mission est d'apporter une approche moderne de l'exploitation des locations saisonnières en Moselle, grâce à une organisation rigoureuse et une attention particulière portée à l'expérience voyageur.<br><br>
          Nous croyons que chaque propriétaire mérite un accompagnement à la hauteur de son investissement — sans compromis sur la qualité ni sur la transparence.
        </p>
        <div class="apropos-signature reveal">
          <div class="apropos-sig-line"></div>
          <div class="apropos-sig-text">Renova Stay · Moselle</div>
        </div>
      </div>

      <div class="apropos-panel reveal">
        <div class="apropos-panel-title">Ce qui nous distingue</div>
        <ul class="apropos-list">
          <li class="apropos-item">
            <span class="apropos-arrow">→</span>
            Une gestion orientée performance, pas seulement réactive aux demandes entrantes.
          </li>
          <li class="apropos-item">
            <span class="apropos-arrow">→</span>
            Un suivi transparent avec des rapports réguliers sur l'activité de votre bien.
          </li>
          <li class="apropos-item">
            <span class="apropos-arrow">→</span>
            Une approche sur mesure adaptée à chaque logement et chaque propriétaire.
          </li>
          <li class="apropos-item">
            <span class="apropos-arrow">→</span>
            Une attention constante portée à la qualité de l'expérience voyageur sur toutes les plateformes.
          </li>
          <li class="apropos-item">
            <span class="apropos-arrow">→</span>
            Un interlocuteur unique, disponible 7j/7 et pleinement impliqué dans votre projet.
          </li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- ─── AUDIT ─── -->
<section id="audit" class="audit-section">
  <div class="container">
    <div class="audit-inner">
      <div class="audit-header reveal">
        <div class="section-tag"><span class="section-tag-bar"></span>Offre de bienvenue</div>
        <h2 class="light">Recevez une analyse<br><em>gratuite</em> de votre annonce</h2>
        <p class="section-desc light">Un audit complet pour identifier les axes d'amélioration et révéler le véritable potentiel de votre logement en Moselle.</p>
      </div>

      <div class="audit-grid reveal">
        <div class="audit-item">
          <span class="audit-icon">📝</span>
          <span class="audit-label">Titre de l'annonce</span>
        </div>
        <div class="audit-item">
          <span class="audit-icon">📸</span>
          <span class="audit-label">Qualité des photos</span>
        </div>
        <div class="audit-item">
          <span class="audit-icon">✍️</span>
          <span class="audit-label">Description du bien</span>
        </div>
        <div class="audit-item">
          <span class="audit-icon">💡</span>
          <span class="audit-label">Points d'amélioration</span>
        </div>
      </div>

      <div class="audit-cta-row reveal">
        <a href="mailto:delio.casciu10@gmail.com?subject=Demande%20d'audit%20gratuit%20-%20Renova%20Stay&body=Bonjour%2C%0A%0AJe%20souhaite%20recevoir%20un%20audit%20gratuit%20de%20mon%20annonce.%0A%0ALien%20de%20mon%20annonce%20%3A%20%0A%0AMerci." class="btn-gold">
          Recevoir mon audit gratuit
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
        <p class="audit-note">Gratuit &amp; sans engagement.<br>Réponse sous 48h.</p>
      </div>
    </div>
  </div>
</section>

<!-- ─── FOOTER ─── -->
<footer>
  <div class="footer-top">
    <div class="footer-brand-block">
      <div class="footer-logo">Renova<span>·</span>Stay</div>
      <p class="footer-tagline">La gestion locative nouvelle génération pour les locations courte durée en Moselle.</p>
    </div>
    <nav class="footer-nav">
      <div class="footer-nav-title">Navigation</div>
      <a href="#services">Services</a>
      <a href="#valeur">Notre approche</a>
      <a href="#processus">Méthode</a>
      <a href="#propos">À propos</a>
    </nav>
    <div class="footer-contact-block">
      <div class="footer-contact-label">Contact</div>
      <a href="mailto:delio.casciu10@gmail.com" class="footer-email">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
        delio.casciu10@gmail.com
      </a>
      <div class="footer-zone">
        <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
        Moselle &amp; alentours
      </div>
    </div>
  </div>
  <div class="footer-bottom">
    <div class="footer-copy">© 2025 Renova Stay. Tous droits réservés.</div>
    <div class="footer-legal">
      <a href="#">Mentions légales</a>
      <a href="#">Politique de confidentialité</a>
    </div>
  </div>
</footer>

<script>
// ── Navbar scroll
const nav = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 40);
}, { passive: true });

// ── Scroll reveal
const revealEls = document.querySelectorAll('.reveal');
const io = new IntersectionObserver((entries) => {
  entries.forEach((entry, i) => {
    if (entry.isIntersecting) {
      // stagger siblings
      const siblings = [...entry.target.parentElement.querySelectorAll('.reveal:not(.visible)')];
      const idx = siblings.indexOf(entry.target);
      setTimeout(() => {
        entry.target.classList.add('visible');
      }, idx * 80);
      io.unobserve(entry.target);
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -40px 0px' });
revealEls.forEach(el => io.observe(el));
</script>

</body>
</html>
