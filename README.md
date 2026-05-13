# project_workifoa2026
Illustrazione project work 

<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Green Control — Mappa Interattiva BI</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root {
  --bg:       #0D1117;
  --bg2:      #161B22;
  --bg3:      #1C2128;
  --border:   #30363D;
  --border2:  #484F58;
  --text:     #F0F0EE;
  --muted:    #8B949E;
  --faint:    #3D4451;
  --green:    #1DB87A;
  --green-d:  #0A1F16;
  --amber:    #F59E0B;
  --amber-d:  #1A1500;
  --purple:   #A78BFA;
  --purple-d: #120D1E;
  --blue:     #60A5FA;
  --blue-d:   #0C1626;
  --panel-w:  440px;
  --font: 'DM Sans', sans-serif;
  --mono: 'DM Mono', monospace;
}

html,body { height:100%; background:var(--bg); font-family:var(--font); color:var(--text); overflow-x:hidden; }

/* ── LAYOUT SHELL ── */
.shell { display:flex; height:100vh; }

.map-area {
  flex:1;
  overflow:auto;
  padding:32px 40px 48px;
  transition:margin-right .35s cubic-bezier(.4,0,.2,1);
}
.map-area.shifted { margin-right:var(--panel-w); }

/* Grid bg */
.map-area::before {
  content:'';
  position:fixed; inset:0;
  background-image:
    linear-gradient(rgba(255,255,255,.025) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,.025) 1px, transparent 1px);
  background-size:40px 40px;
  pointer-events:none; z-index:0;
}

/* ── HEADER ── */
.header {
  display:flex; align-items:flex-start; justify-content:space-between;
  margin-bottom:36px; position:relative; z-index:1;
}
.logo-area { display:flex; align-items:center; gap:14px; }
.logo-dot {
  width:42px; height:42px; background:var(--green); border-radius:11px;
  display:flex; align-items:center; justify-content:center;
  font:600 18px/1 var(--mono); color:var(--bg); flex-shrink:0;
}
.project-title  { font-size:22px; font-weight:600; letter-spacing:-.4px; color:var(--text); }
.project-sub    { font-size:12px; color:var(--faint); margin-top:3px; text-transform:uppercase; letter-spacing:.6px; }
.header-hint    { font:400 11px/1.8 var(--mono); color:var(--faint); text-align:right; }
.hint-pulse     { display:inline-block; width:6px; height:6px; background:var(--green); border-radius:50%; margin-right:6px; animation:pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.4;transform:scale(.7)} }

/* ── PIPELINE GRID ── */
.pipeline {
  display:grid;
  grid-template-columns:repeat(5,220px) ;
  grid-template-columns:220px 44px 220px 44px 220px 44px 220px 44px 220px;
  align-items:start; gap:0;
  position:relative; z-index:1;
  min-width:1100px;
}

.arrow-col {
  display:flex; align-items:flex-start;
  justify-content:center; padding-top:52px;
}
.arrow-svg { width:44px; height:28px; }

/* ── LAYER ── */
.layer { display:flex; flex-direction:column; gap:7px; }
.layer-header { display:flex; align-items:center; gap:8px; margin-bottom:4px; }
.layer-num { font:500 10px/1 var(--mono); color:var(--faint); letter-spacing:1px; }
.layer-name { font-size:11px; font-weight:600; letter-spacing:.8px; text-transform:uppercase; }

/* ── NODE ── */
.node {
  border-radius:9px; padding:11px 13px;
  border:1px solid; position:relative;
  cursor:pointer;
  transition:transform .15s ease, box-shadow .15s ease, border-color .15s ease;
  user-select:none;
}
.node:hover { transform:translateY(-2px); }
.node.active { outline:2px solid; outline-offset:2px; }

.node-title { font-size:12px; font-weight:600; line-height:1.3; margin-bottom:3px; }
.node-desc  { font-size:10px; font-weight:400; line-height:1.5; opacity:.7; }
.node-tag   {
  display:inline-block; font:500 9px/1 var(--mono);
  padding:2px 7px; border-radius:4px; margin-top:6px; letter-spacing:.4px;
}
.node-click-hint {
  position:absolute; top:8px; right:10px;
  font:400 9px/1 var(--mono); opacity:.3; transition:opacity .15s;
}
.node:hover .node-click-hint { opacity:.7; }

/* ── LAYER COLORS ── */
.l1 .layer-name { color:#9CA3AF; }
.l1 .node { background:#161B22; border-color:#30363D; }
.l1 .node:hover,.l1 .node.active { border-color:#6B7280; box-shadow:0 0 0 3px rgba(156,163,175,.08); }
.l1 .node.active { outline-color:#6B7280; }
.l1 .node-title { color:#C9D1D9; }
.l1 .node-desc  { color:#8B949E; }
.l1 .node-tag   { background:#21262D; color:#8B949E; }
.l1 .node.accent{ border-color:#484F58; background:#1C2128; }

.l2 .layer-name { color:#F59E0B; }
.l2 .node { background:#1A1500; border-color:#3D2E00; }
.l2 .node:hover,.l2 .node.active { border-color:#D97706; box-shadow:0 0 0 3px rgba(245,158,11,.08); }
.l2 .node.active { outline-color:#D97706; }
.l2 .node-title { color:#FCD34D; }
.l2 .node-desc  { color:#92740A; }
.l2 .node-tag   { background:#261A00; color:#B45309; }
.l2 .node.accent{ border-color:#78350F; background:#1F1500; }

.l3 .layer-name { color:#1DB87A; }
.l3 .node { background:#0A1F16; border-color:#1A3D2B; }
.l3 .node:hover,.l3 .node.active { border-color:#059669; box-shadow:0 0 0 3px rgba(29,184,122,.08); }
.l3 .node.active { outline-color:#059669; }
.l3 .node-title { color:#6EE7B7; }
.l3 .node-desc  { color:#276446; }
.l3 .node-tag   { background:#0F2B1D; color:#059669; }
.l3 .node.accent{ border-color:#065F46; background:#0D2720; }

.l4 .layer-name { color:#A78BFA; }
.l4 .node { background:#120D1E; border-color:#2D2060; }
.l4 .node:hover,.l4 .node.active { border-color:#7C3AED; box-shadow:0 0 0 3px rgba(167,139,250,.08); }
.l4 .node.active { outline-color:#7C3AED; }
.l4 .node-title { color:#C4B5FD; }
.l4 .node-desc  { color:#5B4B8A; }
.l4 .node-tag   { background:#1A1040; color:#7C3AED; }
.l4 .node.accent{ border-color:#5B21B6; background:#170F2A; }

.l5 .layer-name { color:#60A5FA; }
.l5 .node { background:#0C1626; border-color:#1E3A5F; }
.l5 .node:hover,.l5 .node.active { border-color:#2563EB; box-shadow:0 0 0 3px rgba(96,165,250,.08); }
.l5 .node.active { outline-color:#2563EB; }
.l5 .node-title { color:#93C5FD; }
.l5 .node-desc  { color:#2D5A8E; }
.l5 .node-tag   { background:#0F1E33; color:#2563EB; }
.l5 .node.accent{ border-color:#1D4ED8; background:#101D2E; }

/* ── BOTTOM SECTIONS ── */
.bottom-row { display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-top:20px; position:relative; z-index:1; min-width:1100px; }
.bottom-section { border-radius:11px; padding:16px 18px; border:1px solid; }
.bottom-title {
  font-size:10px; font-weight:600; text-transform:uppercase; letter-spacing:.8px;
  margin-bottom:11px; display:flex; align-items:center; gap:7px;
}
.bottom-title::before { content:''; display:block; width:3px; height:13px; border-radius:2px; }
.kpi-section  { background:#0D1B0D; border-color:#1A3D1A; }
.kpi-section .bottom-title { color:#4ADE80; }
.kpi-section .bottom-title::before { background:#4ADE80; }
.auto-section { background:#120D0D; border-color:#3D1A1A; }
.auto-section .bottom-title { color:#F87171; }
.auto-section .bottom-title::before { background:#F87171; }

.pill-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:7px; }
.auto-grid-inner { display:grid; grid-template-columns:repeat(3,1fr); gap:7px; }

.pill {
  border-radius:7px; padding:9px 10px; border:1px solid;
  cursor:pointer;
  transition:transform .15s, box-shadow .15s;
}
.pill:hover { transform:translateY(-2px); }
.pill.active { outline:2px solid #4ADE80; outline-offset:2px; }
.pill-name { font-size:11px; font-weight:600; margin-bottom:2px; }
.pill-desc { font-size:10px; line-height:1.4; }

.kpi-pill  { background:#0F2B0F; border-color:#1F4D1F; }
.kpi-pill .pill-name { color:#6EE7B7; }
.kpi-pill .pill-desc { color:#276446; }
.kpi-star  { background:#1A2200; border-color:#4A6800; }
.kpi-star .pill-name { color:#D4F576; }
.kpi-star .pill-desc { color:#5A7A10; }
.auto-pill { background:#1A0D0D; border-color:#4D1F1F; }
.auto-pill .pill-name { color:#FCA5A5; }
.auto-pill .pill-desc { color:#7F2020; }

/* ── FOOTER ── */
.footer {
  margin-top:18px; display:flex; align-items:center; justify-content:space-between;
  position:relative; z-index:1; border-top:1px solid #21262D; padding-top:12px;
  min-width:1100px;
}
.tech-row { display:flex; gap:8px; flex-wrap:wrap; }
.tech-badge {
  font:500 10px/1 var(--mono); color:var(--faint);
  padding:3px 8px; border:1px solid #21262D; border-radius:4px;
}
.footer-note { font:400 10px/1 var(--mono); color:var(--faint); }

/* ── SLIDE PANEL ── */
.panel {
  position:fixed; top:0; right:0; bottom:0;
  width:var(--panel-w);
  background:#13181F;
  border-left:1px solid #21262D;
  z-index:100;
  display:flex; flex-direction:column;
  transform:translateX(100%);
  transition:transform .35s cubic-bezier(.4,0,.2,1);
  overflow:hidden;
}
.panel.open { transform:translateX(0); }

.panel-top {
  padding:20px 22px 16px;
  border-bottom:1px solid #21262D;
  display:flex; align-items:flex-start; gap:12px;
  flex-shrink:0;
}
.panel-icon {
  width:34px; height:34px; border-radius:8px;
  display:flex; align-items:center; justify-content:center;
  font-size:16px; flex-shrink:0; margin-top:2px;
}
.panel-titles { flex:1; }
.panel-layer-label { font:600 10px/1 var(--mono); text-transform:uppercase; letter-spacing:.8px; opacity:.5; margin-bottom:5px; }
.panel-title  { font-size:16px; font-weight:600; color:var(--text); line-height:1.2; }
.panel-close  {
  width:28px; height:28px; border-radius:6px;
  background:transparent; border:1px solid #30363D;
  color:var(--muted); cursor:pointer; font-size:16px;
  display:flex; align-items:center; justify-content:center;
  flex-shrink:0; transition:background .1s;
}
.panel-close:hover { background:#21262D; color:var(--text); }

.panel-body { flex:1; overflow-y:auto; padding:20px 22px 32px; }
.panel-body::-webkit-scrollbar { width:4px; }
.panel-body::-webkit-scrollbar-track { background:transparent; }
.panel-body::-webkit-scrollbar-thumb { background:#30363D; border-radius:2px; }

/* Panel content blocks */
.p-section { margin-bottom:20px; }
.p-label {
  font:600 10px/1 var(--mono); text-transform:uppercase;
  letter-spacing:.8px; color:var(--faint); margin-bottom:8px;
}
.p-text { font-size:13px; line-height:1.7; color:#C9D1D9; }
.p-text strong { color:var(--text); font-weight:600; }

.p-list { list-style:none; display:flex; flex-direction:column; gap:5px; }
.p-list li {
  font-size:12px; color:#C9D1D9; line-height:1.5;
  padding-left:14px; position:relative;
}
.p-list li::before {
  content:''; position:absolute; left:0; top:7px;
  width:5px; height:5px; border-radius:50%;
  background:var(--accent, #6B7280);
}

.p-code {
  background:#0D1117; border:1px solid #30363D; border-radius:7px;
  padding:12px 14px; margin-top:8px;
  font:400 11px/1.7 var(--mono); color:#E6EDF3;
  overflow-x:auto; white-space:pre;
}
.p-code .kw  { color:#FF7B72; }
.p-code .fn  { color:#D2A8FF; }
.p-code .str { color:#A5D6FF; }
.p-code .cm  { color:#8B949E; font-style:italic; }
.p-code .nm  { color:#79C0FF; }

.p-badge-row { display:flex; gap:6px; flex-wrap:wrap; margin-top:6px; }
.p-badge {
  font:500 10px/1 var(--mono); padding:3px 9px;
  border-radius:4px; border:1px solid;
}

.p-marketing {
  background:linear-gradient(135deg,#1A2200,#0F2B0F);
  border:1px solid #4A6800; border-radius:8px;
  padding:12px 14px; margin-top:8px;
}
.p-marketing-title { font-size:11px; font-weight:600; color:#D4F576; margin-bottom:6px; }
.p-marketing-text  { font-size:12px; color:#8AAC3A; line-height:1.6; }

.p-sprint-badge {
  display:inline-flex; align-items:center; gap:6px;
  font:500 11px/1 var(--mono); padding:4px 10px;
  border-radius:5px; margin-bottom:14px;
}

/* ── SPRINT PROGRESS BAR ── */
.sprint-bar { margin-top:24px; padding-top:18px; border-top:1px solid #21262D; }
.sprint-bar-label { font:600 10px/1 var(--mono); color:var(--faint); text-transform:uppercase; letter-spacing:.8px; margin-bottom:10px; }
.sprint-steps { display:flex; gap:4px; }
.sprint-step {
  flex:1; height:4px; border-radius:2px; background:#21262D;
  position:relative;
}
.sprint-step.done { background:#1DB87A; }
.sprint-step.current { background:#F59E0B; }
.sprint-step-label {
  position:absolute; top:8px; left:50%; transform:translateX(-50%);
  font:400 9px/1 var(--mono); color:var(--faint); white-space:nowrap;
}

/* ── EMPTY STATE ── */
.panel-empty {
  flex:1; display:flex; flex-direction:column;
  align-items:center; justify-content:center;
  gap:12px; padding:32px; text-align:center;
}
.panel-empty-icon { font-size:32px; opacity:.3; }
.panel-empty-text { font-size:13px; color:var(--faint); line-height:1.6; }

/* ── CLOSE OVERLAY ── */
.overlay {
  display:none; position:fixed; inset:0;
  z-index:99; cursor:pointer;
}
.overlay.visible { display:block; }

/* ── RESPONSIVE: TABLET ── */
@media (max-width:1200px) {
  .map-area { overflow-x:auto; }
  .pipeline {
    grid-template-columns:175px 38px 175px 38px 175px 38px 175px 38px 175px;
    min-width:880px;
  }
  .bottom-row { min-width:880px; }
  .footer     { min-width:880px; }
}

/* ── RESPONSIVE: MOBILE ── */
@media (max-width:768px) {
  :root { --panel-w:100%; }

  html,body { overflow-x:hidden; }

  .map-area {
    padding:14px 14px 40px;
    overflow-x:hidden;
  }
  .map-area.shifted { margin-right:0; }

  /* Header stacks vertically */
  .header {
    flex-direction:column;
    gap:10px;
    margin-bottom:18px;
  }
  .header-hint { text-align:left; font-size:10px; }

  /* Pipeline: vertical stack */
  .pipeline {
    display:flex;
    flex-direction:column;
    min-width:unset;
    gap:0;
  }
  .layer { width:100%; }

  /* Rotate horizontal arrows to point down */
  .arrow-col {
    padding:2px 0;
    transform:rotate(90deg);
    justify-content:center;
    align-items:center;
  }

  /* Bottom sections: single column */
  .bottom-row {
    grid-template-columns:1fr;
    min-width:unset;
    margin-top:14px;
  }
  .pill-grid       { grid-template-columns:repeat(2,1fr); }
  .auto-grid-inner { grid-template-columns:repeat(2,1fr); }

  /* Footer: vertical */
  .footer {
    flex-direction:column;
    gap:10px;
    min-width:unset;
    align-items:flex-start;
  }

  /* Panel: bottom sheet */
  .panel {
    width:100%;
    height:75vh;
    top:auto;
    left:0;
    right:0;
    bottom:0;
    border-left:none;
    border-top:1px solid #21262D;
    border-radius:14px 14px 0 0;
    transform:translateY(100%);
    transition:transform .35s cubic-bezier(.4,0,.2,1);
  }
  .panel.open { transform:translateY(0); }

  /* Drag handle visual cue */
  .panel-top::before {
    content:'';
    display:block;
    width:36px; height:4px;
    background:#30363D;
    border-radius:2px;
    position:absolute;
    top:8px; left:50%;
    transform:translateX(-50%);
  }
  .panel-top { position:relative; padding-top:24px; }
}
</style>
</head>
<body>
<div class="shell">

  <!-- ══ MAP AREA ══ -->
  <div class="map-area" id="mapArea">

    <div class="header">
      <div class="logo-area">
        <div class="logo-dot">GC</div>
        <div>
          <div class="project-title">Green Control Disinfestazioni</div>
          <div class="project-sub">Architettura Business Intelligence — Project Work</div>
        </div>
      </div>
      <div class="header-hint">
        <span class="hint-pulse"></span>Clicca qualsiasi blocco per la spiegazione<br>
        Stack: MySQL · Python · Power BI · Streamlit<br>
        KPI monitorati: 7 · Automazioni: 3
      </div>
    </div>

    <!-- PIPELINE -->
    <div class="pipeline">

      <!-- ─── LAYER 1: DB RAW ─── -->
      <div class="layer l1">
        <div class="layer-header"><span class="layer-num">01</span><span class="layer-name">Database Raw</span></div>
        <div class="node accent" data-id="db1-main">
          <span class="node-click-hint">↗ click</span>
          <div class="node-title">MySQL — DB 1</div>
          <div class="node-desc">Dati grezzi di produzione. Tabelle normalizzate.</div>
          <span class="node-tag">green_control</span>
        </div>
        <div class="node" data-id="db1-clienti">
          <span class="node-click-hint">↗</span>
          <div class="node-title">CLIENTI</div>
          <div class="node-desc">Privato / Azienda / Ente<br>50+ record · fonte lead</div>
        </div>
        <div class="node" data-id="db1-interventi">
          <span class="node-click-hint">↗</span>
          <div class="node-title">INTERVENTI</div>
          <div class="node-desc">220+ record · 2 anni<br>Data_Chiamata + SLA_ore</div>
        </div>
        <div class="node" data-id="db1-sedi">
          <span class="node-click-hint">↗</span>
          <div class="node-title">SEDI · TECNICI</div>
          <div class="node-desc">70 sedi · 6 tecnici<br>specializzazioni ISO</div>
        </div>
        <div class="node" data-id="db1-prodotti">
          <span class="node-click-hint">↗</span>
          <div class="node-title">SERVIZI · PRODOTTI</div>
          <div class="node-desc">8 servizi · 12 prodotti<br>tabella ponte N:N</div>
        </div>
      </div>

      <div class="arrow-col"><svg class="arrow-svg" viewBox="0 0 44 28"><line x1="4" y1="14" x2="32" y2="14" stroke="#3D4451" stroke-width="1.5"/><polyline points="26,9 36,14 26,19" fill="none" stroke="#3D4451" stroke-width="1.5" stroke-linejoin="round"/><text x="20" y="9" text-anchor="middle" fill="#3D4451" font-size="7" font-family="DM Mono">READ</text></svg></div>

      <!-- ─── LAYER 2: PYTHON ETL ─── -->
      <div class="layer l2">
        <div class="layer-header"><span class="layer-num">02</span><span class="layer-name">Python ETL</span></div>
        <div class="node accent" data-id="py-main">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Il motore del progetto</div>
          <div class="node-desc">Cron job notturno alle 2:00.</div>
          <span class="node-tag">pandas · mysql-connector</span>
        </div>
        <div class="node" data-id="py-extract">
          <span class="node-click-hint">↗</span>
          <div class="node-title">① Extract</div>
          <div class="node-desc">Lettura tabelle raw via<br>mysql-connector + pandas</div>
        </div>
        <div class="node" data-id="py-transform">
          <span class="node-click-hint">↗</span>
          <div class="node-title">② Transform</div>
          <div class="node-desc">Calcolo KPI, pulizia,<br>aggregazioni mensili</div>
        </div>
        <div class="node" data-id="py-load">
          <span class="node-click-hint">↗</span>
          <div class="node-title">③ Load</div>
          <div class="node-desc">INSERT INTO DB 2<br>tabelle clean e pronte</div>
        </div>
        <div class="node" data-id="py-alert">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Alert magazzino</div>
          <div class="node-desc">Se giacenza &lt; soglia →<br>email automatica</div>
        </div>
      </div>

      <div class="arrow-col"><svg class="arrow-svg" viewBox="0 0 44 28"><line x1="4" y1="14" x2="32" y2="14" stroke="#78350F" stroke-width="1.5"/><polyline points="26,9 36,14 26,19" fill="none" stroke="#78350F" stroke-width="1.5" stroke-linejoin="round"/><text x="20" y="9" text-anchor="middle" fill="#78350F" font-size="7" font-family="DM Mono">WRITE</text></svg></div>

      <!-- ─── LAYER 3: DB CLEAN ─── -->
      <div class="layer l3">
        <div class="layer-header"><span class="layer-num">03</span><span class="layer-name">Data Warehouse</span></div>
        <div class="node accent" data-id="dw-main">
          <span class="node-click-hint">↗</span>
          <div class="node-title">MySQL — DB 2</div>
          <div class="node-desc">Dati puliti e aggregati.</div>
          <span class="node-tag">green_control_dw</span>
        </div>
        <div class="node" data-id="dw-fact">
          <span class="node-click-hint">↗</span>
          <div class="node-title">fact_interventi</div>
          <div class="node-desc">Tabella dei fatti principale<br>schema star</div>
        </div>
        <div class="node" data-id="dw-kpi">
          <span class="node-click-hint">↗</span>
          <div class="node-title">kpi_mensili</div>
          <div class="node-desc">Fatturato, churn, CLV<br>SLA medio · % sotto soglia</div>
        </div>
        <div class="node" data-id="dw-dim">
          <span class="node-click-hint">↗</span>
          <div class="node-title">dim_clienti · dim_tecnici</div>
          <div class="node-desc">Dimensioni del DW<br>join già ottimizzate</div>
        </div>
        <div class="node" data-id="dw-alert">
          <span class="node-click-hint">↗</span>
          <div class="node-title">alert_magazzino</div>
          <div class="node-desc">Semaforo prodotti<br>rosso / giallo / verde</div>
        </div>
      </div>

      <div class="arrow-col"><svg class="arrow-svg" viewBox="0 0 44 28"><line x1="4" y1="14" x2="32" y2="14" stroke="#1A3D2B" stroke-width="1.5"/><polyline points="26,9 36,14 26,19" fill="none" stroke="#1A3D2B" stroke-width="1.5" stroke-linejoin="round"/><text x="20" y="9" text-anchor="middle" fill="#1A3D2B" font-size="7" font-family="DM Mono">ODBC</text></svg></div>

      <!-- ─── LAYER 4: POWER BI ─── -->
      <div class="layer l4">
        <div class="layer-header"><span class="layer-num">04</span><span class="layer-name">Power BI</span></div>
        <div class="node accent" data-id="pbi-main">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Dashboard direzionale</div>
          <div class="node-desc">Si connette solo al DB 2.</div>
          <span class="node-tag">scheduled refresh 3h</span>
        </div>
        <div class="node" data-id="pbi-p1">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Pagina 1 — Overview KPI</div>
          <div class="node-desc">4 card + trend line<br>slicer anno / zona</div>
        </div>
        <div class="node" data-id="pbi-p2">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Pagina 2 — Mappa + Heatmap</div>
          <div class="node-desc">Clienti per regione<br>stagionalità infestanti</div>
        </div>
        <div class="node" data-id="pbi-p3">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Pagina 3 — Tecnici</div>
          <div class="node-desc">Ranking, tasso successo<br>ricavo generato</div>
        </div>
        <div class="node" data-id="pbi-dax">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Misure DAX</div>
          <div class="node-desc">Churn Rate, CLV, SLA<br>Fatturato MTD / YoY</div>
        </div>
      </div>

      <div class="arrow-col"><svg class="arrow-svg" viewBox="0 0 44 28"><line x1="4" y1="14" x2="32" y2="14" stroke="#2D2060" stroke-width="1.5"/><polyline points="26,9 36,14 26,19" fill="none" stroke="#2D2060" stroke-width="1.5" stroke-linejoin="round"/><text x="20" y="9" text-anchor="middle" fill="#2D2060" font-size="7" font-family="DM Mono">EMBED</text></svg></div>

      <!-- ─── LAYER 5: STREAMLIT ─── -->
      <div class="layer l5">
        <div class="layer-header"><span class="layer-num">05</span><span class="layer-name">Streamlit App</span></div>
        <div class="node accent" data-id="st-main">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Web app gestionale</div>
          <div class="node-desc">4 pagine · Power BI embedded.</div>
          <span class="node-tag">Railway deploy</span>
        </div>
        <div class="node" data-id="st-home">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Home — KPI live</div>
          <div class="node-desc">4 card aggiornate + iframe<br>dashboard Power BI</div>
        </div>
        <div class="node" data-id="st-interventi">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Interventi</div>
          <div class="node-desc">Tabella filtrabile<br>form inserimento</div>
        </div>
        <div class="node" data-id="st-magazzino">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Magazzino</div>
          <div class="node-desc">Semaforo giacenze<br>prodotti sotto soglia</div>
        </div>
        <div class="node" data-id="st-clienti">
          <span class="node-click-hint">↗</span>
          <div class="node-title">Clienti</div>
          <div class="node-desc">Anagrafica ricercabile<br>storico · badge churn</div>
        </div>
      </div>

    </div><!-- /pipeline -->

    <!-- BOTTOM -->
    <div class="bottom-row">
      <div class="bottom-section kpi-section">
        <div class="bottom-title">KPI strategici monitorati</div>
        <div class="pill-grid">
          <div class="pill kpi-star" data-id="kpi-sla"><div class="pill-name">⚡ SLA Risposta</div><div class="pill-desc">Chiamata → intervento · leva marketing</div></div>
          <div class="pill kpi-pill" data-id="kpi-churn"><div class="pill-name">Churn Rate</div><div class="pill-desc">% clienti persi per periodo</div></div>
          <div class="pill kpi-pill" data-id="kpi-clv"><div class="pill-name">CLV</div><div class="pill-desc">Customer Lifetime Value medio</div></div>
          <div class="pill kpi-pill" data-id="kpi-conv"><div class="pill-name">Conv. Rate</div><div class="pill-desc">Lead → cliente pagante</div></div>
          <div class="pill kpi-pill" data-id="kpi-tecnici"><div class="pill-name">Ricavi / Tecnico</div><div class="pill-desc">Performance per zona</div></div>
          <div class="pill kpi-pill" data-id="kpi-costo"><div class="pill-name">Costo intervento</div><div class="pill-desc">Marginalità per servizio</div></div>
          <div class="pill kpi-pill" data-id="kpi-stag"><div class="pill-name">Stagionalità</div><div class="pill-desc">Heatmap infestanti / mese</div></div>
        </div>
      </div>
      <div class="bottom-section auto-section">
        <div class="bottom-title">Automazioni — wow factor</div>
        <div class="auto-grid-inner">
          <div class="pill auto-pill" data-id="auto-alert"><div class="pill-name">Alert magazzino</div><div class="pill-desc">Cron Python · email auto se giacenza &lt; soglia</div></div>
          <div class="pill auto-pill" data-id="auto-refresh"><div class="pill-name">Refresh Power BI</div><div class="pill-desc">Scheduled ogni 3h · Power BI Service cloud</div></div>
          <div class="pill auto-pill" data-id="auto-pdf"><div class="pill-name">Report PDF auto</div><div class="pill-desc">reportlab · KPI mensili + logo + tabella</div></div>
        </div>
      </div>
    </div>

    <div class="footer">
      <div class="tech-row">
        <span class="tech-badge">MySQL 8.0</span><span class="tech-badge">Python 3.11</span>
        <span class="tech-badge">pandas 2.x</span><span class="tech-badge">Power BI Desktop</span>
        <span class="tech-badge">Streamlit 1.x</span><span class="tech-badge">Railway</span>
      </div>
      <div class="footer-note">Green Control Disinfestazioni — Project Work BI Analyst</div>
    </div>

  </div><!-- /map-area -->

  <!-- ══ SLIDE PANEL ══ -->
  <div class="panel" id="panel">
    <div class="panel-top" id="panelTop">
      <div class="panel-icon" id="panelIcon">📦</div>
      <div class="panel-titles">
        <div class="panel-layer-label" id="panelLayer">Layer</div>
        <div class="panel-title" id="panelTitle">Titolo</div>
      </div>
      <button class="panel-close" id="panelClose" aria-label="Chiudi pannello">✕</button>
    </div>
    <div class="panel-body" id="panelBody">
      <div class="panel-empty">
        <div class="panel-empty-icon">↖</div>
        <div class="panel-empty-text">Clicca un blocco nella mappa<br>per leggere la spiegazione pratica.</div>
      </div>
    </div>
  </div>

</div><!-- /shell -->
<div class="overlay" id="overlay"></div>

<script>
const CONTENT = {
  /* ─── DB RAW ─── */
  'db1-main': {
    icon:'🗄️', layer:'01 · Database Raw', title:'MySQL — Database di Produzione',
    sprint:1, sprintLabel:'Sprint 1 — Fondamenta',
    body:`<div class="p-section">
      <div class="p-label">Cosa fa</div>
      <p class="p-text">È il <strong>magazzino dati grezzi</strong> dell'azienda. Contiene le 7 tabelle principali esattamente come arrivano dalla realtà operativa: interventi, clienti, prodotti, tecnici. Niente aggregazioni, niente calcoli — solo i fatti.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Perché MySQL e non Excel</div>
      <p class="p-text">Excel non ha <strong>integrità referenziale</strong>. Se elimini un cliente in Excel, le sue righe negli interventi rimangono orfane. MySQL con le Foreign Keys ti blocca e ti protegge. Inoltre scala senza problemi a milioni di righe.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Schema usato</div>
      <div class="p-code"><span class="cm">-- Nome schema nel tuo MySQL Workbench</span>
<span class="kw">CREATE DATABASE</span> green_control
  <span class="kw">CHARACTER SET</span> utf8mb4
  <span class="kw">COLLATE</span> utf8mb4_unicode_ci;</div>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#C9D1D9;border-color:#30363D">MySQL 8.0</span>
      <span class="p-badge" style="color:#C9D1D9;border-color:#30363D">7 tabelle</span>
      <span class="p-badge" style="color:#C9D1D9;border-color:#30363D">FK constraints</span>
    </div>`
  },
  'db1-clienti': {
    icon:'👥', layer:'01 · Database Raw', title:'Tabella CLIENTI',
    sprint:1,
    body:`<div class="p-section">
      <div class="p-label">Campi chiave</div>
      <ul class="p-list" style="--accent:#6B7280">
        <li>ID_Cliente (PK) · Ragione_Sociale · Tipo_Cliente</li>
        <li>Email_Fatturazione · Telefono_Principale</li>
        <li>Regione · Città · Data_Acquisizione</li>
        <li><strong>Fonte_Lead</strong> — da dove è arrivato il cliente (Google Ads, Passaparola…)</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Perché Fonte_Lead è prezioso</div>
      <p class="p-text">Con questo campo puoi calcolare il <strong>ROI di ogni canale marketing</strong>. Se i clienti da Facebook Ads hanno CLV basso e churno dopo 3 mesi, stai spendendo soldi nel posto sbagliato.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Query utile</div>
      <div class="p-code"><span class="kw">SELECT</span> Fonte_Lead,
       <span class="fn">COUNT</span>(*) <span class="kw">AS</span> n_clienti,
       <span class="fn">ROUND</span>(<span class="fn">AVG</span>(clv),<span class="nm">2</span>) <span class="kw">AS</span> clv_medio
<span class="kw">FROM</span> v_storico_clienti
<span class="kw">GROUP BY</span> Fonte_Lead
<span class="kw">ORDER BY</span> clv_medio <span class="kw">DESC</span>;</div>
    </div>`
  },
  'db1-interventi': {
    icon:'🔧', layer:'01 · Database Raw', title:'Tabella INTERVENTI — cuore del DB',
    sprint:1,
    body:`<div class="p-section">
      <div class="p-label">I campi critici per lo SLA</div>
      <ul class="p-list" style="--accent:#6B7280">
        <li><strong>Data_Chiamata</strong> — quando il cliente ha chiamato (nuovo campo!)</li>
        <li><strong>SLA_ore</strong> — colonna calcolata automaticamente da MySQL</li>
        <li>Data_Intervento + Ora_Inizio — quando il tecnico è arrivato</li>
        <li>Stato — Completato / Annullato / In attesa</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">SQL per aggiungere lo SLA</div>
      <div class="p-code"><span class="kw">ALTER TABLE</span> INTERVENTI
<span class="kw">ADD COLUMN</span> Data_Chiamata <span class="fn">DATETIME</span> <span class="kw">NULL</span>,
<span class="kw">ADD COLUMN</span> SLA_ore <span class="fn">DECIMAL</span>(<span class="nm">5,1</span>)
  <span class="kw">GENERATED ALWAYS AS</span> (
    <span class="fn">TIMESTAMPDIFF</span>(<span class="nm">MINUTE</span>, Data_Chiamata,
      <span class="fn">CONCAT</span>(Data_Intervento,<span class="str">' '</span>,Ora_Inizio)
    ) / <span class="nm">60</span>
  ) <span class="kw">STORED</span>;</div>
    </div>
    <p class="p-text" style="margin-top:8px">Il campo è <strong>STORED</strong>: MySQL lo calcola una volta sola e lo salva. Nessun calcolo a runtime → Power BI va veloce.</p>`
  },
  'db1-sedi': {
    icon:'📍', layer:'01 · Database Raw', title:'Tabelle SEDI e TECNICI',
    sprint:1,
    body:`<div class="p-section">
      <div class="p-label">SEDI — perché separata da CLIENTI</div>
      <p class="p-text">Un cliente aziendale può avere <strong>più sedi</strong> (es. una catena di ristoranti con 5 locali). La relazione è 1:N. Ogni sede ha la sua Tipologia (Residenziale / Commerciale / Industriale) e Superficie_MQ — che influenza il prezzo dell'intervento.</p>
    </div>
    <div class="p-section">
      <div class="p-label">TECNICI — certificazioni ISO</div>
      <ul class="p-list" style="--accent:#6B7280">
        <li>Specializzazione: Entomologia, Chimica Ambientale, Derattizzazione…</li>
        <li>Livello_Certificazione: ISO 16636 Junior / Mid / Senior</li>
        <li>Usata per il ranking performance in Power BI Pagina 3</li>
      </ul>
    </div>`
  },
  'db1-prodotti': {
    icon:'🧪', layer:'01 · Database Raw', title:'Tabelle SERVIZI e PRODOTTI',
    sprint:1,
    body:`<div class="p-section">
      <div class="p-label">La relazione N:N fondamentale</div>
      <p class="p-text">Un intervento può usare <strong>più prodotti</strong>, e un prodotto può essere usato in <strong>molti interventi</strong>. La tabella ponte <code>INTERVENTI_PRODOTTI</code> gestisce questo con i campi <strong>Quantita_Usata</strong> e <strong>Lotto_Produzione</strong> — questo secondo campo è oro per la tracciabilità normativa (D.Lgs. 150/2012).</p>
    </div>
    <div class="p-section">
      <div class="p-label">Prodotti con alert già inseriti</div>
      <ul class="p-list" style="--accent:#6B7280">
        <li>BioRat — giacenza 8, soglia 15 → <strong>RIORDINO URGENTE</strong></li>
        <li>BlattGel — giacenza 6, soglia 15 → <strong>RIORDINO URGENTE</strong></li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Flag Is_Eco_Friendly</div>
      <p class="p-text">Permette di filtrare i prodotti biologici. Utile per clienti pubblici (scuole, ospedali) che hanno vincoli normativi sui biocidi. È anche un <strong>argomento di vendita</strong>.</p>
    </div>`
  },

  /* ─── PYTHON ETL ─── */
  'py-main': {
    icon:'⚙️', layer:'02 · Python ETL', title:'Il motore del progetto',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Cos'è un ETL</div>
      <p class="p-text"><strong>Extract → Transform → Load</strong>. È il pattern standard dell'industria per spostare dati da un sistema all'altro in modo controllato. Il tuo script Python fa esattamente questo: legge dal DB raw, elabora, scrive nel DW.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Struttura del progetto Python</div>
      <div class="p-code">green_control_etl/
├── config.py        <span class="cm"># credenziali DB</span>
├── extract.py       <span class="cm"># lettura DB 1</span>
├── transform.py     <span class="cm"># calcolo KPI</span>
├── load.py          <span class="cm"># scrittura DB 2</span>
├── alert.py         <span class="cm"># email magazzino</span>
├── pdf_report.py    <span class="cm"># report mensile</span>
└── main.py          <span class="cm"># orchestratore</span></div>
    </div>
    <div class="p-section">
      <div class="p-label">Cron job (Linux/Mac) o Task Scheduler (Windows)</div>
      <div class="p-code"><span class="cm"># Esegue main.py ogni notte alle 2:00</span>
<span class="nm">0 2 * * *</span> python3 /path/green_control_etl/main.py</div>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#FCD34D;border-color:#3D2E00">pandas 2.x</span>
      <span class="p-badge" style="color:#FCD34D;border-color:#3D2E00">mysql-connector</span>
      <span class="p-badge" style="color:#FCD34D;border-color:#3D2E00">smtplib</span>
      <span class="p-badge" style="color:#FCD34D;border-color:#3D2E00">reportlab</span>
    </div>`
  },
  'py-extract': {
    icon:'📥', layer:'02 · Python ETL', title:'① Extract — Lettura dal DB Raw',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Il codice reale</div>
      <div class="p-code"><span class="kw">import</span> pandas <span class="kw">as</span> pd
<span class="kw">import</span> mysql.connector

<span class="kw">def</span> <span class="fn">extract</span>(conn):
    <span class="cm"># Legge solo i dati dell'ultimo mese</span>
    <span class="cm"># per non riprocessare tutto ogni notte</span>
    query = <span class="str">"""
      SELECT i.*, s.SLA_ore,
        c.Ragione_Sociale, c.Fonte_Lead,
        t.Nome_Cognome AS tecnico,
        sv.Nome_Servizio, sv.Categoria
      FROM INTERVENTI i
      JOIN SEDI se ON i.ID_Sede = se.ID_Sede
      JOIN CLIENTI c ON se.ID_Cliente = c.ID_Cliente
      JOIN TECNICI t ON i.Cod_Tecnico = t.Cod_Tecnico
      JOIN SERVIZI sv ON i.ID_Servizio = sv.ID_Servizio
      WHERE i.Data_Intervento >= DATE_SUB(NOW(), INTERVAL 1 MONTH)
    """</span>
    <span class="kw">return</span> pd.<span class="fn">read_sql</span>(query, conn)</div>
    </div>`
  },
  'py-transform': {
    icon:'🔄', layer:'02 · Python ETL', title:'② Transform — Calcolo KPI',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">I calcoli principali</div>
      <div class="p-code"><span class="kw">def</span> <span class="fn">transform</span>(df):
    <span class="cm"># Fatturato mensile</span>
    fat = df.<span class="fn">groupby</span>([<span class="str">'anno'</span>,<span class="str">'mese'</span>])[<span class="str">'Prezzo_Applicato'</span>].<span class="fn">sum</span>()

    <span class="cm"># SLA medio e % sotto soglia 24h</span>
    sla_medio = df[<span class="str">'SLA_ore'</span>].<span class="fn">mean</span>()
    sla_ok_pct = (df[<span class="str">'SLA_ore'</span>] &lt; <span class="nm">24</span>).<span class="fn">mean</span>() * <span class="nm">100</span>

    <span class="cm"># Churn: clienti senza interventi da 12 mesi</span>
    last_int = df.<span class="fn">groupby</span>(<span class="str">'ID_Cliente'</span>)[<span class="str">'Data_Intervento'</span>].<span class="fn">max</span>()
    churned = (last_int &lt; pd.<span class="fn">Timestamp</span>.<span class="fn">now</span>() - pd.<span class="fn">DateOffset</span>(months=<span class="nm">12</span>)).<span class="fn">sum</span>()
    churn_rate = churned / df[<span class="str">'ID_Cliente'</span>].<span class="fn">nunique</span>() * <span class="nm">100</span>

    <span class="kw">return</span> {<span class="str">'sla_medio'</span>: sla_medio,
            <span class="str">'sla_ok_pct'</span>: sla_ok_pct,
            <span class="str">'churn_rate'</span>: churn_rate}</div>
    </div>`
  },
  'py-load': {
    icon:'📤', layer:'02 · Python ETL', title:'③ Load — Scrittura nel DW',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Strategia: upsert, non insert cieco</div>
      <p class="p-text">Non cancellare e ricreare tutto ogni notte. Usa <strong>INSERT ... ON DUPLICATE KEY UPDATE</strong> per aggiornare solo le righe cambiate. Più veloce e sicuro.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Il codice</div>
      <div class="p-code"><span class="kw">def</span> <span class="fn">load_kpi</span>(conn_dw, kpi_row):
    sql = <span class="str">"""
      INSERT INTO kpi_mensili
        (anno, mese, fatturato, churn_rate,
         sla_medio_ore, sla_ok_pct, clv_medio)
      VALUES (%s,%s,%s,%s,%s,%s,%s)
      ON DUPLICATE KEY UPDATE
        fatturato      = VALUES(fatturato),
        churn_rate     = VALUES(churn_rate),
        sla_medio_ore  = VALUES(sla_medio_ore),
        sla_ok_pct     = VALUES(sla_ok_pct),
        clv_medio      = VALUES(clv_medio)
    """</span>
    conn_dw.cursor().<span class="fn">execute</span>(sql, kpi_row)
    conn_dw.<span class="fn">commit</span>()</div>
    </div>`
  },
  'py-alert': {
    icon:'🚨', layer:'02 · Python ETL', title:'Alert magazzino automatico',
    sprint:4,
    body:`<div class="p-section">
      <div class="p-label">Come funziona</div>
      <p class="p-text">Ogni mattina alle 8:00 uno script separato interroga la view <code>v_alert_magazzino</code>. Se trova prodotti sotto soglia, invia un'email al responsabile acquisti con l'elenco.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Il codice completo</div>
      <div class="p-code"><span class="kw">import</span> smtplib
<span class="kw">from</span> email.mime.text <span class="kw">import</span> MIMEText

<span class="kw">def</span> <span class="fn">check_and_alert</span>(conn):
    df = pd.<span class="fn">read_sql</span>(
      <span class="str">"SELECT * FROM v_alert_magazzino"</span>, conn)
    <span class="kw">if</span> df.<span class="fn">empty</span>:
        <span class="kw">return</span>  <span class="cm"># tutto OK</span>

    body = <span class="str">"⚠️ PRODOTTI SOTTO SOGLIA:\n\n"</span>
    <span class="kw">for</span> _, row <span class="kw">in</span> df.<span class="fn">iterrows</span>():
        body += <span class="fn">f</span><span class="str">"{row.Nome_Commerciale}: "</span>
        body += <span class="fn">f</span><span class="str">"{row.Giacenza_Magazzino} pz"</span>
        body += <span class="fn">f</span><span class="str">" (soglia: {row.Soglia_Riordino})\n"</span>

    msg = <span class="fn">MIMEText</span>(body)
    msg[<span class="str">'Subject'</span>] = <span class="str">'[GC] Alert Magazzino'</span>
    <span class="cm"># ... invio SMTP</span></div>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">wow factor esame</span>
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">demo live 2 min</span>
    </div>`
  },

  /* ─── DW ─── */
  'dw-main': {
    icon:'🏛️', layer:'03 · Data Warehouse', title:'MySQL — Database Pulito (DW)',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Perché un secondo database</div>
      <p class="p-text">Power BI non deve mai toccare i dati grezzi. Se si connette direttamente al DB di produzione e fa una query pesante, <strong>rallenta tutta l'app</strong>. Il DW è una copia ottimizzata appositamente per la lettura.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Schema star vs snowflake</div>
      <p class="p-text">Usiamo uno <strong>schema star</strong>: una tabella centrale dei fatti (fact_interventi) circondata da tabelle di dimensioni (dim_clienti, dim_tecnici). È il pattern più veloce per Power BI.</p>
    </div>
    <div class="p-section">
      <div class="p-label">SQL creazione schema</div>
      <div class="p-code"><span class="kw">CREATE DATABASE</span> green_control_dw
  <span class="kw">CHARACTER SET</span> utf8mb4;

<span class="cm">-- Stessa istanza MySQL, schema diverso</span>
<span class="cm">-- Power BI si connette solo qui</span></div>
    </div>`
  },
  'dw-fact': {
    icon:'📊', layer:'03 · Data Warehouse', title:'fact_interventi — Tabella dei fatti',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Struttura</div>
      <div class="p-code"><span class="kw">CREATE TABLE</span> fact_interventi (
  id_intervento   <span class="fn">INT</span>         <span class="kw">PRIMARY KEY</span>,
  data_intervento <span class="fn">DATE</span>,
  anno            <span class="fn">INT</span>,
  mese            <span class="fn">INT</span>,
  trimestre       <span class="fn">INT</span>,
  id_cliente      <span class="fn">INT</span>,
  id_tecnico      <span class="fn">INT</span>,
  id_servizio     <span class="fn">INT</span>,
  regione         <span class="fn">VARCHAR</span>(<span class="nm">50</span>),
  tipo_uscita     <span class="fn">VARCHAR</span>(<span class="nm">20</span>),
  prezzo          <span class="fn">DECIMAL</span>(<span class="nm">10,2</span>),
  sla_ore         <span class="fn">DECIMAL</span>(<span class="nm">5,1</span>),  <span class="cm">-- ← SLA qui!</span>
  stato           <span class="fn">VARCHAR</span>(<span class="nm">20</span>)
);</div>
    </div>
    <p class="p-text" style="margin-top:8px">Nota: i campi anno/mese/trimestre sono <strong>pre-esplosi</strong> nella tabella. Così Power BI non deve calcolarli ogni volta con DAX — va 5x più veloce.</p>`
  },
  'dw-kpi': {
    icon:'📈', layer:'03 · Data Warehouse', title:'kpi_mensili — I numeri pronti',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Struttura della tabella</div>
      <div class="p-code"><span class="kw">CREATE TABLE</span> kpi_mensili (
  anno          <span class="fn">INT</span>,
  mese          <span class="fn">INT</span>,
  fatturato     <span class="fn">DECIMAL</span>(<span class="nm">12,2</span>),
  n_interventi  <span class="fn">INT</span>,
  churn_rate    <span class="fn">DECIMAL</span>(<span class="nm">5,2</span>),
  clv_medio     <span class="fn">DECIMAL</span>(<span class="nm">10,2</span>),
  sla_medio_ore <span class="fn">DECIMAL</span>(<span class="nm">5,1</span>),
  sla_ok_pct    <span class="fn">DECIMAL</span>(<span class="nm">5,1</span>),  <span class="cm">-- % &lt;24h</span>
  conv_rate     <span class="fn">DECIMAL</span>(<span class="nm">5,2</span>),
  <span class="kw">PRIMARY KEY</span> (anno, mese)
);</div>
    </div>
    <div class="p-section">
      <div class="p-label">Vantaggio in Power BI</div>
      <p class="p-text">Le card KPI della dashboard leggono direttamente questa tabella con una <code>SELECT</code> di 1 riga. Nessun DAX complesso, nessun JOIN pesante. Il refresh di Power BI dura <strong>secondi invece di minuti</strong>.</p>
    </div>`
  },
  'dw-dim': {
    icon:'🗂️', layer:'03 · Data Warehouse', title:'Tabelle Dimensione (dim_*)',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Cosa sono le dimensioni</div>
      <p class="p-text">Nelle dimensioni trovi i <strong>descrittori</strong> dei fatti: chi è il cliente, chi è il tecnico, quale servizio. La fact table ha solo gli ID — le dimensioni hanno i nomi e gli attributi. Questo evita la ripetizione dei dati.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Esempio dim_clienti</div>
      <div class="p-code"><span class="kw">CREATE TABLE</span> dim_clienti (
  id_cliente      <span class="fn">INT</span> <span class="kw">PRIMARY KEY</span>,
  ragione_sociale <span class="fn">VARCHAR</span>(<span class="nm">100</span>),
  tipo_cliente    <span class="fn">VARCHAR</span>(<span class="nm">20</span>),
  regione         <span class="fn">VARCHAR</span>(<span class="nm">50</span>),
  fonte_lead      <span class="fn">VARCHAR</span>(<span class="nm">50</span>),
  segmento        <span class="fn">VARCHAR</span>(<span class="nm">20</span>)  <span class="cm">-- calcolato dall'ETL</span>
);</div>
    </div>`
  },
  'dw-alert': {
    icon:'🚦', layer:'03 · Data Warehouse', title:'Tabella alert_magazzino',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Il semaforo del magazzino</div>
      <p class="p-text">Questa tabella viene aggiornata dall'ETL ogni notte e da Streamlit viene letta per mostrare il semaforo colorato. Tre stati: <strong style="color:#EF4444">ROSSO</strong> = esaurito, <strong style="color:#F59E0B">GIALLO</strong> = riordino urgente, <strong style="color:#22C55E">VERDE</strong> = OK.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Query Streamlit per il semaforo</div>
      <div class="p-code"><span class="cm"># In Streamlit pagina Magazzino</span>
df = pd.<span class="fn">read_sql</span>(<span class="str">"""
  SELECT Nome_Commerciale,
    Giacenza_Magazzino, Soglia_Riordino,
    stato_scorta
  FROM alert_magazzino
  ORDER BY stato_scorta DESC
"""</span>, conn_dw)

<span class="cm"># Colora le righe</span>
<span class="kw">def</span> <span class="fn">color_row</span>(row):
    c = {<span class="str">'ESAURITO'</span>:<span class="str">'#FF000030'</span>,
         <span class="str">'RIORDINO URGENTE'</span>:<span class="str">'#FF990030'</span>}
    <span class="kw">return</span> [<span class="fn">f</span><span class="str">'background:{c.get(row.stato_scorta,"")}'</span>]*<span class="fn">len</span>(row)</div>
    </div>`
  },

  /* ─── POWER BI ─── */
  'pbi-main': {
    icon:'📉', layer:'04 · Power BI', title:'Dashboard direzionale',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Perché Power BI e non Streamlit per i grafici</div>
      <p class="p-text">Power BI ha il <strong>DAX</strong> e grafici nativi che ci vorrebbero 200 righe di codice Plotly per replicare (funnel, waterfall, KPI card con trend). Soprattutto ha il <strong>drill-through</strong>: clicchi su una regione nella mappa e filtri tutto il report.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Scheduled Refresh — come si imposta</div>
      <ul class="p-list" style="--accent:#7C3AED">
        <li>Pubblica il report su Power BI Service (gratuito con account scolastico)</li>
        <li>Vai su Dataset settings → Scheduled refresh</li>
        <li>Imposta ogni 3 ore, connessione al tuo MySQL su Railway</li>
        <li>Risultato: la dashboard è sempre aggiornata senza toccare niente</li>
      </ul>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#C4B5FD;border-color:#2D2060">Power BI Desktop</span>
      <span class="p-badge" style="color:#C4B5FD;border-color:#2D2060">Power BI Service</span>
      <span class="p-badge" style="color:#C4B5FD;border-color:#2D2060">MySQL ODBC</span>
    </div>`
  },
  'pbi-p1': {
    icon:'🎯', layer:'04 · Power BI', title:'Pagina 1 — Overview KPI',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Cosa mostra</div>
      <ul class="p-list" style="--accent:#7C3AED">
        <li>4 KPI card: Fatturato MTD, Churn Rate, CLV medio, SLA medio ore</li>
        <li>Grafico a linee: fatturato mensile con confronto anno precedente (YoY)</li>
        <li>Slicer: Anno · Regione · Tipo cliente</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Misura DAX per Fatturato MTD</div>
      <div class="p-code">Fatturato MTD =
<span class="fn">CALCULATE</span>(
    <span class="fn">SUM</span>(fact_interventi[prezzo]),
    <span class="fn">DATESMTD</span>(dim_date[data])
)</div>
    </div>
    <div class="p-section">
      <div class="p-label">Cosa dire al prof</div>
      <p class="p-text">Questa pagina risponde alla domanda del titolare: <em>"Sto guadagnando questo mese rispetto all'anno scorso?"</em> In 3 secondi, senza aprire Excel.</p>
    </div>`
  },
  'pbi-p2': {
    icon:'🗺️', layer:'04 · Power BI', title:'Pagina 2 — Mappa + Heatmap',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">La mappa clienti</div>
      <p class="p-text">Visual <strong>Shape Map</strong> di Power BI con le regioni italiane. Colore più intenso = più fatturato. In un secondo vedi dove sono concentrati i clienti e dove c'è potenziale di espansione.</p>
    </div>
    <div class="p-section">
      <div class="p-label">La heatmap stagionalità</div>
      <p class="p-text">Matrice con mesi sulle colonne e tipo di infestante sulle righe. Il colore indica il numero di interventi. Pattern tipico: <strong>blatte e zanzare picco giugno-agosto</strong>, <strong>topi picco ottobre-novembre</strong>. Con questo dato pianifichi le assunzioni stagionali.</p>
    </div>`
  },
  'pbi-p3': {
    icon:'👷', layer:'04 · Power BI', title:'Pagina 3 — Performance Tecnici',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Metriche per tecnico</div>
      <ul class="p-list" style="--accent:#7C3AED">
        <li>Ricavo totale generato nel periodo</li>
        <li>Numero interventi completati vs annullati</li>
        <li>Tasso di successo %</li>
        <li>SLA medio per tecnico — chi risponde più velocemente?</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Misura DAX Tasso Successo</div>
      <div class="p-code">Tasso Successo % =
<span class="fn">DIVIDE</span>(
    <span class="fn">COUNTROWS</span>(<span class="fn">FILTER</span>(fact_interventi,
        fact_interventi[stato] = <span class="str">"Completato"</span>)),
    <span class="fn">COUNTROWS</span>(fact_interventi),
    <span class="nm">0</span>
) * <span class="nm">100</span></div>
    </div>`
  },
  'pbi-dax': {
    icon:'🧮', layer:'04 · Power BI', title:'Misure DAX principali',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">DAX per lo SLA</div>
      <div class="p-code">SLA Medio Ore =
<span class="fn">AVERAGE</span>(fact_interventi[sla_ore])

SLA OK % =
<span class="fn">DIVIDE</span>(
    <span class="fn">COUNTROWS</span>(<span class="fn">FILTER</span>(fact_interventi,
        fact_interventi[sla_ore] &lt; <span class="nm">24</span>)),
    <span class="fn">COUNTROWS</span>(fact_interventi)
) * <span class="nm">100</span></div>
    </div>
    <div class="p-section">
      <div class="p-label">DAX per il Churn Rate</div>
      <div class="p-code">Churn Rate % =
<span class="kw">VAR</span> tot = <span class="fn">DISTINCTCOUNT</span>(dim_clienti[id_cliente])
<span class="kw">VAR</span> churned =
    <span class="fn">CALCULATE</span>(<span class="fn">DISTINCTCOUNT</span>(dim_clienti[id_cliente]),
        <span class="fn">FILTER</span>(dim_clienti,
            dim_clienti[giorni_senza_intervento] > <span class="nm">365</span>))
<span class="kw">RETURN</span> <span class="fn">DIVIDE</span>(churned, tot) * <span class="nm">100</span></div>
    </div>`
  },

  /* ─── STREAMLIT ─── */
  'st-main': {
    icon:'🌐', layer:'05 · Streamlit', title:'Web app gestionale',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Perché Streamlit</div>
      <p class="p-text">Senza Streamlit dovresti imparare HTML, CSS e JavaScript (mesi di lavoro). Con Streamlit scrivi Python e lui costruisce l'interfaccia in automatico. In 3 ore hai un'app funzionante.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Struttura progetto Streamlit</div>
      <div class="p-code">green_control_app/
├── app.py           <span class="cm"># entry point</span>
├── pages/
│   ├── 1_home.py
│   ├── 2_interventi.py
│   ├── 3_magazzino.py
│   └── 4_clienti.py
├── utils/
│   └── db.py        <span class="cm"># connessione DW</span>
└── requirements.txt</div>
    </div>
    <div class="p-section">
      <div class="p-label">Deploy su Railway in 5 minuti</div>
      <ul class="p-list" style="--accent:#2563EB">
        <li>Crea account su railway.app (gratuito)</li>
        <li>Collega il tuo repo GitHub</li>
        <li>Railway deploy automatico ad ogni push</li>
        <li>URL pubblico da mostrare al prof: <code>green-control.up.railway.app</code></li>
      </ul>
    </div>`
  },
  'st-home': {
    icon:'🏠', layer:'05 · Streamlit', title:'Pagina Home — KPI live',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Codice della pagina Home</div>
      <div class="p-code"><span class="kw">import</span> streamlit <span class="kw">as</span> st
<span class="kw">import</span> pandas <span class="kw">as</span> pd
<span class="kw">from</span> utils.db <span class="kw">import</span> get_conn

st.title(<span class="str">"🌿 Green Control — Dashboard"</span>)

conn = <span class="fn">get_conn</span>()
kpi = pd.<span class="fn">read_sql</span>(
  <span class="str">"SELECT * FROM kpi_mensili ORDER BY anno DESC, mese DESC LIMIT 1"</span>,
  conn).<span class="fn">iloc</span>[<span class="nm">0</span>]

col1, col2, col3, col4 = st.<span class="fn">columns</span>(<span class="nm">4</span>)
col1.<span class="fn">metric</span>(<span class="str">"Fatturato MTD"</span>, <span class="fn">f</span><span class="str">"€{kpi.fatturato:,.0f}"</span>)
col2.<span class="fn">metric</span>(<span class="str">"SLA Medio"</span>,    <span class="fn">f</span><span class="str">"{kpi.sla_medio_ore:.1f}h"</span>)
col3.<span class="fn">metric</span>(<span class="str">"Churn Rate"</span>,   <span class="fn">f</span><span class="str">"{kpi.churn_rate:.1f}%"</span>)
col4.<span class="fn">metric</span>(<span class="str">"SLA ok &lt;24h"</span>,  <span class="fn">f</span><span class="str">"{kpi.sla_ok_pct:.0f}%"</span>)

<span class="cm"># Embed Power BI</span>
st.<span class="fn">components.v1.iframe</span>(
  <span class="str">"https://app.powerbi.com/reportEmbed?..."</span>,
  height=<span class="nm">600</span>)</div>
    </div>`
  },
  'st-interventi': {
    icon:'📋', layer:'05 · Streamlit', title:'Pagina Interventi',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Funzionalità</div>
      <ul class="p-list" style="--accent:#2563EB">
        <li>Tabella filtrabile per tecnico, data, tipo servizio, stato</li>
        <li>Form per inserire un nuovo intervento (scrive direttamente nel DB 1)</li>
        <li>Indicatore visivo SLA: verde se &lt;24h, rosso se oltre</li>
        <li>Export CSV della selezione con un click</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Filtri interattivi</div>
      <div class="p-code">tecnico = st.<span class="fn">selectbox</span>(<span class="str">"Tecnico"</span>, tecnici)
date_range = st.<span class="fn">date_input</span>(<span class="str">"Periodo"</span>, [])
stato = st.<span class="fn">multiselect</span>(<span class="str">"Stato"</span>,
    [<span class="str">"Completato"</span>,<span class="str">"Annullato"</span>,<span class="str">"In attesa"</span>])

df_filt = df[
  (df.tecnico == tecnico) &
  (df.stato.<span class="fn">isin</span>(stato))
]
st.<span class="fn">dataframe</span>(df_filt, use_container_width=<span class="kw">True</span>)</div>
    </div>`
  },
  'st-magazzino': {
    icon:'📦', layer:'05 · Streamlit', title:'Pagina Magazzino — semaforo',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Il semaforo visivo</div>
      <p class="p-text">Ogni prodotto ha un cerchio colorato: 🔴 esaurito · 🟡 riordino urgente · 🟢 OK. Il titolare capisce la situazione magazzino in 2 secondi senza leggere numeri.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Codice semaforo</div>
      <div class="p-code"><span class="kw">def</span> <span class="fn">semaforo</span>(stato):
    icons = {
      <span class="str">'ESAURITO'</span>:        <span class="str">'🔴 Esaurito'</span>,
      <span class="str">'RIORDINO URGENTE'</span>: <span class="str">'🟡 Riordina'</span>,
      <span class="str">'SCORTA BASSA'</span>:     <span class="str">'🟠 Attenzione'</span>,
      <span class="str">'OK'</span>:               <span class="str">'🟢 OK'</span>
    }
    <span class="kw">return</span> icons.<span class="fn">get</span>(stato, stato)

df[<span class="str">'Stato'</span>] = df[<span class="str">'stato_scorta'</span>].<span class="fn">apply</span>(semaforo)
st.<span class="fn">dataframe</span>(df[[<span class="str">'Nome_Commerciale'</span>,
  <span class="str">'Giacenza_Magazzino'</span>, <span class="str">'Stato'</span>]])</div>
    </div>`
  },
  'st-clienti': {
    icon:'👤', layer:'05 · Streamlit', title:'Pagina Clienti — anagrafica live',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Badge churn risk</div>
      <p class="p-text">Ogni cliente ha un badge colorato basato su <code>giorni_senza_intervento</code>: 🔴 A rischio churn (&gt;365gg) · 🟡 Attenzione (&gt;180gg) · 🟢 Attivo.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Ricerca e storico</div>
      <ul class="p-list" style="--accent:#2563EB">
        <li>Search box per cercare per nome/città/tipo</li>
        <li>Click su un cliente → espande lo storico interventi</li>
        <li>CLV storico, data ultimo intervento, fonte lead</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Valore commerciale</div>
      <p class="p-text">L'IA Setter (responsabile commerciale) apre questa pagina ogni mattina e chiama i clienti a rischio churn. <strong>Questo è il dato che salva i contratti.</strong></p>
    </div>`
  },

  /* ─── KPI ─── */
  'kpi-sla': {
    icon:'⚡', layer:'KPI Strategici', title:'SLA Risposta — la leva marketing',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Definizione precisa</div>
      <p class="p-text">Service Level Agreement di risposta: <strong>ore trascorse tra Data_Chiamata e l'inizio dell'intervento</strong>. Misura la reattività operativa dell'azienda alle urgenze.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Come si calcola nel DB</div>
      <div class="p-code"><span class="cm">-- Colonna GENERATED (calcolata da MySQL)</span>
SLA_ore = <span class="fn">TIMESTAMPDIFF</span>(<span class="nm">MINUTE</span>,
  Data_Chiamata,
  <span class="fn">CONCAT</span>(Data_Intervento,<span class="str">' '</span>,Ora_Inizio)
) / <span class="nm">60</span></div>
    </div>
    <div class="p-marketing">
      <div class="p-marketing-title">💡 Come usarlo nel marketing</div>
      <div class="p-marketing-text">
        Se il tuo SLA medio è 6 ore, quella diventa la headline del sito e dei preventivi:<br><br>
        <strong style="color:#D4F576">"Interveniamo entro 6 ore dalla tua chiamata — garantito"</strong><br><br>
        In Power BI mostri il trend mensile: se migliora nel tempo, hai un argomento commerciale che i competitor non possono copiare senza misurarlo. È un dato che <strong>vende contratti di abbonamento</strong>.
      </div>
    </div>
    <div class="p-section" style="margin-top:16px">
      <div class="p-label">Soglie consigliate</div>
      <ul class="p-list" style="--accent:#D4F576">
        <li>Urgenze (infestazioni attive): obiettivo &lt; 4 ore</li>
        <li>Interventi standard: obiettivo &lt; 24 ore</li>
        <li>Monitoraggi programmati: &lt; 72 ore</li>
      </ul>
    </div>`
  },
  'kpi-churn': {
    icon:'📉', layer:'KPI Strategici', title:'Churn Rate',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Definizione</div>
      <p class="p-text">Percentuale di clienti che non hanno fatto interventi negli ultimi 12 mesi sul totale clienti attivi. Un churn alto significa che stai perdendo contratti di abbonamento.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Formula SQL</div>
      <div class="p-code"><span class="kw">SELECT</span>
  <span class="fn">COUNT</span>(<span class="kw">CASE WHEN</span> giorni_senza_intervento > <span class="nm">365</span>
        <span class="kw">THEN</span> <span class="nm">1</span> <span class="kw">END</span>) * <span class="nm">100.0</span>
  / <span class="fn">COUNT</span>(*) <span class="kw">AS</span> churn_rate_pct
<span class="kw">FROM</span> v_storico_clienti;</div>
    </div>
    <div class="p-marketing">
      <div class="p-marketing-title">💡 Valore operativo</div>
      <div class="p-marketing-text">Se il churn scende dal 20% al 12% dopo aver introdotto i reminder automatici, hai dimostrato il ROI del sistema BI. Questo è l'argomento per vendere il progetto a un cliente reale.</div>
    </div>`
  },
  'kpi-clv': {
    icon:'💰', layer:'KPI Strategici', title:'CLV — Customer Lifetime Value',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Definizione</div>
      <p class="p-text">Valore totale generato da un cliente per tutta la durata del rapporto commerciale. Un cliente con CLV alto vale molto di più di uno con contratto di poco valore anche se i prezzi mensili sono simili.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Calcolo</div>
      <div class="p-code">CLV storico = <span class="fn">SUM</span>(Prezzo_Applicato)
  <span class="kw">WHERE</span> Stato = <span class="str">'Completato'</span>
  <span class="kw">GROUP BY</span> ID_Cliente

<span class="cm">-- Segmentazione utile:</span>
<span class="cm">-- TOP: CLV > 2000€  → coccola e non perdere</span>
<span class="cm">-- MID: CLV 500-2000 → upsell abbonamento</span>
<span class="cm">-- LOW: CLV < 500    → a rischio non rinnovare</span></div>
    </div>`
  },
  'kpi-conv': {
    icon:'🎯', layer:'KPI Strategici', title:'Conversion Rate — Lead → Cliente',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Cosa misura</div>
      <p class="p-text">Quanti dei lead (potenziali clienti) inseriti nel sistema diventano effettivamente clienti paganti. Fondamentale per valutare l'efficacia delle campagne marketing per canale.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Esempio pratico</div>
      <ul class="p-list" style="--accent:#6EE7B7">
        <li>Facebook Ads: 100 lead → 8 clienti = 8% conv rate</li>
        <li>Passaparola: 20 lead → 16 clienti = 80% conv rate</li>
        <li>Conclusione: il passaparola vale 10x il paid. Investire in referral program.</li>
      </ul>
    </div>`
  },
  'kpi-tecnici': {
    icon:'👷', layer:'KPI Strategici', title:'Ricavi per Tecnico',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">Metriche per tecnico</div>
      <ul class="p-list" style="--accent:#6EE7B7">
        <li>Ricavo totale generato nel periodo selezionato</li>
        <li>Numero interventi completati / totali</li>
        <li>Tasso di successo %</li>
        <li>SLA medio — chi è più reattivo?</li>
        <li>Zone geografiche coperte</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Uso pratico</div>
      <p class="p-text">Il titolare usa questa pagina per i colloqui di performance. Non è solo un numero — è un <strong>sistema obiettivo di valutazione</strong> che evita discussioni soggettive.</p>
    </div>`
  },
  'kpi-costo': {
    icon:'💸', layer:'KPI Strategici', title:'Costo medio intervento',
    sprint:2,
    body:`<div class="p-section">
      <div class="p-label">Marginalità per servizio</div>
      <p class="p-text">Ogni servizio ha un costo diverso (prodotti usati + ore tecnico). Confrontando il prezzo applicato con il costo stimato ottieni la <strong>marginalità per tipo di intervento</strong>.</p>
    </div>
    <div class="p-section">
      <div class="p-label">Insight tipico</div>
      <p class="p-text">Spesso la derattizzazione sembra più redditizia del monitoraggio ma ha costi prodotti più alti. Il dato reale può sorprendere. Con questo KPI il titolare decide <strong>quali servizi spingere</strong>.</p>
    </div>`
  },
  'kpi-stag': {
    icon:'📅', layer:'KPI Strategici', title:'Stagionalità infestanti',
    sprint:3,
    body:`<div class="p-section">
      <div class="p-label">La heatmap mensile</div>
      <p class="p-text">Matrice: infestante sulle righe, mesi dell'anno sulle colonne, colore = intensità interventi. Pattern storicamente confermati nei dati Green Control:</p>
    </div>
    <div class="p-section">
      <div class="p-label">Pattern stagionali</div>
      <ul class="p-list" style="--accent:#6EE7B7">
        <li>Blatte e blattelle: picco giugno–settembre (caldo umido)</li>
        <li>Zanzare: picco maggio–agosto</li>
        <li>Ratti: picco ottobre–dicembre (cercano caldo)</li>
        <li>Piccioni: costante tutto l'anno</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Uso pratico</div>
      <p class="p-text">Con questo dato pianifichi le <strong>campagne preventive</strong>: mandate email ai clienti con cucine o magazzini a maggio per prevenire l'infestazione estiva. Marketing proattivo invece che reattivo.</p>
    </div>`
  },

  /* ─── AUTOMAZIONI ─── */
  'auto-alert': {
    icon:'🚨', layer:'Automazioni', title:'Alert magazzino — Python cron',
    sprint:4,
    body:`<div class="p-section">
      <div class="p-label">Il flusso completo</div>
      <ul class="p-list" style="--accent:#F87171">
        <li>Ore 8:00: cron job lancia <code>alert.py</code></li>
        <li>Script legge <code>v_alert_magazzino</code> dal DB</li>
        <li>Se trova prodotti sotto soglia → compone l'email</li>
        <li>Invia via SMTP (Gmail app password o SendGrid)</li>
        <li>Log dell'invio su file per audit</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Demo all'esame</div>
      <p class="p-text">Abbassa manualmente la giacenza di BioRat a 0 nel database. Lancia lo script. L'email arriva in 3 secondi. Il prof vede la <strong>pipeline completa funzionante in tempo reale</strong>.</p>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">wow factor #1</span>
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">demo live 2 min</span>
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">sprint 4</span>
    </div>`
  },
  'auto-refresh': {
    icon:'🔄', layer:'Automazioni', title:'Scheduled Refresh Power BI',
    sprint:4,
    body:`<div class="p-section">
      <div class="p-label">Come si configura</div>
      <ul class="p-list" style="--accent:#F87171">
        <li>Pubblica il .pbix su Power BI Service (powerbi.com)</li>
        <li>Crea un Gateway dati per connettere il tuo MySQL</li>
        <li>Dataset Settings → Scheduled refresh → ogni 3 ore</li>
        <li>La dashboard è sempre aggiornata senza aprire Power BI Desktop</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Perché è importante</div>
      <p class="p-text">Senza questo il titolare dovrebbe aprire Power BI Desktop, fare refresh manuale e ricaricare il report. Con lo scheduled refresh <strong>apre il browser e i dati sono già aggiornati</strong>. Questo è il valore del cloud.</p>
    </div>`
  },
  'auto-pdf': {
    icon:'📄', layer:'Automazioni', title:'Report PDF automatico — reportlab',
    sprint:4,
    body:`<div class="p-section">
      <div class="p-label">Cosa genera</div>
      <ul class="p-list" style="--accent:#F87171">
        <li>Prima pagina: logo Green Control + KPI del mese in grande</li>
        <li>Tabella interventi del mese con tecnico, cliente, prezzo, SLA</li>
        <li>Grafico a barre fatturato ultimi 6 mesi (generato con matplotlib)</li>
        <li>Alert magazzino se presenti</li>
      </ul>
    </div>
    <div class="p-section">
      <div class="p-label">Come si attiva</div>
      <p class="p-text">Un pulsante in Streamlit chiama <code>genera_report_pdf(mese, anno)</code>. Il PDF viene generato in memoria e scaricato subito. Oppure inviato automaticamente via email il primo del mese.</p>
    </div>
    <div class="p-badge-row">
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">reportlab</span>
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">matplotlib</span>
      <span class="p-badge" style="color:#FCA5A5;border-color:#4D1F1F">wow factor #3</span>
    </div>`
  }
};

/* ── SPRINT META ── */
const SPRINT_INFO = {
  1: { label:'Sprint 1', color:'#9CA3AF', desc:'Fondamenta — 8 ore' },
  2: { label:'Sprint 2', color:'#F59E0B', desc:'Python ETL + DW — 10 ore' },
  3: { label:'Sprint 3+4', color:'#A78BFA', desc:'Power BI + Streamlit — 17 ore' },
  4: { label:'Sprint 4', color:'#F87171', desc:'Automazioni — 7 ore' },
};

/* ── DOM refs ── */
const panel    = document.getElementById('panel');
const mapArea  = document.getElementById('mapArea');
const overlay  = document.getElementById('overlay');
const panelBody= document.getElementById('panelBody');
const panelTitle= document.getElementById('panelTitle');
const panelLayer= document.getElementById('panelLayer');
const panelIcon = document.getElementById('panelIcon');
const panelTop  = document.getElementById('panelTop');

let activeEl = null;

function isMobile() { return window.innerWidth <= 768; }

function openPanel(id, el) {
  const d = CONTENT[id];
  if (!d) return;
  if (activeEl) activeEl.classList.remove('active');
  activeEl = el;
  el.classList.add('active');

  panelIcon.textContent  = d.icon;
  panelLayer.textContent = d.layer;
  panelTitle.textContent = d.title;

  const sp = SPRINT_INFO[d.sprint] || SPRINT_INFO[1];
  const spBadge = `<div class="p-sprint-badge" style="background:${sp.color}20;border:1px solid ${sp.color}40;color:${sp.color}">${sp.label} — ${sp.desc}</div>`;
  const spBar = `<div class="sprint-bar">
    <div class="sprint-bar-label">Posizione nel piano 40 ore</div>
    <div class="sprint-steps">
      ${[1,2,3,4,5].map(i=>{
        const cls = i < d.sprint ? 'done' : i === d.sprint ? 'current' : '';
        return `<div class="sprint-step ${cls}"><span class="sprint-step-label">S${i}</span></div>`;
      }).join('')}
    </div>
  </div>`;

  panelBody.innerHTML = spBadge + d.body + spBar;

  panel.classList.add('open');
  if (!isMobile()) mapArea.classList.add('shifted');
  overlay.classList.add('visible');
}

function closePanel() {
  panel.classList.remove('open');
  mapArea.classList.remove('shifted');
  overlay.classList.remove('visible');
  if (activeEl) { activeEl.classList.remove('active'); activeEl = null; }
}

/* ── EVENT DELEGATION ── */
document.addEventListener('click', e => {
  const node = e.target.closest('[data-id]');
  if (!node) return;
  const id = node.dataset.id;
  if (activeEl === node && panel.classList.contains('open')) {
    closePanel(); return;
  }
  openPanel(id, node);
});

document.getElementById('panelClose').addEventListener('click', closePanel);
overlay.addEventListener('click', closePanel);

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closePanel();
});
</script>
</body>
</html>