<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Renova·Stay — Gestion Locative Nouvelle Génération · Moselle</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400;1,600&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box}
:root{
  --navy:#0B1629;
  --navy2:#0F1E38;
  --navy3:#162644;
  --navy4:#1E3358;
  --gold:#C4993A;
  --gold2:#E2B94A;
  --gold3:#F5D878;
  --gold-dim:rgba(196,153,58,0.15);
  --text:#F0EDE6;
  --text2:#A8B4C8;
  --text3:#6B7F99;
  --white:#FFFFFF;
  --border:rgba(196,153,58,0.18);
  --border2:rgba(196,153,58,0.08);
}
html{scroll-behavior:smooth}
body{background:var(--navy);color:var(--text);font-family:'DM Sans',sans-serif;overflow-x:hidden;cursor:none}

/* ─── CURSOR ─── */
#cur{position:fixed;width:10px;height:10px;background:var(--gold);border-radius:50%;pointer-events:none;z-index:9999;transform:translate(-50%,-50%);transition:width .25s,height .25s,background .25s}
#cur-ring{position:fixed;width:36px;height:36px;border:1px solid rgba(196,153,58,.5);border-radius:50%;pointer-events:none;z-index:9998;transform:translate(-50%,-50%)}
body.cur-hover #cur{width:20px;height:20px;background:var(--gold2)}
body.cur-hover #cur-ring{width:52px;height:52px;border-color:rgba(196,153,58,.8)}

/* ─── PROGRESS BAR ─── */
#progress{position:fixed;top:0;left:0;height:2px;background:linear-gradient(90deg,var(--gold),var(--gold3));width:0%;z-index:10001;transition:width .1s}

/* ─── LOADING ─── */
#loader{position:fixed;inset:0;background:var(--navy);z-index:10000;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2rem;transition:opacity .7s,visibility .7s}
#loader.out{opacity:0;visibility:hidden}
.loader-logo{font-family:'Playfair Display',serif;font-size:2.2rem;color:var(--gold);letter-spacing:.05em}
.loader-logo span{font-style:italic;color:var(--text)}
.loader-track{width:180px;height:1px;background:rgba(255,255,255,.08);position:relative;overflow:hidden}
.loader-fill{height:100%;background:linear-gradient(90deg,var(--gold),var(--gold3));width:0;transition:width .05s linear}
.loader-pct{font-size:.65rem;letter-spacing:.3em;color:var(--text3)}

/* ─── CANVAS PARTICLES ─── */
#stars{position:fixed;inset:0;pointer-events:none;z-index:0;opacity:.7}

/* ─── NAV ─── */
nav{position:fixed;top:0;left:0;right:0;z-index:1000;padding:1.4rem 5vw;display:flex;align-items:center;justify-content:space-between;transition:background .4s,backdrop-filter .4s,padding .4s}
nav.stuck{background:rgba(11,22,41,.93);backdrop-filter:blur(14px);padding:1rem 5vw;border-bottom:1px solid var(--border2)}
.logo{font-family:'Playfair Display',serif;font-size:1.4rem;color:var(--text);text-decoration:none;letter-spacing:.02em}
.logo b{color:var(--gold);font-weight:700}
.logo span{font-style:italic;font-weight:400}
.nav-links{display:flex;gap:2.5rem;list-style:none}
.nav-links a{font-size:.72rem;letter-spacing:.18em;text-transform:uppercase;color:var(--text2);text-decoration:none;transition:color .3s;font-weight:500}
.nav-links a:hover{color:var(--gold)}
.nav-cta{font-size:.7rem;letter-spacing:.15em;text-transform:uppercase;border:1px solid var(--gold);color:var(--gold);padding:.55rem 1.4rem;background:transparent;cursor:none;font-family:'DM Sans',sans-serif;font-weight:500;transition:all .3s}
.nav-cta:hover{background:var(--gold);color:var(--navy)}
.burger{display:none;flex-direction:column;gap:5px;cursor:none;width:28px;padding:4px 0}
.burger span{height:1px;background:var(--text);transition:all .4s;display:block}
.burger.open span:nth-child(1){transform:translateY(6px) rotate(45deg)}
.burger.open span:nth-child(2){opacity:0;transform:scaleX(0)}
.burger.open span:nth-child(3){transform:translateY(-6px) rotate(-45deg)}
.mob-menu{display:none;position:fixed;inset:0;background:var(--navy);z-index:999;flex-direction:column;align-items:center;justify-content:center;gap:2.5rem;opacity:0;transition:opacity .4s}
.mob-menu.open{display:flex;opacity:1}
.mob-menu a{font-family:'Playfair Display',serif;font-size:2rem;color:var(--text);text-decoration:none;font-style:italic;transition:color .3s}
.mob-menu a:hover{color:var(--gold)}

/* ─── FLOATING ACTIONS ─── */
.float-bar{position:fixed;bottom:2rem;right:2rem;display:flex;flex-direction:column;gap:.8rem;z-index:900}
.fab{width:50px;height:50px;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:none;transition:transform .3s,box-shadow .3s;text-decoration:none;border:none}
.fab:hover{transform:scale(1.12)}
.fab-wa{background:#25D366}
.fab-mail{background:var(--gold)}
.fab svg{width:20px;height:20px;fill:white;stroke:none}
.fab-label{position:absolute;right:58px;background:var(--navy2);color:var(--text);font-size:.65rem;letter-spacing:.1em;padding:.3rem .7rem;white-space:nowrap;opacity:0;pointer-events:none;transition:opacity .2s;border:1px solid var(--border)}
.fab:hover .fab-label{opacity:1}

/* ─── HERO ─── */
#hero{min-height:100vh;display:flex;align-items:center;padding:0 5vw;position:relative;overflow:hidden}
.hero-bg-grad{position:absolute;inset:0;background:radial-gradient(ellipse 75% 65% at 65% 45%,rgba(196,153,58,.07) 0%,transparent 65%)}
.hero-grid-lines{position:absolute;inset:0;overflow:hidden;opacity:.025;pointer-events:none}
.hero-grid-lines svg{width:100%;height:100%}
.hero-inner{position:relative;z-index:1;max-width:800px}
.hero-eyebrow{display:inline-flex;align-items:center;gap:.7rem;font-size:.65rem;letter-spacing:.3em;text-transform:uppercase;color:var(--gold);border:1px solid var(--border);padding:.4rem 1rem .4rem .8rem;margin-bottom:2.5rem;opacity:0;animation:fadeUp .9s .3s ease forwards}
.hero-eyebrow::before{content:'';width:6px;height:6px;background:var(--gold);border-radius:50%;animation:blink 2s infinite}
.hero-h1{font-family:'Playfair Display',serif;font-size:clamp(2.8rem,6vw,5.5rem);line-height:1.07;margin-bottom:1.6rem;opacity:0;animation:fadeUp .9s .5s ease forwards}
.hero-h1 em{font-style:italic;color:var(--gold)}
.hero-p{font-size:.95rem;color:var(--text2);line-height:1.9;max-width:520px;font-weight:300;margin-bottom:2.8rem;opacity:0;animation:fadeUp .9s .7s ease forwards}
.hero-btns{display:flex;gap:1.2rem;flex-wrap:wrap;opacity:0;animation:fadeUp .9s .9s ease forwards}
.btn-primary{background:var(--gold);color:var(--navy);border:none;padding:.85rem 2.2rem;font-size:.75rem;letter-spacing:.15em;text-transform:uppercase;cursor:none;font-family:'DM Sans',sans-serif;font-weight:600;position:relative;overflow:hidden;transition:box-shadow .3s}
.btn-primary::after{content:'';position:absolute;inset:0;background:var(--gold3);transform:scaleX(0);transform-origin:left;transition:transform .35s}
.btn-primary:hover{box-shadow:0 8px 32px rgba(196,153,58,.35)}
.btn-primary:hover::after{transform:scaleX(1)}
.btn-primary span{position:relative;z-index:1}
.btn-secondary{background:transparent;color:var(--text);border:1px solid rgba(240,237,230,.2);padding:.85rem 2.2rem;font-size:.75rem;letter-spacing:.15em;text-transform:uppercase;cursor:none;font-family:'DM Sans',sans-serif;font-weight:400;transition:all .3s}
.btn-secondary:hover{border-color:var(--gold);color:var(--gold)}
.hero-badges{display:flex;gap:1.5rem;margin-top:3rem;flex-wrap:wrap;opacity:0;animation:fadeUp .9s 1.1s ease forwards}
.hbadge{display:flex;align-items:center;gap:.6rem;font-size:.72rem;color:var(--text2);font-weight:300}
.hbadge::before{content:'';width:6px;height:6px;background:var(--gold);border-radius:50%;flex-shrink:0}
.hbadge.active::after{content:'Active';font-size:.55rem;letter-spacing:.15em;text-transform:uppercase;color:var(--gold);border:1px solid var(--border);padding:.15rem .5rem;margin-left:.3rem}
.hero-scroll{position:absolute;bottom:2.2rem;left:5vw;display:flex;align-items:center;gap:.8rem;opacity:0;animation:fadeUp .9s 1.3s ease forwards}
.scroll-line{width:40px;height:1px;background:linear-gradient(90deg,var(--gold),transparent)}
.scroll-txt{font-size:.6rem;letter-spacing:.25em;text-transform:uppercase;color:var(--text3)}

/* ─── TICKER ─── */
.ticker-wrap{overflow:hidden;border-top:1px solid var(--border2);border-bottom:1px solid var(--border2);padding:.7rem 0;background:var(--navy2)}
.ticker-inner{display:flex;gap:3rem;white-space:nowrap;animation:ticker 25s linear infinite}
.ticker-item{font-size:.65rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3);display:flex;align-items:center;gap:.7rem;flex-shrink:0}
.ticker-dot{width:4px;height:4px;background:var(--gold);border-radius:50%;flex-shrink:0}

/* ─── SECTION COMMONS ─── */
section{padding:7rem 5vw;position:relative}
.s-tag{display:flex;align-items:center;gap:.8rem;font-size:.62rem;letter-spacing:.28em;text-transform:uppercase;color:var(--gold);margin-bottom:1rem}
.s-tag::before{content:'';width:28px;height:1px;background:var(--gold)}
.s-title{font-family:'Playfair Display',serif;font-size:clamp(2rem,4vw,3.2rem);line-height:1.15;margin-bottom:1rem}
.s-title em{font-style:italic;color:var(--gold)}
.s-sub{font-size:.85rem;color:var(--text2);line-height:1.9;font-weight:300;max-width:560px}

/* ─── SERVICES ─── */
#services{background:var(--navy2)}
.services-header{max-width:700px;margin-bottom:4rem}
.services-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:1px;background:var(--border2)}
.svc{background:var(--navy2);padding:3rem 2.8rem;position:relative;overflow:hidden;cursor:none;transition:background .4s}
.svc:hover{background:var(--navy3)}
.svc-top-line{position:absolute;top:0;left:0;right:0;height:1px;background:linear-gradient(90deg,var(--gold),transparent);transform:scaleX(0);transform-origin:left;transition:transform .5s}
.svc:hover .svc-top-line{transform:scaleX(1)}
.svc-num{font-family:'Playfair Display',serif;font-size:.8rem;color:var(--gold);margin-bottom:1.5rem;letter-spacing:.1em}
.svc-label{font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3);margin-bottom:.5rem}
.svc-title{font-family:'Playfair Display',serif;font-size:1.4rem;margin-bottom:1.2rem;line-height:1.2}
.svc-desc{font-size:.8rem;color:var(--text2);line-height:1.9;font-weight:300}
.svc-ghost{position:absolute;right:-1rem;bottom:-2rem;font-family:'Playfair Display',serif;font-size:9rem;color:var(--gold);opacity:.025;line-height:1;pointer-events:none;user-select:none;transition:opacity .4s}
.svc:hover .svc-ghost{opacity:.06}
.platforms{display:flex;flex-wrap:wrap;gap:.6rem;margin-top:3rem}
.plat{font-size:.65rem;letter-spacing:.15em;text-transform:uppercase;border:1px solid var(--border);color:var(--text2);padding:.35rem .9rem}

/* ─── VALEUR ─── */
#valeur{background:var(--navy)}
.valeur-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:2rem;margin-top:4rem}
.val-card{border:1px solid var(--border2);padding:2.5rem 2rem;position:relative;overflow:hidden;cursor:none;transition:border-color .4s,transform .4s}
.val-card:hover{border-color:rgba(196,153,58,.4);transform:translateY(-4px)}
.val-card::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at top left,rgba(196,153,58,.06),transparent 60%);opacity:0;transition:opacity .4s}
.val-card:hover::before{opacity:1}
.val-icon{width:44px;height:44px;border:1px solid var(--border);display:flex;align-items:center;justify-content:center;margin-bottom:1.8rem;font-size:1.2rem}
.val-num{font-family:'Playfair Display',serif;font-size:.75rem;color:var(--gold);margin-bottom:.6rem;letter-spacing:.1em}
.val-title{font-family:'Playfair Display',serif;font-size:1.2rem;margin-bottom:.9rem}
.val-desc{font-size:.78rem;color:var(--text2);line-height:1.9;font-weight:300}

/* ─── PROCESSUS ─── */
#processus{background:var(--navy2)}
.process-wrap{display:grid;grid-template-columns:1fr 1fr;gap:6rem;align-items:start;margin-top:4rem}
.process-steps{display:flex;flex-direction:column;gap:0}
.ps{border-bottom:1px solid var(--border2);padding:1.8rem 0;cursor:none;transition:padding .3s}
.ps:hover,.ps.act{padding-left:.8rem}
.ps:hover .ps-num,.ps.act .ps-num{color:var(--gold)}
.ps-row{display:flex;align-items:center;gap:1.2rem}
.ps-num{font-family:'Playfair Display',serif;font-size:.8rem;color:var(--text3);transition:color .3s;width:2rem;flex-shrink:0}
.ps-name{font-size:.95rem;font-weight:500}
.ps-body{font-size:.78rem;color:var(--text2);line-height:1.9;font-weight:300;padding-left:3.2rem;margin-top:.7rem;display:none}
.ps.act .ps-body{display:block}
.process-visual{position:sticky;top:6rem}
.pv-box{background:var(--navy3);border:1px solid var(--border2);padding:2.5rem;margin-bottom:1.5rem}
.pv-title{font-family:'Playfair Display',serif;font-size:1.1rem;margin-bottom:.5rem}
.pv-sub{font-size:.75rem;color:var(--text2);line-height:1.8;font-weight:300}
.pv-checklist{margin-top:1.5rem;display:flex;flex-direction:column;gap:.7rem}
.pv-check{display:flex;align-items:center;gap:.8rem;font-size:.78rem;color:var(--text2)}
.pv-check::before{content:'';width:16px;height:16px;border:1px solid var(--gold);flex-shrink:0;background:rgba(196,153,58,.1);display:flex;align-items:center;justify-content:center}
.pv-check.done::before{background:var(--gold);background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='10' viewBox='0 0 24 24' fill='none' stroke='%230B1629' stroke-width='3'%3E%3Cpolyline points='20 6 9 17 4 12'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:center}

/* ─── ZONE / MAP ─── */
#zone{background:var(--navy)}
.zone-wrap{display:grid;grid-template-columns:1fr 1fr;gap:5rem;align-items:center;margin-top:4rem}
.zone-text p{font-size:.82rem;color:var(--text2);line-height:1.9;font-weight:300;margin-bottom:1.2rem}
.zone-cities{display:flex;flex-direction:column;gap:.6rem;margin-top:1.5rem}
.zcity{display:flex;align-items:center;gap:.8rem;font-size:.78rem;color:var(--text2);padding:.6rem 0;border-bottom:1px solid var(--border2);cursor:none;transition:color .3s,padding .3s}
.zcity:hover{color:var(--gold);padding-left:.5rem}
.zcity::before{content:'📍';font-size:.7rem}
.map-svg-wrap{position:relative;background:var(--navy2);border:1px solid var(--border);padding:1.5rem}
.map-badge{font-size:.6rem;letter-spacing:.2em;text-transform:uppercase;color:var(--text3);margin-bottom:1rem;display:flex;align-items:center;gap:.5rem}
.map-pulse{width:7px;height:7px;background:var(--gold);border-radius:50%;animation:blink 1.8s infinite}

/* ─── PROPOS ─── */
#propos{background:var(--navy2)}
.propos-grid{display:grid;grid-template-columns:1fr 1fr;gap:6rem;align-items:start;margin-top:4rem}
.propos-quote{font-family:'Playfair Display',serif;font-size:1.15rem;font-style:italic;line-height:1.7;color:var(--text);border-left:2px solid var(--gold);padding-left:1.5rem;margin:2rem 0}
.propos-p{font-size:.82rem;color:var(--text2);line-height:1.9;font-weight:300;margin-bottom:1.2rem}
.distinctions{display:flex;flex-direction:column;gap:0}
.dist{display:flex;align-items:start;gap:1rem;padding:1.2rem 0;border-bottom:1px solid var(--border2)}
.dist-arrow{color:var(--gold);font-size:.9rem;flex-shrink:0;margin-top:.1rem}
.dist-text{font-size:.8rem;color:var(--text2);line-height:1.7;font-weight:300}
.propos-badge{background:var(--navy3);border:1px solid var(--border);padding:2rem;margin-top:2rem}
.pb-label{font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3);margin-bottom:1rem}
.pb-items{display:flex;flex-direction:column;gap:.7rem}
.pb-item{display:flex;align-items:center;gap:.8rem;font-size:.78rem;color:var(--text2)}
.pb-dot{width:6px;height:6px;background:var(--gold);border-radius:50%;flex-shrink:0}

/* ─── SIMULATEUR ─── */
#simulateur{background:var(--navy3)}
.sim-wrap{max-width:700px;margin:4rem auto 0}
.sim-grid{display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(--border2);margin-bottom:1px}
.sim-field{background:var(--navy2);padding:1.5rem}
.sim-field label{font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3);display:block;margin-bottom:.7rem}
.sim-field input,.sim-field select{width:100%;background:transparent;border:none;border-bottom:1px solid var(--border);color:var(--text);font-size:1.1rem;font-family:'Playfair Display',serif;padding:.3rem 0;outline:none;cursor:none}
.sim-field select{cursor:none;font-family:'DM Sans',sans-serif;font-size:.85rem}
.sim-field select option{background:var(--navy2);color:var(--text)}
.sim-result{background:var(--navy);border:1px solid var(--border);padding:2.5rem;display:flex;justify-content:space-between;align-items:center;gap:2rem}
.sim-out-label{font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3)}
.sim-out-val{font-family:'Playfair Display',serif;font-size:2.8rem;color:var(--gold);line-height:1.1;margin-top:.3rem}
.sim-disclaimer{font-size:.65rem;color:var(--text3);margin-top:.5rem;font-style:italic}
.sim-cta{margin-top:2rem;text-align:center}

/* ─── AUDIT ─── */
#audit{background:var(--navy);text-align:center}
.audit-inner{max-width:680px;margin:0 auto}
.audit-items{display:grid;grid-template-columns:repeat(4,1fr);gap:1.5rem;margin:3rem 0}
.audit-item{border:1px solid var(--border2);padding:1.5rem 1rem;transition:border-color .3s,transform .3s;cursor:none}
.audit-item:hover{border-color:rgba(196,153,58,.4);transform:translateY(-3px)}
.ai-ico{font-size:1.4rem;margin-bottom:.7rem}
.ai-label{font-size:.7rem;color:var(--text2);letter-spacing:.05em;font-weight:300}
.audit-cta-wrap{display:flex;flex-direction:column;align-items:center;gap:1rem}
.audit-note{font-size:.68rem;color:var(--text3);letter-spacing:.05em}

/* ─── FAQ ─── */
#faq{background:var(--navy2)}
.faq-list{margin-top:3.5rem;max-width:800px;margin-left:auto;margin-right:auto}
.faq-item{border-bottom:1px solid var(--border2)}
.faq-q{display:flex;justify-content:space-between;align-items:center;padding:1.4rem 0;cursor:none;font-size:.9rem;font-weight:500;transition:color .3s}
.faq-q:hover{color:var(--gold)}
.faq-q.open{color:var(--gold)}
.faq-icon{width:22px;height:22px;border:1px solid var(--border);display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:transform .35s,border-color .3s}
.faq-q.open .faq-icon{transform:rotate(45deg);border-color:rgba(196,153,58,.5)}
.faq-icon::before{content:'+';font-size:1rem;color:var(--gold);line-height:1}
.faq-a{font-size:.8rem;color:var(--text2);line-height:1.9;font-weight:300;max-height:0;overflow:hidden;transition:max-height .45s ease,padding .45s}
.faq-a.open{max-height:300px;padding-bottom:1.4rem}

/* ─── FOOTER ─── */
footer{padding:4rem 5vw 2rem;background:var(--navy2);border-top:1px solid var(--border2)}
.footer-top{display:grid;grid-template-columns:2fr 1fr 1fr 1fr;gap:4rem;margin-bottom:3rem}
.ft-logo{font-family:'Playfair Display',serif;font-size:1.3rem;color:var(--text);margin-bottom:1rem}
.ft-logo b{color:var(--gold)}
.ft-desc{font-size:.75rem;color:var(--text3);line-height:1.8;font-weight:300;margin-bottom:1rem}
.ft-contact a{font-size:.75rem;color:var(--gold);text-decoration:none;display:block;margin-bottom:.4rem;transition:color .3s}
.ft-contact a:hover{color:var(--gold3)}
.ft-col-title{font-size:.6rem;letter-spacing:.25em;text-transform:uppercase;color:var(--text3);margin-bottom:1.2rem}
.ft-links{display:flex;flex-direction:column;gap:.6rem}
.ft-links a{font-size:.75rem;color:var(--text2);text-decoration:none;transition:color .3s;cursor:none}
.ft-links a:hover{color:var(--gold)}
.footer-bot{border-top:1px solid var(--border2);padding-top:1.5rem;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:1rem}
.footer-copy{font-size:.62rem;color:var(--text3);letter-spacing:.05em}
.footer-legal{display:flex;gap:2rem}
.footer-legal a{font-size:.62rem;color:var(--text3);text-decoration:none;transition:color .3s;cursor:none}
.footer-legal a:hover{color:var(--gold)}

/* ─── CHATBOT ─── */
#chatbot-btn{position:fixed;bottom:7rem;right:2rem;z-index:900}
.chat-bubble{width:50px;height:50px;background:var(--navy3);border:1px solid var(--border);border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:none;transition:all .3s}
.chat-bubble:hover{border-color:var(--gold);background:var(--navy4)}
.chat-bubble svg{width:20px;height:20px;stroke:var(--gold);fill:none;stroke-width:1.8}
.chat-panel{position:fixed;bottom:7rem;right:6rem;width:320px;background:var(--navy2);border:1px solid var(--border);display:none;flex-direction:column;z-index:901;overflow:hidden}
.chat-panel.open{display:flex}
.chat-head{background:var(--navy3);padding:1rem 1.2rem;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border2)}
.chat-head-name{font-size:.85rem;font-weight:500}
.chat-head-sub{font-size:.62rem;color:var(--gold);letter-spacing:.05em}
.chat-close{cursor:none;background:none;border:none;color:var(--text2);font-size:1.2rem;line-height:1;transition:color .3s}
.chat-close:hover{color:var(--gold)}
.chat-msgs{height:250px;overflow-y:auto;padding:1rem;display:flex;flex-direction:column;gap:.8rem}
.chat-msgs::-webkit-scrollbar{width:3px}
.chat-msgs::-webkit-scrollbar-track{background:transparent}
.chat-msgs::-webkit-scrollbar-thumb{background:var(--border)}
.msg{max-width:82%;font-size:.75rem;line-height:1.6;padding:.6rem .9rem}
.msg.bot{background:var(--navy3);color:var(--text);align-self:flex-start;border-left:2px solid var(--gold)}
.msg.user{background:rgba(196,153,58,.12);color:var(--text);align-self:flex-end;border:1px solid var(--border)}
.chat-quick{display:flex;flex-direction:column;gap:.4rem;padding:.5rem 1rem}
.cq{font-size:.7rem;border:1px solid var(--border);color:var(--text2);padding:.4rem .7rem;cursor:none;background:transparent;text-align:left;transition:all .3s;font-family:'DM Sans',sans-serif}
.cq:hover{border-color:var(--gold);color:var(--gold)}
.chat-input-row{display:flex;border-top:1px solid var(--border2)}
.chat-input{flex:1;background:transparent;border:none;padding:.8rem 1rem;font-size:.75rem;color:var(--text);font-family:'DM Sans',sans-serif;outline:none}
.chat-input::placeholder{color:var(--text3)}
.chat-send{background:var(--gold);border:none;padding:.8rem 1rem;cursor:none;font-size:.75rem;font-weight:600;color:var(--navy);font-family:'DM Sans',sans-serif;transition:background .3s}
.chat-send:hover{background:var(--gold2)}

/* ─── ANIMATIONS ─── */
@keyframes fadeUp{from{opacity:0;transform:translateY(28px)}to{opacity:1;transform:translateY(0)}}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.25}}
@keyframes ticker{from{transform:translateX(0)}to{transform:translateX(-50%)}}
@keyframes floatY{0%,100%{transform:translateY(0)}50%{transform:translateY(-7px)}}
.reveal{opacity:0;transform:translateY(32px);transition:opacity .75s ease,transform .75s ease}
.reveal.vis{opacity:1;transform:none}
.rd1{transition-delay:.1s}.rd2{transition-delay:.2s}.rd3{transition-delay:.3s}.rd4{transition-delay:.4s}

/* ─── RESPONSIVE ─── */
@media(max-width:900px){
  .nav-links,.nav-cta{display:none}
  .burger{display:flex}
  .services-grid,.valeur-grid,.process-wrap,.zone-wrap,.propos-grid,.sim-grid,.audit-items,.footer-top{grid-template-columns:1fr}
  .svc-ghost{display:none}
}
</style>
</head>
<body>

<div id="cur"></div>
<div id="cur-ring"></div>
<div id="progress"></div>
<canvas id="stars"></canvas>

<!-- LOADER -->
<div id="loader">
  <div class="loader-logo"><b>Renova</b><span>·Stay</span></div>
  <div class="loader-track"><div class="loader-fill" id="lfill"></div></div>
  <div class="loader-pct" id="lpct">0%</div>
</div>

<!-- NAV -->
<nav id="nav">
  <a href="#hero" class="logo"><b>Renova</b><span>·Stay</span></a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#valeur">Approche</a></li>
    <li><a href="#processus">Méthode</a></li>
    <li><a href="#zone">Zone</a></li>
    <li><a href="#propos">À propos</a></li>
  </ul>
  <button class="nav-cta" onclick="scrollTo('#audit')">Audit gratuit</button>
  <div class="burger" id="burger" onclick="toggleMob()">
    <span></span><span></span><span></span>
  </div>
</nav>

<!-- MENU MOB -->
<div class="mob-menu" id="mobmenu">
  <a href="#services" onclick="toggleMob()">Services</a>
  <a href="#valeur" onclick="toggleMob()">Approche</a>
  <a href="#processus" onclick="toggleMob()">Méthode</a>
  <a href="#zone" onclick="toggleMob()">Zone</a>
  <a href="#propos" onclick="toggleMob()">À propos</a>
  <a href="#audit" onclick="toggleMob()" style="color:var(--gold)">Audit gratuit</a>
</div>

<!-- FLOATING ACTIONS -->
<div class="float-bar">
  <div style="position:relative">
    <a href="https://wa.me/33783367640" target="_blank" class="fab fab-wa" title="WhatsApp">
      <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"/></svg>
    </a>
  </div>
  <div style="position:relative">
    <a href="mailto:delio.casciu10@gmail.com" class="fab fab-mail" title="Email">
      <svg viewBox="0 0 24 24" fill="white"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
    </a>
  </div>
</div>

<!-- CHATBOT -->
<div id="chatbot-btn">
  <div class="chat-bubble" onclick="toggleChat()" title="Chat">
    <svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
  </div>
</div>
<div class="chat-panel" id="chatPanel">
  <div class="chat-head">
    <div><div class="chat-head-name">Renova·Stay</div><div class="chat-head-sub">● En ligne · Répond rapidement</div></div>
    <button class="chat-close" onclick="toggleChat()">✕</button>
  </div>
  <div class="chat-msgs" id="chatMsgs">
    <div class="msg bot">Bonjour 👋 Je suis là pour répondre à vos questions sur la gestion locative en Moselle. Comment puis-je vous aider ?</div>
  </div>
  <div class="chat-quick" id="quickReplies">
    <button class="cq" onclick="botReply('services')">Vos services en détail ?</button>
    <button class="cq" onclick="botReply('zone')">Vous intervenez où ?</button>
    <button class="cq" onclick="botReply('audit')">Comment obtenir un audit ?</button>
  </div>
  <div class="chat-input-row">
    <input class="chat-input" id="chatIn" placeholder="Votre message..." onkeydown="if(event.key==='Enter')sendMsg()">
    <button class="chat-send" onclick="sendMsg()">→</button>
  </div>
</div>

<!-- ═══════════ HERO ═══════════ -->
<section id="hero">
  <div class="hero-bg-grad"></div>
  <div class="hero-grid-lines">
    <svg viewBox="0 0 1400 800" preserveAspectRatio="xMidYMid slice">
      <defs><pattern id="g" width="80" height="80" patternUnits="userSpaceOnUse"><path d="M80 0L0 0 0 80" fill="none" stroke="#C4993A" stroke-width=".4"/></pattern></defs>
      <rect width="1400" height="800" fill="url(#g)"/>
    </svg>
  </div>
  <div class="hero-inner">
    <div class="hero-eyebrow">Gestion locative · Moselle · Courte durée</div>
    <h1 class="hero-h1">La gestion locative<br><em>nouvelle génération</em><br>pour la courte durée.</h1>
    <p class="hero-p">Nous accompagnons les propriétaires dans l'optimisation, l'exploitation et la gestion quotidienne de leurs biens afin d'améliorer leur rentabilité et l'expérience des voyageurs.</p>
    <div class="hero-btns">
      <button class="btn-primary" onclick="scrollTo('#audit')"><span>Demander un audit gratuit</span></button>
      <button class="btn-secondary" onclick="scrollTo('#services')">Découvrir nos services</button>
    </div>
    <div class="hero-badges">
      <div class="hbadge active">Réactivité 7j/7</div>
      <div class="hbadge">Accompagnement personnalisé</div>
      <div class="hbadge">Service sur mesure</div>
    </div>
  </div>
  <div class="hero-scroll">
    <div class="scroll-line"></div>
    <div class="scroll-txt">Défiler</div>
  </div>
</section>

<!-- TICKER -->
<div class="ticker-wrap" aria-hidden="true">
  <div class="ticker-inner">
    <span class="ticker-item"><span class="ticker-dot"></span>Gestion locative</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Moselle</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Optimisation d'annonces</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Renova·Stay</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Courte durée</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Airbnb · Booking</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Expérience voyageur</span>
    <span class="ticker-item"><span class="ticker-dot"></span>7j/7</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Gestion locative</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Moselle</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Optimisation d'annonces</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Renova·Stay</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Courte durée</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Airbnb · Booking</span>
    <span class="ticker-item"><span class="ticker-dot"></span>Expérience voyageur</span>
    <span class="ticker-item"><span class="ticker-dot"></span>7j/7</span>
  </div>
</div>

<!-- ═══════════ SERVICES ═══════════ -->
<section id="services">
  <div class="services-header reveal">
    <div class="s-tag">Services</div>
    <h2 class="s-title">Une prise en charge <em>complète</em></h2>
    <p class="s-sub">De la communication voyageurs à la coordination terrain — nous gérons chaque aspect de votre activité locative sur toutes les plateformes.</p>
  </div>
  <div class="services-grid">
    <div class="svc reveal">
      <div class="svc-top-line"></div>
      <div class="svc-ghost">01</div>
      <div class="svc-num">01</div>
      <div class="svc-label">Communication</div>
      <h3 class="svc-title">Gestion voyageurs</h3>
      <p class="svc-desc">Nous assurons les échanges avec les voyageurs avant, pendant et après leur séjour pour une expérience irréprochable à chaque étape.</p>
    </div>
    <div class="svc reveal rd1">
      <div class="svc-top-line"></div>
      <div class="svc-ghost">02</div>
      <div class="svc-num">02</div>
      <div class="svc-label">Performance</div>
      <h3 class="svc-title">Optimisation des annonces</h3>
      <p class="svc-desc">Analyse des performances, amélioration des descriptions, optimisation du positionnement et valorisation du bien sur toutes les plateformes.</p>
    </div>
    <div class="svc reveal rd1">
      <div class="svc-top-line"></div>
      <div class="svc-ghost">03</div>
      <div class="svc-num">03</div>
      <div class="svc-label">Opérationnel</div>
      <h3 class="svc-title">Coordination opérationnelle</h3>
      <p class="svc-desc">Organisation des arrivées, départs et interventions nécessaires au bon fonctionnement du logement, sans que vous ayez à intervenir.</p>
    </div>
    <div class="svc reveal rd2">
      <div class="svc-top-line"></div>
      <div class="svc-ghost">04</div>
      <div class="svc-num">04</div>
      <div class="svc-label">Suivi</div>
      <h3 class="svc-title">Accompagnement propriétaire</h3>
      <p class="svc-desc">Un interlocuteur unique et un suivi transparent de l'activité, avec des rapports réguliers et une communication directe en permanence.</p>
    </div>
  </div>
  <div class="platforms reveal" style="margin-top:2rem">
    <div class="plat">Airbnb</div>
    <div class="plat">Booking.com</div>
    <div class="plat">Abritel</div>
    <div class="plat">Expedia</div>
    <div class="plat" style="color:var(--text3);border-style:dashed">& autres</div>
  </div>
</section>

<!-- ═══════════ VALEUR ═══════════ -->
<section id="valeur">
  <div class="reveal">
    <div class="s-tag">Notre approche</div>
    <h2 class="s-title">Plus qu'un simple <em>co-hôte</em></h2>
    <p class="s-sub">Notre approche repose sur trois piliers fondamentaux qui font la différence sur le long terme.</p>
  </div>
  <div class="valeur-grid">
    <div class="val-card reveal">
      <div class="val-icon">📈</div>
      <div class="val-num">01</div>
      <h3 class="val-title">Performance</h3>
      <p class="val-desc">Chaque logement possède un potentiel inexploité que nous aidons à révéler grâce à une analyse rigoureuse du marché et des données de performance.</p>
    </div>
    <div class="val-card reveal rd1">
      <div class="val-icon">⭐</div>
      <div class="val-num">02</div>
      <h3 class="val-title">Expérience voyageur</h3>
      <p class="val-desc">Une expérience fluide et mémorable génère de meilleurs avis, favorise les réservations et fidélise les voyageurs sur la durée.</p>
    </div>
    <div class="val-card reveal rd2">
      <div class="val-icon">🔍</div>
      <div class="val-num">03</div>
      <h3 class="val-title">Transparence</h3>
      <p class="val-desc">Une communication claire, des comptes-rendus réguliers et un suivi détaillé de chaque action réalisée sur votre bien, sans zone d'ombre.</p>
    </div>
  </div>
</section>

<!-- ═══════════ PROCESSUS ═══════════ -->
<section id="processus">
  <div class="reveal">
    <div class="s-tag">Méthode</div>
    <h2 class="s-title">Comment nous <em>travaillons</em></h2>
  </div>
  <div class="process-wrap">
    <div class="process-steps">
      <div class="ps act" onclick="togglePs(this)">
        <div class="ps-row"><span class="ps-num">01</span><span class="ps-name">Analyse du logement</span></div>
        <div class="ps-body">Étude approfondie de votre annonce, de votre positionnement et de votre marché local en Moselle pour établir un diagnostic précis.</div>
      </div>
      <div class="ps" onclick="togglePs(this)">
        <div class="ps-row"><span class="ps-num">02</span><span class="ps-name">Recommandations</span></div>
        <div class="ps-body">Identification des opportunités d'amélioration concrètes, priorisées par impact potentiel sur votre rentabilité.</div>
      </div>
      <div class="ps" onclick="togglePs(this)">
        <div class="ps-row"><span class="ps-num">03</span><span class="ps-name">Mise en place</span></div>
        <div class="ps-body">Déploiement des optimisations et organisation structurée de la gestion quotidienne sans perturber votre activité.</div>
      </div>
      <div class="ps" onclick="togglePs(this)">
        <div class="ps-row"><span class="ps-num">04</span><span class="ps-name">Suivi continu</span></div>
        <div class="ps-body">Amélioration continue des performances et de l'expérience voyageur, semaine après semaine, avec des rapports transparents.</div>
      </div>
    </div>
    <div class="process-visual reveal rd2">
      <div class="pv-box">
        <div class="pv-title">Approche Renova·Stay</div>
        <div class="pv-sub">Une gestion rigoureuse, des résultats durables.</div>
        <div class="pv-checklist">
          <div class="pv-check done">Disponibilité 7 jours sur 7</div>
          <div class="pv-check done">Rapports mensuels détaillés</div>
          <div class="pv-check done">Interlocuteur unique dédié</div>
          <div class="pv-check done">Coordination opérationnelle complète</div>
          <div class="pv-check done">Toutes plateformes gérées</div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════ SIMULATEUR ═══════════ -->
<section id="simulateur">
  <div class="reveal" style="text-align:center;max-width:600px;margin:0 auto 0">
    <div class="s-tag" style="justify-content:center">Outil interactif</div>
    <h2 class="s-title">Estimez votre <em>potentiel</em></h2>
    <p class="s-sub" style="margin:0 auto">Obtenez une estimation indicative des revenus mensuels de votre bien selon son type et sa localisation en Moselle.</p>
  </div>
  <div class="sim-wrap reveal">
    <div class="sim-grid">
      <div class="sim-field">
        <label>Type de logement</label>
        <select id="simType" onchange="calcSim()">
          <option value="studio">Studio / T1</option>
          <option value="t2">T2 — 2 pièces</option>
          <option value="t3">T3 — 3 pièces</option>
          <option value="maison">Maison / Villa</option>
        </select>
      </div>
      <div class="sim-field">
        <label>Secteur Moselle</label>
        <select id="simZone" onchange="calcSim()">
          <option value="metz">Metz & agglomération</option>
          <option value="thionville">Luxembourg et Frontière</option>
          <option value="sarreguemines">Sarreguemines</option>
          <option value="forbach">Forbach & St avold</option>
          <option value="rural">Zone rurale / autre</option>
        </select>
      </div>
      <div class="sim-field">
        <label>Nuits estimées / mois</label>
        <input type="range" id="simNuits" min="5" max="28" value="16" oninput="calcSim();document.getElementById('simNuitsVal').textContent=this.value" style="margin-top:.5rem;width:100%;accent-color:var(--gold)">
        <div style="font-size:.7rem;color:var(--text2);margin-top:.4rem"><span id="simNuitsVal">16</span> nuits / mois</div>
      </div>
      <div class="sim-field">
        <label>Prix / nuit estimé</label>
        <input type="range" id="simPrice" min="30" max="200" value="70" step="5" oninput="calcSim();document.getElementById('simPriceVal').textContent=this.value+'€'" style="margin-top:.5rem;width:100%;accent-color:var(--gold)">
        <div style="font-size:.7rem;color:var(--text2);margin-top:.4rem"><span id="simPriceVal">70€</span> par nuit</div>
      </div>
    </div>
    <div class="sim-result">
      <div>
        <div class="sim-out-label">Revenu brut estimé</div>
        <div class="sim-out-val" id="simOut">1 120€</div>
        <div class="sim-disclaimer">Estimation indicative · Résultats variables selon le bien</div>
      </div>
      <div style="text-align:right">
        <div class="sim-out-label">Avec Renova·Stay</div>
        <div style="font-size:.8rem;color:var(--text2);margin-top:.4rem;line-height:1.7">Optimisation des<br>annonces incluse</div>
      </div>
    </div>
    <div class="sim-cta">
      <button class="btn-primary" onclick="scrollTo('#audit')"><span>Obtenir un audit réel gratuit →</span></button>
    </div>
  </div>
</section>

<!-- ═══════════ ZONE ═══════════ -->
<section id="zone">
  <div class="reveal">
    <div class="s-tag">Zone d'intervention</div>
    <h2 class="s-title">Basés en <em>Moselle</em>,<br>à votre service.</h2>
  </div>
  <div class="zone-wrap">
    <div class="zone-text">
      <p>Nous intervenons sur l'ensemble du département de la Moselle et ses alentours, avec une connaissance fine du marché local.</p>
      <div class="zone-cities">
        <div class="zcity">Metz &amp; agglomération</div>
        <div class="zcity">Luxembourg &amp; vallée de la Moselle</div>
        <div class="zcity">Sarreguemines &amp; Est mosellan</div>
        <div class="zcity">Forbach &amp; St avold</div>
        <div class="zcity">Ensemble du département 57</div>
      </div>
      <div style="margin-top:2rem;padding:1.5rem;border:1px solid var(--border2);background:var(--navy2)">
        <div style="font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--text3);margin-bottom:.5rem">Zone active</div>
        <div style="font-size:.85rem;color:var(--text2)">Moselle (57) · Grand Est</div>
      </div>
    </div>
    <div class="map-svg-wrap reveal rd1">
      <div class="map-badge"><span class="map-pulse"></span>Département de la Moselle (57)</div>
      <svg viewBox="0 0 360 260" width="100%" style="display:block">
        <rect width="360" height="260" fill="var(--navy2)" rx="0"/>
        <!-- Background grid -->
        <defs>
          <pattern id="mapgrid" width="20" height="20" patternUnits="userSpaceOnUse">
            <path d="M20 0L0 0 0 20" fill="none" stroke="rgba(196,153,58,0.06)" stroke-width=".5"/>
          </pattern>
        </defs>
        <rect width="360" height="260" fill="url(#mapgrid)"/>
        <!-- Moselle department shape -->
        <path d="M75 35 Q95 22 135 26 Q175 18 215 28 Q255 22 285 48 Q315 68 308 105 Q318 135 290 162 Q262 188 222 194 Q182 204 142 194 Q102 200 72 176 Q42 153 46 118 Q36 88 50 63 Q60 42 75 35Z" fill="rgba(14,30,56,0.8)" stroke="rgba(196,153,58,0.35)" stroke-width="1.2"/>
        <!-- Rivière Moselle -->
        <path d="M118 42 Q128 68 138 98 Q143 128 158 153 Q168 172 172 193" fill="none" stroke="rgba(100,160,255,0.35)" stroke-width="2.5" stroke-linecap="round"/>
        <text x="175" y="115" fill="rgba(100,160,255,0.45)" font-size="7" font-family="DM Sans" letter-spacing="1">Moselle</text>
        <!-- Routes -->
        <path d="M88 78 Q138 73 188 78 Q238 83 278 108" fill="none" stroke="rgba(196,153,58,0.1)" stroke-width="1.5" stroke-dasharray="4,3"/>
        <!-- METZ - principal -->
        <g id="mtz">
          <circle cx="163" cy="97" r="7" fill="rgba(196,153,58,0.2)" stroke="var(--gold)" stroke-width="1.2"/>
          <circle cx="163" cy="97" r="14" fill="none" stroke="rgba(196,153,58,0.35)" stroke-width=".8">
            <animate attributeName="r" values="10;20;10" dur="3s" repeatCount="indefinite"/>
            <animate attributeName="opacity" values=".6;0;.6" dur="3s" repeatCount="indefinite"/>
          </circle>
          <circle cx="163" cy="97" r="3.5" fill="var(--gold)"/>
          <text x="174" y="93" fill="var(--gold)" font-size="9.5" font-family="DM Sans" font-weight="600">METZ</text>
          <text x="174" y="104" fill="rgba(196,153,58,0.55)" font-size="7" font-family="DM Sans">Préfecture · 57000</text>
        </g>
        <!-- THIONVILLE -->
        <g>
          <circle cx="168" cy="50" r="4.5" fill="rgba(196,153,58,0.5)" stroke="var(--gold)" stroke-width=".8"/>
          <text x="178" y="47" fill="rgba(196,153,58,0.9)" font-size="8" font-family="DM Sans" font-weight="500">THIONVILLE</text>
          <text x="178" y="57" fill="rgba(196,153,58,0.45)" font-size="6.5" font-family="DM Sans">57100</text>
        </g>
        <!-- SARREGUEMINES -->
        <g>
          <circle cx="230" cy="68" r="4" fill="rgba(196,153,58,0.4)" stroke="var(--gold)" stroke-width=".7"/>
          <text x="240" y="65" fill="rgba(196,153,58,0.75)" font-size="7.5" font-family="DM Sans">SARREGUEMINES</text>
        </g>
        <!-- FORBACH -->
        <g>
          <circle cx="102" cy="128" r="4" fill="rgba(196,153,58,0.4)" stroke="var(--gold)" stroke-width=".7"/>
          <text x="58" y="125" fill="rgba(196,153,58,0.75)" font-size="7.5" font-family="DM Sans">FORBACH</text>
        </g>
        <!-- SARREBOURG -->
        <g>
          <circle cx="192" cy="155" r="3.5" fill="rgba(196,153,58,0.35)" stroke="var(--gold)" stroke-width=".7"/>
          <text x="200" y="152" fill="rgba(196,153,58,0.6)" font-size="7" font-family="DM Sans">SARREBOURG</text>
        </g>
        <!-- Zone couverte halo -->
        <ellipse cx="183" cy="118" rx="120" ry="90" fill="rgba(196,153,58,0.025)" stroke="none"/>
        <!-- Légende -->
        <rect x="12" y="220" width="335" height="32" fill="rgba(11,22,41,0.6)" rx="2"/>
        <circle cx="26" cy="237" r="5" fill="rgba(196,153,58,0.6)" stroke="var(--gold)" stroke-width=".7"/>
        <text x="36" y="240" fill="rgba(196,153,58,0.7)" font-size="7" font-family="DM Sans">Ville principale</text>
        <rect x="125" y="232" width="8" height="8" fill="rgba(196,153,58,0.15)" stroke="rgba(196,153,58,.3)" stroke-width=".7"/>
        <text x="137" y="240" fill="rgba(196,153,58,0.6)" font-size="7" font-family="DM Sans">Zone d'intervention</text>
        <path d="M230 234 Q240 234 250 236" fill="none" stroke="rgba(100,160,255,0.4)" stroke-width="1.5" stroke-linecap="round"/>
        <text x="256" y="240" fill="rgba(100,160,255,0.5)" font-size="7" font-family="DM Sans">Rivière</text>
      </svg>
    </div>
  </div>
</section>

<!-- ═══════════ PROPOS ═══════════ -->
<section id="propos">
  <div class="reveal">
    <div class="s-tag">À propos</div>
    <h2 class="s-title">Notre <em>vision</em></h2>
  </div>
  <div class="propos-grid">
    <div>
      <blockquote class="propos-quote">"La gestion locative courte durée doit être plus simple, plus transparente et plus performante pour les propriétaires."</blockquote>
      <p class="propos-p">Notre mission est d'apporter une approche moderne de l'exploitation des locations saisonnières en Moselle, grâce à une organisation rigoureuse et une attention particulière portée à l'expérience voyageur.</p>
      <p class="propos-p">Nous croyons que chaque propriétaire mérite un accompagnement à la hauteur de son investissement — sans compromis sur la qualité ni sur la transparence.</p>
      <div style="font-size:.72rem;color:var(--text3);margin-top:1rem;letter-spacing:.05em">Renova·Stay · Moselle, France</div>
    </div>
    <div>
      <div class="distinctions">
        <div class="dist"><span class="dist-arrow">→</span><span class="dist-text">Une gestion orientée performance, pas seulement réactive aux demandes entrantes.</span></div>
        <div class="dist"><span class="dist-arrow">→</span><span class="dist-text">Un suivi transparent avec des rapports réguliers sur l'activité de votre bien.</span></div>
        <div class="dist"><span class="dist-arrow">→</span><span class="dist-text">Une approche sur mesure adaptée à chaque logement et chaque propriétaire.</span></div>
        <div class="dist"><span class="dist-arrow">→</span><span class="dist-text">Une attention constante portée à la qualité de l'expérience voyageur sur toutes les plateformes.</span></div>
        <div class="dist"><span class="dist-arrow">→</span><span class="dist-text">Un interlocuteur unique, disponible 7j/7 et pleinement impliqué dans votre projet.</span></div>
      </div>
      <div class="propos-badge">
        <div class="pb-label">Ce que vous obtenez</div>
        <div class="pb-items">
          <div class="pb-item"><span class="pb-dot"></span>Disponibilité 7 jours sur 7</div>
          <div class="pb-item"><span class="pb-dot"></span>Rapports mensuels détaillés</div>
          <div class="pb-item"><span class="pb-dot"></span>Interlocuteur unique dédié</div>
          <div class="pb-item"><span class="pb-dot"></span>Coordination opérationnelle complète</div>
          <div class="pb-item"><span class="pb-dot"></span>Toutes plateformes gérées</div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ═══════════ AUDIT ═══════════ -->
<section id="audit">
  <div class="audit-inner">
    <div class="reveal">
      <div class="s-tag" style="justify-content:center">Offre de bienvenue</div>
      <h2 class="s-title" style="text-align:center">Recevez une analyse <em>gratuite</em><br>de votre annonce</h2>
      <p class="s-sub" style="text-align:center;margin:0 auto 0">Un audit complet pour identifier les axes d'amélioration et révéler le véritable potentiel de votre logement en Moselle.</p>
    </div>
    <div class="audit-items">
      <div class="audit-item reveal">
        <div class="ai-ico">📝</div>
        <div class="ai-label">Titre de l'annonce</div>
      </div>
      <div class="audit-item reveal rd1">
        <div class="ai-ico">📸</div>
        <div class="ai-label">Qualité des photos</div>
      </div>
      <div class="audit-item reveal rd2">
        <div class="ai-ico">✍️</div>
        <div class="ai-label">Description du bien</div>
      </div>
      <div class="audit-item reveal rd3">
        <div class="ai-ico">💡</div>
        <div class="ai-label">Points d'amélioration</div>
      </div>
    </div>
    <div class="audit-cta-wrap">
      <a href="mailto:delio.casciu10@gmail.com?subject=Demande%20d'audit%20gratuit%20-%20Renova%20Stay&body=Bonjour%2C%0A%0AJe%20souhaite%20recevoir%20un%20audit%20gratuit%20de%20mon%20annonce.%0A%0ALien%20de%20mon%20annonce%20%3A%20%0A%0AMerci." class="btn-primary" style="text-decoration:none;display:inline-block"><span>Recevoir mon audit gratuit</span></a>
      <div class="audit-note">Gratuit &amp; sans engagement · Réponse sous 48h</div>
    </div>
  </div>
</section>

<!-- ═══════════ FAQ ═══════════ -->
<section id="faq">
  <div class="reveal" style="text-align:center">
    <div class="s-tag" style="justify-content:center">Questions fréquentes</div>
    <h2 class="s-title" style="text-align:center">Tout ce que vous <em>devez savoir</em></h2>
  </div>
  <div class="faq-list">
    <div class="faq-item">
      <div class="faq-q" onclick="toggleFaq(this)"><span>Comment fonctionne la gestion complète ?</span><span class="faq-icon"></span></div>
      <div class="faq-a">Nous prenons en charge l'intégralité de la gestion : communication avec les voyageurs, coordination des entrées/sorties, optimisation de vos annonces et suivi régulier. Vous restez informé à chaque étape sans avoir à intervenir au quotidien.</div>
    </div>
    <div class="faq-item">
      <div class="faq-q" onclick="toggleFaq(this)"><span>Quelles plateformes gérez-vous ?</span><span class="faq-icon"></span></div>
      <div class="faq-a">Nous gérons votre présence sur Airbnb, Booking.com, Abritel, Expedia et d'autres plateformes selon votre profil et vos objectifs. La gestion multi-plateforme est incluse dans notre accompagnement.</div>
    </div>
    <div class="faq-item">
      <div class="faq-q" onclick="toggleFaq(this)"><span>Comment se passe le premier contact ?</span><span class="faq-icon"></span></div>
      <div class="faq-a">Il suffit de nous écrire via le formulaire d'audit gratuit. Nous analysons votre annonce existante ou votre bien, puis nous vous proposons un rendez-vous pour discuter des possibilités d'optimisation.</div>
    </div>
    <div class="faq-item">
      <div class="faq-q" onclick="toggleFaq(this)"><span>Intervenez-vous partout en Moselle ?</span><span class="faq-icon"></span></div>
      <div class="faq-a">Oui, nous couvrons l'ensemble du département 57 : Metz et son agglomération, Thionville, Sarreguemines, Forbach, Sarrebourg et les zones rurales alentour.</div>
    </div>
    <div class="faq-item">
      <div class="faq-q" onclick="toggleFaq(this)"><span>L'audit gratuit est-il vraiment sans engagement ?</span><span class="faq-icon"></span></div>
      <div class="faq-a">Absolument. L'audit est offert sans condition ni engagement. C'est notre façon de vous démontrer concrètement la valeur que nous pouvons apporter avant toute collaboration.</div>
    </div>
  </div>
</section>

<!-- ═══════════ FOOTER ═══════════ -->
<footer>
  <div class="footer-top">
    <div>
      <div class="ft-logo"><b>Renova</b>·Stay</div>
      <p class="ft-desc">La gestion locative nouvelle génération pour les locations courte durée en Moselle.</p>
      <div class="ft-contact">
        <a href="mailto:delio.casciu10@gmail.com">delio.casciu10@gmail.com</a>
        <a href="https://wa.me/33783367640">07 83 36 76 40</a>
        <div style="font-size:.72rem;color:var(--text3);margin-top:.4rem">Moselle (57) · Grand Est · France</div>
      </div>
    </div>
    <div>
      <div class="ft-col-title">Navigation</div>
      <div class="ft-links">
        <a href="#services">Services</a>
        <a href="#valeur">Notre approche</a>
        <a href="#processus">Méthode</a>
        <a href="#zone">Zone d'intervention</a>
        <a href="#propos">À propos</a>
      </div>
    </div>
    <div>
      <div class="ft-col-title">Plateformes</div>
      <div class="ft-links">
        <a href="#">Airbnb</a>
        <a href="#">Booking.com</a>
        <a href="#">Abritel</a>
        <a href="#">Expedia</a>
      </div>
    </div>
    <div>
      <div class="ft-col-title">Légal</div>
      <div class="ft-links">
        <a href="#">Mentions légales</a>
        <a href="#">Politique de confidentialité</a>
      </div>
    </div>
  </div>
  <div class="footer-bot">
    <div class="footer-copy">© 2025 Renova·Stay. Tous droits réservés.</div>
    <div class="footer-legal">
      <a href="#">Mentions légales</a>
      <a href="#">Confidentialité</a>
    </div>
  </div>
</footer>

<script>
/* ─── CURSOR ─── */
const cur=document.getElementById('cur'),ring=document.getElementById('cur-ring');
let mx=0,my=0,rx=0,ry=0;
document.addEventListener('mousemove',e=>{
  mx=e.clientX;my=e.clientY;
  cur.style.left=mx+'px';cur.style.top=my+'px';
});
(function anim(){rx+=(mx-rx)*.1;ry+=(my-ry)*.1;ring.style.left=rx+'px';ring.style.top=ry+'px';requestAnimationFrame(anim)})();
document.querySelectorAll('a,button,.svc,.val-card,.ps,.faq-q,.zcity,.audit-item,.temo-btn,.cq,.chat-bubble').forEach(el=>{
  el.addEventListener('mouseenter',()=>document.body.classList.add('cur-hover'));
  el.addEventListener('mouseleave',()=>document.body.classList.remove('cur-hover'));
});

/* ─── PROGRESS BAR ─── */
window.addEventListener('scroll',()=>{
  const h=document.documentElement.scrollHeight-window.innerHeight;
  document.getElementById('progress').style.width=(scrollY/h*100)+'%';
});

/* ─── LOADER ─── */
let p=0;
const lf=document.getElementById('lfill'),lp=document.getElementById('lpct');
const li=setInterval(()=>{
  p+=Math.random()*18;
  if(p>=100){p=100;clearInterval(li);setTimeout(()=>document.getElementById('loader').classList.add('out'),400)}
  lf.style.width=Math.min(p,100)+'%';
  lp.textContent=Math.floor(Math.min(p,100))+'%';
},70);

/* ─── STARS ─── */
const cv=document.getElementById('stars'),cx=cv.getContext('2d');
function rs(){cv.width=innerWidth;cv.height=innerHeight}rs();
window.addEventListener('resize',rs);
const pts=Array.from({length:90},()=>({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.2+.3,vx:(Math.random()-.5)*.2,vy:(Math.random()-.5)*.2,o:Math.random()*.35+.08}));
(function drawStars(){
  cx.clearRect(0,0,cv.width,cv.height);
  pts.forEach(p=>{
    p.x+=p.vx;p.y+=p.vy;
    if(p.x<0)p.x=cv.width;if(p.x>cv.width)p.x=0;
    if(p.y<0)p.y=cv.height;if(p.y>cv.height)p.y=0;
    cx.beginPath();cx.arc(p.x,p.y,p.r,0,Math.PI*2);
    cx.fillStyle=`rgba(196,153,58,${p.o})`;cx.fill();
  });
  pts.forEach((a,i)=>pts.slice(i+1).forEach(b=>{
    const d=Math.hypot(a.x-b.x,a.y-b.y);
    if(d<100){cx.beginPath();cx.moveTo(a.x,a.y);cx.lineTo(b.x,b.y);cx.strokeStyle=`rgba(196,153,58,${.05*(1-d/100)})`;cx.lineWidth=.5;cx.stroke()}
  }));
  requestAnimationFrame(drawStars);
})();

/* ─── NAV STUCK ─── */
window.addEventListener('scroll',()=>document.getElementById('nav').classList.toggle('stuck',scrollY>60));

/* ─── REVEAL ─── */
const ro=new IntersectionObserver(es=>es.forEach(e=>{if(e.isIntersecting)e.target.classList.add('vis')}),{threshold:.12});
document.querySelectorAll('.reveal').forEach(r=>ro.observe(r));

/* ─── SCROLL HELPER ─── */
function scrollTo(id){document.querySelector(id).scrollIntoView({behavior:'smooth'})}

/* ─── BURGER ─── */
function toggleMob(){
  document.getElementById('burger').classList.toggle('open');
  document.getElementById('mobmenu').classList.toggle('open');
}

/* ─── PROCESS STEPS ─── */
function togglePs(el){
  document.querySelectorAll('.ps').forEach(p=>p.classList.remove('act'));
  el.classList.add('act');
}

/* ─── FAQ ─── */
function toggleFaq(q){
  const a=q.nextElementSibling;
  const open=a.classList.contains('open');
  document.querySelectorAll('.faq-a').forEach(x=>x.classList.remove('open'));
  document.querySelectorAll('.faq-q').forEach(x=>x.classList.remove('open'));
  if(!open){a.classList.add('open');q.classList.add('open')}
}

/* ─── SIMULATEUR ─── */
const baseRates={studio:{metz:60,thionville:55,sarreguemines:45,forbach:40,rural:35},t2:{metz:80,thionville:70,sarreguemines:58,forbach:52,rural:45},t3:{metz:100,thionville:90,sarreguemines:75,forbach:65,rural:58},maison:{metz:130,thionville:115,sarreguemines:95,forbach:85,rural:75}};
function calcSim(){
  const nuits=+document.getElementById('simNuits').value;
  const price=+document.getElementById('simPrice').value;
  const total=nuits*price;
  document.getElementById('simOut').textContent=total.toLocaleString('fr-FR')+'€';
}
calcSim();

/* ─── CHATBOT ─── */
const responses={
  services:"Renova·Stay prend en charge : la gestion voyageurs 7j/7, l'optimisation de vos annonces sur toutes les plateformes, la coordination opérationnelle (arrivées/départs), et l'accompagnement propriétaire avec rapports réguliers. 📋",
  zone:"Nous intervenons sur tout le département de la Moselle (57) : Metz et agglomération, Thionville, Sarreguemines, Forbach, Sarrebourg et les zones rurales. 📍",
  audit:"Pour votre audit gratuit, envoyez-nous un email à delio.casciu10@gmail.com ou utilisez le bouton 'Audit gratuit' en haut de page. Réponse sous 48h, sans engagement. ✉️",
  default:"Merci pour votre message ! Pour toute question spécifique, contactez-nous directement : delio.casciu10@gmail.com ou au 07 83 36 76 40. Nous répondons rapidement ! 😊"
};
function toggleChat(){document.getElementById('chatPanel').classList.toggle('open')}
function botReply(key){
  const msgs=document.getElementById('chatMsgs');
  const qr=document.getElementById('quickReplies');
  const labels={services:'Vos services en détail ?',zone:'Vous intervenez où ?',audit:'Comment obtenir un audit ?'};
  const um=document.createElement('div');um.className='msg user';um.textContent=labels[key]||key;msgs.appendChild(um);
  qr.style.display='none';
  setTimeout(()=>{
    const bm=document.createElement('div');bm.className='msg bot';bm.textContent=responses[key]||responses.default;msgs.appendChild(bm);msgs.scrollTop=msgs.scrollHeight;
  },600);
  msgs.scrollTop=msgs.scrollHeight;
}
function sendMsg(){
  const inp=document.getElementById('chatIn');
  const txt=inp.value.trim();if(!txt)return;
  const msgs=document.getElementById('chatMsgs');
  document.getElementById('quickReplies').style.display='none';
  const um=document.createElement('div');um.className='msg user';um.textContent=txt;msgs.appendChild(um);
  inp.value='';msgs.scrollTop=msgs.scrollHeight;
  const k=txt.toLowerCase().includes('service')?'services':txt.toLowerCase().includes('zone')||txt.toLowerCase().includes('interven')?'zone':txt.toLowerCase().includes('audit')?'audit':'default';
  setTimeout(()=>{const bm=document.createElement('div');bm.className='msg bot';bm.textContent=responses[k];msgs.appendChild(bm);msgs.scrollTop=msgs.scrollHeight;},700);
}

/* ─── MAGNETIC BUTTONS ─── */
document.querySelectorAll('.btn-primary,.btn-secondary,.nav-cta').forEach(btn=>{
  btn.addEventListener('mousemove',e=>{
    const r=btn.getBoundingClientRect();
    const dx=(e.clientX-r.left-r.width/2)*.25;
    const dy=(e.clientY-r.top-r.height/2)*.25;
    btn.style.transform=`translate(${dx}px,${dy}px)`;
  });
  btn.addEventListener('mouseleave',()=>btn.style.transform='');
});
</script>
</body>
</html>
HTMLEOF
echo "Done"
