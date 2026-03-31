<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Uniplace — From Lost to Settled</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Rethink+Sans:ital,wght@0,400;0,500;0,600;0,700;0,800;1,400&display=swap" rel="stylesheet">
<style>
:root {
  --dark: #000122;
  --light: #f0eae2;
  --white: #fcfcfc;
  --purple: #635fe5;
  --orange: #ff9052;
  --yellow: #efc041;
  --teal: #7de8c0;
  --font: 'Rethink Sans', sans-serif;
  --ease-out: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);
  --pad-x: clamp(20px, 6vw, 72px);
  --pad-y: clamp(44px, 6vh, 60px);
  --gap: clamp(8px, 1.5vw, 14px);
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { overflow: hidden; -webkit-text-size-adjust: 100%; }
body {
  font-family: var(--font);
  background: var(--dark);
  width: 100vw; height: 100vh;
  overflow: hidden;
  -webkit-tap-highlight-color: transparent;
}

/* PROGRESS BAR */
#progress-bar {
  position: fixed; top: 0; left: 0; height: 2px;
  background: linear-gradient(90deg, var(--purple), var(--orange));
  z-index: 200; transition: width 0.5s var(--ease-out);
  border-radius: 0 2px 2px 0; pointer-events: none; width: 0;
}

/* SLIDE COUNTER */
#slide-counter {
  position: fixed; top: 16px; left: var(--pad-x);
  font-size: clamp(9px, 1.5vw, 10px); letter-spacing: .14em;
  text-transform: uppercase; font-weight: 600;
  color: rgba(252,252,252,0.22); z-index: 200;
  transition: color 0.4s; pointer-events: none;
}

/* SLIDE TITLE */
#slide-title {
  position: fixed; top: 16px; left: 50%; transform: translateX(-50%);
  font-size: clamp(8px, 1.3vw, 10px); letter-spacing: .14em;
  text-transform: uppercase; font-weight: 600;
  color: rgba(252,252,252,0.16); z-index: 200;
  white-space: nowrap; pointer-events: none;
}
@media (max-width: 480px) { #slide-title { display: none; } }

/* KBD HINT */
#kbd-hint {
  position: fixed; bottom: clamp(20px, 4vh, 32px); right: clamp(16px, 3vw, 32px);
  font-size: 9px; letter-spacing: .1em; text-transform: uppercase;
  color: rgba(252,252,252,0.14); display: flex; align-items: center; gap: 5px;
  transition: opacity 0.6s; pointer-events: none; z-index: 100;
}
#kbd-hint kbd {
  display: inline-flex; align-items: center; justify-content: center;
  width: 18px; height: 18px; border: 1px solid rgba(255,255,255,0.1);
  border-radius: 4px; font-size: 10px; font-family: var(--font);
}
@media (pointer: coarse) { #kbd-hint { display: none; } }

/* SLIDE BASE */
#deck { width: 100vw; height: 100vh; position: relative; }
.slide {
  width: 100%; height: 100%;
  position: absolute; top: 0; left: 0;
  display: flex; flex-direction: column; justify-content: center;
  padding: var(--pad-y) var(--pad-x);
  opacity: 0; pointer-events: none;
  overflow: hidden; will-change: transform, opacity;
}
.slide.active { opacity: 1; pointer-events: all; }

.enter-from-right { transform: translateX(6vw) !important; opacity: 0 !important; }
.enter-from-left  { transform: translateX(-6vw) !important; opacity: 0 !important; }
.exit-to-left     { transform: translateX(-6vw) !important; opacity: 0 !important; }
.exit-to-right    { transform: translateX(6vw) !important; opacity: 0 !important; }
.animating        { transition: transform 0.6s var(--ease-out), opacity 0.6s var(--ease-out) !important; }
.active.animating { transform: translateX(0) !important; opacity: 1 !important; }

/* STAGGER ENTRY */
.anim-el {
  opacity: 0; transform: translateY(16px);
  transition: opacity 0.5s var(--ease-out), transform 0.5s var(--ease-out);
}
.slide.active .anim-el { opacity: 1; transform: translateY(0); }
.anim-el:nth-child(1) { transition-delay: .06s; }
.anim-el:nth-child(2) { transition-delay: .14s; }
.anim-el:nth-child(3) { transition-delay: .22s; }
.anim-el:nth-child(4) { transition-delay: .30s; }
.anim-el:nth-child(5) { transition-delay: .38s; }
.anim-el:nth-child(6) { transition-delay: .46s; }

.anim-card {
  opacity: 0; transform: translateY(20px) scale(0.97);
  transition: opacity 0.48s var(--ease-out), transform 0.48s var(--ease-out);
}
.slide.active .anim-card { opacity: 1; transform: translateY(0) scale(1); }
.anim-card:nth-child(1) { transition-delay: .28s; }
.anim-card:nth-child(2) { transition-delay: .38s; }
.anim-card:nth-child(3) { transition-delay: .48s; }
.anim-card:nth-child(4) { transition-delay: .58s; }

/* SHARED */
.eyebrow {
  font-size: clamp(8px, 1.4vw, 10px); letter-spacing: .2em;
  text-transform: uppercase; font-weight: 700;
  margin-bottom: clamp(8px, 1.5vh, 14px);
}
.accent-bar {
  width: 36px; height: 3px; border-radius: 2px;
  margin-bottom: clamp(10px, 2.5vh, 22px);
  transition: width 0.7s var(--ease-spring);
}
.slide.active .accent-bar { width: 52px; }
.slide-h2 {
  font-size: clamp(20px, 4vw, 46px);
  font-weight: 800; letter-spacing: -.03em; line-height: 1.06;
  margin-bottom: clamp(18px, 4vh, 40px);
}

/* SLIDE 1 — COVER */
#s1 { background: var(--dark); color: var(--white); }
.ring-pulse {
  position: absolute; border-radius: 50%; border: 1px solid;
  pointer-events: none; animation: ring-breathe 6s ease-in-out infinite;
}
.r1 { width: clamp(220px,50vw,480px); height: clamp(220px,50vw,480px); top:-24%; right:-8%; border-color:rgba(99,95,229,.22); }
.r2 { width: clamp(140px,30vw,300px); height: clamp(140px,30vw,300px); top:-10%; right:5%; border-color:rgba(99,95,229,.14); animation-delay:2s; }
.r3 { width: clamp(280px,62vw,600px); height: clamp(280px,62vw,600px); bottom:-34%; left:-18%; border-color:rgba(255,144,82,.12); animation-delay:4s; }
@keyframes ring-breathe { 0%,100%{opacity:.6;transform:scale(1)} 50%{opacity:1;transform:scale(1.03)} }

#s1 h1 {
  font-size: clamp(46px, 10vw, 96px); font-weight: 800;
  letter-spacing: -.04em; line-height: .92;
  margin-bottom: clamp(8px, 1.8vh, 18px);
}
#s1 h1 .pu { color: var(--purple); }
#s1 .sub {
  font-size: clamp(9px, 1.5vw, 13px); letter-spacing: .1em;
  text-transform: uppercase; color: rgba(252,252,252,.35);
  margin-bottom: clamp(24px, 5vh, 56px); font-weight: 500;
}
.authors { display: flex; gap: clamp(12px, 3vw, 24px); flex-wrap: wrap; }
.author-chip { display: flex; align-items: center; gap: clamp(6px, 1.5vw, 10px); }
.av {
  width: clamp(24px, 4vw, 32px); height: clamp(24px, 4vw, 32px);
  border-radius: 50%; display: flex; align-items: center; justify-content: center;
  font-size: clamp(7px, 1.3vw, 10px); font-weight: 700; flex-shrink: 0;
  transition: transform 0.3s var(--ease-spring);
}
.av:hover { transform: scale(1.18); }
.av-p { background: rgba(99,95,229,.25); color: #a8a5f5; }
.av-o { background: rgba(255,144,82,.22); color: #ffb899; }
.av-y { background: rgba(239,192,65,.22); color: #f5d88a; }
.author-name { font-size: clamp(10px, 1.8vw, 12px); color: rgba(252,252,252,.55); }

/* SLIDE 2 — PROBLEM */
#s2 { background: var(--light); color: var(--dark); flex-direction: row; align-items: center; gap: clamp(16px,4vw,40px); }
#s2 .s2-left { flex: 1; min-width: 0; }
#s2 .eyebrow { color: var(--purple); }
#s2 h2 { font-size: clamp(22px, 5vw, 56px); margin-bottom: clamp(10px, 2vh, 18px); }
#s2 h2 em { font-style: normal; color: var(--purple); }
#s2 .desc { font-size: clamp(11px, 1.8vw, 14px); color: rgba(0,1,34,.5); max-width: 400px; line-height: 1.6; }
.s2-visual {
  flex-shrink: 0; width: clamp(150px,30vw,300px); height: clamp(94px,19vw,188px);
  background: var(--dark); border-radius: clamp(10px, 2vw, 16px);
  overflow: hidden; box-shadow: 0 20px 56px rgba(0,0,0,0.2);
  transition: transform 0.4s var(--ease-spring), box-shadow 0.4s;
}
.s2-visual:hover { transform: scale(1.04) rotate(-1deg); box-shadow: 0 32px 80px rgba(0,0,0,0.28); }
@media (max-width: 600px) {
  #s2 { flex-direction: column; justify-content: center; }
  .s2-visual { width: 100%; max-width: 280px; height: 152px; align-self: center; }
  #s2 .desc { max-width: 100%; }
}
.figure-body { animation: fig-sway 4s ease-in-out infinite; transform-origin: 150px 154px; }
@keyframes fig-sway { 0%,100%{transform:rotate(0)} 25%{transform:rotate(3deg)} 75%{transform:rotate(-3deg)} }
.q-mark { animation: q-float 2.5s ease-in-out infinite; }
@keyframes q-float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }
.grid-ln { stroke-dasharray: 5; animation: gm 3s linear infinite; }
@keyframes gm { from{stroke-dashoffset:0} to{stroke-dashoffset:20} }

/* SLIDE 3 — ENVIRONMENT */
#s3 { background: var(--dark); color: var(--white); }
#s3 .eyebrow { color: var(--orange); }
.cards3 { display: grid; grid-template-columns: repeat(3,minmax(0,1fr)); gap: var(--gap); max-width: 820px; }
@media (max-width: 680px)  { .cards3 { grid-template-columns: 1fr; } }
@media (min-width: 681px) and (max-width: 900px) { .cards3 { grid-template-columns: 1fr 1fr; } }
.card3 {
  background: rgba(252,252,252,.05); border: .5px solid rgba(252,252,252,.1);
  border-radius: clamp(10px,2vw,16px); padding: clamp(14px,3vw,28px) clamp(12px,2.5vw,22px);
  transition: transform 0.3s var(--ease-spring), border-color 0.3s, background 0.3s;
}
.card3:hover { transform: translateY(-4px); border-color: rgba(252,252,252,.2); background: rgba(252,252,252,.07); }
.c3-icon {
  width: clamp(32px,5vw,42px); height: clamp(32px,5vw,42px);
  border-radius: 10px; display: flex; align-items: center;
  justify-content: center; margin-bottom: clamp(10px,2vh,16px);
  transition: transform 0.3s var(--ease-spring);
}
.card3:hover .c3-icon { transform: scale(1.15) rotate(-5deg); }
.card3 h3 { font-size: clamp(11px,1.8vw,13px); font-weight: 700; margin-bottom: 6px; line-height: 1.3; }
.card3 p  { font-size: clamp(10px,1.5vw,11.5px); color: rgba(252,252,252,.45); line-height: 1.55; }

/* SLIDE 4 — STARTING POINT */
#s4 { background: var(--light); color: var(--dark); }
#s4 .eyebrow { color: var(--orange); }
.pillars { display: grid; grid-template-columns: repeat(3,minmax(0,1fr)); gap: var(--gap); max-width: 820px; }
@media (max-width: 680px)  { .pillars { grid-template-columns: 1fr; } }
@media (min-width: 681px) and (max-width: 900px) { .pillars { grid-template-columns: 1fr 1fr; } }
.pillar {
  background: var(--white); border-radius: clamp(10px,2vw,16px);
  border: .5px solid rgba(0,1,34,.07);
  padding: clamp(14px,3vw,28px) clamp(12px,2.5vw,22px);
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
  position: relative; overflow: hidden;
}
.pillar::after {
  content:''; position:absolute; bottom:0; left:0; right:0; height:3px;
  background:currentColor; transform:scaleX(0); transition:transform 0.4s var(--ease-out);
}
.pillar:hover::after { transform: scaleX(1); }
.pillar:hover { transform: translateY(-4px); box-shadow: 0 16px 48px rgba(0,0,0,0.1); }
.p-num { font-size: clamp(22px,4vw,32px); font-weight: 800; line-height: 1; margin-bottom: clamp(8px,1.5vh,12px); }
.pillar h3 { font-size: clamp(11px,1.8vw,13px); font-weight: 700; color: var(--dark); margin-bottom: 6px; line-height: 1.3; }
.pillar p  { font-size: clamp(10px,1.5vw,11.5px); color: rgba(0,1,34,.5); line-height: 1.55; }

/* SLIDE 5 — OPPORTUNITY */
#s5 { background: var(--dark); color: var(--white); }
#s5 .eyebrow { color: rgba(99,95,229,.8); }
.opp-grid { display: grid; grid-template-columns: repeat(3,minmax(0,1fr)); gap: var(--gap); max-width: 860px; }
@media (max-width: 680px)  { .opp-grid { grid-template-columns: 1fr; } }
@media (min-width: 681px) and (max-width: 900px) { .opp-grid { grid-template-columns: 1fr 1fr; } }
.opp-card {
  border-radius: clamp(10px,2vw,14px); padding: clamp(14px,2.5vw,22px) clamp(12px,2.5vw,20px);
  transition: transform 0.3s var(--ease-spring);
}
.opp-card:hover { transform: translateY(-4px) scale(1.01); }
.opp-card.issue { background: rgba(99,95,229,.12); border: .5px solid rgba(99,95,229,.3); }
.opp-card.opp   { background: rgba(255,144,82,.10); border: .5px solid rgba(255,144,82,.25); }
.opp-card.need  { background: rgba(239,192,65,.09); border: .5px solid rgba(239,192,65,.22); }
.olabel { font-size: clamp(7px,1.2vw,9px); letter-spacing:.15em; text-transform:uppercase; font-weight:700; margin-bottom:12px; }
.issue .olabel{color:#a8a5f5} .opp .olabel{color:#ffb899} .need .olabel{color:#f5d88a}
.opp-card li {
  font-size: clamp(9px,1.5vw,11px); color:rgba(252,252,252,.5); line-height:1.6;
  list-style:none; padding-left:12px; position:relative; margin-bottom:5px;
}
.opp-card li::before { content:'—'; position:absolute; left:0; opacity:.4; font-size:10px; }
.s5-bar {
  max-width:860px; margin-top:clamp(10px,2vh,18px);
  background:rgba(252,252,252,.05); border:.5px solid rgba(252,252,252,.1);
  border-radius:8px; padding:clamp(10px,2vw,12px) clamp(14px,2.5vw,18px);
  font-size:clamp(10px,1.6vw,12px); color:rgba(252,252,252,.5);
  position:relative; overflow:hidden;
}
.s5-bar::before {
  content:''; position:absolute; top:0; left:0; width:3px; height:100%;
  background:linear-gradient(180deg,var(--purple),var(--orange)); border-radius:3px 0 0 3px;
}
.s5-bar strong { color:var(--white); font-weight:700; }

/* SLIDE 6 — METHODOLOGY */
#s6 { background: var(--light); color: var(--dark); }
#s6 .eyebrow { color: var(--orange); }
.approach-row { display:grid; grid-template-columns:1fr 1fr; gap:var(--gap); max-width:860px; margin-bottom:var(--gap); }
@media (max-width: 500px) { .approach-row { grid-template-columns: 1fr; } }
.approach-chip {
  background:var(--white); border-radius:clamp(8px,1.5vw,12px);
  border:.5px solid rgba(0,1,34,.07);
  padding:clamp(10px,2vw,14px) clamp(12px,2.5vw,18px);
  display:flex; align-items:center; gap:clamp(10px,2vw,14px);
  transition:transform 0.3s var(--ease-spring), box-shadow 0.3s;
}
.approach-chip:hover { transform:translateY(-2px); box-shadow:0 8px 24px rgba(0,0,0,0.08); }
.a-tag {
  width:clamp(30px,5vw,38px); height:clamp(30px,5vw,38px);
  border-radius:8px; flex-shrink:0;
  display:flex; align-items:center; justify-content:center;
  font-size:clamp(7px,1.2vw,9px); font-weight:800; letter-spacing:.04em;
}
.a-seq{background:rgba(99,95,229,.12);color:var(--purple)}
.a-agi{background:rgba(255,144,82,.12);color:var(--orange)}
.approach-chip p { font-size:clamp(10px,1.6vw,12px); font-weight:700; color:var(--dark); }
.approach-chip span { font-size:clamp(9px,1.4vw,10.5px); color:rgba(0,1,34,.45); display:block; margin-top:2px; }
.method-grid { display:grid; grid-template-columns:repeat(4,minmax(0,1fr)); gap:var(--gap); max-width:860px; }
@media (max-width: 600px) { .method-grid { grid-template-columns: 1fr 1fr; } }
@media (max-width: 360px) { .method-grid { grid-template-columns: 1fr; } }
.m-card {
  background:var(--white); border-radius:clamp(8px,1.5vw,12px);
  border:.5px solid rgba(0,1,34,.07);
  padding:clamp(12px,2vw,18px) clamp(10px,1.8vw,16px);
  transition:transform 0.3s var(--ease-spring), box-shadow 0.3s;
}
.m-card:hover { transform:translateY(-3px); box-shadow:0 12px 32px rgba(0,0,0,0.09); }
.m-tag { font-size:clamp(7px,1.2vw,9px); letter-spacing:.13em; text-transform:uppercase; font-weight:700; margin-bottom:8px; }
.m-card h3 { font-size:clamp(10px,1.6vw,12px); font-weight:700; color:var(--dark); margin-bottom:8px; }
.m-card li {
  font-size:clamp(9px,1.4vw,10.5px); color:rgba(0,1,34,.5); line-height:1.55;
  list-style:none; padding-left:10px; position:relative; margin-bottom:3px;
}
.m-card li::before { content:'·'; position:absolute; left:2px; font-size:14px; line-height:1; }

/* SLIDE 7 — KEY INSIGHTS */
#s7 {
  background: var(--dark); color: var(--white);
  justify-content: flex-start;
  padding-top: clamp(44px, 7vh, 64px);
  overflow-y: auto;
  overflow-x: hidden;
}
#s7::-webkit-scrollbar { width: 3px; }
#s7::-webkit-scrollbar-track { background: transparent; }
#s7::-webkit-scrollbar-thumb { background: rgba(99,95,229,.3); border-radius: 2px; }
#s7 .eyebrow { color: rgba(99,95,229,.8); }

/* Tab navigation */
.ins-tabs {
  display: flex; gap: 6px; max-width: 960px;
  margin-bottom: clamp(10px,2.5vh,18px); flex-wrap: wrap;
}
.ins-tab {
  font-size: clamp(8px,1.3vw,10px); letter-spacing: .12em; text-transform: uppercase;
  font-weight: 700; padding: clamp(5px,1vh,7px) clamp(10px,1.8vw,14px);
  border-radius: 40px; border: .5px solid transparent; cursor: pointer;
  transition: background 0.25s, border-color 0.25s, color 0.25s;
  background: rgba(252,252,252,.06); color: rgba(252,252,252,.35);
}
.ins-tab:hover { background: rgba(252,252,252,.1); color: rgba(252,252,252,.6); }
.ins-tab.active-tab { color: var(--white); }
.ins-tab.t-student.active-tab  { background: rgba(99,95,229,.22); border-color: rgba(99,95,229,.4); }
.ins-tab.t-b2b.active-tab      { background: rgba(255,144,82,.18); border-color: rgba(255,144,82,.35); }
.ins-tab.t-beta.active-tab     { background: rgba(125,232,192,.14); border-color: rgba(125,232,192,.3); }

/* Survey panel */
.ins-panel { display: none; max-width: 960px; }
.ins-panel.active-panel { display: flex; flex-direction: column; gap: var(--gap); }

/* Section header strip */
.ins-survey-header {
  display: flex; align-items: center; gap: clamp(10px,2vw,16px);
  padding: clamp(10px,1.8vh,14px) clamp(14px,2.5vw,20px);
  border-radius: clamp(8px,1.5vw,12px);
  margin-bottom: 2px;
}
.ish-student { background: rgba(99,95,229,.1); border: .5px solid rgba(99,95,229,.2); }
.ish-b2b     { background: rgba(255,144,82,.09); border: .5px solid rgba(255,144,82,.2); }
.ish-beta    { background: rgba(125,232,192,.08); border: .5px solid rgba(125,232,192,.2); }
.ish-icon {
  width: clamp(28px,4vw,36px); height: clamp(28px,4vw,36px); border-radius: 8px;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.ish-student .ish-icon { background: rgba(99,95,229,.2); }
.ish-b2b     .ish-icon { background: rgba(255,144,82,.18); }
.ish-beta    .ish-icon { background: rgba(125,232,192,.15); }
.ish-meta { flex: 1; min-width: 0; }
.ish-label {
  font-size: clamp(7px,1.1vw,9px); letter-spacing: .18em; text-transform: uppercase;
  font-weight: 700; margin-bottom: 2px;
}
.ish-student .ish-label { color: #a8a5f5; }
.ish-b2b     .ish-label { color: #ffb899; }
.ish-beta    .ish-label { color: var(--teal); }
.ish-title { font-size: clamp(11px,1.8vw,13px); font-weight: 700; color: var(--white); }
.ish-subtitle { font-size: clamp(9px,1.4vw,10.5px); color: rgba(252,252,252,.38); margin-top: 1px; }
.ish-n { font-size: clamp(8px,1.3vw,10px); color: rgba(252,252,252,.25); margin-left: auto; flex-shrink: 0; }

/* Finding cards grid */
.ins-findings {
  display: grid;
  grid-template-columns: repeat(3, minmax(0,1fr));
  gap: var(--gap);
}
@media (max-width: 720px) { .ins-findings { grid-template-columns: 1fr 1fr; } }
@media (max-width: 480px) { .ins-findings { grid-template-columns: 1fr; } }

.ins-finding {
  border-radius: clamp(8px,1.5vw,12px);
  padding: clamp(10px,2vw,16px) clamp(12px,2.2vw,18px);
  transition: transform 0.28s var(--ease-spring);
  position: relative; overflow: hidden;
}
.ins-finding:hover { transform: translateY(-3px); }
.ins-finding::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0; height: 2px;
}
/* student colour */
.if-s { background: rgba(99,95,229,.09); border: .5px solid rgba(99,95,229,.2); }
.if-s::before { background: linear-gradient(90deg, #635fe5, rgba(99,95,229,0)); }
/* b2b colour */
.if-b { background: rgba(255,144,82,.08); border: .5px solid rgba(255,144,82,.18); }
.if-b::before { background: linear-gradient(90deg, #ff9052, rgba(255,144,82,0)); }
/* beta colour */
.if-t { background: rgba(125,232,192,.07); border: .5px solid rgba(125,232,192,.18); }
.if-t::before { background: linear-gradient(90deg, #7de8c0, rgba(125,232,192,0)); }

.if-tag {
  font-size: clamp(6px,1vw,8px); letter-spacing: .14em; text-transform: uppercase;
  font-weight: 800; margin-bottom: clamp(4px,1vh,7px); display: flex; align-items: center; gap: 5px;
}
.if-s .if-tag { color: #a8a5f5; }
.if-b .if-tag { color: #ffb899; }
.if-t .if-tag { color: var(--teal); }
.if-tag::before {
  content: ''; display: inline-block; width: 5px; height: 5px;
  border-radius: 50%; background: currentColor; flex-shrink: 0;
}
.ins-finding h4 {
  font-size: clamp(10px,1.6vw,12px); font-weight: 700; color: var(--white);
  margin-bottom: clamp(3px,.8vh,5px); line-height: 1.3;
}
.ins-finding p {
  font-size: clamp(9px,1.35vw,10.5px); color: rgba(252,252,252,.38); line-height: 1.55;
}

/* feature bridge row */
.ins-bridge {
  display: flex; align-items: center; gap: clamp(8px,1.5vw,12px);
  padding: clamp(8px,1.5vh,12px) clamp(12px,2.5vw,18px);
  background: rgba(252,252,252,.04); border: .5px solid rgba(252,252,252,.08);
  border-radius: clamp(8px,1.5vw,10px); flex-wrap: wrap;
  margin-top: 4px;
}
.ins-bridge-label {
  font-size: clamp(7px,1.1vw,9px); letter-spacing: .14em; text-transform: uppercase;
  font-weight: 700; color: rgba(252,252,252,.25); flex-shrink: 0;
}
.ins-bridge-tags { display: flex; gap: 6px; flex-wrap: wrap; }
.ib-tag {
  font-size: clamp(8px,1.3vw,9.5px); padding: 3px 8px; border-radius: 20px;
  font-weight: 600; border: .5px solid;
}
.ib-p { color: #a8a5f5; border-color: rgba(99,95,229,.3); background: rgba(99,95,229,.1); }
.ib-o { color: #ffb899; border-color: rgba(255,144,82,.3); background: rgba(255,144,82,.1); }
.ib-y { color: #f5d88a; border-color: rgba(239,192,65,.3); background: rgba(239,192,65,.09); }
.ib-t { color: var(--teal); border-color: rgba(125,232,192,.3); background: rgba(125,232,192,.08); }

/* SLIDE 8 — SOLUTION */
#s8 { background: var(--light); color: var(--dark); }
#s8 .eyebrow { color: var(--purple); }
.sol-grid { display:grid; grid-template-columns:repeat(3,minmax(0,1fr)); gap:var(--gap); max-width:860px; }
@media (max-width: 680px)  { .sol-grid { grid-template-columns: 1fr; } }
@media (min-width: 681px) and (max-width: 900px) { .sol-grid { grid-template-columns: 1fr 1fr; } }
.sol-card {
  background:var(--white); border-radius:clamp(10px,2vw,16px);
  border:.5px solid rgba(0,1,34,.07);
  padding:clamp(14px,3vw,24px) clamp(12px,2.5vw,20px);
  transition:transform 0.3s var(--ease-spring), box-shadow 0.3s;
  position:relative; overflow:hidden;
}
.sol-card::before {
  content:''; position:absolute; top:0; left:0; right:0; height:3px;
  opacity:0; transition:opacity 0.3s;
}
.sol-card:nth-child(1)::before{background:var(--purple)}
.sol-card:nth-child(2)::before{background:var(--orange)}
.sol-card:nth-child(3)::before{background:var(--yellow)}
.sol-card:hover::before { opacity:1; }
.sol-card:hover { transform:translateY(-5px) scale(1.01); box-shadow:0 20px 56px rgba(0,0,0,0.12); }
.s-label { font-size:clamp(7px,1.2vw,9px); letter-spacing:.14em; text-transform:uppercase; font-weight:700; margin-bottom:8px; }
.sol-card h3 { font-size:clamp(11px,1.8vw,13px); font-weight:700; color:var(--dark); margin-bottom:10px; }
.sol-card li {
  font-size:clamp(9px,1.5vw,11px); color:rgba(0,1,34,.5); line-height:1.55;
  list-style:none; padding-left:12px; position:relative; margin-bottom:4px;
}
.sol-card li::before { content:'·'; position:absolute; left:2px; font-size:16px; line-height:.9; }

/* SLIDE 9 — RECOMMENDATIONS */
#s9 { background: var(--dark); color: var(--white); }
#s9 .eyebrow { color: #f5d88a; }
.reco-grid { display:grid; grid-template-columns:1fr 1fr; gap:var(--gap); max-width:860px; }
@media (max-width: 580px) { .reco-grid { grid-template-columns: 1fr; } }
.reco-card {
  background:rgba(252,252,252,.05); border:.5px solid rgba(252,252,252,.09);
  border-radius:clamp(10px,2vw,14px); padding:clamp(12px,2.5vw,20px);
  transition:transform 0.3s var(--ease-spring), background 0.3s;
}
.reco-card:hover { transform:translateY(-3px); background:rgba(252,252,252,.08); }
.r-num { font-size:clamp(18px,3.5vw,28px); font-weight:800; line-height:1; margin-bottom:clamp(6px,1.5vh,10px); }
.rc1 .r-num{color:#a8a5f5} .rc2 .r-num{color:#ffb899}
.rc3 .r-num{color:#f5d88a} .rc4 .r-num{color:var(--teal)}
.reco-card h3 { font-size:clamp(11px,1.8vw,13px); font-weight:700; margin-bottom:4px; }
.reco-card .r-sub { font-size:clamp(9px,1.5vw,11px); color:rgba(252,252,252,.45); margin-bottom:8px; }
.reco-card li {
  font-size:clamp(9px,1.4vw,10.5px); color:rgba(252,252,252,.4); line-height:1.55;
  list-style:none; padding-left:12px; position:relative; margin-bottom:3px;
}
.reco-card li::before { content:'—'; position:absolute; left:0; opacity:.4; font-size:9px; top:2px; }
.impact-row {
  max-width:860px; margin-top:clamp(10px,2vh,16px);
  background:rgba(252,252,252,.05); border:.5px solid rgba(252,252,252,.09);
  border-radius:8px; padding:clamp(10px,2vw,12px) clamp(14px,2.5vw,18px);
  font-size:clamp(10px,1.6vw,12px); color:rgba(252,252,252,.45);
  position:relative; overflow:hidden;
}
.impact-row::before {
  content:''; position:absolute; top:0; left:0; width:3px; height:100%;
  background:linear-gradient(180deg,#f5d88a,var(--teal));
}
.impact-row strong { color:var(--white); font-weight:700; }

/* SLIDE 10 — THANK YOU */
#s10 {
  background:var(--dark); color:var(--white);
  justify-content:space-between;
  padding-top:clamp(44px,7vh,64px); padding-bottom:clamp(40px,7vh,56px);
}
.ty-eyebrow { font-size:clamp(8px,1.3vw,10px); letter-spacing:.2em; text-transform:uppercase; font-weight:700; color:rgba(99,95,229,.7); margin-bottom:clamp(8px,2vh,14px); }
.ty-bar { width:36px; height:3px; background:var(--purple); border-radius:2px; margin-bottom:clamp(12px,3vh,24px); transition:width 0.8s var(--ease-spring); }
#s10.active .ty-bar { width:72px; }
.ty-h1 {
  font-size:clamp(46px,9.5vw,110px); font-weight:800;
  letter-spacing:-.05em; line-height:.88; margin-bottom:clamp(10px,2.5vh,22px);
}
.ty-h1 .dot { color:var(--purple); }
.ty-tagline { font-size:clamp(11px,1.8vw,14px); color:rgba(252,252,252,.4); max-width:460px; line-height:1.6; }
.ty-mid {
  display:flex; gap:clamp(20px,5vw,48px); align-items:center; flex-wrap:wrap;
  padding:clamp(14px,3vh,28px) 0;
  border-top:.5px solid rgba(252,252,252,.08);
  border-bottom:.5px solid rgba(252,252,252,.08);
}
.ty-stat .val { font-size:clamp(11px,2vw,16px); font-weight:800; }
.ty-stat .lbl { font-size:clamp(8px,1.3vw,10px); color:rgba(252,252,252,.3); margin-top:3px; letter-spacing:.05em; }
.ty-stat.p .val{color:var(--purple)} .ty-stat.o .val{color:var(--orange)} .ty-stat.y .val{color:var(--yellow)}
.ty-footer { display:flex; justify-content:space-between; align-items:flex-start; flex-wrap:wrap; gap:clamp(10px,3vw,24px); }
.ty-logo { font-size:clamp(16px,3vw,22px); font-weight:800; letter-spacing:-.03em; color:var(--white); opacity:.85; }
.ty-logo span { color:var(--purple); }
.ty-contacts { display:flex; gap:clamp(14px,4vw,32px); flex-wrap:wrap; }
.ty-contact .tc-name { font-size:clamp(10px,1.6vw,12px); font-weight:600; color:rgba(252,252,252,.7); }
.ty-contact .tc-email { font-size:clamp(9px,1.4vw,10.5px); color:rgba(252,252,252,.3); margin-top:2px; }
.ty-copy { font-size:clamp(9px,1.3vw,10px); color:rgba(252,252,252,.2); line-height:1.7; }
</style>
</head>
<body>

<div id="progress-bar"></div>
<div id="slide-counter">01 / 10</div>
<div id="slide-title"></div>
<div id="kbd-hint"><kbd>&#8592;</kbd><kbd>&#8594;</kbd> navigate</div>

<div id="deck">

<!-- S1 — COVER -->
<section class="slide" id="s1">
  <div class="ring-pulse r1"></div>
  <div class="ring-pulse r2"></div>
  <div class="ring-pulse r3"></div>
  <p class="eyebrow anim-el" style="color:rgba(99,95,229,.75)">From lost to settled</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h1 class="anim-el">uni<span class="pu">place</span></h1>
  <p class="sub anim-el">The student relocation companion</p>
  <div class="authors anim-el">
    <div class="author-chip"><div class="av av-p">CM</div><span class="author-name">Capucine Montaudon</span></div>
    <div class="author-chip"><div class="av av-o">LU</div><span class="author-name">Lauriane Uhlenbusch</span></div>
    <div class="author-chip"><div class="av av-y">CO</div><span class="author-name">Cedric Obeid</span></div>
  </div>
</section>

<!-- S2 — PROBLEM -->
<section class="slide" id="s2">
  <div class="s2-left">
    <p class="eyebrow anim-el">The challenge</p>
    <div class="accent-bar anim-el" style="background:var(--purple)"></div>
    <h2 class="slide-h2 anim-el">Arriving in a new country&#8230;<br><em>Where do you start?</em></h2>
    <p class="desc anim-el">International students face a maze of unknowns from the moment they land &#8212; admin, housing, social integration.</p>
  </div>
  <div class="s2-visual anim-el">
    <svg viewBox="0 0 300 188" xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <rect width="300" height="188" fill="#000122"/>
      <line class="grid-ln" x1="40" y1="52" x2="260" y2="52" stroke="rgba(99,95,229,0.22)" stroke-width="0.5"/>
      <line class="grid-ln" x1="40" y1="70" x2="260" y2="70" stroke="rgba(99,95,229,0.15)" stroke-width="0.5" style="animation-delay:.5s"/>
      <line class="grid-ln" x1="40" y1="88" x2="100" y2="88" stroke="rgba(99,95,229,0.15)" stroke-width="0.5" style="animation-delay:1s"/>
      <line class="grid-ln" x1="200" y1="88" x2="260" y2="88" stroke="rgba(99,95,229,0.15)" stroke-width="0.5" style="animation-delay:1.5s"/>
      <g class="figure-body">
        <ellipse cx="150" cy="178" rx="100" ry="9" fill="rgba(99,95,229,0.08)"/>
        <rect x="134" y="108" width="24" height="46" rx="7" fill="#12153a"/>
        <circle cx="146" cy="100" r="12" fill="#12153a"/>
        <rect x="158" y="135" width="13" height="18" rx="3" fill="#0a0d28"/>
        <line x1="165" y1="133" x2="165" y2="137" stroke="#635fe5" stroke-width="1.5"/>
      </g>
      <g class="q-mark">
        <text x="222" y="58" font-size="30" font-family="sans-serif" fill="rgba(255,144,82,0.8)" font-weight="700">?</text>
      </g>
    </svg>
  </div>
</section>

<!-- S3 — ENVIRONMENT -->
<section class="slide" id="s3">
  <p class="eyebrow anim-el">Context</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="slide-h2 anim-el">A changing environment</h2>
  <div class="cards3">
    <div class="card3 anim-card">
      <div class="c3-icon" style="background:rgba(99,95,229,.18)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><circle cx="10" cy="10" r="7.5" stroke="#a8a5f5" stroke-width="1.4"/><path d="M10 2.5C7.5 5.5 7.5 14.5 10 17.5M10 2.5C12.5 5.5 12.5 14.5 10 17.5" stroke="#a8a5f5" stroke-width="1.1" fill="none"/><line x1="2.5" y1="10" x2="17.5" y2="10" stroke="#a8a5f5" stroke-width="1.1"/></svg>
      </div>
      <h3>Global student mobility</h3>
      <p>International enrollment growing steadily across Europe year over year.</p>
    </div>
    <div class="card3 anim-card">
      <div class="c3-icon" style="background:rgba(255,144,82,.18)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><rect x="2" y="4" width="16" height="12" rx="2" stroke="#ffb899" stroke-width="1.4"/><path d="M6 8h8M6 11h5" stroke="#ffb899" stroke-width="1.1" stroke-linecap="round"/><circle cx="15" cy="4" r="3" fill="#ff9052"/><path d="M14 4h2M15 3v2" stroke="white" stroke-width=".8" stroke-linecap="round"/></svg>
      </div>
      <h3>Digital tools everywhere</h3>
      <p>New generation expects seamless, app-first experiences at every touchpoint.</p>
    </div>
    <div class="card3 anim-card">
      <div class="c3-icon" style="background:rgba(239,192,65,.16)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><path d="M10 17V7" stroke="#f5d88a" stroke-width="1.4" stroke-linecap="round"/><path d="M6 13l4 4 4-4" stroke="#f5d88a" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/><path d="M4 11V5M8 11V3M12 11V6M16 11V2" stroke="rgba(245,216,138,.35)" stroke-width="1.1" stroke-linecap="round"/></svg>
      </div>
      <h3>Higher expectations</h3>
      <p>Students demand faster access to services and more relevant, personalised support.</p>
    </div>
  </div>
</section>

<!-- S4 — STARTING POINT -->
<section class="slide" id="s4">
  <p class="eyebrow anim-el">Foundation</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="slide-h2 anim-el">Our starting point</h2>
  <div class="pillars">
    <div class="pillar anim-card" style="color:var(--purple)">
      <div class="p-num" style="color:var(--purple)">01</div>
      <h3>Collaboration with PlaceNet</h3>
      <p>Built on an established network with deep local expertise and internationalization ambitions.</p>
    </div>
    <div class="pillar anim-card" style="color:var(--orange)">
      <div class="p-num" style="color:var(--orange)">02</div>
      <h3>Location-based technology</h3>
      <p>Leveraging geospatial tools to connect people to nearby services and communities.</p>
    </div>
    <div class="pillar anim-card" style="color:var(--yellow)">
      <div class="p-num" style="color:var(--yellow)">03</div>
      <h3>Explore a new opportunity</h3>
      <p>Identifying an unmet market gap at the intersection of EdTech and student mobility.</p>
    </div>
  </div>
</section>

<!-- S5 — OPPORTUNITY -->
<section class="slide" id="s5">
  <p class="eyebrow anim-el">Problem &amp; Opportunity</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="slide-h2 anim-el">The European academic market</h2>
  <div class="opp-grid">
    <div class="opp-card issue anim-card">
      <div class="olabel">The issue</div>
      <ul>
        <li>Students struggle with info access</li>
        <li>Institutions face fragmented tooling</li>
        <li>No clear dominant platform</li>
      </ul>
    </div>
    <div class="opp-card opp anim-card">
      <div class="olabel">Opportunity</div>
      <ul>
        <li>PlaceNet&#8217;s internationalization ambitions</li>
        <li>Digitalization &amp; EdTech trends</li>
        <li>Rising international student mobility</li>
      </ul>
    </div>
    <div class="opp-card need anim-card">
      <div class="olabel">Need</div>
      <ul>
        <li>B2B &#8212; institutions need better tools</li>
        <li>B2C &#8212; students need simplicity</li>
        <li>Centralised platform for Europe</li>
      </ul>
    </div>
  </div>
  <div class="s5-bar anim-el"><strong>Uniplace</strong> &#8212; a centralised platform for international students across Europe, bridging student and institutional needs.</div>
</section>

<!-- S6 — METHODOLOGY -->
<section class="slide" id="s6">
  <p class="eyebrow anim-el">Approach</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="slide-h2 anim-el">Methodology</h2>
  <div class="approach-row anim-el">
    <div class="approach-chip">
      <div class="a-tag a-seq">SEQ</div>
      <div><p>Sequential approach</p><span>Structure &amp; stepwise validation to start</span></div>
    </div>
    <div class="approach-chip">
      <div class="a-tag a-agi">AGI</div>
      <div><p>Agile approach</p><span>Flexibility &amp; rapid iterations in development</span></div>
    </div>
  </div>
  <div class="method-grid">
    <div class="m-card anim-card">
      <div class="m-tag" style="color:var(--purple)">Strategic</div>
      <h3>Key milestones</h3>
      <ul>
        <li>Academic expansion scenarios</li>
        <li>SWOT, PESTEL, market study</li>
        <li>Beta test</li>
      </ul>
    </div>
    <div class="m-card anim-card">
      <div class="m-tag" style="color:var(--orange)">Data</div>
      <h3>Collection</h3>
      <ul>
        <li>Primary: student surveys</li>
        <li>Secondary: Eurostat, OECD</li>
        <li>Beta: 10&#8211;15 students</li>
      </ul>
    </div>
    <div class="m-card anim-card">
      <div class="m-tag" style="color:var(--yellow)">PM tools</div>
      <h3>Project mgmt</h3>
      <ul>
        <li>Monday.com, Google Workspace</li>
        <li>WBS, Gantt, PERT, RACI</li>
      </ul>
    </div>
    <div class="m-card anim-card">
      <div class="m-tag" style="color:var(--teal)">Testing</div>
      <h3>Validation</h3>
      <ul>
        <li>Continuous client feedback</li>
        <li>Beta testing each phase</li>
      </ul>
    </div>
  </div>
</section>

<!-- S7 — KEY INSIGHTS -->
<section class="slide" id="s7">
  <p class="eyebrow anim-el">Research findings</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="slide-h2 anim-el" style="margin-bottom:clamp(10px,2vh,16px)">Key insights</h2>

  <!-- Tab bar -->
  <div class="ins-tabs anim-el">
    <button class="ins-tab t-student active-tab" onclick="switchTab('student')">&#128100; Student Survey</button>
    <button class="ins-tab t-b2b"     onclick="switchTab('b2b')">&#127970; B2B Survey</button>
    <button class="ins-tab t-beta"    onclick="switchTab('beta')">&#9881;&#65039; Beta Test</button>
  </div>

  <!-- ── PANEL 1: Student Survey ── -->
  <div class="ins-panel active-panel" id="panel-student">
    <div class="ins-survey-header ish-student">
      <div class="ish-icon">
        <svg width="18" height="18" viewBox="0 0 20 20" fill="none"><circle cx="10" cy="7" r="4" stroke="#a8a5f5" stroke-width="1.4"/><path d="M3 17c0-3.31 3.13-6 7-6s7 2.69 7 6" stroke="#a8a5f5" stroke-width="1.4" stroke-linecap="round"/></svg>
      </div>
      <div class="ish-meta">
        <div class="ish-label">B2C · Student Survey</div>
        <div class="ish-title">International Student Experience &amp; Arrival Survey</div>
        <div class="ish-subtitle">Pain points across arrival, admin, social integration &amp; daily life</div>
      </div>
      <div class="ish-n">n ≈ 60 responses</div>
    </div>
    <div class="ins-findings">
      <div class="ins-finding if-s">
        <div class="if-tag">Admin &amp; info overload</div>
        <h4>Administrative chaos on arrival</h4>
        <p>The most cited missing information before arrival was administrative procedures — students lacked step-by-step guides and ended up spending too much time figuring out basics alone.</p>
      </div>
      <div class="ins-finding if-s">
        <div class="if-tag">Tool fragmentation</div>
        <h4>Too many apps, still no clear solution</h4>
        <p>Students juggled Google Maps, transport apps, translate tools, and university portals — yet many confirmed they still felt lost. No single hub brought it all together.</p>
      </div>
      <div class="ins-finding if-s">
        <div class="if-tag">Social isolation</div>
        <h4>Making local friends was the hardest part</h4>
        <p>Over 60% rated making local (non-international) friends as "hard" or "very hard". Language barriers, cultural differences, and lack of structured events were the top blockers.</p>
      </div>
      <div class="ins-finding if-s">
        <div class="if-tag">Mental health</div>
        <h4>Loneliness &amp; homesickness are real</h4>
        <p>A significant portion of students reported feeling lonely "often" or "always". Most said they would want access to mental health support or a buddy/mentor during their stay.</p>
      </div>
      <div class="ins-finding if-s">
        <div class="if-tag">Dream feature</div>
        <h4>One hub to rule them all</h4>
        <p>When asked what they wished existed, students consistently described a single app combining admin tracking, campus map, events, local tips, and peer connection — exactly what Uniplace addresses.</p>
      </div>
      <div class="ins-finding if-s">
        <div class="if-tag">Daily life tasks</div>
        <h4>Grocery, admin &amp; healthcare hardest solo</h4>
        <p>Administrative tasks, seeing a doctor, grocery shopping and phone plan setup were the most cited challenges. Students lost "some" to "a lot of" time navigating these without guidance.</p>
      </div>
    </div>
    <div class="ins-bridge">
      <span class="ins-bridge-label">Features informed ↗</span>
      <div class="ins-bridge-tags">
        <span class="ib-tag ib-p">Admin step tracker</span>
        <span class="ib-tag ib-p">Centralised info hub</span>
        <span class="ib-tag ib-o">Event discovery</span>
        <span class="ib-tag ib-o">Buddy / mentor matching</span>
        <span class="ib-tag ib-y">Local services map</span>
        <span class="ib-tag ib-t">Mental health support</span>
      </div>
    </div>
  </div>

  <!-- ── PANEL 2: B2B Survey ── -->
  <div class="ins-panel" id="panel-b2b">
    <div class="ins-survey-header ish-b2b">
      <div class="ish-icon">
        <svg width="18" height="18" viewBox="0 0 20 20" fill="none"><rect x="3" y="4" width="14" height="12" rx="2" stroke="#ffb899" stroke-width="1.4"/><path d="M7 4V3a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v1" stroke="#ffb899" stroke-width="1.3"/><path d="M3 9h14" stroke="#ffb899" stroke-width="1.1" stroke-linecap="round"/></svg>
      </div>
      <div class="ish-meta">
        <div class="ish-label">B2B · Institutional Survey</div>
        <div class="ish-title">International Student Integration &amp; Services — University Responses</div>
        <div class="ish-subtitle">Challenges, tool gaps, and platform appetite across European institutions</div>
      </div>
      <div class="ish-n">n = 26 institutions</div>
    </div>
    <div class="ins-findings">
      <div class="ins-finding if-b">
        <div class="if-tag">Communication gap</div>
        <h4>Students don't read the information provided</h4>
        <p>Across institutions, the most common complaint: emails, PDFs and web guides go unread. Multiple staff noted students send repeat emails asking for information already published.</p>
      </div>
      <div class="ins-finding if-b">
        <div class="if-tag">Fragmented tooling</div>
        <h4>No integrated communication stack</h4>
        <p>Most institutions rely on email + website + PDF. Only a minority use a dedicated platform, and those that do (MoveOn, Guidebook, Mobility Online) report partial satisfaction at best.</p>
      </div>
      <div class="ins-finding if-b">
        <div class="if-tag">Top pain points</div>
        <h4>Admin complexity &amp; info dispersion dominate</h4>
        <p>Administrative complexity and information dispersion were cited by the overwhelming majority as primary integration difficulties — directly mirroring what students themselves reported.</p>
      </div>
      <div class="ins-finding if-b">
        <div class="if-tag">Platform appetite</div>
        <h4>Strong interest — with clear hesitations</h4>
        <p>Over 70% said they would be interested or open to a centralised digital platform. Main concerns: data protection, cost, implementation complexity, and staff training overhead.</p>
      </div>
      <div class="ins-finding if-b">
        <div class="if-tag">Desired features</div>
        <h4>Centralised hub &amp; direct comms top the list</h4>
        <p>The most requested features were a centralised information hub, direct communication channels with students, event calendar, and administrative procedure support — all core Uniplace pillars.</p>
      </div>
      <div class="ins-finding if-b">
        <div class="if-tag">Strategic priority</div>
        <h4>Integration improvement is on institutions' radar</h4>
        <p>A majority rated improving international student integration as a "strategic priority" or "medium" priority for the next 2 years, indicating strong institutional motivation to act.</p>
      </div>
    </div>
    <div class="ins-bridge">
      <span class="ins-bridge-label">Features informed ↗</span>
      <div class="ins-bridge-tags">
        <span class="ib-tag ib-p">Centralised info hub</span>
        <span class="ib-tag ib-p">Direct student comms</span>
        <span class="ib-tag ib-o">Event calendar</span>
        <span class="ib-tag ib-o">Admin procedures module</span>
        <span class="ib-tag ib-y">B2B dashboard</span>
        <span class="ib-tag ib-t">GDPR-compliant architecture</span>
      </div>
    </div>
  </div>

  <!-- ── PANEL 3: Beta Test ── -->
  <div class="ins-panel" id="panel-beta">
    <div class="ins-survey-header ish-beta">
      <div class="ish-icon">
        <svg width="18" height="18" viewBox="0 0 20 20" fill="none"><rect x="4" y="3" width="12" height="14" rx="2" stroke="#7de8c0" stroke-width="1.4"/><path d="M7 7h6M7 10h4M7 13h3" stroke="#7de8c0" stroke-width="1.1" stroke-linecap="round"/></svg>
      </div>
      <div class="ish-meta">
        <div class="ish-label">Validation · Beta Test Survey</div>
        <div class="ish-title">User Testing — Beta Prototype of the Application</div>
        <div class="ish-subtitle">First impressions, usability, feature feedback &amp; priorities from prototype testers</div>
      </div>
      <div class="ish-n">n = 10 testers</div>
    </div>
    <div class="ins-findings">
      <div class="ins-finding if-t">
        <div class="if-tag">First impression</div>
        <h4>Clean, intuitive first impression</h4>
        <p>The majority praised the design as clean, minimal and visually pleasant. Testers used words like "épuré", "sobre", "instinctif" — the aesthetic direction resonated immediately.</p>
      </div>
      <div class="ins-finding if-t">
        <div class="if-tag">Usability concern</div>
        <h4>Onboarding gap — no guide or tutorial</h4>
        <p>Several testers noted the app felt clear but lacked an introductory walkthrough. Without onboarding, some missed features entirely or found certain sections confusing on first use.</p>
      </div>
      <div class="ins-finding if-t">
        <div class="if-tag">Favourite feature</div>
        <h4>Chat &amp; events drove the most excitement</h4>
        <p>The in-app chat (peer-to-peer messaging) and event discovery were the standout hits. Testers valued the social connection angle — it directly validated the integration thesis.</p>
      </div>
      <div class="ins-finding if-t">
        <div class="if-tag">Feature request</div>
        <h4>Teacher &amp; administrative info missing</h4>
        <p>One tester specifically asked for a directory of professors and their offices. Others flagged the need for quick-access links to institutional resources (timetables, sport, internships).</p>
      </div>
      <div class="ins-finding if-t">
        <div class="if-tag">Technical issues</div>
        <h4>Bugs around locked features &amp; event timing</h4>
        <p>Locked/upcoming features caused confusion. The 7-day max lead time for event posting was flagged as restrictive. A profile picture bug required re-login to resolve.</p>
      </div>
      <div class="ins-finding if-t">
        <div class="if-tag">Priority feedback</div>
        <h4>Simplicity above all — don't over-complicate</h4>
        <p>The clearest mandate: keep it simple and focus on chat, useful links, and events. Testers warned against feature bloat and highlighted that ease of use must come first.</p>
      </div>
    </div>
    <div class="ins-bridge">
      <span class="ins-bridge-label">Next priorities ↗</span>
      <div class="ins-bridge-tags">
        <span class="ib-tag ib-t">Onboarding tutorial</span>
        <span class="ib-tag ib-t">Bug fixes (profile, display)</span>
        <span class="ib-tag ib-p">More events &amp; earlier posting</span>
        <span class="ib-tag ib-o">Faculty &amp; office directory</span>
        <span class="ib-tag ib-y">Feature unlock flow</span>
      </div>
    </div>
  </div>
</section>

<!-- S8 — SOLUTION -->
<section class="slide" id="s8">
  <p class="eyebrow anim-el">The product</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="slide-h2 anim-el">Solution</h2>
  <div class="sol-grid">
    <div class="sol-card anim-card">
      <div class="s-label" style="color:var(--purple)">Purpose</div>
      <h3>Why it exists</h3>
      <ul><li>Help students integrate into university &amp; city life</li><li>Support daily activities and social connections</li></ul>
    </div>
    <div class="sol-card anim-card">
      <div class="s-label" style="color:var(--orange)">Key features</div>
      <h3>What it does</h3>
      <ul><li>Personalised profiles by interests &amp; status</li><li>Geolocation: discover nearby places &amp; services</li><li>Real-time student interactions</li></ul>
    </div>
    <div class="sol-card anim-card">
      <div class="s-label" style="color:var(--yellow)">User experience</div>
      <h3>How it feels</h3>
      <ul><li>Home screen: start point &amp; profile access</li><li>Suggestions tailored to each user</li><li>Social &amp; location-based connectivity</li></ul>
    </div>
  </div>
</section>

<!-- S9 — RECOMMENDATIONS -->
<section class="slide" id="s9">
  <p class="eyebrow anim-el">Next steps</p>
  <div class="accent-bar anim-el" style="background:var(--yellow)"></div>
  <h2 class="slide-h2 anim-el">Recommendations &amp; impact</h2>
  <div class="reco-grid">
    <div class="reco-card rc1 anim-card">
      <div class="r-num">01</div><h3>UX research</h3>
      <div class="r-sub">Build an intuitive, relevant, easy-to-use app.</div>
      <ul><li>User journey mapping &amp; usability testing</li><li>Interviews with students &amp; institutions</li></ul>
    </div>
    <div class="reco-card rc2 anim-card">
      <div class="r-num">02</div><h3>Gamification</h3>
      <div class="r-sub">Create an interactive, rewarding experience.</div>
      <ul><li>Progress tracking for admin steps</li><li>Rewards, badges &amp; missions</li></ul>
    </div>
    <div class="reco-card rc3 anim-card">
      <div class="r-num">03</div><h3>Product improvement</h3>
      <div class="r-sub">Prioritise features that avoid confusion.</div>
      <ul><li>Onboarding tutorial &amp; guided navigation</li><li>Admin support &amp; info centralisation</li></ul>
    </div>
    <div class="reco-card rc4 anim-card">
      <div class="r-num">04</div><h3>B2B partnerships</h3>
      <div class="r-sub">Define a clear go-to-market strategy.</div>
      <ul><li>Prioritise target institutions</li><li>Value proposition &amp; data protection</li></ul>
    </div>
  </div>
  <div class="impact-row anim-el"><strong>Strong foundation for an impactful solution</strong> &#8212; built on research, real user needs, and a scalable B2B/B2C model.</div>
</section>

<!-- S10 — THANK YOU -->
<section class="slide" id="s10">
  <div class="ring-pulse r1"></div>
  <div class="ring-pulse r2"></div>
  <div class="ty-top">
    <p class="ty-eyebrow anim-el">Uniplace &#8212; from lost to settled</p>
    <div class="ty-bar anim-el"></div>
    <div class="ty-h1 anim-el">Thank<br>you<span class="dot">.</span></div>
    <p class="ty-tagline anim-el">We believe every international student deserves a smooth start. Let&#8217;s build it together.</p>
  </div>
  <div class="ty-mid anim-el">
    <div class="ty-stat p"><div class="val">B2B + B2C</div><div class="lbl">Dual market model</div></div>
    <div class="ty-stat o"><div class="val">Europe-wide</div><div class="lbl">Target geography</div></div>
    <div class="ty-stat y"><div class="val">EdTech + Geo</div><div class="lbl">Core technology</div></div>
  </div>
  <div class="ty-footer anim-el">
    <div class="ty-logo">uni<span>place</span></div>
    <div class="ty-contacts">
      <div class="ty-contact"><div class="tc-name">Capucine Montaudon</div><div class="tc-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b4d7d5c4c1d7dddad1f4c1daddc4d8d5d7d19ad1c1">[email&#160;protected]</a></div></div>
      <div class="ty-contact"><div class="tc-name">Lauriane Uhlenbusch</div><div class="tc-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="86eae7f3f4efe7e8e3c6f3e8eff6eae7e5e3a8e3f3">[email&#160;protected]</a></div></div>
      <div class="ty-contact"><div class="tc-name">Cedric Obeid</div><div class="tc-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c4a7a1a0b6ada784b1aaadb4a8a5a7a1eaa1b1">[email&#160;protected]</a></div></div>
    </div>
    <div class="ty-copy">uniplace.eu<br>Confidential &#183; 2025</div>
  </div>
</section>

</div>

<script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script><script>
(function () {
  var TOTAL = 10;
  var current = 0;
  var busy = false;

  var slides = document.querySelectorAll('.slide');
  var bar    = document.getElementById('progress-bar');
  var counter= document.getElementById('slide-counter');
  var title  = document.getElementById('slide-title');

  var TITLES = ['Cover','The Challenge','Environment','Foundation','Opportunity','Methodology','Key Insights','Solution','Recommendations','Thank You'];
  var LIGHT  = [1, 3, 5, 7]; // 0-indexed — light background slides

  /* ── init ── */
  slides[0].classList.add('active');
  updateUI(0);

  /* ── helpers ── */
  function pad(n) { return n < 10 ? '0' + n : '' + n; }

  function updateUI(idx) {
    bar.style.width = (idx / (TOTAL - 1) * 100) + '%';
    counter.textContent = pad(idx + 1) + ' / ' + TOTAL;
    title.textContent = TITLES[idx];
    counter.style.color = LIGHT.indexOf(idx) !== -1 ? 'rgba(0,1,34,0.3)' : 'rgba(252,252,252,0.22)';
  }

  function goTo(idx, dir) {
    if (busy || idx === current || idx < 0 || idx >= TOTAL) return;
    dir = dir || (idx > current ? 'fwd' : 'bwd');
    var prev = current;
    current = idx;
    busy = true;

    var pS = slides[prev];
    var nS = slides[idx];

    nS.classList.add(dir === 'fwd' ? 'enter-from-right' : 'enter-from-left');
    nS.classList.add('active');
    void nS.offsetWidth; /* force reflow */
    pS.classList.add('animating');
    nS.classList.add('animating');
    pS.classList.add(dir === 'fwd' ? 'exit-to-left' : 'exit-to-right');
    nS.classList.remove('enter-from-right', 'enter-from-left');

    var done = false;
    function finish() {
      if (done) return;
      done = true;
      pS.classList.remove('active', 'animating', 'exit-to-left', 'exit-to-right');
      nS.classList.remove('animating');
      busy = false;
    }
    nS.addEventListener('transitionend', finish, { once: true });
    setTimeout(finish, 700);
    updateUI(idx);
  }

  function next() { goTo(current + 1, 'fwd'); }
  function prev() { goTo(current - 1, 'bwd'); }

  /* ── keyboard ── */
  document.addEventListener('keydown', function (e) {
    if (e.key === 'ArrowRight' || e.key === 'ArrowDown' || e.key === ' ') { e.preventDefault(); next(); }
    else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') { e.preventDefault(); prev(); }
    else if (e.key === 'Home') { goTo(0); }
    else if (e.key === 'End')  { goTo(TOTAL - 1); }
  });

  /* ── touch swipe ── */
  var tx = 0, ty = 0, moved = false;
  document.addEventListener('touchstart', function (e) {
    tx = e.touches[0].clientX;
    ty = e.touches[0].clientY;
    moved = false;
  }, { passive: true });
  document.addEventListener('touchmove', function () { moved = true; }, { passive: true });
  document.addEventListener('touchend', function (e) {
    if (!moved) return;
    var dx = e.changedTouches[0].clientX - tx;
    var dy = e.changedTouches[0].clientY - ty;
    if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 44) {
      dx < 0 ? next() : prev();
    }
  }, { passive: true });

  /* ── insight tab switching ── */
  window.switchTab = function(which) {
    var tabs   = document.querySelectorAll('.ins-tab');
    var panels = document.querySelectorAll('.ins-panel');
    tabs.forEach(function(t) {
      t.classList.toggle('active-tab',
        (which === 'student' && t.classList.contains('t-student')) ||
        (which === 'b2b'     && t.classList.contains('t-b2b'))     ||
        (which === 'beta'    && t.classList.contains('t-beta'))
      );
    });
    panels.forEach(function(p) {
      p.classList.toggle('active-panel', p.id === 'panel-' + which);
    });
    // reset scroll of s7
    var s7 = document.getElementById('s7');
    if (s7) s7.scrollTop = 0;
  };

  /* ── mouse wheel (throttled) ── */
  var wTimer, lastW = 0;
  document.addEventListener('wheel', function(e) {
    var now = Date.now();
    if (now - lastW < 600) return;
    if (Math.abs(e.deltaY) < 20) return;
    lastW = now;
    e.deltaY > 0 ? next() : prev();
  }, { passive: true });

  /* ── click navigation zones ── */
  document.getElementById('deck').addEventListener('click', function(e) {
    var s7 = document.getElementById('s7');
    if (s7 && s7.classList.contains('active')) return; // don't nav on insight slide clicks
    var x = e.clientX / window.innerWidth;
    if (x > 0.6) next();
    else if (x < 0.4) prev();
  });

})();
</script>
</body>
</html>
