# uniplacenet
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { overflow: hidden; }

body {
  font-family: var(--font);
  background: var(--dark);
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  cursor: none;
}

/* ── CURSOR ── */
#cursor {
  position: fixed;
  width: 10px; height: 10px;
  background: var(--purple);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%);
  transition: transform 0.1s, width 0.3s var(--ease-spring), height 0.3s var(--ease-spring), background 0.3s;
  mix-blend-mode: screen;
}
#cursor-ring {
  position: fixed;
  width: 36px; height: 36px;
  border: 1.5px solid rgba(99,95,229,0.5);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9998;
  transform: translate(-50%, -50%);
  transition: transform 0.12s var(--ease-out), width 0.3s var(--ease-spring), height 0.3s var(--ease-spring), border-color 0.3s;
}
body:has(a:hover) #cursor, body:has(button:hover) #cursor { width: 20px; height: 20px; }

/* ── SLIDE CONTAINER ── */
#deck {
  width: 100vw;
  height: 100vh;
  position: relative;
}

.slide {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0; left: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 60px 72px;
  opacity: 0;
  pointer-events: none;
  overflow: hidden;
  transition: none;
}

.slide.active {
  opacity: 1;
  pointer-events: all;
}

/* ── TRANSITION STATES ── */
.slide.enter-from-right { transform: translateX(60px); opacity: 0; }
.slide.enter-from-left  { transform: translateX(-60px); opacity: 0; }
.slide.exit-to-left     { transform: translateX(-60px); opacity: 0; }
.slide.exit-to-right    { transform: translateX(60px); opacity: 0; }

.slide.animating {
  transition: transform 0.65s var(--ease-out), opacity 0.65s var(--ease-out);
}
.slide.active.animating {
  transform: translateX(0) !important;
  opacity: 1 !important;
}

/* ── SLIDE LABEL ── */
.slide-label {
  position: absolute; top: 28px; right: 40px;
  font-size: 10px; letter-spacing: .18em;
  text-transform: uppercase; font-weight: 600;
}

/* ── NAV ── */
#nav {
  position: fixed;
  bottom: 28px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
  display: flex;
  align-items: center;
  gap: 14px;
  background: rgba(255,255,255,0.06);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 0.5px solid rgba(255,255,255,0.1);
  border-radius: 100px;
  padding: 10px 20px;
}

.nav-dots-wrap {
  display: flex;
  gap: 6px;
  align-items: center;
}

.dot {
  width: 5px; height: 5px;
  border-radius: 50%;
  background: rgba(252,252,252,0.25);
  transition: width 0.3s var(--ease-spring), background 0.3s, border-radius 0.3s;
  cursor: pointer;
}
.dot.active-dot {
  width: 20px;
  border-radius: 3px;
  background: var(--purple);
}

.nav-btn {
  background: none;
  border: none;
  cursor: pointer;
  width: 28px; height: 28px;
  display: flex; align-items: center; justify-content: center;
  border-radius: 50%;
  transition: background 0.2s;
  color: rgba(252,252,252,0.5);
  padding: 0;
}
.nav-btn:hover { background: rgba(255,255,255,0.1); color: white; }
.nav-btn svg { width: 14px; height: 14px; }

/* ── SLIDE COUNTER ── */
#slide-counter {
  position: fixed;
  top: 28px;
  left: 72px;
  font-size: 10px;
  letter-spacing: .14em;
  text-transform: uppercase;
  font-weight: 600;
  color: rgba(252,252,252,0.25);
  z-index: 100;
  transition: color 0.4s;
}

/* ── PROGRESS BAR ── */
#progress-bar {
  position: fixed;
  top: 0; left: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--purple), var(--orange));
  z-index: 100;
  transition: width 0.5s var(--ease-out);
  border-radius: 0 2px 2px 0;
}

/* ── ANIMATED ELEMENTS (stagger on slide entry) ── */
.anim-el {
  opacity: 0;
  transform: translateY(18px);
  transition: opacity 0.55s var(--ease-out), transform 0.55s var(--ease-out);
}
.slide.active .anim-el {
  opacity: 1;
  transform: translateY(0);
}
.anim-el:nth-child(1) { transition-delay: 0.08s; }
.anim-el:nth-child(2) { transition-delay: 0.16s; }
.anim-el:nth-child(3) { transition-delay: 0.24s; }
.anim-el:nth-child(4) { transition-delay: 0.32s; }
.anim-el:nth-child(5) { transition-delay: 0.40s; }
.anim-el:nth-child(6) { transition-delay: 0.48s; }
.anim-el:nth-child(7) { transition-delay: 0.56s; }

/* custom delays for card grids */
.anim-card { opacity: 0; transform: translateY(24px) scale(0.97); transition: opacity 0.5s var(--ease-out), transform 0.5s var(--ease-out); }
.slide.active .anim-card { opacity: 1; transform: translateY(0) scale(1); }
.anim-card:nth-child(1) { transition-delay: 0.3s; }
.anim-card:nth-child(2) { transition-delay: 0.42s; }
.anim-card:nth-child(3) { transition-delay: 0.54s; }
.anim-card:nth-child(4) { transition-delay: 0.66s; }

/* ── HOVER CARD LIFT ── */
.hoverable {
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
}
.hoverable:hover {
  transform: translateY(-3px) scale(1.01);
  box-shadow: 0 12px 40px rgba(0,0,0,0.25);
}

/* ══ SLIDE 1 — COVER ══ */
#s1 { background: var(--dark); color: var(--white); }
#s1 .slide-label { color: rgba(252,252,252,.3); }

.ring-pulse {
  position: absolute;
  border-radius: 50%;
  border: 1px solid;
  pointer-events: none;
  animation: ring-breathe 6s ease-in-out infinite;
}
.r1 { width: 480px; height: 480px; top: -180px; right: -80px; border-color: rgba(99,95,229,.22); animation-delay: 0s; }
.r2 { width: 300px; height: 300px; top: -80px; right: 40px; border-color: rgba(99,95,229,.14); animation-delay: 2s; }
.r3 { width: 600px; height: 600px; bottom: -300px; left: -180px; border-color: rgba(255,144,82,.12); animation-delay: 4s; }

@keyframes ring-breathe {
  0%, 100% { opacity: 0.6; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.03); }
}

#s1 h1 {
  font-size: clamp(64px, 9vw, 96px);
  font-weight: 800;
  letter-spacing: -.04em;
  line-height: .92;
  margin-bottom: 18px;
}
#s1 h1 .pu { color: var(--purple); }
#s1 .sub {
  font-size: 13px; letter-spacing: .1em; text-transform: uppercase;
  color: rgba(252,252,252,.35); margin-bottom: 56px; font-weight: 500;
}
.authors { display: flex; gap: 24px; flex-wrap: wrap; }
.author-chip { display: flex; align-items: center; gap: 10px; }
.av {
  width: 32px; height: 32px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 10px; font-weight: 700; flex-shrink: 0;
  letter-spacing: .02em;
  transition: transform 0.3s var(--ease-spring);
}
.av:hover { transform: scale(1.2); }
.av-p { background: rgba(99,95,229,.25); color: #a8a5f5; }
.av-o { background: rgba(255,144,82,.22); color: #ffb899; }
.av-y { background: rgba(239,192,65,.22); color: #f5d88a; }
.author-name { font-size: 12px; color: rgba(252,252,252,.55); font-weight: 400; }

.eyebrow {
  font-size: 10px; letter-spacing: .2em; text-transform: uppercase;
  font-weight: 700; margin-bottom: 14px;
}
.accent-bar {
  width: 40px; height: 3px; border-radius: 2px; margin-bottom: 22px;
  transition: width 0.6s var(--ease-spring);
}
.slide.active .accent-bar { width: 52px; }

/* ══ SLIDE 2 — PROBLEM ══ */
#s2 { background: var(--light); color: var(--dark); }
#s2 .slide-label { color: rgba(0,1,34,.3); }
#s2 .eyebrow { color: var(--purple); }
#s2 h2 {
  font-size: clamp(36px, 5.5vw, 56px); font-weight: 800;
  letter-spacing: -.03em; line-height: 1.05; margin-bottom: 14px;
}
#s2 h2 em { font-style: normal; color: var(--purple); }
#s2 .desc { font-size: 14px; color: rgba(0,1,34,.5); max-width: 400px; line-height: 1.6; }
#s2 .visual {
  position: absolute; right: 72px; top: 50%; transform: translateY(-50%);
  width: 300px; height: 188px; background: var(--dark); border-radius: 16px;
  overflow: hidden; display: flex; align-items: center; justify-content: center;
  box-shadow: 0 24px 60px rgba(0,0,0,0.18);
  transition: transform 0.4s var(--ease-spring), box-shadow 0.4s;
}
#s2 .visual:hover { transform: translateY(-50%) scale(1.03) rotate(-1deg); box-shadow: 0 32px 80px rgba(0,0,0,0.25); }
#s2 .visual svg { width: 100%; height: 100%; }

/* animated SVG figure */
.figure-body { animation: figure-sway 4s ease-in-out infinite; transform-origin: 146px 154px; }
@keyframes figure-sway {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(3deg); }
  75% { transform: rotate(-3deg); }
}
.question-mark { animation: question-float 2.5s ease-in-out infinite; }
@keyframes question-float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-6px); }
}
.grid-line { stroke-dasharray: 4; animation: grid-march 3s linear infinite; }
@keyframes grid-march {
  from { stroke-dashoffset: 0; }
  to { stroke-dashoffset: 20; }
}

/* ══ SLIDE 3 — ENVIRONMENT ══ */
#s3 { background: var(--dark); color: var(--white); }
#s3 .slide-label { color: rgba(252,252,252,.3); }
#s3 .eyebrow { color: var(--orange); }
#s3 h2 { font-size: clamp(30px, 4.5vw, 46px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 40px; }

.cards3 { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 14px; max-width: 780px; }
.card3 {
  background: rgba(252,252,252,.05);
  border: .5px solid rgba(252,252,252,.08);
  border-radius: 16px;
  padding: 28px 22px;
  position: relative;
  overflow: hidden;
  transition: background 0.3s, border-color 0.3s, transform 0.3s var(--ease-spring);
}
.card3::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.04), transparent 70%);
  opacity: 0;
  transition: opacity 0.3s;
}
.card3:hover::before { opacity: 1; }
.card3:hover { transform: translateY(-4px); border-color: rgba(252,252,252,.15); }
.c3-icon {
  width: 42px; height: 42px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center; margin-bottom: 16px;
  transition: transform 0.3s var(--ease-spring);
}
.card3:hover .c3-icon { transform: scale(1.15) rotate(-5deg); }
.card3 h3 { font-size: 13px; font-weight: 700; margin-bottom: 6px; line-height: 1.3; }
.card3 p { font-size: 11.5px; color: rgba(252,252,252,.45); line-height: 1.55; }

/* ══ SLIDE 4 — STARTING POINT ══ */
#s4 { background: var(--light); color: var(--dark); }
#s4 .slide-label { color: rgba(0,1,34,.3); }
#s4 .eyebrow { color: var(--orange); }
#s4 h2 { font-size: clamp(30px, 4.5vw, 46px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 40px; }

.pillars { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 14px; max-width: 780px; }
.pillar {
  background: var(--white);
  border-radius: 16px;
  border: .5px solid rgba(0,1,34,.07);
  padding: 28px 22px;
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}
.pillar::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 3px;
  background: linear-gradient(90deg, transparent, currentColor, transparent);
  transform: scaleX(0);
  transition: transform 0.4s var(--ease-out);
}
.pillar:hover::after { transform: scaleX(1); }
.pillar:hover { transform: translateY(-4px); box-shadow: 0 16px 48px rgba(0,0,0,0.1); }
.p-num { font-size: 32px; font-weight: 800; line-height: 1; margin-bottom: 12px; }
.pillar h3 { font-size: 13px; font-weight: 700; color: var(--dark); margin-bottom: 6px; line-height: 1.3; }
.pillar p { font-size: 11.5px; color: rgba(0,1,34,.5); line-height: 1.55; }

/* ══ SLIDE 5 — OPPORTUNITY ══ */
#s5 { background: var(--dark); color: var(--white); }
#s5 .slide-label { color: rgba(252,252,252,.3); }
#s5 .eyebrow { color: rgba(99,95,229,.8); }
#s5 h2 { font-size: clamp(28px, 4vw, 42px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 32px; }

.opp-grid { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 14px; max-width: 820px; }
.opp-card {
  border-radius: 14px;
  padding: 22px 20px;
  transition: transform 0.3s var(--ease-spring);
  cursor: default;
}
.opp-card:hover { transform: translateY(-4px) scale(1.01); }
.opp-card.issue { background: rgba(99,95,229,.12); border: .5px solid rgba(99,95,229,.3); }
.opp-card.opp   { background: rgba(255,144,82,.10); border: .5px solid rgba(255,144,82,.25); }
.opp-card.need  { background: rgba(239,192,65,.09); border: .5px solid rgba(239,192,65,.22); }
.opp-card .olabel { font-size: 9px; letter-spacing: .15em; text-transform: uppercase; font-weight: 700; margin-bottom: 8px; }
.issue .olabel { color: #a8a5f5; }
.opp .olabel   { color: #ffb899; }
.need .olabel  { color: #f5d88a; }
.opp-card h3 { font-size: 13px; font-weight: 700; margin-bottom: 10px; }
.opp-card li {
  font-size: 11px; color: rgba(252,252,252,.5); line-height: 1.55;
  list-style: none; padding-left: 12px; position: relative; margin-bottom: 3px;
}
.opp-card li::before { content: '—'; position: absolute; left: 0; opacity: .4; font-size: 10px; }
.s5-bar {
  max-width: 820px; margin-top: 18px;
  background: rgba(252,252,252,.05); border: .5px solid rgba(252,252,252,.1);
  border-radius: 8px; padding: 12px 18px;
  font-size: 12px; color: rgba(252,252,252,.5);
  position: relative; overflow: hidden;
}
.s5-bar::before {
  content: '';
  position: absolute;
  top: 0; left: 0;
  width: 3px; height: 100%;
  background: linear-gradient(180deg, var(--purple), var(--orange));
  border-radius: 3px 0 0 3px;
}
.s5-bar strong { color: var(--white); font-weight: 700; }

/* ══ SLIDE 6 — METHODOLOGY ══ */
#s6 { background: var(--light); color: var(--dark); }
#s6 .slide-label { color: rgba(0,1,34,.3); }
#s6 .eyebrow { color: var(--orange); }
#s6 h2 { font-size: clamp(28px, 4vw, 42px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 26px; }

.approach-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; max-width: 820px; margin-bottom: 14px; }
.approach-chip {
  background: var(--white); border-radius: 12px;
  border: .5px solid rgba(0,1,34,.07); padding: 14px 18px;
  display: flex; align-items: center; gap: 14px;
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
}
.approach-chip:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(0,0,0,0.08); }
.a-tag {
  width: 38px; height: 38px; border-radius: 8px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
  font-size: 9px; font-weight: 800; letter-spacing: .04em;
}
.a-seq { background: rgba(99,95,229,.12); color: var(--purple); }
.a-agi { background: rgba(255,144,82,.12); color: var(--orange); }
.approach-chip p { font-size: 12px; font-weight: 700; color: var(--dark); }
.approach-chip span { font-size: 10.5px; color: rgba(0,1,34,.45); display: block; margin-top: 2px; }

.method-grid { display: grid; grid-template-columns: repeat(4, minmax(0,1fr)); gap: 12px; max-width: 820px; }
.m-card {
  background: var(--white); border-radius: 12px;
  border: .5px solid rgba(0,1,34,.07); padding: 18px 16px;
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}
.m-card:hover { transform: translateY(-3px); box-shadow: 0 12px 32px rgba(0,0,0,0.09); }
.m-tag { font-size: 9px; letter-spacing: .13em; text-transform: uppercase; font-weight: 700; margin-bottom: 8px; }
.m-card h3 { font-size: 12px; font-weight: 700; color: var(--dark); margin-bottom: 8px; }
.m-card li {
  font-size: 10.5px; color: rgba(0,1,34,.5); line-height: 1.55;
  list-style: none; padding-left: 10px; position: relative; margin-bottom: 3px;
}
.m-card li::before { content: '·'; position: absolute; left: 2px; font-size: 14px; line-height: 1; }

/* ══ SLIDE 7 — KEY INSIGHTS ══ */
#s7 { background: var(--dark); color: var(--white); }
#s7 .slide-label { color: rgba(252,252,252,.3); }
#s7 .eyebrow { color: rgba(99,95,229,.8); }
#s7 h2 { font-size: clamp(28px, 4vw, 42px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 28px; }

.insights { display: flex; flex-direction: column; gap: 12px; max-width: 740px; }
.insight {
  border-radius: 14px; padding: 20px 24px;
  display: flex; gap: 20px; align-items: flex-start;
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
  cursor: default;
  position: relative;
  overflow: hidden;
}
.insight::after {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(255,255,255,0.02);
  opacity: 0;
  transition: opacity 0.3s;
}
.insight:hover::after { opacity: 1; }
.insight:hover { transform: translateX(6px); }
.i1 { background: rgba(99,95,229,.12); border: .5px solid rgba(99,95,229,.28); }
.i2 { background: rgba(255,144,82,.09); border: .5px solid rgba(255,144,82,.22); }
.i3 { background: rgba(239,192,65,.09); border: .5px solid rgba(239,192,65,.2); }
.i-num { font-size: 26px; font-weight: 800; flex-shrink: 0; width: 34px; line-height: 1; padding-top: 2px; }
.i1 .i-num { color: #a8a5f5; }
.i2 .i-num { color: #ffb899; }
.i3 .i-num { color: #f5d88a; }
.i-body h3 { font-size: 13px; font-weight: 700; margin-bottom: 3px; }
.i-hl { font-size: 11.5px; color: rgba(252,252,252,.5); font-style: italic; margin-bottom: 4px; }
.i-body p { font-size: 11px; color: rgba(252,252,252,.4); line-height: 1.5; }

/* ══ SLIDE 8 — SOLUTION ══ */
#s8 { background: var(--light); color: var(--dark); }
#s8 .slide-label { color: rgba(0,1,34,.3); }
#s8 .eyebrow { color: var(--purple); }
#s8 h2 { font-size: clamp(28px, 4vw, 42px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 32px; }

.sol-grid { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 14px; max-width: 820px; }
.sol-card {
  background: var(--white); border-radius: 16px;
  border: .5px solid rgba(0,1,34,.07); padding: 24px 20px;
  transition: transform 0.3s var(--ease-spring), box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}
.sol-card:hover { transform: translateY(-5px) scale(1.01); box-shadow: 0 20px 56px rgba(0,0,0,0.12); }
.sol-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  opacity: 0;
  transition: opacity 0.3s;
}
.sol-card:nth-child(1)::before { background: var(--purple); }
.sol-card:nth-child(2)::before { background: var(--orange); }
.sol-card:nth-child(3)::before { background: var(--yellow); }
.sol-card:hover::before { opacity: 1; }
.s-label { font-size: 9px; letter-spacing: .14em; text-transform: uppercase; font-weight: 700; margin-bottom: 8px; }
.sol-card h3 { font-size: 13px; font-weight: 700; color: var(--dark); margin-bottom: 10px; }
.sol-card li {
  font-size: 11px; color: rgba(0,1,34,.5); line-height: 1.55;
  list-style: none; padding-left: 12px; position: relative; margin-bottom: 4px;
}
.sol-card li::before { content: '·'; position: absolute; left: 2px; font-size: 16px; line-height: .9; }

/* ══ SLIDE 9 — RECOMMENDATIONS ══ */
#s9 { background: var(--dark); color: var(--white); }
#s9 .slide-label { color: rgba(252,252,252,.3); }
#s9 .eyebrow { color: #f5d88a; }
#s9 h2 { font-size: clamp(26px, 3.8vw, 40px); font-weight: 800; letter-spacing: -.03em; margin-bottom: 28px; }

.reco-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; max-width: 820px; }
.reco-card {
  background: rgba(252,252,252,.05); border: .5px solid rgba(252,252,252,.08);
  border-radius: 14px; padding: 20px 20px;
  transition: transform 0.3s var(--ease-spring), background 0.3s;
}
.reco-card:hover { transform: translateY(-3px); background: rgba(252,252,252,.08); }
.r-num { font-size: 28px; font-weight: 800; line-height: 1; margin-bottom: 10px; }
.rc1 .r-num { color: #a8a5f5; }
.rc2 .r-num { color: #ffb899; }
.rc3 .r-num { color: #f5d88a; }
.rc4 .r-num { color: var(--teal); }
.reco-card h3 { font-size: 13px; font-weight: 700; margin-bottom: 4px; }
.reco-card .r-sub { font-size: 11px; color: rgba(252,252,252,.45); margin-bottom: 8px; }
.reco-card li {
  font-size: 10.5px; color: rgba(252,252,252,.4); line-height: 1.55;
  list-style: none; padding-left: 12px; position: relative; margin-bottom: 3px;
}
.reco-card li::before { content: '—'; position: absolute; left: 0; opacity: .4; font-size: 9px; top: 2px; }
.impact-row {
  max-width: 820px; margin-top: 16px;
  background: rgba(252,252,252,.05); border: .5px solid rgba(252,252,252,.09);
  border-radius: 8px; padding: 12px 18px;
  font-size: 12px; color: rgba(252,252,252,.45);
  position: relative; overflow: hidden;
}
.impact-row::before {
  content: '';
  position: absolute;
  top: 0; left: 0;
  width: 3px; height: 100%;
  background: linear-gradient(180deg, #f5d88a, var(--teal));
}
.impact-row strong { color: var(--white); font-weight: 700; }

/* ══ SLIDE 10 — THANK YOU ══ */
#s10 {
  background: var(--dark); color: var(--white);
  justify-content: space-between;
  padding-top: 52px; padding-bottom: 44px;
}
#s10 .slide-label { color: rgba(252,252,252,.3); }
#s10 .ring-pulse { border-color: rgba(99,95,229,.18); }

.ty-top { display: flex; flex-direction: column; }
.ty-eyebrow { font-size: 10px; letter-spacing: .2em; text-transform: uppercase; font-weight: 700; color: rgba(99,95,229,.7); margin-bottom: 14px; }
.ty-bar { width: 36px; height: 3px; background: var(--purple); border-radius: 2px; margin-bottom: 24px; transition: width 0.8s var(--ease-spring); }
#s10.active .ty-bar { width: 72px; }
.ty-h1 {
  font-size: clamp(68px, 10vw, 110px); font-weight: 800;
  letter-spacing: -.05em; line-height: .88; margin-bottom: 22px;
}
.ty-h1 .dot { color: var(--purple); }
.ty-tagline { font-size: 14px; color: rgba(252,252,252,.4); max-width: 460px; line-height: 1.6; font-weight: 400; }

.ty-mid {
  display: flex; gap: 48px; align-items: center;
  padding: 28px 0;
  border-top: .5px solid rgba(252,252,252,.08);
  border-bottom: .5px solid rgba(252,252,252,.08);
}
.ty-stat .val { font-size: 16px; font-weight: 800; }
.ty-stat .lbl { font-size: 10px; color: rgba(252,252,252,.3); margin-top: 3px; letter-spacing: .05em; }
.ty-stat.p .val { color: var(--purple); }
.ty-stat.o .val { color: var(--orange); }
.ty-stat.y .val { color: var(--yellow); }

.ty-footer { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px; }
.ty-contacts { display: flex; gap: 32px; flex-wrap: wrap; }
.ty-contact .tc-name { font-size: 12px; font-weight: 600; color: rgba(252,252,252,.7); }
.ty-contact .tc-email { font-size: 10.5px; color: rgba(252,252,252,.3); margin-top: 2px; }
.ty-copy { font-size: 10px; color: rgba(252,252,252,.2); text-align: right; line-height: 1.7; }

/* ── KEYBOARD HINT ── */
#kbd-hint {
  position: fixed;
  bottom: 70px;
  right: 32px;
  font-size: 9px;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: rgba(252,252,252,0.18);
  display: flex;
  align-items: center;
  gap: 6px;
  transition: opacity 0.5s;
  pointer-events: none;
}
#kbd-hint kbd {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px; height: 20px;
  border: 1px solid rgba(255,255,255,0.12);
  border-radius: 4px;
  font-size: 10px;
  font-family: var(--font);
}

/* ── TOOLTIP / SLIDE TITLE HINT ── */
#slide-title {
  position: fixed;
  top: 28px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 10px;
  letter-spacing: .14em;
  text-transform: uppercase;
  font-weight: 600;
  color: rgba(252,252,252,0.22);
  z-index: 100;
  white-space: nowrap;
  transition: opacity 0.4s;
}

/* Counter for light slides */
.slide-label-dark { color: rgba(0,1,34,.3); }
.slide-label-light { color: rgba(252,252,252,.3); }

/* number counter anim */
.count-anim {
  display: inline-block;
  transition: transform 0.3s var(--ease-spring), opacity 0.3s;
}
</style>
</head>
<body>

<!-- cursor -->
<div id="cursor"></div>
<div id="cursor-ring"></div>

<!-- progress bar -->
<div id="progress-bar"></div>

<!-- slide counter -->
<div id="slide-counter">01 / 10</div>

<!-- slide title -->
<div id="slide-title"></div>

<!-- keyboard hint -->
<div id="kbd-hint">
  <kbd>←</kbd><kbd>→</kbd> navigate &nbsp; <kbd>ESC</kbd> overview
</div>

<!-- nav -->
<div id="nav">
  <button class="nav-btn" id="btn-prev" title="Previous" aria-label="Previous slide">
    <svg viewBox="0 0 14 14" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9 2L4 7l5 5"/></svg>
  </button>
  <div class="nav-dots-wrap" id="nav-dots"></div>
  <button class="nav-btn" id="btn-next" title="Next" aria-label="Next slide">
    <svg viewBox="0 0 14 14" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M5 2l5 5-5 5"/></svg>
  </button>
</div>

<!-- ══ DECK ══ -->
<div id="deck">

<!-- SLIDE 1 — COVER -->
<section class="slide" id="s1">
  <div class="ring-pulse r1"></div>
  <div class="ring-pulse r2"></div>
  <div class="ring-pulse r3"></div>
  <span class="slide-label" style="color:rgba(252,252,252,.25)">01 / 10</span>

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

<!-- SLIDE 2 — PROBLEM -->
<section class="slide" id="s2">
  <span class="slide-label slide-label-dark">02 / 10</span>
  <p class="eyebrow anim-el">The challenge</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="anim-el">Arriving in a new country…<br><em>Where do you start?</em></h2>
  <p class="desc anim-el" style="margin-top:14px">International students face a maze of unknowns from the moment they land — admin, housing, social integration.</p>

  <div class="visual anim-el">
    <svg viewBox="0 0 300 188" xmlns="http://www.w3.org/2000/svg">
      <rect width="300" height="188" fill="#000122"/>
      <!-- city grid lines -->
      <line class="grid-line" x1="40" y1="52" x2="260" y2="52" stroke="rgba(99,95,229,0.2)" stroke-width="0.5"/>
      <line class="grid-line" x1="40" y1="70" x2="260" y2="70" stroke="rgba(99,95,229,0.14)" stroke-width="0.5"/>
      <line class="grid-line" x1="40" y1="88" x2="100" y2="88" stroke="rgba(99,95,229,0.14)" stroke-width="0.5"/>
      <line class="grid-line" x1="200" y1="88" x2="260" y2="88" stroke="rgba(99,95,229,0.14)" stroke-width="0.5"/>
      <!-- figure -->
      <g class="figure-body">
        <rect x="134" y="108" width="24" height="46" rx="7" fill="#12153a"/>
        <circle cx="146" cy="100" r="12" fill="#12153a"/>
        <rect x="158" y="135" width="13" height="18" rx="3" fill="#0a0d28"/>
        <line x1="165" y1="133" x2="165" y2="137" stroke="#635fe5" stroke-width="1.5"/>
        <ellipse cx="150" cy="178" rx="100" ry="9" fill="rgba(99,95,229,0.08)"/>
      </g>
      <!-- question mark floating -->
      <g class="question-mark">
        <text x="230" y="54" font-size="28" font-family="sans-serif" fill="rgba(255,144,82,0.75)" font-weight="700">?</text>
      </g>
    </svg>
  </div>
</section>

<!-- SLIDE 3 — ENVIRONMENT -->
<section class="slide" id="s3">
  <span class="slide-label" style="color:rgba(252,252,252,.25)">03 / 10</span>
  <p class="eyebrow anim-el">Context</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="anim-el">A changing environment</h2>
  <div class="cards3">
    <div class="card3 anim-card hoverable">
      <div class="c3-icon" style="background:rgba(99,95,229,.18)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><circle cx="10" cy="10" r="7.5" stroke="#a8a5f5" stroke-width="1.4"/><path d="M10 2.5C7.5 5.5 7.5 14.5 10 17.5M10 2.5C12.5 5.5 12.5 14.5 10 17.5" stroke="#a8a5f5" stroke-width="1.1" fill="none"/><line x1="2.5" y1="10" x2="17.5" y2="10" stroke="#a8a5f5" stroke-width="1.1"/><path d="M13 5.5l2.5 2.5-2.5-.8z" fill="#a8a5f5"/></svg>
      </div>
      <h3>Global student mobility</h3>
      <p>International enrollment growing steadily across Europe year over year.</p>
    </div>
    <div class="card3 anim-card hoverable">
      <div class="c3-icon" style="background:rgba(255,144,82,.18)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><rect x="2" y="4" width="16" height="12" rx="2" stroke="#ffb899" stroke-width="1.4"/><path d="M6 8h8M6 11h5" stroke="#ffb899" stroke-width="1.1" stroke-linecap="round"/><circle cx="15" cy="4" r="3" fill="#ff9052"/><path d="M14 4h2M15 3v2" stroke="white" stroke-width=".8" stroke-linecap="round"/></svg>
      </div>
      <h3>Digital tools everywhere</h3>
      <p>New generation expects seamless, app-first experiences at every touchpoint.</p>
    </div>
    <div class="card3 anim-card hoverable">
      <div class="c3-icon" style="background:rgba(239,192,65,.16)">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none"><path d="M10 17V7" stroke="#f5d88a" stroke-width="1.4" stroke-linecap="round"/><path d="M6 13l4 4 4-4" stroke="#f5d88a" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/><path d="M4 11V5M8 11V3M12 11V6M16 11V2" stroke="rgba(245,216,138,.35)" stroke-width="1.1" stroke-linecap="round"/></svg>
      </div>
      <h3>Higher expectations</h3>
      <p>Students demand faster access to services and more relevant, personalised support.</p>
    </div>
  </div>
</section>

<!-- SLIDE 4 — STARTING POINT -->
<section class="slide" id="s4">
  <span class="slide-label slide-label-dark">04 / 10</span>
  <p class="eyebrow anim-el">Foundation</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="anim-el">Our starting point</h2>
  <div class="pillars">
    <div class="pillar anim-card" style="color:var(--purple)">
      <div class="p-num" style="color:var(--purple)">01</div>
      <h3>Collaboration with PlaceNet</h3>
      <p>Built on an established network with deep local expertise and internationalization ambitions.</p>
    </div>
    <div class="pillar anim-card" style="color:var(--orange)">
      <div class="p-num" style="color:var(--orange)">02</div>
      <h3>Location-based technology</h3>
      <p>Leveraging geospatial tools to connect students to nearby services and communities.</p>
    </div>
    <div class="pillar anim-card" style="color:var(--yellow)">
      <div class="p-num" style="color:var(--yellow)">03</div>
      <h3>Explore a new opportunity</h3>
      <p>Identifying an unmet market gap at the intersection of EdTech and student mobility.</p>
    </div>
  </div>
</section>

<!-- SLIDE 5 — OPPORTUNITY -->
<section class="slide" id="s5">
  <span class="slide-label" style="color:rgba(252,252,252,.25)">05 / 10</span>
  <p class="eyebrow anim-el">Problem &amp; Opportunity</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="anim-el">The European academic market</h2>
  <div class="opp-grid">
    <div class="opp-card issue anim-card">
      <div class="olabel">The issue</div>
      <h3>Analysis</h3>
      <ul><li>PlaceNet's internationalization ambitions</li><li>Digitalization &amp; EdTech trends</li><li>Rising international student mobility</li></ul>
    </div>
    <div class="opp-card opp anim-card">
      <div class="olabel">Opportunity</div>
      <h3>Growing market demand</h3>
      <ul><li>Students struggle with info access</li><li>Institutions face fragmented tooling</li><li>No clear dominant platform</li></ul>
    </div>
    <div class="opp-card need anim-card">
      <div class="olabel">Need</div>
      <h3>Dual perspective</h3>
      <ul><li>B2B — institutions need better tools</li><li>B2C — students need simplicity</li><li>Centralised platform for Europe</li></ul>
    </div>
  </div>
  <div class="s5-bar anim-el"><strong>UniPlacenet</strong> — a centralised platform for international students across Europe, bridging both student and institutional needs.</div>
</section>

<!-- SLIDE 6 — METHODOLOGY -->
<section class="slide" id="s6">
  <span class="slide-label slide-label-dark">06 / 10</span>
  <p class="eyebrow anim-el">Approach</p>
  <div class="accent-bar anim-el" style="background:var(--orange)"></div>
  <h2 class="anim-el">Methodology</h2>
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
    <div class="m-card anim-card"><div class="m-tag" style="color:var(--purple)">Strategic</div><h3>Key milestones</h3><ul><li>Academic expansion scenarios</li><li>SWOT, PESTEL, market study</li></ul></div>
    <div class="m-card anim-card"><div class="m-tag" style="color:var(--orange)">Data</div><h3>Collection</h3><ul><li>Secondary: Eurostat, OECD</li><li>Primary: student surveys</li><li>Beta: 10–15 students</li></ul></div>
    <div class="m-card anim-card"><div class="m-tag" style="color:var(--yellow)">PM tools</div><h3>Project mgmt</h3><ul><li>Monday.com, Google Workspace</li><li>WBS, Gantt, PERT, RACI</li></ul></div>
    <div class="m-card anim-card"><div class="m-tag" style="color:var(--teal)">Testing</div><h3>Validation</h3><ul><li>Continuous client feedback</li><li>Beta testing each phase</li></ul></div>
  </div>
</section>

<!-- SLIDE 7 — KEY INSIGHTS -->
<section class="slide" id="s7">
  <span class="slide-label" style="color:rgba(252,252,252,.25)">07 / 10</span>
  <p class="eyebrow anim-el">Research findings</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="anim-el">Key insights</h2>
  <div class="insights">
    <div class="insight i1 anim-card">
      <div class="i-num">01</div>
      <div class="i-body"><h3>Fragmentation</h3><p class="i-hl">Not lacking tools — lacking structure</p><p>Information exists but is scattered across disconnected platforms, making the student journey unnecessarily complex.</p></div>
    </div>
    <div class="insight i2 anim-card">
      <div class="i-num">02</div>
      <div class="i-body"><h3>Misalignment</h3><p class="i-hl">Student needs &amp; institutional tools not aligned</p><p>Students want simplicity; institutions rely on complex systems. The gap creates friction at every touchpoint.</p></div>
    </div>
    <div class="insight i3 anim-card">
      <div class="i-num">03</div>
      <div class="i-body"><h3>Opportunity</h3><p class="i-hl">Space for a centralised, user-driven solution</p><p>Strong demand and no clear dominant platform signals an open market ready for a focused product.</p></div>
    </div>
  </div>
</section>

<!-- SLIDE 8 — SOLUTION -->
<section class="slide" id="s8">
  <span class="slide-label slide-label-dark">08 / 10</span>
  <p class="eyebrow anim-el">The product</p>
  <div class="accent-bar anim-el" style="background:var(--purple)"></div>
  <h2 class="anim-el">Solution</h2>
  <div class="sol-grid">
    <div class="sol-card anim-card"><div class="s-label" style="color:var(--purple)">Purpose</div><h3>Why it exists</h3><ul><li>Help students integrate into university &amp; city life</li><li>Support daily activities and social connections</li></ul></div>
    <div class="sol-card anim-card"><div class="s-label" style="color:var(--orange)">Key features</div><h3>What it does</h3><ul><li>Personalised profiles by interests &amp; status</li><li>Geolocation: discover nearby places &amp; services</li><li>Real-time student interactions</li></ul></div>
    <div class="sol-card anim-card"><div class="s-label" style="color:var(--yellow)">User experience</div><h3>How it feels</h3><ul><li>Home screen: start point &amp; profile access</li><li>Suggestions tailored to each user</li><li>Social &amp; location-based connectivity</li></ul></div>
  </div>
</section>

<!-- SLIDE 9 — RECOMMENDATIONS -->
<section class="slide" id="s9">
  <span class="slide-label" style="color:rgba(252,252,252,.25)">09 / 10</span>
  <p class="eyebrow anim-el">Next steps</p>
  <div class="accent-bar anim-el" style="background:var(--yellow)"></div>
  <h2 class="anim-el">Recommendations &amp; impact</h2>
  <div class="reco-grid">
    <div class="reco-card rc1 anim-card"><div class="r-num">01</div><h3>UX research</h3><div class="r-sub">Build an intuitive, relevant, easy-to-use app.</div><ul><li>User journey mapping &amp; usability testing</li><li>Interviews with students &amp; institutions</li></ul></div>
    <div class="reco-card rc2 anim-card"><div class="r-num">02</div><h3>Gamification</h3><div class="r-sub">Create an interactive, rewarding experience.</div><ul><li>Progress tracking for admin steps</li><li>Rewards, badges &amp; missions</li></ul></div>
    <div class="reco-card rc3 anim-card"><div class="r-num">03</div><h3>Product improvement</h3><div class="r-sub">Prioritise features that avoid confusion.</div><ul><li>Onboarding tutorial &amp; guided navigation</li><li>Admin support &amp; info centralisation</li></ul></div>
    <div class="reco-card rc4 anim-card"><div class="r-num">04</div><h3>B2B partnerships</h3><div class="r-sub">Define a clear go-to-market strategy.</div><ul><li>Prioritise target institutions</li><li>Value proposition &amp; data protection</li></ul></div>
  </div>
  <div class="impact-row anim-el"><strong>Strong foundation for an impactful solution</strong> — built on research, real user needs, and a scalable B2B/B2C model.</div>
</section>

<!-- SLIDE 10 — THANK YOU -->
<section class="slide" id="s10">
  <div class="ring-pulse r1"></div>
  <div class="ring-pulse r2"></div>
  <span class="slide-label" style="color:rgba(252,252,252,.25)">10 / 10</span>

  <div class="ty-top">
    <p class="ty-eyebrow anim-el">Uniplace — from lost to settled</p>
    <div class="ty-bar anim-el"></div>
    <div class="ty-h1 anim-el">Thank<br>you<span class="dot">.</span></div>
    <p class="ty-tagline anim-el">We believe every international student deserves a smooth start. Let's build it together.</p>
  </div>

  <div class="ty-mid anim-el">
    <div class="ty-stat p"><div class="val">B2B + B2C</div><div class="lbl">Dual market model</div></div>
    <div class="ty-stat o"><div class="val">Europe-wide</div><div class="lbl">Target geography</div></div>
    <div class="ty-stat y"><div class="val">EdTech + Geo</div><div class="lbl">Core technology</div></div>
  </div>

  <div class="ty-footer anim-el">
    <div style="font-size:22px;font-weight:800;letter-spacing:-.03em;color:var(--white);opacity:.85">uni<span style="color:var(--purple)">place</span></div>
    <div class="ty-contacts">
      <div class="ty-contact"><div class="tc-name">Capucine Montaudon</div><div class="tc-email">capucine@uniplace.eu</div></div>
      <div class="ty-contact"><div class="tc-name">Lauriane Uhlenbusch</div><div class="tc-email">lauriane@uniplace.eu</div></div>
      <div class="ty-contact"><div class="tc-name">Cedric Obeid</div><div class="tc-email">cedric@uniplace.eu</div></div>
    </div>
    <div class="ty-copy">uniplace.eu<br>Confidential · 2025</div>
  </div>
</section>

</div><!-- /deck -->

<script>
// ── STATE ──
const TOTAL = 10;
let current = 0;
let isAnimating = false;

const slides = document.querySelectorAll('.slide');
const dotsWrap = document.getElementById('nav-dots');
const progressBar = document.getElementById('progress-bar');
const slideCounter = document.getElementById('slide-counter');
const slideTitle = document.getElementById('slide-title');

const slideTitles = [
  'Cover', 'The Challenge', 'Environment', 'Foundation',
  'Opportunity', 'Methodology', 'Key Insights', 'Solution',
  'Recommendations', 'Thank You'
];

// ── BUILD DOTS ──
for (let i = 0; i < TOTAL; i++) {
  const d = document.createElement('div');
  d.className = 'dot';
  d.addEventListener('click', () => goTo(i));
  dotsWrap.appendChild(d);
}

// ── INIT ──
function init() {
  slides[0].classList.add('active');
  updateUI(0);
}

// ── GO TO SLIDE ──
function goTo(idx, dir = null) {
  if (isAnimating || idx === current) return;
  if (idx < 0 || idx >= TOTAL) return;

  const direction = dir ?? (idx > current ? 'forward' : 'backward');
  const prev = current;
  current = idx;
  isAnimating = true;

  const prevSlide = slides[prev];
  const nextSlide = slides[idx];

  // set enter state
  nextSlide.classList.add(direction === 'forward' ? 'enter-from-right' : 'enter-from-left');
  nextSlide.classList.add('active');

  // force reflow
  nextSlide.getBoundingClientRect();

  // animate
  prevSlide.classList.add('animating');
  nextSlide.classList.add('animating');

  prevSlide.classList.add(direction === 'forward' ? 'exit-to-left' : 'exit-to-right');
  nextSlide.classList.remove('enter-from-right', 'enter-from-left');

  function onDone() {
    prevSlide.classList.remove('active', 'animating', 'exit-to-left', 'exit-to-right');
    nextSlide.classList.remove('animating');
    isAnimating = false;
  }

  nextSlide.addEventListener('transitionend', onDone, { once: true });
  setTimeout(onDone, 720); // fallback

  updateUI(idx);
}

function next() { goTo(current + 1, 'forward'); }
function prev() { goTo(current - 1, 'backward'); }

// ── UPDATE UI ──
function updateUI(idx) {
  // progress
  const pct = ((idx) / (TOTAL - 1)) * 100;
  progressBar.style.width = pct + '%';

  // counter
  slideCounter.textContent = String(idx + 1).padStart(2, '0') + ' / ' + TOTAL;

  // title
  slideTitle.textContent = slideTitles[idx];

  // dots
  const dots = dotsWrap.querySelectorAll('.dot');
  dots.forEach((d, i) => {
    d.classList.toggle('active-dot', i === idx);
  });

  // slide label counter
  document.querySelectorAll('.slide-label').forEach(el => {
    el.textContent = String(idx + 1).padStart(2, '0') + ' / ' + TOTAL;
  });
}

// ── KEYBOARD ──
document.addEventListener('keydown', e => {
  if (e.key === 'ArrowRight' || e.key === 'ArrowDown' || e.key === ' ') {
    e.preventDefault(); next();
  } else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
    e.preventDefault(); prev();
  } else if (e.key === 'Home') {
    goTo(0);
  } else if (e.key === 'End') {
    goTo(TOTAL - 1);
  }
});

// ── NAV BUTTONS ──
document.getElementById('btn-next').addEventListener('click', next);
document.getElementById('btn-prev').addEventListener('click', prev);

// ── TOUCH / SWIPE ──
let touchStartX = 0;
let touchStartY = 0;
document.addEventListener('touchstart', e => {
  touchStartX = e.touches[0].clientX;
  touchStartY = e.touches[0].clientY;
}, { passive: true });
document.addEventListener('touchend', e => {
  const dx = e.changedTouches[0].clientX - touchStartX;
  const dy = e.changedTouches[0].clientY - touchStartY;
  if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
    dx < 0 ? next() : prev();
  }
});

// ── MOUSE WHEEL ──
let wheelTimeout;
document.addEventListener('wheel', e => {
  clearTimeout(wheelTimeout);
  wheelTimeout = setTimeout(() => {
    e.deltaY > 0 ? next() : prev();
  }, 50);
}, { passive: true });

// ── CUSTOM CURSOR ──
const cursor = document.getElementById('cursor');
const cursorRing = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  cursor.style.left = mx + 'px';
  cursor.style.top = my + 'px';
});

function animRing() {
  rx += (mx - rx) * 0.12;
  ry += (my - ry) * 0.12;
  cursorRing.style.left = rx + 'px';
  cursorRing.style.top = ry + 'px';
  requestAnimationFrame(animRing);
}
animRing();

// ── HIDE HINT AFTER 6s ──
setTimeout(() => {
  document.getElementById('kbd-hint').style.opacity = '0';
}, 6000);

// ── INIT ──
init();
</script>
</body>
</html>
