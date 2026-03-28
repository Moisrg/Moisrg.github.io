<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PROJECT ZERO — HELIX Corp</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=IBM+Plex+Mono:wght@400;700&family=Crimson+Text:ital,wght@0,400;0,600;1,400;1,600&display=swap" rel="stylesheet">
<style>
  /* ══════════════════════════════════════
     RESET & BASE
  ══════════════════════════════════════ */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --black:    #080C10;
    --ink:      #0D1219;
    --panel:    #111820;
    --card:     #161E28;
    --border:   #1E2D3D;
    --line:     #2A3D52;

    --white:    #F0F4FF;
    --dim:      #8899AA;
    --muted:    #4A5668;

    --cyan:     #00C8F0;
    --cyan-dim: #007A99;
    --gold:     #E8A820;
    --gold-dim: #7A5810;
    --red:      #FF2E2E;
    --red-dim:  #7A1010;
    --green:    #00E87A;
    --green-dim:#006633;
    --violet:   #A855F7;
    --violet-dim:#4A1880;

    --font-display: 'Bebas Neue', sans-serif;
    --font-mono:    'IBM Plex Mono', monospace;
    --font-body:    'Crimson Text', serif;
  }

  html { scroll-behavior: smooth; }

  body {
    background: var(--black);
    color: var(--white);
    font-family: var(--font-body);
    font-size: 18px;
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* Scanlines overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.15) 2px,
      rgba(0,0,0,0.15) 4px
    );
    pointer-events: none;
    z-index: 1000;
    opacity: 0.4;
  }

  /* ══════════════════════════════════════
     ANIMATIONS
  ══════════════════════════════════════ */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes glitch {
    0%,100% { clip-path: inset(0 0 100% 0); }
    10%  { clip-path: inset(10% 0 80% 0); transform: translateX(-4px); }
    20%  { clip-path: inset(40% 0 50% 0); transform: translateX(4px); }
    30%  { clip-path: inset(70% 0 20% 0); transform: translateX(-2px); }
    40%  { clip-path: inset(20% 0 70% 0); transform: translateX(2px); }
    50%  { clip-path: inset(0 0 0 0);     transform: translateX(0); }
  }
  @keyframes scanIn {
    from { opacity: 0; transform: scaleX(0); transform-origin: left; }
    to   { opacity: 1; transform: scaleX(1); }
  }
  @keyframes pulse {
    0%,100% { opacity: 1; }
    50%     { opacity: 0.4; }
  }
  @keyframes flicker {
    0%,95%,100% { opacity: 1; }
    96%          { opacity: 0.6; }
    98%          { opacity: 0.8; }
  }

  .anim-up  { animation: fadeUp 0.7s ease both; }
  .d1 { animation-delay: 0.1s; }
  .d2 { animation-delay: 0.25s; }
  .d3 { animation-delay: 0.4s; }
  .d4 { animation-delay: 0.55s; }
  .d5 { animation-delay: 0.7s; }

  /* ══════════════════════════════════════
     TOP BAR — una línea, semitransparente
  ══════════════════════════════════════ */
  .topbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 400;
    background: rgba(80, 6, 6, 0.55);
    border-bottom: 1px solid rgba(255, 46, 46, 0.35);
    padding: 0 10px;
    display: flex;
    align-items: center;
    gap: 7px;
    height: 26px;
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    animation: flicker 8s infinite;
    overflow: hidden;
  }
  .topbar-dot {
    width: 5px;
    height: 5px;
    background: var(--red);
    border-radius: 50%;
    flex-shrink: 0;
    animation: pulse 1.5s infinite;
  }
  .topbar-msg {
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    color: rgba(255, 200, 200, 0.9);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    white-space: nowrap;
    overflow: hidden;
    flex: 1;
    min-width: 0;
  }

  /* ══════════════════════════════════════
     HERO / PORTADA
  ══════════════════════════════════════ */
  .hero {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    padding: 28px 20px 28px;
    position: relative;
    overflow: hidden;
  }

  /* Grid background */
  .hero::after {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(var(--border) 1px, transparent 1px),
      linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 40px 40px;
    opacity: 0.25;
    pointer-events: none;
  }

  .hero-corp-wrap {
    margin-bottom: 16px;
    animation: fadeUp 0.5s ease both;
  }

  .hero-corp {
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.15em;
    color: var(--cyan);
    text-transform: uppercase;
    display: block;
    line-height: 1.8;
    word-break: break-word;
  }

  .hero-title-wrap {
    position: relative;
    z-index: 1;
  }

  .hero-project {
    font-family: var(--font-display);
    font-size: clamp(48px, 15vw, 140px);
    line-height: 0.9;
    color: var(--white);
    letter-spacing: 0.02em;
    white-space: nowrap;
    animation: fadeUp 0.6s ease 0.1s both;
  }

  .hero-zero {
    font-family: var(--font-display);
    font-size: clamp(48px, 15vw, 140px);
    line-height: 0.9;
    color: var(--cyan);
    letter-spacing: 0.02em;
    display: block;
    white-space: nowrap;
    position: relative;
    animation: fadeUp 0.6s ease 0.2s both;
    text-shadow: 0 0 40px rgba(0, 200, 240, 0.4);
  }

  .hero-zero::after {
    content: 'ZERO';
    position: absolute;
    left: 2px; top: 2px;
    color: var(--red);
    opacity: 0.3;
    animation: glitch 6s infinite 3s;
    pointer-events: none;
  }

  .hero-divider {
    height: 3px;
    background: linear-gradient(90deg, var(--cyan), transparent);
    margin: 20px 0;
    animation: scanIn 0.8s ease 0.4s both;
  }

  .hero-sub {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--dim);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    animation: fadeUp 0.6s ease 0.5s both;
    margin-bottom: 20px;
    overflow: hidden;
    white-space: nowrap;
  }
  .hero-sub-inner {
    display: inline-block;
    animation: tickerMove 12s linear infinite;
    padding-right: 60px;
  }
  @keyframes tickerMove {
    0%   { transform: translateX(0); }
    100% { transform: translateX(-50%); }
  }

  /* Metadata cards */
  .meta-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
    animation: fadeUp 0.6s ease 0.6s both;
    margin-bottom: 0;
  }
  .meta-cell {
    background: var(--card);
    border: 1px solid var(--border);
    padding: 10px 14px;
  }
  .meta-label {
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    color: var(--cyan);
    letter-spacing: 0.18em;
    text-transform: uppercase;
    display: block;
    margin-bottom: 3px;
  }
  .meta-value {
    font-family: var(--font-mono);
    font-size: 14px;
    font-weight: 700;
    color: var(--white);
  }

  .hero-warning {
    display: none;
  }

  /* ══════════════════════════════════════
     NAV / ÍNDICE
  ══════════════════════════════════════ */
  .nav {
    background: var(--ink);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    padding: 20px;
  }
  .nav-title {
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    color: var(--muted);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 14px;
  }
  .nav-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 4px;
  }
  .nav-list .nav-btn {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 8px 12px;
    background: var(--panel);
    border: 1px solid var(--border);
    width: 100%;
    cursor: pointer;
    text-align: left;
    transition: all 0.2s;
  }
  .nav-list .nav-btn:hover, .nav-list .nav-btn:active {
    border-color: var(--cyan);
    background: var(--card);
  }
  .nav-num {
    font-family: var(--font-mono);
    font-size: 11px;
    font-weight: 700;
    color: var(--cyan);
    width: 24px;
    flex-shrink: 0;
  }
  .nav-text {
    font-family: var(--font-mono);
    font-size: 12px;
    color: var(--dim);
    letter-spacing: 0.05em;
  }

  /* ══════════════════════════════════════
     SECCIONES
  ══════════════════════════════════════ */
  .section {
    padding: 0 0 60px;
    border-bottom: 1px solid var(--border);
  }

  /* Chapter header */
  .chapter-header {
    margin-bottom: 28px;
  }
  .chapter-code {
    display: block;
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    color: var(--cyan);
    letter-spacing: 0.2em;
    padding: 20px 20px 6px;
  }
  .chapter-band {
    background: var(--white);
    padding: 14px 20px 12px;
  }
  .chapter-title {
    font-family: var(--font-display);
    font-size: clamp(44px, 13vw, 72px);
    line-height: 0.9;
    color: var(--black);
    letter-spacing: 0.02em;
  }
  .chapter-sub-band {
    padding: 8px 20px;
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    color: #fff;
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }
  .chapter-body { padding: 0 20px; }

  /* Accent colors por capítulo */
  .accent-cyan  .chapter-sub-band { background: var(--cyan-dim); }
  .accent-red   .chapter-sub-band { background: var(--red-dim); }
  .accent-green .chapter-sub-band { background: var(--green-dim); }
  .accent-violet .chapter-sub-band { background: var(--violet-dim); }
  .accent-gold  .chapter-sub-band { background: var(--gold-dim); }

  /* Section tag */
  .stag {
    display: flex;
    align-items: center;
    gap: 10px;
    margin: 24px 0 12px;
  }
  .stag-bar {
    width: 4px;
    height: 18px;
    flex-shrink: 0;
    border-radius: 2px;
  }
  .stag-text {
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
  }
  .accent-cyan  .stag-bar { background: var(--cyan); }
  .accent-cyan  .stag-text { color: var(--cyan); }
  .accent-red   .stag-bar { background: var(--red); }
  .accent-red   .stag-text { color: var(--red); }
  .accent-green .stag-bar { background: var(--green); }
  .accent-green .stag-text { color: var(--green); }
  .accent-violet .stag-bar { background: var(--violet); }
  .accent-violet .stag-text { color: var(--violet); }
  .accent-gold  .stag-bar { background: var(--gold); }
  .accent-gold  .stag-text { color: var(--gold); }

  /* Body text */
  .body-text {
    font-family: var(--font-body);
    font-size: 17px;
    line-height: 1.75;
    color: #BCC8D8;
    margin-bottom: 14px;
  }
  .body-text.bold { color: var(--white); font-weight: 600; }
  .body-text.danger { color: var(--red); font-weight: 600; }

  /* Bullets */
  .dot-list { list-style: none; display: flex; flex-direction: column; gap: 6px; margin-bottom: 16px; }
  .dot-list li {
    display: flex;
    gap: 10px;
    align-items: flex-start;
    padding: 10px 14px;
    background: var(--panel);
    border: 1px solid var(--border);
    border-left: 3px solid var(--cyan);
  }
  .dot-mark {
    font-family: var(--font-mono);
    font-size: 11px;
    font-weight: 700;
    color: var(--cyan);
    flex-shrink: 0;
    margin-top: 2px;
  }
  .dot-main {
    font-family: var(--font-body);
    font-size: 16px;
    color: var(--white);
    font-weight: 600;
  }
  .dot-detail {
    font-family: var(--font-body);
    font-size: 15px;
    color: var(--dim);
    display: block;
    font-weight: 400;
  }

  /* Stamp / classified banner */
  .stamp {
    border-left: 4px solid var(--red);
    border-top: 1px solid var(--red-dim);
    border-bottom: 1px solid var(--red-dim);
    border-right: 1px solid var(--red-dim);
    background: rgba(122,16,16,0.15);
    padding: 14px 16px;
    margin: 20px 0;
    text-align: center;
  }
  .stamp-text {
    font-family: var(--font-mono);
    font-size: 11px;
    font-weight: 700;
    color: var(--red);
    letter-spacing: 0.12em;
    text-transform: uppercase;
    line-height: 1.5;
  }
  .stamp.violet {
    border-color: var(--violet);
    background: rgba(74,24,128,0.15);
  }
  .stamp.violet .stamp-text { color: var(--violet); }

  /* Label-Value pairs */
  .lv-list { display: flex; flex-direction: column; gap: 1px; margin-bottom: 16px; }
  .lv-item {
    display: flex;
    flex-direction: column;
    padding: 10px 14px;
    background: var(--panel);
    border-bottom: 1px solid var(--border);
  }
  .lv-label {
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    margin-bottom: 2px;
  }
  .lv-value {
    font-family: var(--font-body);
    font-size: 16px;
    color: var(--white);
  }
  .accent-green .lv-label { color: var(--green); }
  .accent-violet .lv-label { color: var(--violet); }

  /* Pull quote */
  .pull-quote {
    margin: 28px 0;
    padding: 22px 20px;
    position: relative;
  }
  .pull-quote::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
  }
  .pull-quote::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 3px;
  }
  .accent-gold .pull-quote::before,
  .accent-gold .pull-quote::after  { background: var(--gold); }
  .accent-red  .pull-quote::before,
  .accent-red  .pull-quote::after  { background: var(--red); }
  .accent-green .pull-quote::before,
  .accent-green .pull-quote::after { background: var(--green); }
  .accent-violet .pull-quote::before,
  .accent-violet .pull-quote::after{ background: var(--violet); }

  .pull-quote-mark {
    font-family: var(--font-display);
    font-size: 60px;
    line-height: 0.6;
    display: block;
    margin-bottom: 8px;
  }
  .accent-gold .pull-quote-mark   { color: var(--gold); }
  .accent-red  .pull-quote-mark   { color: var(--red); }
  .accent-green .pull-quote-mark  { color: var(--green); }
  .accent-violet .pull-quote-mark { color: var(--violet); }

  .pull-quote-text {
    font-family: var(--font-body);
    font-size: 20px;
    font-style: italic;
    font-weight: 600;
    color: var(--white);
    line-height: 1.5;
  }
  .pull-quote-attr {
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.15em;
    margin-top: 10px;
    display: block;
  }
  .accent-gold .pull-quote-attr   { color: var(--gold); }
  .accent-red  .pull-quote-attr   { color: var(--red); }
  .accent-green .pull-quote-attr  { color: var(--green); }
  .accent-violet .pull-quote-attr { color: var(--violet); }

  /* ══════════════════════════════════════
     FICHA DE PERSONAJE
  ══════════════════════════════════════ */
  .char-card {
    padding: 18px 20px;
    margin-bottom: 20px;
    border-bottom: 2px solid;
  }
  .accent-green .char-card  { background: rgba(0,104,51,0.15); border-color: var(--green); }
  .accent-violet .char-card { background: rgba(74,24,128,0.15); border-color: var(--violet); }

  .char-name {
    font-family: var(--font-display);
    font-size: clamp(38px, 11vw, 60px);
    line-height: 0.9;
    margin-bottom: 6px;
  }
  .accent-green .char-name  { color: var(--green); }
  .accent-violet .char-name { color: var(--violet); }

  .char-role {
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.15em;
    color: var(--dim);
    text-transform: uppercase;
  }

  /* ══════════════════════════════════════
     TABLA S-GEN
  ══════════════════════════════════════ */
  .sgen-table { margin: 20px 0; }
  .sgen-header {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
    margin-bottom: 2px;
  }
  .sgen-hdr {
    padding: 12px 14px;
    font-family: var(--font-mono);
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    text-align: center;
  }
  .sgen-hdr.green { background: var(--green-dim); color: var(--green); }
  .sgen-hdr.red   { background: var(--red-dim);   color: var(--red); }

  .sgen-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
    margin-bottom: 2px;
  }
  .sgen-cell {
    padding: 12px 14px;
    border: 1px solid var(--border);
    display: flex;
    gap: 8px;
    align-items: flex-start;
  }
  .sgen-cell.green { background: rgba(0,104,51,0.08); border-left: 3px solid var(--green-dim); }
  .sgen-cell.red   { background: rgba(122,16,16,0.08); border-left: 3px solid var(--red-dim); }
  .sgen-marker {
    font-family: var(--font-mono);
    font-size: 12px;
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 1px;
  }
  .sgen-cell.green .sgen-marker { color: var(--green); }
  .sgen-cell.red   .sgen-marker { color: var(--red); }
  .sgen-cell-text {
    font-family: var(--font-body);
    font-size: 14px;
    line-height: 1.4;
  }
  .sgen-cell.green .sgen-cell-text { color: #AAEEC8; }
  .sgen-cell.red   .sgen-cell-text { color: #FFAAAA; }

  /* ══════════════════════════════════════
     TABLA VS
  ══════════════════════════════════════ */
  .vs-table { margin: 20px 0; }
  .vs-header {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 2px;
    margin-bottom: 2px;
  }
  .vs-hdr {
    padding: 10px 8px;
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    text-align: center;
    color: var(--white);
  }
  .vs-hdr.dim    { background: #1A1A1A; color: var(--muted); }
  .vs-hdr.green  { background: var(--green-dim); }
  .vs-hdr.violet { background: var(--violet-dim); }

  .vs-row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 2px;
    margin-bottom: 2px;
  }
  .vs-cell {
    padding: 10px 8px;
    border: 1px solid var(--border);
    font-family: var(--font-body);
    font-size: 13px;
    text-align: center;
    line-height: 1.3;
  }
  .vs-cell.label  { background: var(--panel); color: var(--dim); font-family: var(--font-mono); font-size: 10px; font-weight: 700; letter-spacing: 0.1em; }
  .vs-cell.green  { background: rgba(0,104,51,0.1); color: #AAEEC8; }
  .vs-cell.violet { background: rgba(74,24,128,0.1); color: #CCAAFF; }
  .vs-cell.danger { background: rgba(122,16,16,0.1); color: #FFAAAA; }
  .vs-row.total .vs-cell.green  { background: var(--green-dim); color: var(--white); font-weight: 600; }
  .vs-row.total .vs-cell.violet { background: var(--violet-dim); color: var(--white); font-weight: 600; }
  .vs-row.total .vs-cell.label  { background: #1A1A1A; }

  /* ══════════════════════════════════════
     SEPARADOR
  ══════════════════════════════════════ */
  .sep {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 24px 20px;
  }
  .sep-line { flex: 1; height: 1px; background: var(--border); }
  .sep-text {
    font-family: var(--font-mono);
    font-size: 9px;
    font-weight: 700;
    color: var(--muted);
    letter-spacing: 0.2em;
  }

  /* ══════════════════════════════════════
     FOOTER
  ══════════════════════════════════════ */
  .footer {
    background: var(--ink);
    border-top: 3px solid var(--white);
    padding: 40px 20px 60px;
    text-align: center;
  }
  .footer-title {
    font-family: var(--font-display);
    font-size: clamp(36px, 11vw, 60px);
    color: var(--white);
    margin-bottom: 6px;
  }
  .footer-sub {
    font-family: var(--font-body);
    font-style: italic;
    font-size: 16px;
    color: var(--dim);
    margin-bottom: 24px;
  }
  .footer-line {
    width: 60px;
    height: 3px;
    background: var(--cyan);
    margin: 0 auto 20px;
  }
  .footer-meta {
    font-family: var(--font-mono);
    font-size: 9px;
    color: var(--muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    line-height: 2;
  }

  /* ══════════════════════════════════════
     TABLET — 600px a 1023px
  ══════════════════════════════════════ */
  @media (min-width: 600px) {

    .topbar { padding: 6px 32px; }
    .topbar-msg { font-size: 10px; }

    .hero {
 
