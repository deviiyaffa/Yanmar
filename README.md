<!DOCTYPE html>

<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Yanmar — Suivi Temps de Cycle</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;600;700;800;900&family=Barlow:wght@400;500;600&display=swap');

:root {
–red: #C8102E;
–red-dark: #9e0c24;
–red-glow: rgba(200,16,46,0.3);
–black: #0f0f0f;
–dark: #1a1a1a;
–panel: #222222;
–border: #333333;
–text: #f0f0f0;
–muted: #888;
–green: #22c55e;
–orange: #f97316;
–yellow: #eab308;
}

- { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

body {
background: var(–black);
color: var(–text);
font-family: ‘Barlow’, sans-serif;
min-height: 100vh;
overflow-x: hidden;
}

/* ── HEADER ── */
header {
background: var(–dark);
border-bottom: 3px solid var(–red);
padding: 12px 16px;
display: flex;
align-items: center;
justify-content: space-between;
position: sticky;
top: 0;
z-index: 100;
}

.logo-area {
display: flex;
align-items: center;
gap: 10px;
}

.logo-chevron {
width: 36px;
height: 26px;
}

.app-title {
font-family: ‘Barlow Condensed’, sans-serif;
font-weight: 800;
font-size: 18px;
letter-spacing: 1px;
line-height: 1.1;
}
.app-title span { color: var(–red); }
.app-subtitle { font-size: 10px; color: var(–muted); letter-spacing: 2px; text-transform: uppercase; }

.header-right {
text-align: right;
}
.date-display {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 13px;
font-weight: 600;
color: var(–muted);
letter-spacing: 1px;
}
.time-display {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 20px;
font-weight: 800;
color: var(–text);
letter-spacing: 2px;
}

/* ── INFOS OPÉRATEUR ── */
.operator-bar {
background: var(–panel);
padding: 10px 16px;
display: flex;
gap: 8px;
border-bottom: 1px solid var(–border);
}
.op-field {
flex: 1;
}
.op-label {
font-size: 9px;
color: var(–red);
font-weight: 600;
letter-spacing: 1.5px;
text-transform: uppercase;
margin-bottom: 3px;
}
.op-input {
background: var(–dark);
border: 1px solid var(–border);
border-radius: 6px;
color: var(–text);
font-family: ‘Barlow’, sans-serif;
font-size: 13px;
padding: 6px 8px;
width: 100%;
outline: none;
}
.op-input:focus { border-color: var(–red); }

/* ── STATS BAR ── */
.stats-bar {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
gap: 1px;
background: var(–border);
border-bottom: 2px solid var(–border);
}
.stat-box {
background: var(–panel);
padding: 10px 8px;
text-align: center;
}
.stat-val {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 22px;
font-weight: 900;
line-height: 1;
color: var(–text);
}
.stat-val.red { color: var(–red); }
.stat-val.green { color: var(–green); }
.stat-val.orange { color: var(–orange); }
.stat-label {
font-size: 9px;
color: var(–muted);
letter-spacing: 1px;
text-transform: uppercase;
margin-top: 3px;
}

/* ── PROGRESS BAR ── */
.progress-wrap {
padding: 10px 16px 6px;
background: var(–dark);
}
.progress-header {
display: flex;
justify-content: space-between;
font-size: 11px;
color: var(–muted);
margin-bottom: 6px;
}
.progress-track {
height: 8px;
background: var(–border);
border-radius: 4px;
overflow: hidden;
}
.progress-fill {
height: 100%;
border-radius: 4px;
background: var(–green);
transition: width 0.4s ease, background 0.3s;
}
.progress-fill.warn { background: var(–orange); }
.progress-fill.over { background: var(–red); }

/* ── MACHINE ACTIVE (CHRONO) ── */
.chrono-section {
padding: 12px 16px;
background: var(–dark);
border-bottom: 1px solid var(–border);
}

.machine-selector {
display: flex;
align-items: center;
gap: 10px;
margin-bottom: 12px;
}
.machine-label {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 13px;
font-weight: 700;
color: var(–muted);
letter-spacing: 2px;
text-transform: uppercase;
white-space: nowrap;
}
.machine-num {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 36px;
font-weight: 900;
color: var(–red);
line-height: 1;
min-width: 50px;
}

/* Prefix toggle */
.prefix-toggle {
display: flex;
gap: 8px;
flex: 1;
justify-content: flex-end;
}
.prefix-btn {
padding: 8px 16px;
border-radius: 8px;
border: 2px solid var(–border);
background: var(–panel);
color: var(–muted);
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 16px;
font-weight: 800;
letter-spacing: 1px;
cursor: pointer;
transition: all 0.15s;
}
.prefix-btn.active {
border-color: var(–red);
background: var(–red);
color: white;
}

/* Ref input */
.ref-row {
display: flex;
align-items: center;
gap: 8px;
margin-bottom: 14px;
}
.ref-prefix-tag {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 18px;
font-weight: 800;
color: var(–red);
min-width: 36px;
}
.ref-input {
flex: 1;
background: var(–panel);
border: 1px solid var(–border);
border-radius: 8px;
color: var(–text);
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 20px;
font-weight: 700;
padding: 10px 14px;
outline: none;
letter-spacing: 2px;
}
.ref-input:focus { border-color: var(–red); }
.ref-input::placeholder { color: var(–border); }

/* Big chrono display */
.chrono-display {
text-align: center;
padding: 16px 0 8px;
}
.chrono-time {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 72px;
font-weight: 900;
letter-spacing: 4px;
line-height: 1;
color: var(–text);
transition: color 0.3s;
}
.chrono-time.running { color: var(–green); }
.chrono-time.warn { color: var(–orange); }
.chrono-unit {
font-size: 13px;
color: var(–muted);
letter-spacing: 3px;
text-transform: uppercase;
margin-top: 4px;
}

/* Checkboxes */
.checks-row {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
gap: 8px;
margin: 12px 0;
}
.check-item {
background: var(–panel);
border: 2px solid var(–border);
border-radius: 10px;
padding: 10px 6px;
text-align: center;
cursor: pointer;
transition: all 0.15s;
user-select: none;
}
.check-item.checked {
border-color: var(–green);
background: rgba(34,197,94,0.1);
}
.check-icon {
font-size: 20px;
line-height: 1;
margin-bottom: 4px;
}
.check-name {
font-size: 9px;
font-weight: 600;
letter-spacing: 0.5px;
text-transform: uppercase;
color: var(–muted);
}
.check-item.checked .check-name { color: var(–green); }

/* Notes */
.notes-input {
width: 100%;
background: var(–panel);
border: 1px solid var(–border);
border-radius: 8px;
color: var(–text);
font-family: ‘Barlow’, sans-serif;
font-size: 13px;
padding: 10px 12px;
resize: none;
outline: none;
margin-bottom: 12px;
}
.notes-input:focus { border-color: var(–red); }

/* Buttons */
.btn-row {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 10px;
}
.btn {
padding: 18px;
border-radius: 12px;
border: none;
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 20px;
font-weight: 800;
letter-spacing: 2px;
cursor: pointer;
transition: all 0.15s;
text-transform: uppercase;
}
.btn:active { transform: scale(0.97); }

.btn-start {
background: var(–green);
color: white;
grid-column: 1 / -1;
}
.btn-stop {
background: var(–red);
color: white;
}
.btn-cancel {
background: var(–panel);
color: var(–muted);
border: 2px solid var(–border);
}
.btn-save {
background: var(–red);
color: white;
grid-column: 1 / -1;
font-size: 22px;
}

/* ── LISTE MACHINES ── */
.list-section {
padding: 12px 16px;
}
.list-title {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 13px;
font-weight: 700;
color: var(–muted);
letter-spacing: 3px;
text-transform: uppercase;
margin-bottom: 10px;
display: flex;
justify-content: space-between;
align-items: center;
}
.clear-btn {
font-size: 10px;
color: var(–red);
cursor: pointer;
letter-spacing: 1px;
background: none;
border: none;
font-family: ‘Barlow’, sans-serif;
font-weight: 600;
padding: 4px 8px;
border-radius: 4px;
border: 1px solid var(–red);
}

.machine-row {
background: var(–panel);
border-radius: 10px;
padding: 10px 12px;
margin-bottom: 8px;
display: flex;
align-items: center;
gap: 10px;
border-left: 4px solid var(–border);
position: relative;
}
.machine-row.vio { border-left-color: var(–red); }
.machine-row.sv  { border-left-color: #3b82f6; }

.row-num {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 22px;
font-weight: 900;
color: var(–muted);
min-width: 28px;
}
.row-ref {
flex: 1;
}
.row-ref-main {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 18px;
font-weight: 800;
letter-spacing: 1px;
}
.row-ref-checks {
font-size: 10px;
color: var(–muted);
margin-top: 2px;
}
.row-ref-notes {
font-size: 11px;
color: var(–orange);
margin-top: 2px;
font-style: italic;
}
.row-time {
text-align: right;
}
.row-time-val {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 28px;
font-weight: 900;
color: var(–text);
}
.row-time-unit {
font-size: 10px;
color: var(–muted);
letter-spacing: 1px;
}
.row-cumul {
text-align: right;
min-width: 50px;
}
.row-cumul-val {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 13px;
font-weight: 700;
color: var(–muted);
}

.delete-btn {
background: none;
border: none;
color: var(–border);
font-size: 18px;
cursor: pointer;
padding: 4px;
}
.delete-btn:active { color: var(–red); }

/* ── RÉCAP ── */
.recap-section {
padding: 16px;
background: var(–panel);
margin: 12px 16px;
border-radius: 12px;
border: 1px solid var(–border);
}
.recap-title {
font-family: ‘Barlow Condensed’, sans-serif;
font-weight: 800;
font-size: 14px;
letter-spacing: 2px;
color: var(–red);
text-transform: uppercase;
margin-bottom: 12px;
}
.recap-grid {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 10px;
margin-bottom: 14px;
}
.recap-item {
background: var(–dark);
border-radius: 8px;
padding: 10px;
text-align: center;
}
.recap-item-val {
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 28px;
font-weight: 900;
}
.recap-item-label {
font-size: 9px;
color: var(–muted);
letter-spacing: 1.5px;
text-transform: uppercase;
margin-top: 2px;
}

.empty-state {
text-align: center;
padding: 30px;
color: var(–muted);
font-size: 13px;
letter-spacing: 1px;
}
.empty-icon { font-size: 36px; margin-bottom: 8px; }

/* ── RECAP TABLE for copy ── */
.recap-table {
width: 100%;
border-collapse: collapse;
font-size: 11px;
margin-top: 10px;
}
.recap-table th {
background: var(–dark);
color: var(–red);
padding: 6px 8px;
text-align: left;
font-family: ‘Barlow Condensed’, sans-serif;
letter-spacing: 1px;
font-size: 10px;
text-transform: uppercase;
}
.recap-table td {
padding: 7px 8px;
border-bottom: 1px solid var(–border);
font-family: ‘Barlow Condensed’, sans-serif;
font-size: 14px;
font-weight: 600;
}
.recap-table tr:nth-child(even) td { background: rgba(255,255,255,0.02); }

.bottom-space { height: 40px; }
</style>

</head>
<body>

<!-- HEADER -->

<header>
  <div class="logo-area">
    <svg class="logo-chevron" viewBox="0 0 100 73" fill="none">
      <path d="M0 0 L50 35 L100 0 L100 18 L50 53 L0 18 Z" fill="#C8102E"/>
      <path d="M0 25 L50 60 L100 25 L100 43 L50 73 L0 43 Z" fill="#C8102E" opacity="0.7"/>
    </svg>
    <div>
      <div class="app-title"><span>YANMAR</span> CHRONO</div>
      <div class="app-subtitle">Temps de cycle</div>
    </div>
  </div>
  <div class="header-right">
    <div class="date-display" id="dateDisplay"></div>
    <div class="time-display" id="clockDisplay"></div>
  </div>
</header>

<!-- OPÉRATEUR -->

<div class="operator-bar">
  <div class="op-field">
    <div class="op-label">Opérateur</div>
    <input class="op-input" id="operatorInput" type="text" placeholder="Votre nom">
  </div>
  <div class="op-field" style="max-width:110px">
    <div class="op-label">Poste / Ligne</div>
    <input class="op-input" id="posteInput" type="text" placeholder="ex: L3">
  </div>
</div>

<!-- STATS -->

<div class="stats-bar">
  <div class="stat-box">
    <div class="stat-val red" id="statDone">0</div>
    <div class="stat-label">/ 23 machines</div>
  </div>
  <div class="stat-box">
    <div class="stat-val" id="statTotal">0</div>
    <div class="stat-label">min total</div>
  </div>
  <div class="stat-box">
    <div class="stat-val green" id="statRestant">460</div>
    <div class="stat-label">min restantes</div>
  </div>
  <div class="stat-box">
    <div class="stat-val orange" id="statTaux">0%</div>
    <div class="stat-label">réalisation</div>
  </div>
</div>

<!-- PROGRESS -->

<div class="progress-wrap">
  <div class="progress-header">
    <span>Progression journée</span>
    <span id="progressLabel">0 / 460 min</span>
  </div>
  <div class="progress-track">
    <div class="progress-fill" id="progressFill" style="width:0%"></div>
  </div>
</div>

<!-- CHRONO SECTION -->

<div class="chrono-section">
  <div class="machine-selector">
    <div>
      <div class="machine-label">Machine</div>
      <div class="machine-num" id="machineNum">01</div>
    </div>
    <div class="prefix-toggle">
      <button class="prefix-btn active" id="btnVIO" onclick="setPrefix('VIO')">VIO</button>
      <button class="prefix-btn" id="btnSV" onclick="setPrefix('SV')">SV</button>
    </div>
  </div>

  <div class="ref-row">
    <div class="ref-prefix-tag" id="refPrefixTag">VIO-</div>
    <input class="ref-input" id="refInput" type="text" placeholder="référence…" inputmode="text">
  </div>

  <div class="chrono-display">
    <div class="chrono-time" id="chronoDisplay">00:00</div>
    <div class="chrono-unit">min : sec</div>
  </div>

  <div class="checks-row">
    <div class="check-item" id="chk0" onclick="toggleCheck(0)">
      <div class="check-icon">🔗</div>
      <div class="check-name">Att. Rap.</div>
    </div>
    <div class="check-item" id="chk1" onclick="toggleCheck(1)">
      <div class="check-icon">⚡</div>
      <div class="check-name">½ Circuits</div>
    </div>
    <div class="check-item" id="chk2" onclick="toggleCheck(2)">
      <div class="check-icon">💧</div>
      <div class="check-name">Drain</div>
    </div>
    <div class="check-item" id="chk3" onclick="toggleCheck(3)">
      <div class="check-icon">3️⃣</div>
      <div class="check-name">3ème</div>
    </div>
  </div>

  <textarea class="notes-input" id="notesInput" rows="2" placeholder="Inconvénients / Problèmes / Pause (min)…"></textarea>

  <div class="btn-row" id="btnRow">
    <button class="btn btn-start" onclick="startChrono()">▶ DÉMARRER</button>
  </div>
</div>

<!-- LISTE MACHINES -->

<div class="list-section">
  <div class="list-title">
    <span>Machines enregistrées</span>
    <button class="clear-btn" onclick="clearAll()">TOUT EFFACER</button>
  </div>
  <div id="machineList">
    <div class="empty-state">
      <div class="empty-icon">⏱️</div>
      Aucune machine enregistrée
    </div>
  </div>
</div>

<!-- RÉCAP -->

<div class="recap-section" id="recapSection" style="display:none">
  <div class="recap-title">📋 Récapitulatif journée</div>
  <div class="recap-grid">
    <div class="recap-item">
      <div class="recap-item-val red" id="recapDone">0</div>
      <div class="recap-item-label">machines / 23</div>
    </div>
    <div class="recap-item">
      <div class="recap-item-val" id="recapTotal">0</div>
      <div class="recap-item-label">min total</div>
    </div>
    <div class="recap-item">
      <div class="recap-item-val green" id="recapTaux">0%</div>
      <div class="recap-item-label">taux réalisation</div>
    </div>
    <div class="recap-item">
      <div class="recap-item-val orange" id="recapRestant">460</div>
      <div class="recap-item-label">min restantes</div>
    </div>
  </div>

  <table class="recap-table" id="recapTable">
    <thead>
      <tr>
        <th>N°</th>
        <th>Référence</th>
        <th>Temps</th>
        <th>Cumulé</th>
        <th>Options</th>
        <th>Notes</th>
      </tr>
    </thead>
    <tbody id="recapBody"></tbody>
  </table>
</div>

<div class="bottom-space"></div>

<script>
  // ── State ──
  let machines = JSON.parse(localStorage.getItem('yanmar_machines') || '[]');
  let currentPrefix = 'VIO';
  let chronoRunning = false;
  let chronoStart = null;
  let chronoInterval = null;
  let checks = [false, false, false, false];

  // ── Clock ──
  function updateClock() {
    const now = new Date();
    const days = ['Dim','Lun','Mar','Mer','Jeu','Ven','Sam'];
    const months = ['Jan','Fév','Mar','Avr','Mai','Jun','Jul','Aoû','Sep','Oct','Nov','Déc'];
    document.getElementById('dateDisplay').textContent =
      `${days[now.getDay()]} ${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`;
    document.getElementById('clockDisplay').textContent =
      `${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
  }
  setInterval(updateClock, 1000);
  updateClock();

  // ── Prefix ──
  function setPrefix(p) {
    currentPrefix = p;
    document.getElementById('btnVIO').classList.toggle('active', p === 'VIO');
    document.getElementById('btnSV').classList.toggle('active', p === 'SV');
    document.getElementById('refPrefixTag').textContent = p + '-';
  }

  // ── Checks ──
  function toggleCheck(i) {
    checks[i] = !checks[i];
    document.getElementById('chk' + i).classList.toggle('checked', checks[i]);
  }
  function resetChecks() {
    checks = [false, false, false, false];
    for (let i = 0; i < 4; i++)
      document.getElementById('chk' + i).classList.remove('checked');
  }

  // ── Chrono ──
  function formatTime(ms) {
    const totalSec = Math.floor(ms / 1000);
    const m = Math.floor(totalSec / 60);
    const s = totalSec % 60;
    return `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
  }
  function formatMin(ms) {
    return (ms / 60000).toFixed(1);
  }

  function startChrono() {
    chronoRunning = true;
    chronoStart = Date.now();
    chronoInterval = setInterval(() => {
      const elapsed = Date.now() - chronoStart;
      const display = document.getElementById('chronoDisplay');
      display.textContent = formatTime(elapsed);
      const mins = elapsed / 60000;
      display.className = 'chrono-time' + (mins > 25 ? ' warn' : ' running');
    }, 500);
    document.getElementById('btnRow').innerHTML = `
      <button class="btn btn-stop" onclick="stopChrono()">⏹ STOP</button>
      <button class="btn btn-cancel" onclick="cancelChrono()">✕ ANNULER</button>
    `;
  }

  function stopChrono() {
    if (!chronoRunning) return;
    clearInterval(chronoInterval);
    chronoRunning = false;
    const elapsed = Date.now() - chronoStart;
    const mins = parseFloat(formatMin(elapsed));
    document.getElementById('btnRow').innerHTML = `
      <button class="btn btn-save" onclick="saveMachine(${mins})">✔ ENREGISTRER — ${mins} min</button>
    `;
  }

  function cancelChrono() {
    clearInterval(chronoInterval);
    chronoRunning = false;
    document.getElementById('chronoDisplay').textContent = '00:00';
    document.getElementById('chronoDisplay').className = 'chrono-time';
    resetBtnRow();
  }

  function saveMachine(mins) {
    const ref = document.getElementById('refInput').value.trim() || '—';
    const notes = document.getElementById('notesInput').value.trim();
    const checkLabels = ['Att.Rap', '½Circ', 'Drain', '3ème'];
    const activeChecks = checkLabels.filter((_, i) => checks[i]);

    const totalSoFar = machines.reduce((s, m) => s + m.mins, 0) + mins;

    machines.push({
      num: machines.length + 1,
      prefix: currentPrefix,
      ref,
      mins,
      cumul: parseFloat(totalSoFar.toFixed(1)),
      checks: [...checks],
      activeChecks,
      notes
    });

    localStorage.setItem('yanmar_machines', JSON.stringify(machines));

    // Reset UI
    document.getElementById('chronoDisplay').textContent = '00:00';
    document.getElementById('chronoDisplay').className = 'chrono-time';
    document.getElementById('refInput').value = '';
    document.getElementById('notesInput').value = '';
    resetChecks();
    resetBtnRow();
    updateMachineNum();
    renderList();
    updateStats();
  }

  function resetBtnRow() {
    document.getElementById('btnRow').innerHTML = `
      <button class="btn btn-start" onclick="startChrono()">▶ DÉMARRER</button>
    `;
  }

  function updateMachineNum() {
    document.getElementById('machineNum').textContent =
      String(machines.length + 1).padStart(2, '0');
  }

  // ── Stats ──
  function updateStats() {
    const done = machines.length;
    const total = parseFloat(machines.reduce((s, m) => s + m.mins, 0).toFixed(1));
    const restant = parseFloat((460 - total).toFixed(1));
    const taux = done > 0 ? Math.round((done / 23) * 100) : 0;

    document.getElementById('statDone').textContent = done;
    document.getElementById('statTotal').textContent = total;
    document.getElementById('statRestant').textContent = restant;
    document.getElementById('statTaux').textContent = taux + '%';
    document.getElementById('progressLabel').textContent = `${total} / 460 min`;

    const pct = Math.min((total / 460) * 100, 100);
    const fill = document.getElementById('progressFill');
    fill.style.width = pct + '%';
    fill.className = 'progress-fill' + (pct > 100 ? ' over' : pct > 85 ? ' warn' : '');

    // Récap
    if (done > 0) {
      document.getElementById('recapSection').style.display = 'block';
      document.getElementById('recapDone').textContent = done;
      document.getElementById('recapTotal').textContent = total;
      document.getElementById('recapTaux').textContent = taux + '%';
      document.getElementById('recapRestant').textContent = restant;
      renderRecapTable();
    }
  }

  // ── Render list ──
  function renderList() {
    const el = document.getElementById('machineList');
    if (machines.length === 0) {
      el.innerHTML = `<div class="empty-state"><div class="empty-icon">⏱️</div>Aucune machine enregistrée</div>`;
      return;
    }
    el.innerHTML = machines.map((m, i) => `
      <div class="machine-row ${m.prefix.toLowerCase()}">
        <div class="row-num">${String(m.num).padStart(2,'0')}</div>
        <div class="row-ref">
          <div class="row-ref-main">${m.prefix}-${m.ref}</div>
          ${m.activeChecks.length ? `<div class="row-ref-checks">✓ ${m.activeChecks.join(' · ')}</div>` : ''}
          ${m.notes ? `<div class="row-ref-notes">⚠ ${m.notes}</div>` : ''}
        </div>
        <div class="row-time">
          <div class="row-time-val">${m.mins}</div>
          <div class="row-time-unit">MIN</div>
        </div>
        <div class="row-cumul">
          <div class="row-cumul-val">${m.cumul}</div>
          <div class="row-time-unit" style="font-size:8px">CUMUL</div>
        </div>
        <button class="delete-btn" onclick="deleteMachine(${i})">✕</button>
      </div>
    `).reverse().join('');
  }

  function renderRecapTable() {
    const tbody = document.getElementById('recapBody');
    const checkNames = ['A.R', '½C', 'Dr', '3e'];
    tbody.innerHTML = machines.map(m => `
      <tr>
        <td>${m.num}</td>
        <td>${m.prefix}-${m.ref}</td>
        <td style="color:var(--red);font-weight:900">${m.mins} min</td>
        <td style="color:var(--muted)">${m.cumul}</td>
        <td>${checkNames.filter((_, i) => m.checks[i]).join(' ')}</td>
        <td style="color:var(--orange);font-size:11px">${m.notes || '—'}</td>
      </tr>
    `).join('');
  }

  function deleteMachine(i) {
    machines.splice(i, 1);
    // Recalculate nums and cumuls
    let cumul = 0;
    machines = machines.map((m, idx) => {
      cumul += m.mins;
      return { ...m, num: idx + 1, cumul: parseFloat(cumul.toFixed(1)) };
    });
    localStorage.setItem('yanmar_machines', JSON.stringify(machines));
    updateMachineNum();
    renderList();
    updateStats();
    if (machines.length === 0) document.getElementById('recapSection').style.display = 'none';
  }

  function clearAll() {
    if (confirm('Effacer toutes les machines de la journée ?')) {
      machines = [];
      localStorage.removeItem('yanmar_machines');
      updateMachineNum();
      renderList();
      updateStats();
      document.getElementById('recapSection').style.display = 'none';
    }
  }

  // ── Init ──
  updateMachineNum();
  renderList();
  updateStats();
</script>

</body>
</html>