<!DOCTYPE html>
<html lang="hr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>Evidencija pića</title>

<!-- PWA meta -->
<meta name="application-name" content="Evidencija pića">
<meta name="description" content="Evidencija prodanih pića — unos artikala, praćenje prodaje i rekapitulacija zarade.">
<meta name="theme-color" content="#2d7a4f">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Pića">

<!-- Manifest -->
<link rel="manifest" href="manifest.json">

<!-- Favicon -->
<link rel="icon" type="image/png" sizes="32x32" href="icons/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="icons/favicon-16x16.png">
<link rel="apple-touch-icon" href="icons/apple-touch-icon.png">

<!-- Splash / tile -->
<meta name="msapplication-TileImage" content="icons/icon-144x144.png">
<meta name="msapplication-TileColor" content="#2d7a4f">

<style>
  :root {
    --bg: #f8f7f4;
    --surface: #ffffff;
    --surface2: #f1efeb;
    --border: #e0ddd8;
    --border2: #cbc8c2;
    --text: #1a1917;
    --text2: #6b6862;
    --text3: #a09d98;
    --green: #2d7a4f;
    --green-bg: #eaf5ef;
    --red: #b83232;
    --red-bg: #fdf0f0;
    --blue: #1a5fa8;
    --blue-bg: #eaf0fb;
    --radius: 12px;
    --radius-sm: 8px;
    --safe-top: env(safe-area-inset-top, 0px);
    --safe-bot: env(safe-area-inset-bottom, 0px);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    min-height: 100dvh;
    font-size: 15px;
    overscroll-behavior: none;
  }
  #app {
    max-width: 460px;
    margin: 0 auto;
    padding-top: var(--safe-top);
    padding-bottom: calc(32px + var(--safe-bot));
  }

  .header {
    padding: 18px 16px 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .header-title { font-size: 19px; font-weight: 600; letter-spacing: -0.3px; }
  .header-sub { font-size: 12px; color: var(--text3); margin-top: 2px; }
  .badge {
    background: var(--surface2);
    border: 0.5px solid var(--border);
    border-radius: 20px;
    padding: 5px 12px;
    font-size: 13px;
    color: var(--text2);
  }

  .tabs {
    display: flex;
    gap: 6px;
    padding: 14px 16px;
    border-bottom: 0.5px solid var(--border);
  }
  .tab {
    flex: 1;
    padding: 9px 4px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    background: transparent;
    color: var(--text2);
    font-size: 13px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
    font-family: inherit;
    transition: all 0.12s;
  }
  .tab.active {
    border: 1.5px solid var(--blue);
    background: var(--blue-bg);
    color: var(--blue);
    font-weight: 500;
  }
  .tab svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 1.8; flex-shrink: 0; }

  .content { padding: 16px; }

  .card {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 13px 14px;
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 9px;
  }
  .card-info { flex: 1; min-width: 0; }
  .card-name { font-weight: 500; font-size: 15px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .card-price { font-size: 13px; color: var(--text2); margin-top: 2px; }

  .counter { display: flex; align-items: center; gap: 8px; }
  .count-num { min-width: 28px; text-align: center; font-size: 18px; font-weight: 500; }
  .count-num.zero { color: var(--text3); }
  .btn-round {
    width: 40px; height: 40px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    background: transparent;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
    transition: transform 0.1s;
  }
  .btn-round:active { transform: scale(0.91); }
  .btn-round.add { background: var(--green-bg); border-color: #b2d9c0; }
  .btn-round.add svg { stroke: var(--green); }
  .btn-round.sub svg { stroke: var(--red); }
  .btn-round:disabled { opacity: 0.3; cursor: default; }
  .btn-round svg { width: 17px; height: 17px; stroke: currentColor; fill: none; stroke-width: 2; }

  .form-box {
    background: var(--surface2);
    border-radius: var(--radius);
    padding: 14px;
    margin-bottom: 16px;
  }
  .form-label { font-size: 13px; font-weight: 500; color: var(--text2); margin-bottom: 10px; }
  input[type=text], input[type=number] {
    width: 100%;
    padding: 11px 12px;
    border: 0.5px solid var(--border2);
    border-radius: var(--radius-sm);
    font-size: 15px;
    font-family: inherit;
    background: var(--surface);
    color: var(--text);
    margin-bottom: 8px;
    outline: none;
    transition: border-color 0.15s;
    -webkit-appearance: none;
  }
  input:focus { border-color: var(--blue); }
  .btn-row { display: flex; gap: 8px; }
  .btn {
    flex: 1;
    padding: 11px;
    border-radius: var(--radius-sm);
    font-size: 14px;
    font-family: inherit;
    cursor: pointer;
    font-weight: 500;
    border: 0.5px solid var(--border);
    background: transparent;
    color: var(--text2);
    transition: transform 0.1s;
  }
  .btn:active { transform: scale(0.97); }
  .btn.primary { background: var(--green-bg); border-color: #b2d9c0; color: var(--green); }
  .btn.neutral { flex: 0 0 auto; padding: 11px 14px; }

  .btn-icon {
    width: 36px; height: 36px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    background: transparent;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
  }
  .btn-icon:active { transform: scale(0.91); }
  .btn-icon svg { width: 16px; height: 16px; stroke: currentColor; fill: none; stroke-width: 1.8; }
  .btn-icon.edit svg { stroke: var(--text2); }
  .btn-icon.del svg { stroke: var(--red); }

  .metric-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
  .metric { background: var(--surface2); border-radius: var(--radius-sm); padding: 14px 12px; }
  .metric-label { font-size: 12px; color: var(--text2); margin-bottom: 4px; }
  .metric-val { font-size: 20px; font-weight: 600; }
  .metric-val.green { color: var(--green); }

  .recap-row {
    background: var(--surface);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 12px 14px;
    display: flex; align-items: center;
    margin-bottom: 9px;
  }
  .recap-total { font-weight: 500; font-size: 16px; color: var(--green); }
  .total-line {
    border-top: 0.5px solid var(--border2);
    padding-top: 12px; margin-top: 4px;
    display: flex; justify-content: space-between; align-items: center;
  }
  .total-line span:first-child { font-weight: 500; font-size: 15px; }
  .total-line span:last-child { font-weight: 600; font-size: 18px; color: var(--green); }

  .empty { text-align: center; padding: 36px 16px; color: var(--text2); font-size: 14px; line-height: 1.7; }
  .empty svg { width: 36px; height: 36px; stroke: var(--text3); fill: none; stroke-width: 1.4; display: block; margin: 0 auto 10px; }

  .reset-btn {
    width: 100%; margin-top: 8px;
    padding: 11px;
    border-radius: var(--radius-sm);
    border: 0.5px solid #f0b8b8;
    background: transparent;
    color: var(--red);
    font-size: 14px; font-family: inherit;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center; gap: 6px;
  }
  .reset-btn svg { width: 15px; height: 15px; stroke: currentColor; fill: none; stroke-width: 2; }

  .toast {
    position: fixed;
    bottom: calc(24px + var(--safe-bot));
    left: 50%;
    transform: translateX(-50%) translateY(80px);
    background: #1a1917;
    color: #fff;
    padding: 10px 20px;
    border-radius: 20px;
    font-size: 14px;
    transition: transform 0.25s ease;
    pointer-events: none;
    z-index: 99;
    white-space: nowrap;
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  /* Install banner */
  #install-banner {
    display: none;
    margin: 12px 16px 0;
    background: var(--blue-bg);
    border: 0.5px solid #aac8ee;
    border-radius: var(--radius);
    padding: 12px 14px;
    align-items: center;
    gap: 10px;
  }
  #install-banner.visible { display: flex; }
  #install-banner p { flex: 1; font-size: 13px; color: var(--blue); line-height: 1.4; }
  #install-banner button {
    padding: 7px 13px;
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--blue);
    background: var(--blue);
    color: #fff;
    font-size: 13px; font-family: inherit; font-weight: 500;
    cursor: pointer;
    flex-shrink: 0;
  }
  #install-close {
    background: transparent !important;
    border: none !important;
    color: var(--text3) !important;
    padding: 4px !important;
    font-size: 18px !important;
  }
</style>
</head>
<body>
<div id="app">
  <div class="header">
    <div>
      <div class="header-title">Evidencija pića</div>
      <div class="header-sub" id="sync-status">Učitavanje...</div>
    </div>
    <div class="badge" id="total-badge">0 prodaja</div>
  </div>

  <!-- PWA install prompt -->
  <div id="install-banner">
    <p>Instalirajte aplikaciju za brži pristup.</p>
    <button id="install-btn">Instaliraj</button>
    <button id="install-close">✕</button>
  </div>

  <div class="tabs">
    <button class="tab active" onclick="setTab('prodaja')" id="tab-prodaja">
      <svg viewBox="0 0 24 24"><circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"/></svg>
      Prodaja
    </button>
    <button class="tab" onclick="setTab('artikli')" id="tab-artikli">
      <svg viewBox="0 0 24 24"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></svg>
      Artikli
    </button>
    <button class="tab" onclick="setTab('recap')" id="tab-recap">
      <svg viewBox="0 0 24 24"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
      Recap
    </button>
  </div>

  <div class="content" id="content"></div>
</div>

<div class="toast" id="toast"></div>

<script>
const SK_A = 'evidencija_artikli';
const SK_P = 'evidencija_prodaja';

let artikli = [], prodaja = [], currentTab = 'prodaja', editId = null;

const fmt = n => n.toLocaleString('hr-HR', {minimumFractionDigits:2,maximumFractionDigits:2}) + ' €';
const esc = s => String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');

function load() {
  try {
    const a = localStorage.getItem(SK_A);
    const p = localStorage.getItem(SK_P);
    if (a) artikli = JSON.parse(a);
    if (p) prodaja = JSON.parse(p);
  } catch(e) {}
  render();
  document.getElementById('sync-status').textContent =
    'Sinkronizirano · ' + new Date().toLocaleTimeString('hr-HR',{hour:'2-digit',minute:'2-digit'});
  document.getElementById('total-badge').textContent = prodaja.length + ' prodaja';
}

function saveA() {
  localStorage.setItem(SK_A, JSON.stringify(artikli));
  document.getElementById('sync-status').textContent = 'Spremljeno';
}
function saveP() {
  localStorage.setItem(SK_P, JSON.stringify(prodaja));
  document.getElementById('sync-status').textContent = 'Spremljeno';
  document.getElementById('total-badge').textContent = prodaja.length + ' prodaja';
}

function setTab(t) {
  currentTab = t; editId = null;
  ['prodaja','artikli','recap'].forEach(id =>
    document.getElementById('tab-'+id).classList.toggle('active', id === t));
  render();
}

function toast(msg) {
  const el = document.getElementById('toast');
  el.textContent = msg; el.classList.add('show');
  setTimeout(() => el.classList.remove('show'), 2200);
}

function render() {
  document.getElementById('content').innerHTML =
    currentTab === 'prodaja' ? renderProdaja() :
    currentTab === 'artikli' ? renderArtikli() : renderRecap();
}

function renderProdaja() {
  if (!artikli.length) return `
    <div class="empty">
      <svg viewBox="0 0 24 24"><path d="M8 6V4a4 4 0 0 1 8 0v2"/><rect x="2" y="6" width="20" height="16" rx="2"/></svg>
      Nema artikala.<br>Dodajte ih u kartici <strong>Artikli</strong>.
    </div>`;
  const rows = artikli.map(a => {
    const kol = prodaja.filter(p => p.aid === a.id).length;
    return `<div class="card">
      <div class="card-info">
        <div class="card-name">${esc(a.naziv)}</div>
        <div class="card-price">${fmt(a.cijena)}</div>
      </div>
      <div class="counter">
        <button class="btn-round sub" onclick="ukloni('${a.id}')" ${!kol?'disabled':''}>
          <svg viewBox="0 0 24 24"><line x1="5" y1="12" x2="19" y2="12"/></svg>
        </button>
        <div class="count-num ${!kol?'zero':''}">${kol}</div>
        <button class="btn-round add" onclick="dodaj('${a.id}')">
          <svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
        </button>
      </div>
    </div>`;
  }).join('');
  const rst = prodaja.length ? `
    <button class="reset-btn" onclick="resetiraj()">
      <svg viewBox="0 0 24 24"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 .49-4.5"/></svg>
      Resetiraj prodaju
    </button>` : '';
  return rows + rst;
}

function renderArtikli() {
  const a = editId ? artikli.find(x=>x.id===editId)||{} : {};
  const cancel = editId ? `<button class="btn neutral" onclick="cancelEdit()">Odustani</button>` : '';
  const list = artikli.length ? artikli.map(a => `
    <div class="card">
      <div class="card-info">
        <div class="card-name">${esc(a.naziv)}</div>
        <div class="card-price">${fmt(a.cijena)}</div>
      </div>
      <button class="btn-icon edit" onclick="startEdit('${a.id}')">
        <svg viewBox="0 0 24 24"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
      </button>
      <button class="btn-icon del" onclick="obrisi('${a.id}')">
        <svg viewBox="0 0 24 24"><polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14H6L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M9 6V4h6v2"/></svg>
      </button>
    </div>`).join('') :
    `<div class="empty"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>Nema artikala.</div>`;
  return `
  <div class="form-box">
    <div class="form-label">${editId ? 'Uredi artikal' : 'Novi artikal'}</div>
    <input type="text" id="inp-naziv" placeholder="Naziv (npr. Pivo, Vino, Kava...)" value="${esc(a.naziv||'')}">
    <input type="number" id="inp-cijena" placeholder="Cijena (€)" value="${a.cijena||''}" step="0.01" min="0" inputmode="decimal">
    <div class="btn-row">
      <button class="btn primary" onclick="spremi()">${editId ? 'Spremi izmjene' : 'Dodaj artikal'}</button>
      ${cancel}
    </div>
  </div>${list}`;
}

function renderRecap() {
  const rec = artikli.map(a => {
    const kol = prodaja.filter(p=>p.aid===a.id).length;
    return {...a, kol, uk: kol*a.cijena};
  }).filter(r=>r.kol>0).sort((a,b)=>b.uk-a.uk);
  const tz = rec.reduce((s,r)=>s+r.uk,0);
  const tk = rec.reduce((s,r)=>s+r.kol,0);
  const cards = `<div class="metric-row">
    <div class="metric"><div class="metric-label">Ukupna zarada</div><div class="metric-val green">${fmt(tz)}</div></div>
    <div class="metric"><div class="metric-label">Prodano komada</div><div class="metric-val">${tk}</div></div>
  </div>`;
  if (!rec.length) return cards + `<div class="empty">Nema evidentirane prodaje.</div>`;
  return cards + rec.map(r=>`
    <div class="recap-row">
      <div class="card-info">
        <div class="card-name">${esc(r.naziv)}</div>
        <div class="card-price">${r.kol} × ${fmt(r.cijena)}</div>
      </div>
      <div class="recap-total">${fmt(r.uk)}</div>
    </div>`).join('') +
    `<div class="total-line"><span>Ukupno</span><span>${fmt(tz)}</span></div>`;
}

function dodaj(id) {
  const a = artikli.find(x=>x.id===id); if(!a) return;
  prodaja.push({id:Date.now()+Math.random()+'', aid:id, naziv:a.naziv, cijena:a.cijena, ts:new Date().toISOString()});
  saveP(); render();
}
function ukloni(id) {
  const rev = [...prodaja].reverse(), i = rev.findIndex(p=>p.aid===id); if(i===-1) return;
  prodaja.splice(prodaja.length-1-i,1); saveP(); render();
}
function resetiraj() {
  if(!confirm('Resetirati svu prodaju? Artikli ostaju.')) return;
  prodaja=[]; saveP(); render(); toast('Prodaja resetirana');
}
function spremi() {
  const naziv = document.getElementById('inp-naziv').value.trim();
  const cijena = parseFloat(document.getElementById('inp-cijena').value.replace(',','.'));
  if(!naziv||isNaN(cijena)||cijena<=0){toast('Unesite naziv i ispravnu cijenu');return;}
  if(editId){
    const a=artikli.find(x=>x.id===editId); if(a){a.naziv=naziv;a.cijena=cijena;}
    editId=null; toast('Artikal ažuriran');
  } else {
    artikli.push({id:Date.now().toString(),naziv,cijena}); toast('Artikal dodan');
  }
  saveA(); render();
}
function startEdit(id){editId=id;render();setTimeout(()=>{const el=document.getElementById('inp-naziv');if(el)el.focus();},50);}
function cancelEdit(){editId=null;render();}
function obrisi(id){
  if(!confirm('Obrisati artikal?'))return;
  artikli=artikli.filter(a=>a.id!==id);
  prodaja=prodaja.filter(p=>p.aid!==id);
  saveA(); saveP(); render(); toast('Artikal obrisan');
}

// Storage sync across tabs
window.addEventListener('storage', e => {
  if(e.key===SK_A||e.key===SK_P) load();
});

load();
setInterval(load, 10000);

// Service Worker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('./sw.js').catch(()=>{});
  });
}

// PWA install prompt
let deferredPrompt = null;
window.addEventListener('beforeinstallprompt', e => {
  e.preventDefault();
  deferredPrompt = e;
  document.getElementById('install-banner').classList.add('visible');
});
document.getElementById('install-btn').addEventListener('click', async () => {
  if(!deferredPrompt) return;
  deferredPrompt.prompt();
  const {outcome} = await deferredPrompt.userChoice;
  deferredPrompt = null;
  document.getElementById('install-banner').classList.remove('visible');
  if(outcome==='accepted') toast('Aplikacija instalirana!');
});
document.getElementById('install-close').addEventListener('click', () => {
  document.getElementById('install-banner').classList.remove('visible');
});
window.addEventListener('appinstalled', () => {
  document.getElementById('install-banner').classList.remove('visible');
});
</script>
</body>
</html>
