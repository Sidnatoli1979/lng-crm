<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LNG — CRM v2</title>
<style>
:root {
  --bg:#0a0a0a;--bg2:#111;--bg3:#1a1a1a;--bg4:#222;
  --border:#2a2a2a;--border2:#333;
  --text:#f0f0f0;--text2:#888;--text3:#555;
  --gold:#c9a84c;--gold2:#e8c97a;
  --green:#22c55e;--orange:#f97316;--red:#ef4444;--blue:#3b82f6;--purple:#a855f7;--pink:#ec4899;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;font-size:14px;min-height:100vh;}
 
/* LAYOUT */
.sidebar{position:fixed;left:0;top:0;bottom:0;width:220px;background:var(--bg2);border-right:1px solid var(--border);display:flex;flex-direction:column;z-index:100;overflow-y:auto;}
.main{margin-left:220px;padding:28px 32px;min-height:100vh;}
 
/* SIDEBAR */
.logo{padding:20px 20px 16px;border-bottom:1px solid var(--border);}
.logo-text{font-size:15px;font-weight:700;letter-spacing:.15em;color:var(--gold2);}
.logo-sub{font-size:10px;color:var(--text3);margin-top:2px;letter-spacing:.08em;text-transform:uppercase;}
.sync-row{display:flex;align-items:center;gap:6px;margin-top:6px;font-size:10px;color:var(--text3);}
.sync-dot{width:6px;height:6px;border-radius:50%;background:var(--text3);}
.sync-dot.live{background:var(--green);animation:pulse 2s infinite;}
.sync-dot.loading{background:var(--orange);animation:pulse .5s infinite;}
.sync-dot.error{background:var(--red);}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
 
.nav{padding:12px 10px;flex:1;}
.nav-section{font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.1em;padding:0 8px;margin-bottom:4px;margin-top:14px;}
.nav-item{display:flex;align-items:center;gap:9px;padding:8px 10px;border-radius:8px;cursor:pointer;color:var(--text2);font-size:13px;transition:all .15s;margin-bottom:2px;border:1px solid transparent;}
.nav-item:hover{background:var(--bg3);color:var(--text);}
.nav-item.active{background:var(--bg4);color:var(--gold2);border-color:var(--border2);}
.nav-icon{font-size:14px;width:18px;text-align:center;flex-shrink:0;}
.nav-badge{margin-left:auto;font-size:10px;background:var(--bg4);color:var(--text3);padding:2px 7px;border-radius:10px;}
.nav-item.active .nav-badge{background:rgba(201,168,76,.15);color:var(--gold);}
 
/* PAGE HEADER */
.page-header{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:24px;flex-wrap:wrap;gap:14px;}
.page-title{font-size:20px;font-weight:700;letter-spacing:.04em;}
.page-sub{font-size:12px;color:var(--text2);margin-top:3px;}
 
/* KPI */
.kpi-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:24px;}
.kpi{background:var(--bg2);border:1px solid var(--border);border-radius:12px;padding:16px 18px;position:relative;overflow:hidden;}
.kpi::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;}
.kpi.gold::before{background:linear-gradient(90deg,var(--gold),var(--gold2));}
.kpi.green::before{background:linear-gradient(90deg,#16a34a,var(--green));}
.kpi.blue::before{background:linear-gradient(90deg,#1d4ed8,var(--blue));}
.kpi.purple::before{background:linear-gradient(90deg,#7c3aed,var(--purple));}
.kpi.pink::before{background:linear-gradient(90deg,#be185d,var(--pink));}
.kpi.orange::before{background:linear-gradient(90deg,#c2410c,var(--orange));}
.kpi-n{font-size:26px;font-weight:700;margin-bottom:3px;}
.kpi.gold .kpi-n{color:var(--gold2);}
.kpi.green .kpi-n{color:var(--green);}
.kpi.blue .kpi-n{color:var(--blue);}
.kpi.purple .kpi-n{color:var(--purple);}
.kpi.pink .kpi-n{color:var(--pink);}
.kpi.orange .kpi-n{color:var(--orange);}
.kpi-l{font-size:11px;color:var(--text2);text-transform:uppercase;letter-spacing:.06em;}
.kpi-sub{font-size:11px;color:var(--text3);margin-top:4px;}
 
/* CARDS */
.card{background:var(--bg2);border:1px solid var(--border);border-radius:12px;padding:18px 20px;margin-bottom:16px;}
.card-title{font-size:12px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;margin-bottom:14px;}
 
/* SOIREE CARDS */
.soirees-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px;margin-bottom:24px;}
.soiree-card{background:var(--bg2);border:1px solid var(--border);border-radius:14px;overflow:hidden;transition:border-color .2s;}
.soiree-card:hover{border-color:var(--border2);}
.soiree-card.tonight{border-color:var(--gold);box-shadow:0 0 20px rgba(201,168,76,.1);}
.soiree-header{padding:16px 18px 12px;border-bottom:1px solid var(--border);}
.soiree-badge{font-size:10px;font-weight:600;padding:3px 9px;border-radius:20px;display:inline-block;margin-bottom:8px;}
.badge-tonight{background:rgba(201,168,76,.15);color:var(--gold2);border:1px solid rgba(201,168,76,.3);}
.badge-upcoming{background:var(--bg3);color:var(--text3);border:1px solid var(--border);}
.badge-recurring{background:rgba(59,130,246,.1);color:var(--blue);border:1px solid rgba(59,130,246,.2);}
.soiree-name{font-size:16px;font-weight:700;margin-bottom:4px;}
.soiree-date{font-size:12px;color:var(--text2);}
.soiree-body{padding:14px 18px;}
.soiree-stat{display:flex;justify-content:space-between;padding:5px 0;border-bottom:1px solid var(--border);font-size:12px;}
.soiree-stat:last-child{border-bottom:none;}
.soiree-stat-label{color:var(--text3);}
.soiree-stat-val{font-weight:600;}
.countdown-badge{font-size:18px;font-weight:700;color:var(--gold2);margin-top:4px;}
 
/* TOP VENDEURS */
.top-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:24px;}
.rank-row{display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--border);}
.rank-row:last-child{border-bottom:none;}
.rank-n{font-size:16px;font-weight:700;min-width:24px;color:var(--text3);}
.rank-n.r1{color:#ffd700;}
.rank-n.r2{color:#c0c0c0;}
.rank-n.r3{color:#cd7f32;}
.rank-avatar{width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;flex-shrink:0;}
.rank-info{flex:1;min-width:0;}
.rank-name{font-size:13px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.rank-event{font-size:10px;color:var(--text3);}
.rank-ventes{font-size:14px;font-weight:700;color:var(--green);}
.rank-bar-bg{height:3px;background:var(--bg4);border-radius:2px;margin-top:4px;overflow:hidden;}
.rank-bar{height:3px;border-radius:2px;background:var(--green);}
 
/* INSTAGRAM STATS */
.ig-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:16px;}
.ig-stat{background:var(--bg3);border-radius:10px;padding:14px;text-align:center;}
.ig-stat-n{font-size:22px;font-weight:700;color:var(--pink);margin-bottom:3px;}
.ig-stat-l{font-size:11px;color:var(--text3);}
.ig-post-row{display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--border);font-size:12px;}
.ig-post-row:last-child{border-bottom:none;}
.ig-post-thumb{width:36px;height:36px;border-radius:6px;background:var(--bg3);display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
.ig-post-name{flex:1;font-weight:500;}
.ig-post-stat{color:var(--text3);}
.ig-post-stat span{color:var(--text);font-weight:600;margin-left:4px;}
 
/* CONTROLS */
.controls{display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;align-items:center;}
.search-box{flex:1;min-width:180px;position:relative;}
.search-box input{width:100%;padding:8px 12px 8px 34px;background:var(--bg2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-size:12px;outline:none;}
.search-box input:focus{border-color:var(--border2);}
.search-icon{position:absolute;left:11px;top:50%;transform:translateY(-50%);color:var(--text3);font-size:12px;pointer-events:none;}
.filter-btn{padding:7px 12px;border:1px solid var(--border);border-radius:8px;background:transparent;color:var(--text2);font-size:11px;cursor:pointer;transition:all .15s;white-space:nowrap;}
.filter-btn:hover{border-color:var(--border2);color:var(--text);}
.filter-btn.active{background:var(--bg3);border-color:var(--gold);color:var(--gold2);}
.add-btn{padding:8px 16px;border:1px solid var(--gold);border-radius:8px;background:rgba(201,168,76,.1);color:var(--gold2);font-size:12px;cursor:pointer;font-weight:500;transition:all .15s;white-space:nowrap;}
.add-btn:hover{background:rgba(201,168,76,.2);}
 
/* CONTACT CARDS */
.contacts-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(300px,1fr));gap:14px;}
.contact-card{background:var(--bg2);border:1px solid var(--border);border-radius:14px;overflow:hidden;transition:border-color .15s;}
.contact-card:hover{border-color:var(--border2);}
.contact-header{padding:14px 16px;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:12px;}
.contact-avatar{width:40px;height:40px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;flex-shrink:0;}
.contact-name{font-weight:600;font-size:14px;}
.contact-role{font-size:11px;color:var(--text3);margin-top:2px;}
.contact-body{padding:12px 16px;}
.contact-field{display:flex;align-items:center;gap:8px;padding:5px 0;font-size:12px;color:var(--text2);}
.contact-field-icon{width:16px;text-align:center;flex-shrink:0;font-size:13px;}
.contact-field a{color:var(--text2);text-decoration:none;transition:color .1s;}
.contact-field a:hover{color:var(--text);}
.contact-field a.wa{color:#25d366;}
.contact-field a.ig{color:var(--pink);}
.contact-tags{padding:10px 16px 14px;display:flex;flex-wrap:wrap;gap:5px;}
.contact-tag{font-size:10px;font-weight:600;padding:3px 9px;border-radius:20px;}
.tag-commerciale{background:rgba(34,197,94,.1);color:#86efac;border:1px solid rgba(34,197,94,.2);}
.tag-wildside{background:rgba(168,85,247,.1);color:#c4b5fd;border:1px solid rgba(168,85,247,.2);}
.tag-wonderland{background:rgba(249,115,22,.1);color:#fdba74;border:1px solid rgba(249,115,22,.2);}
.tag-fomo{background:rgba(239,68,68,.1);color:#fca5a5;border:1px solid rgba(239,68,68,.2);}
.tag-savage{background:rgba(201,168,76,.1);color:var(--gold2);border:1px solid rgba(201,168,76,.2);}
.tag-ambassadeur{background:rgba(59,130,246,.1);color:#93c5fd;border:1px solid rgba(59,130,246,.2);}
.tag-promoteur{background:rgba(236,72,153,.1);color:#f9a8d4;border:1px solid rgba(236,72,153,.2);}
.tag-bde{background:rgba(20,184,166,.1);color:#5eead4;border:1px solid rgba(20,184,166,.2);}
.contact-actions{display:flex;gap:6px;padding:0 16px 14px;}
.c-btn{flex:1;padding:7px;border-radius:8px;border:1px solid var(--border);background:transparent;color:var(--text3);font-size:11px;cursor:pointer;transition:all .15s;text-align:center;}
.c-btn:hover{background:var(--bg3);color:var(--text);}
.c-btn.edit{border-color:rgba(201,168,76,.3);color:var(--gold);}
.bde-group{background:var(--bg3);border-radius:8px;padding:8px 10px;margin:8px 16px 0;font-size:11px;color:var(--text3);}
.bde-group-name{font-weight:600;color:var(--text2);margin-bottom:4px;}
.bde-member{display:flex;justify-content:space-between;padding:3px 0;}
 
/* TABLE */
.table-wrap{background:var(--bg2);border:1px solid var(--border);border-radius:12px;overflow:hidden;}
table{width:100%;border-collapse:collapse;}
thead tr{border-bottom:1px solid var(--border);}
th{padding:11px 14px;text-align:left;font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.08em;font-weight:600;cursor:pointer;white-space:nowrap;user-select:none;}
th:hover{color:var(--text2);}
th.sort-asc::after{content:' ↑';color:var(--gold);}
th.sort-desc::after{content:' ↓';color:var(--gold);}
tbody tr{border-bottom:1px solid var(--border);transition:background .1s;}
tbody tr:last-child{border-bottom:none;}
tbody tr:hover{background:var(--bg3);}
td{padding:11px 14px;vertical-align:middle;}
.avatar{width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;flex-shrink:0;}
.name-cell{display:flex;align-items:center;gap:8px;}
.amb-name{font-weight:600;font-size:13px;}
.amb-school{font-size:10px;color:var(--text3);}
.both-tag{font-size:9px;padding:1px 5px;border-radius:4px;background:rgba(201,168,76,.15);color:var(--gold);border:1px solid rgba(201,168,76,.3);margin-left:5px;}
.code-badge{font-size:10px;font-weight:600;padding:2px 8px;border-radius:6px;background:var(--bg3);color:var(--text2);border:1px solid var(--border2);letter-spacing:.05em;font-family:'SF Mono',monospace;}
.ventes-wrap{display:flex;align-items:center;gap:8px;}
.ventes-num{font-size:13px;font-weight:700;min-width:22px;text-align:center;cursor:pointer;padding:3px 7px;border-radius:6px;transition:background .1s;}
.ventes-num:hover{background:var(--bg3);color:var(--gold2);}
.vbar-bg{width:50px;height:3px;background:var(--bg4);border-radius:2px;overflow:hidden;}
.vbar{height:3px;border-radius:2px;}
.ig-link{color:var(--blue);text-decoration:none;font-size:11px;}
.ig-link:hover{text-decoration:underline;}
.status-pill{font-size:10px;font-weight:600;padding:3px 9px;border-radius:20px;white-space:nowrap;}
.s-top{background:rgba(34,197,94,.15);color:var(--green);border:1px solid rgba(34,197,94,.3);}
.s-actif{background:rgba(34,197,94,.1);color:#86efac;border:1px solid rgba(34,197,94,.2);}
.s-relance{background:rgba(249,115,22,.1);color:#fdba74;border:1px solid rgba(249,115,22,.2);}
.s-cold{background:var(--bg3);color:var(--text3);border:1px solid var(--border);}
.act-btn{padding:4px 9px;border-radius:6px;border:1px solid var(--border);background:transparent;color:var(--text3);font-size:11px;cursor:pointer;transition:all .15s;}
.act-btn:hover{border-color:var(--border2);color:var(--text2);background:var(--bg3);}
.act-btn.primary{border-color:rgba(59,130,246,.4);color:var(--blue);}
 
/* MODAL */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.75);z-index:1000;display:flex;align-items:center;justify-content:center;backdrop-filter:blur(4px);}
.modal-overlay.hidden{display:none;}
.modal{background:var(--bg2);border:1px solid var(--border2);border-radius:16px;padding:26px;width:520px;max-width:95vw;max-height:90vh;overflow-y:auto;}
.modal-title{font-size:15px;font-weight:700;margin-bottom:18px;color:var(--gold2);}
.form-group{margin-bottom:14px;}
.form-label{font-size:10px;color:var(--text3);text-transform:uppercase;letter-spacing:.06em;margin-bottom:5px;display:block;}
.form-input{width:100%;padding:9px 12px;background:var(--bg3);border:1px solid var(--border);border-radius:8px;color:var(--text);font-size:13px;outline:none;transition:border .15s;font-family:inherit;}
.form-input:focus{border-color:var(--gold);}
.form-select{width:100%;padding:9px 12px;background:var(--bg3);border:1px solid var(--border);border-radius:8px;color:var(--text);font-size:13px;outline:none;font-family:inherit;}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.form-row3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;}
.checkbox-group{display:flex;flex-wrap:wrap;gap:8px;margin-top:4px;}
.checkbox-item{display:flex;align-items:center;gap:6px;font-size:12px;color:var(--text2);cursor:pointer;}
.checkbox-item input{accent-color:var(--gold);}
.modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:20px;}
.btn-cancel{padding:8px 16px;border:1px solid var(--border);border-radius:8px;background:transparent;color:var(--text2);font-size:13px;cursor:pointer;}
.btn-save{padding:8px 16px;border:1px solid var(--gold);border-radius:8px;background:rgba(201,168,76,.15);color:var(--gold2);font-size:13px;cursor:pointer;font-weight:600;}
 
/* TOAST */
.toast{position:fixed;bottom:24px;right:24px;background:var(--bg3);border:1px solid var(--border2);border-radius:10px;padding:11px 16px;font-size:13px;color:var(--text);z-index:9999;opacity:0;transform:translateY(10px);transition:all .25s;pointer-events:none;max-width:280px;}
.toast.show{opacity:1;transform:translateY(0);}
 
/* LOADING */
.loading-screen{position:fixed;inset:0;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:8000;}
.loading-screen.hidden{display:none;}
.spinner{width:34px;height:34px;border:2px solid var(--border2);border-top-color:var(--gold);border-radius:50%;animation:spin .8s linear infinite;margin-bottom:14px;}
@keyframes spin{to{transform:rotate(360deg)}}
.empty{text-align:center;padding:50px 20px;color:var(--text3);}
.empty-icon{font-size:28px;margin-bottom:10px;}
 
@media(max-width:900px){
  .sidebar{width:58px;}
  .sidebar .logo-text,.sidebar .logo-sub,.sidebar .sync-row span,.sidebar .nav-item>span:not(.nav-icon),.sidebar .nav-badge,.sidebar .nav-section{display:none;}
  .sidebar .nav-item{justify-content:center;padding:11px;}
  .main{margin-left:58px;padding:18px 14px;}
  .kpi-grid{grid-template-columns:repeat(2,1fr);}
  .top-grid,.ig-grid{grid-template-columns:1fr;}
  .contacts-grid{grid-template-columns:1fr;}
}
@media(max-width:560px){.sidebar{display:none;}.main{margin-left:0;}}
</style>
</head>
<body>
 
<!-- LOADING -->
<div class="loading-screen hidden" id="loading-screen">
  <div class="spinner"></div>
  <div style="font-size:13px;color:var(--text2)" id="loading-text">Chargement...</div>
</div>
 
<!-- SIDEBAR -->
<div class="sidebar">
  <div class="logo">
    <div class="logo-text">LNG</div>
    <div class="logo-sub">Lyon Night Group</div>
    <div class="sync-row"><div class="sync-dot live" id="sync-dot"></div><span id="sync-label">Live</span></div>
  </div>
  <nav class="nav">
    <div class="nav-section">Menu</div>
    <div class="nav-item active" id="nav-dashboard" onclick="showPage('dashboard')"><span class="nav-icon">📊</span><span>Dashboard</span></div>
    <div class="nav-section">Ambassadeurs</div>
    <div class="nav-item" id="nav-commerciale" onclick="showPage('commerciale')"><span class="nav-icon">🎉</span><span>La Commerciale</span><span class="nav-badge" id="nb-c">—</span></div>
    <div class="nav-item" id="nav-wildside" onclick="showPage('wildside')"><span class="nav-icon">🐰</span><span>WildSide</span><span class="nav-badge" id="nb-w">—</span></div>
    <div class="nav-item" id="nav-both" onclick="showPage('both')"><span class="nav-icon">⭐</span><span>Les deux events</span><span class="nav-badge" id="nb-b">—</span></div>
    <div class="nav-section">Réseau</div>
    <div class="nav-item" id="nav-contacts" onclick="showPage('contacts')"><span class="nav-icon">👥</span><span>Contacts</span><span class="nav-badge" id="nb-contacts">—</span></div>
    <div class="nav-section">Outils</div>
    <div class="nav-item" onclick="exportCSV()"><span class="nav-icon">📥</span><span>Exporter CSV</span></div>
    <div class="nav-item" onclick="refreshData()"><span class="nav-icon">🔄</span><span>Actualiser</span></div>
  </nav>
</div>
 
<!-- MAIN -->
<div class="main" id="main-content"></div>
 
<!-- MODAL AMBASSADEUR -->
<div class="modal-overlay hidden" id="modal-amb">
  <div class="modal">
    <div class="modal-title" id="modal-amb-title">Ajouter un ambassadeur</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Nom complet</label><input class="form-input" id="fa-nom" placeholder="Prénom Nom"></div>
      <div class="form-group"><label class="form-label">Code promo</label><input class="form-input" id="fa-code" placeholder="TITO"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Instagram</label><input class="form-input" id="fa-ig" placeholder="pseudo"></div>
      <div class="form-group"><label class="form-label">WhatsApp / Tél</label><input class="form-input" id="fa-tel" placeholder="06 00 00 00 00"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">École</label><input class="form-input" id="fa-ecole" placeholder="EM Lyon"></div>
      <div class="form-group"><label class="form-label">Ventes</label><input class="form-input" id="fa-ventes" type="number" min="0" placeholder="0"></div>
    </div>
    <div class="form-group"><label class="form-label">Soirées</label>
      <div class="checkbox-group">
        <label class="checkbox-item"><input type="checkbox" id="fa-ev-c" value="commerciale"> La Commerciale</label>
        <label class="checkbox-item"><input type="checkbox" id="fa-ev-w" value="wildside"> WildSide</label>
        <label class="checkbox-item"><input type="checkbox" id="fa-ev-wl" value="wonderland"> Wonderland</label>
        <label class="checkbox-item"><input type="checkbox" id="fa-ev-f" value="fomo"> FOMO</label>
        <label class="checkbox-item"><input type="checkbox" id="fa-ev-s" value="savage"> Savage</label>
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModals()">Annuler</button>
      <button class="btn-save" onclick="saveAmb()">Enregistrer</button>
    </div>
  </div>
</div>
 
<!-- MODAL CONTACT -->
<div class="modal-overlay hidden" id="modal-contact">
  <div class="modal">
    <div class="modal-title" id="modal-contact-title">Ajouter un contact</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Prénom</label><input class="form-input" id="fc-prenom" placeholder="Nelly"></div>
      <div class="form-group"><label class="form-label">Nom</label><input class="form-input" id="fc-nom" placeholder="Martin"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">WhatsApp / Tél</label><input class="form-input" id="fc-tel" placeholder="07 83 45 15 83"></div>
      <div class="form-group"><label class="form-label">Instagram</label><input class="form-input" id="fc-ig" placeholder="@pseudo"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Rôle</label>
        <select class="form-select" id="fc-role">
          <option value="ambassadeur">Ambassadeur</option>
          <option value="promoteur">Promoteur</option>
          <option value="bde">BDE</option>
          <option value="club">Club / Partenaire</option>
          <option value="autre">Autre</option>
        </select>
      </div>
      <div class="form-group"><label class="form-label">Post / Fonction</label><input class="form-input" id="fc-post" placeholder="Pôle Event, VP..."></div>
    </div>
    <div class="form-group"><label class="form-label">BDE / Organisation</label><input class="form-input" id="fc-bde" placeholder="BDE Synapsound, ESC Lyon..."></div>
    <div class="form-group"><label class="form-label">Code promo (si ambassadeur)</label><input class="form-input" id="fc-code" placeholder="TITO"></div>
    <div class="form-group"><label class="form-label">Soirées</label>
      <div class="checkbox-group">
        <label class="checkbox-item"><input type="checkbox" id="fc-ev-c" value="commerciale"> La Commerciale</label>
        <label class="checkbox-item"><input type="checkbox" id="fc-ev-w" value="wildside"> WildSide</label>
        <label class="checkbox-item"><input type="checkbox" id="fc-ev-wl" value="wonderland"> Wonderland</label>
        <label class="checkbox-item"><input type="checkbox" id="fc-ev-f" value="fomo"> FOMO</label>
        <label class="checkbox-item"><input type="checkbox" id="fc-ev-s" value="savage"> Savage</label>
      </div>
    </div>
    <div class="form-group"><label class="form-label">Notes</label><input class="form-input" id="fc-notes" placeholder="Notes libres..."></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModals()">Annuler</button>
      <button class="btn-save" onclick="saveContact()">Enregistrer</button>
    </div>
  </div>
</div>
 
<div class="toast" id="toast"></div>
 
<script>
// ─── DATA ─────────────────────────────────────────────────────────────────────
const COMMERCIALE = [
  {id:'c1',nom:'Kremer Milan',code:'MILAN',ig:'milan_kr01',tel:'766510622',ecole:'Cibeins',ventes:0},
  {id:'c2',nom:'Rivière Léo',code:'LEO10',ig:'leo_riviere_42',tel:'618405533',ecole:'Lyon 2',ventes:0},
  {id:'c3',nom:'De Loynes Camille',code:'CAM69',ig:'Camille_dlyns',tel:'767935858',ecole:'',ventes:0},
  {id:'c4',nom:'Belmellat Manil',code:'MANIL',ig:'manil.blm',tel:'603279427',ecole:'La Doua Lyon1',ventes:0},
  {id:'c5',nom:'Paschen Clara',code:'CLARA',ig:'Clara.pcshn',tel:'767007443',ecole:'CREAD',ventes:0},
  {id:'c6',nom:'Roehr Noemi',code:'NOEMIE10',ig:'noemie_rhr',tel:'643105325',ecole:'Vatel',ventes:0},
  {id:'c7',nom:'Nier Lena',code:'LENA',ig:'lena_nier',tel:'769990712',ecole:'CESI',ventes:0},
  {id:'c8',nom:'Civic Arno',code:'ARNO10',ig:'Arno_c__',tel:'626992336',ecole:'',ventes:0},
  {id:'c9',nom:'Ctz Lya',code:'LYA',ig:'lya.ctz',tel:'767159263',ecole:'Cours Gallien',ventes:0},
  {id:'c10',nom:"Chopard D'Andréa Lola",code:'LOULAX',ig:'ll_oaal',tel:'765675940',ecole:'Pôle Social / Lyon 2',ventes:0},
  {id:'c11',nom:'Marcou Kylian',code:'K2B',ig:'k2b.691',tel:'698015776',ecole:'',ventes:0},
  {id:'c12',nom:'Mendez Tino',code:'TINO',ig:'t.m_692',tel:'601133362',ecole:'Idrac Business School',ventes:0},
  {id:'c13',nom:'Cornier Eddy',code:'EDDY',ig:'eddycornier_',tel:'613568317',ecole:'Forma BTP',ventes:0},
  {id:'c14',nom:'Denis Ambre',code:'AMBREDNS',ig:'ambre.dns',tel:'640206037',ecole:'',ventes:0},
  {id:'c15',nom:'Chabroud Charlotte',code:'CHARLOTTE',ig:'charlottee.chd',tel:'618675750',ecole:'CREAD',ventes:0},
  {id:'c16',nom:'Magnaval Lucas',code:'LUCASM24',ig:'',tel:'665147002',ecole:'ESG Sport',ventes:0},
  {id:'c17',nom:'Brossard Antoine',code:'ANTOINE',ig:'antoine_brsd69',tel:'607538260',ecole:'',ventes:0},
  {id:'c18',nom:'Gueret Axel',code:'AG01',ig:'grt_axel',tel:'765868428',ecole:'ESDES Business School',ventes:0},
  {id:'c19',nom:'Blancheton Théo',code:'TITO',ig:'theo_blctn',tel:'788270827',ecole:'',ventes:0},
  {id:'c20',nom:'Pupin Charline',code:'CHARLINE',ig:'chrlnppn',tel:'783446494',ecole:'EMLYON',ventes:0},
  {id:'c21',nom:'Imam Joshua',code:'JOSHUA',ig:'joshua_ipey',tel:'625136368',ecole:'ESSCA',ventes:0},
  {id:'c22',nom:'Charreton Jules',code:'JULESC',ig:'_jules.ch',tel:'770031731',ecole:'EM Lyon',ventes:0},
  {id:'c23',nom:'Moreaux Léa',code:'LEAXX',ig:'leax._.mrx',tel:'617383000',ecole:'',ventes:0},
  {id:'c24',nom:'Huon Léo',code:'LEO10H',ig:'lh47__',tel:'673560579',ecole:'',ventes:0},
  {id:'c25',nom:'Costa Mattéo',code:'MATTEO',ig:'matteo.dsc_',tel:'627450026',ecole:'Idrac Business School',ventes:0},
  {id:'c26',nom:'Montillot Syrine',code:'SYSY',ig:'_syrinemtl',tel:'783889767',ecole:'UCLy',ventes:0},
  {id:'c27',nom:'Leclère Timothy',code:'TIMOTHY',ig:'timothy.lcr',tel:'659685620',ecole:'Lyon 3',ventes:0},
  {id:'c28',nom:'Coquet Valentin',code:'VALENTINC',ig:'Valentin__cqt',tel:'784135266',ecole:'',ventes:0},
  {id:'c29',nom:'Boxberger Garance',code:'GARANCE10',ig:'garance_boxberger',tel:'770299405',ecole:'',ventes:0},
  {id:'c30',nom:'Renard Maxime',code:'MAXIME10',ig:'69_renard_max',tel:'644028459',ecole:'',ventes:0},
  {id:'c31',nom:'Mamcarz Flora',code:'FLORA',ig:'floramczz',tel:'783486616',ecole:'Idrac',ventes:0},
  {id:'c32',nom:'Renault Jeanne',code:'JEANNE10',ig:'jeanne_rlt.a',tel:'650996838',ecole:'EM Lyon',ventes:0},
  {id:'c33',nom:'Khouya Ghita',code:'GHITA',ig:'ghxtaa',tel:'753980041',ecole:'EM Lyon',ventes:0},
  {id:'c34',nom:'Fontaine Mathéo',code:'MATHEO',ig:'Mxxtheoo_',tel:'783138111',ecole:'',ventes:0},
  {id:'c35',nom:'Blard Kylian',code:'KYLIAN10',ig:'Kylian.blr',tel:'640603184',ecole:'René Cassin',ventes:0},
  {id:'c36',nom:'Stephien Jules',code:'—',ig:'',tel:'789083355',ecole:'',ventes:0},
];
 
const WILDSIDE = [
  {id:'w1',nom:'Blancheton Théo',code:'TITO',ig:'theo_blctn',tel:'788270827',ecole:'',ventes:0},
  {id:'w2',nom:'Marcou Kylian',code:'K2B',ig:'k2b.691',tel:'698015776',ecole:'',ventes:0},
  {id:'w3',nom:'Blard Kylian',code:'KYLIAN10',ig:'Kylian.blr',tel:'640603184',ecole:'René Cassin',ventes:0},
  {id:'w4',nom:'Renard Maxime',code:'MAXIME10',ig:'69_renard_max',tel:'644028459',ecole:'',ventes:0},
  {id:'w5',nom:'Racureanu Darius',code:'DARIIUS',ig:'darius_rcn',tel:'648853623',ecole:'',ventes:0},
  {id:'w6',nom:'Montillot Syrine',code:'SYSY',ig:'_syrinemtl',tel:'783889767',ecole:'UCLy',ventes:0},
  {id:'w7',nom:"Chopard D'Andréa Lola",code:'LOULAX',ig:'ll_oaal',tel:'765675940',ecole:'',ventes:0},
  {id:'w8',nom:'Lagarde Théo',code:'THEO10',ig:'Theo_lb69',tel:'788367005',ecole:'',ventes:0},
  {id:'w9',nom:'Desmons Anne Gaelle',code:'AG10',ig:'ag_.dsm',tel:'637197471',ecole:'',ventes:0},
  {id:'w10',nom:'Romy',code:'ROMY',ig:'Romy.attr',tel:'652362363',ecole:'',ventes:0},
  {id:'w11',nom:'Guitton Axel',code:'Gtem',ig:'Axel.gtem',tel:'607678752',ecole:'',ventes:0},
  {id:'w12',nom:'Khouya Ghita',code:'GHITA',ig:'ghxtaa',tel:'753980041',ecole:'EM Lyon',ventes:0},
  {id:'w13',nom:'Nier Lena',code:'LENA',ig:'lena_nier',tel:'769990712',ecole:'CESI',ventes:0},
  {id:'w14',nom:'Ambroise',code:'AMBROISE',ig:'ambroiseguerrero',tel:'614488054',ecole:'',ventes:0},
  {id:'w15',nom:'Mendez Tino',code:'TINO',ig:'t.m_692',tel:'601133362',ecole:'Idrac',ventes:0},
  {id:'w16',nom:'Belmellat Manil',code:'MANIL',ig:'manil.blm',tel:'603279427',ecole:'La Doua',ventes:0},
  {id:'w17',nom:'Coquet Valentin',code:'VALENTINC',ig:'Valentin__cqt',tel:'784135266',ecole:'',ventes:0},
  {id:'w18',nom:'Alice Fiorelli',code:'ALICE',ig:'a.lc',tel:'691662679',ecole:'',ventes:0},
  {id:'w19',nom:'Safinez Ait Mekideche',code:'SAFINEZ',ig:'s.a.f.i.n.e.z',tel:'650476289',ecole:'',ventes:0},
  {id:'w20',nom:'Garcia-Chapelut Clement',code:'CLEMENT',ig:'clement.gc_',tel:'766893599',ecole:'',ventes:0},
  {id:'w21',nom:'Pelletier Océane',code:'OCÉANE',ig:'oceane.pltrd',tel:'745060039',ecole:'',ventes:0},
  {id:'w22',nom:'Guigout Lou-Anne',code:'LOUWILD',ig:'louaannneee',tel:'651967245',ecole:'',ventes:0},
  {id:'w23',nom:'Perez Sienna',code:'Sienna',ig:'siennap.r',tel:'772331195',ecole:'',ventes:0},
  {id:'w24',nom:'Varenne Wendy',code:'WENDY',ig:'wxndx_v',tel:'763891909',ecole:'',ventes:0},
  {id:'w25',nom:'Pasquier Mia',code:'MIA',ig:'Mia.psq',tel:'628379461',ecole:'',ventes:0},
  {id:'w26',nom:'Vialle Charlize',code:'CHARLIZE',ig:'Charlize.vll',tel:'781174692',ecole:'',ventes:0},
  {id:'w27',nom:'Cali Lea',code:'LEA',ig:'leaa.cl_',tel:'626976938',ecole:'',ventes:0},
  {id:'w28',nom:'Sasha',code:'SASHA69',ig:'delepine_sasha',tel:'767302369',ecole:'',ventes:0},
  {id:'w29',nom:'Delorme Imbert Appoline',code:'Appoline',ig:'app0linee_',tel:'770419989',ecole:'',ventes:0},
  {id:'w30',nom:'Boxberger Garance',code:'GARANCE10',ig:'garance_boxberger',tel:'770299405',ecole:'',ventes:0},
  {id:'w31',nom:'Tom Peron',code:'TOMTOM',ig:'tomtom_o_',tel:'781775540',ecole:'',ventes:0},
  {id:'w32',nom:'Huon Léo',code:'LEO10',ig:'lh47__',tel:'673560579',ecole:'',ventes:0},
  {id:'w33',nom:'Anjélika Coraux',code:'ANJIE07',ig:'anjiecoraux',tel:'768977765',ecole:'',ventes:0},
];
 
// CONTACTS (BDE, promoteurs, etc.)
let CONTACTS = [];
let editContactId = null;
let editAmbId = null;
let editAmbEvent = null;
let currentPage = 'dashboard';
let filterStatus = 'tous';
let contactFilter = 'tous';
let sortKey = 'nom'; let sortDir = 1;
 
const AVT = [['#1e3a5f','#60a5fa'],['#14402a','#4ade80'],['#3d2a0f','#fbbf24'],['#3b0f1e','#f472b6'],['#2d1f4e','#a78bfa'],['#1f3040','#7dd3fc']];
 
function loadStorage() {
  try {
    const c = localStorage.getItem('lng_v2_commerciale');
    const w = localStorage.getItem('lng_v2_wildside');
    const ct = localStorage.getItem('lng_v2_contacts');
    if(c){const d=JSON.parse(c);d.forEach((a,i)=>{if(COMMERCIALE[i])COMMERCIALE[i].ventes=a.ventes||0;});}
    if(w){const d=JSON.parse(w);d.forEach((a,i)=>{if(WILDSIDE[i])WILDSIDE[i].ventes=a.ventes||0;});}
    if(ct) CONTACTS = JSON.parse(ct);
  } catch(e){}
}
function saveStorage() {
  try {
    localStorage.setItem('lng_v2_commerciale', JSON.stringify(COMMERCIALE));
    localStorage.setItem('lng_v2_wildside', JSON.stringify(WILDSIDE));
    localStorage.setItem('lng_v2_contacts', JSON.stringify(CONTACTS));
  } catch(e){}
}
 
function initials(n){return n.trim().split(/\s+/).map(w=>w[0]).join('').slice(0,2).toUpperCase();}
function getStatus(v){
  if(v>=20)return{label:'TOP 🔥',cls:'s-top'};
  if(v>=5)return{label:'Actif',cls:'s-actif'};
  if(v>=1)return{label:'À relancer',cls:'s-relance'};
  return{label:'0 ventes',cls:'s-cold'};
}
function getBothCodes(){const wc=new Set(WILDSIDE.map(a=>a.code));return new Set(COMMERCIALE.filter(a=>wc.has(a.code)).map(a=>a.code));}
function getData(){
  if(currentPage==='commerciale')return COMMERCIALE;
  if(currentPage==='wildside')return WILDSIDE;
  if(currentPage==='both'){const bc=getBothCodes();return COMMERCIALE.filter(a=>bc.has(a.code));}
  return [];
}
function formatTel(t){return t?t.replace(/\s/g,'').replace(/^0/,'+330'):''}
function cleanTel(t){return t?t.replace(/[\s.]/g,''):''}
 
function navActivate(page){
  ['dashboard','commerciale','wildside','both','contacts'].forEach(p=>{
    const el=document.getElementById('nav-'+p);
    if(el)el.classList.toggle('active',p===page);
  });
}
 
function showPage(page){
  currentPage=page;
  filterStatus='tous';
  contactFilter='tous';
  navActivate(page);
  if(page==='dashboard')renderDashboard();
  else if(page==='contacts')renderContacts();
  else renderAmb();
}
 
function updateBadges(){
  const bc=getBothCodes();
  document.getElementById('nb-c').textContent=COMMERCIALE.length;
  document.getElementById('nb-w').textContent=WILDSIDE.length;
  document.getElementById('nb-b').textContent=bc.size;
  document.getElementById('nb-contacts').textContent=CONTACTS.length;
}
 
// ─── DASHBOARD ────────────────────────────────────────────────────────────────
function renderDashboard(){
  const allAmb=[...COMMERCIALE,...WILDSIDE];
  const totalV=allAmb.reduce((s,a)=>s+a.ventes,0);
  const totalAmb=COMMERCIALE.length+WILDSIDE.length;
  const actifs=allAmb.filter(a=>a.ventes>0).length;
 
  // Top vendeurs tous events confondus
  const merged={};
  allAmb.forEach(a=>{
    if(!merged[a.code])merged[a.code]={nom:a.nom,code:a.code,ventes:0,events:[]};
    merged[a.code].ventes+=a.ventes;
    if(!merged[a.code].events.includes(a.code.startsWith('w')?'WildSide':'Commerciale'))
      merged[a.code].events.push(COMMERCIALE.find(x=>x.code===a.code)?'Commerciale':'WildSide');
  });
  const topVendeurs=Object.values(merged).sort((a,b)=>b.ventes-a.ventes).slice(0,8);
  const maxV=Math.max(...topVendeurs.map(a=>a.ventes),1);
 
  // Soirées agenda
  const now=new Date();
  const soirees=[
    {name:'La Commerciale',lieu:'Azar Club',color:'#4ade80',dot:'#22c55e',recurrent:true,freq:'Tous les jeudis',next:nextThursday(),ambassadeurs:COMMERCIALE.length,ventes:COMMERCIALE.reduce((s,a)=>s+a.ventes,0),objectif:COMMERCIALE.length*8},
    {name:'WildSide 2Y Birthday',lieu:'Club TBD',color:'#c4b5fd',dot:'#a855f7',recurrent:false,next:new Date('2026-04-18'),ambassadeurs:WILDSIDE.length,ventes:WILDSIDE.reduce((s,a)=>s+a.ventes,0),objectif:WILDSIDE.length*8},
  ];
 
  function nextThursday(){
    const d=new Date();
    const day=d.getDay();
    const diff=(4-day+7)%7||7;
    const n=new Date(d);n.setDate(d.getDate()+diff);n.setHours(23,30,0,0);return n;
  }
 
  function countdown(date){
    const diff=date-now;
    if(diff<0)return'Passé';
    const h=Math.floor(diff/3600000);
    const d=Math.floor(h/24);
    if(d>0)return`J-${d}`;
    return`J-0 · dans ${h}h`;
  }
 
  function isTonight(date){
    return date.toDateString()===now.toDateString();
  }
 
  document.getElementById('main-content').innerHTML=`
    <div class="page-header">
      <div><div class="page-title" style="color:var(--gold2)">Dashboard LNG</div><div class="page-sub">Vue globale · ${new Date().toLocaleDateString('fr-FR',{weekday:'long',day:'numeric',month:'long'})}</div></div>
    </div>
 
    <div class="kpi-grid" style="grid-template-columns:repeat(4,1fr)">
      <div class="kpi gold"><div class="kpi-n">${totalAmb}</div><div class="kpi-l">Ambassadeurs total</div><div class="kpi-sub">${COMMERCIALE.length} Commerciale · ${WILDSIDE.length} WildSide</div></div>
      <div class="kpi green"><div class="kpi-n">${totalV}</div><div class="kpi-l">Tickets vendus</div><div class="kpi-sub">objectif global : ${totalAmb*8}</div></div>
      <div class="kpi blue"><div class="kpi-n">${actifs}</div><div class="kpi-l">Ont vendu</div><div class="kpi-sub">${totalAmb-actifs} à relancer</div></div>
      <div class="kpi purple"><div class="kpi-n">${CONTACTS.length}</div><div class="kpi-l">Contacts réseau</div><div class="kpi-sub">BDE · Promoteurs · Ambassadeurs</div></div>
    </div>
 
    <div style="font-size:12px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:.07em;margin-bottom:10px">📅 Agenda soirées</div>
    <div class="soirees-grid">
      ${soirees.map(s=>`
        <div class="soiree-card ${isTonight(s.next)?'tonight':''}">
          <div class="soiree-header">
            <div class="soiree-badge ${isTonight(s.next)?'badge-tonight':s.recurrent?'badge-recurring':'badge-upcoming'}">${isTonight(s.next)?'🔴 CE SOIR':s.recurrent?'🔁 Récurrent':'📅 À venir'}</div>
            <div class="soiree-name" style="color:${s.color}">${s.name}</div>
            <div class="soiree-date">${s.recurrent?s.freq+' · ':''} ${s.next.toLocaleDateString('fr-FR',{weekday:'long',day:'numeric',month:'long'})} · ${s.lieu}</div>
            ${isTonight(s.next)?`<div class="countdown-badge" id="cd-${s.name.replace(/\s/g,'')}">...</div>`:`<div style="font-size:18px;font-weight:700;margin-top:4px;color:var(--text2)">${countdown(s.next)}</div>`}
          </div>
          <div class="soiree-body">
            <div class="soiree-stat"><span class="soiree-stat-label">Ambassadeurs</span><span class="soiree-stat-val">${s.ambassadeurs}</span></div>
            <div class="soiree-stat"><span class="soiree-stat-label">Tickets vendus</span><span class="soiree-stat-val" style="color:var(--green)">${s.ventes}</span></div>
            <div class="soiree-stat"><span class="soiree-stat-label">Objectif</span><span class="soiree-stat-val">${s.objectif}</span></div>
            <div class="soiree-stat"><span class="soiree-stat-label">Progression</span><span class="soiree-stat-val">${s.objectif>0?Math.round(s.ventes/s.objectif*100):0}%</span></div>
          </div>
        </div>
      `).join('')}
    </div>
 
    <div class="top-grid">
      <div class="card">
        <div class="card-title">🏆 Top vendeurs du mois</div>
        ${topVendeurs.length===0?'<div class="empty"><div class="empty-icon">📊</div>Aucune vente enregistrée</div>':
        topVendeurs.map((a,i)=>{
          const[bg,fg]=AVT[i%AVT.length];
          return`<div class="rank-row">
            <div class="rank-n ${i===0?'r1':i===1?'r2':i===2?'r3':''}">${i+1}</div>
            <div class="rank-avatar" style="background:${bg};color:${fg}">${initials(a.nom)}</div>
            <div class="rank-info">
              <div class="rank-name">${a.nom}</div>
              <div class="rank-bar-bg"><div class="rank-bar" style="width:${Math.round(a.ventes/maxV*100)}%"></div></div>
            </div>
            <div class="rank-ventes">${a.ventes}</div>
          </div>`;
        }).join('')}
      </div>
 
      <div class="card">
        <div class="card-title">📱 Instagram @lyonnightgroup</div>
        <div class="ig-grid" style="grid-template-columns:repeat(3,1fr)">
          <div class="ig-stat"><div class="ig-stat-n">379</div><div class="ig-stat-l">Followers</div></div>
          <div class="ig-stat"><div class="ig-stat-n" style="color:var(--orange)">96.7k</div><div class="ig-stat-l">Vues 30j</div></div>
          <div class="ig-stat"><div class="ig-stat-n" style="color:var(--blue)">1 099</div><div class="ig-stat-l">Interactions</div></div>
          <div class="ig-stat"><div class="ig-stat-n" style="color:var(--green)">73.8%</div><div class="ig-stat-l">Non-followers</div></div>
          <div class="ig-stat"><div class="ig-stat-n">2 125</div><div class="ig-stat-l">Visites profil</div></div>
          <div class="ig-stat"><div class="ig-stat-n" style="color:var(--gold2)">169</div><div class="ig-stat-l">Clics liens</div></div>
        </div>
        <div style="margin-top:12px">
          <div class="card-title" style="margin-bottom:8px">Top contenus</div>
          <div class="ig-post-row"><div class="ig-post-thumb">🐆</div><div class="ig-post-name">SAVAGE · léopard</div><div class="ig-post-stat">👁️<span>#1</span></div></div>
          <div class="ig-post-row"><div class="ig-post-thumb">🔴</div><div class="ig-post-name">FOMO · rouge néon</div><div class="ig-post-stat">👁️<span>#2</span></div></div>
          <div class="ig-post-row"><div class="ig-post-thumb">🐆</div><div class="ig-post-name">SAVAGE · visuel 2</div><div class="ig-post-stat">👁️<span>#3</span></div></div>
        </div>
        <div style="margin-top:12px;padding-top:12px;border-top:1px solid var(--border);font-size:11px;color:var(--text3)">
          ⚠️ Mettre à jour les stats Instagram manuellement ci-dessous
        </div>
        <button class="add-btn" style="margin-top:8px;width:100%;text-align:center" onclick="editIgStats()">Mettre à jour les stats</button>
      </div>
    </div>
  `;
 
  // Countdown live pour ce soir
  if(isTonight(soirees[0].next)){
    setInterval(()=>{
      const el=document.getElementById('cdLaCommerciale');
      if(!el)return;
      const diff=soirees[0].next-new Date();
      if(diff<0){el.textContent='C\'est l\'heure 🔥';return;}
      const h=Math.floor(diff/3600000);
      const m=Math.floor((diff%3600000)/60000);
      const s=Math.floor((diff%60000)/1000);
      el.textContent=`Dans ${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
    },1000);
  }
}
 
function nextThursday(){
  const d=new Date();const day=d.getDay();const diff=(4-day+7)%7||7;
  const n=new Date(d);n.setDate(d.getDate()+diff);n.setHours(23,30,0,0);return n;
}
 
function editIgStats(){toast('💡 Connecte Airtable ou mets à jour le code avec tes nouvelles stats Instagram');}
 
// ─── AMBASSADEURS ─────────────────────────────────────────────────────────────
function renderAmb(){
  const data=getData();
  const bc=getBothCodes();
  const eventLabel=currentPage==='commerciale'?'La Commerciale':currentPage==='wildside'?'WildSide':'Les deux événements';
  const eventColor=currentPage==='commerciale'?'#4ade80':currentPage==='wildside'?'#c4b5fd':'#fbbf24';
  const totalV=data.reduce((s,a)=>s+a.ventes,0);
  const actifs=data.filter(a=>a.ventes>0).length;
  const ecoles=new Set(data.filter(a=>a.ecole).map(a=>a.ecole)).size;
  const searchVal=document.getElementById('srch')?document.getElementById('srch').value:'';
 
  let list=[...data];
  if(filterStatus==='actifs')list=list.filter(a=>a.ventes>0);
  if(filterStatus==='zero')list=list.filter(a=>a.ventes===0);
  if(filterStatus==='top')list=list.filter(a=>a.ventes>=10);
  if(searchVal){const s=searchVal.toLowerCase();list=list.filter(a=>a.nom.toLowerCase().includes(s)||a.code.toLowerCase().includes(s)||(a.ecole||'').toLowerCase().includes(s)||(a.ig||'').toLowerCase().includes(s));}
  list.sort((a,b)=>{const va=sortKey==='ventes'?a.ventes:(a[sortKey]||'');const vb=sortKey==='ventes'?b.ventes:(b[sortKey]||'');if(typeof va==='number')return sortDir*(vb-va);return sortDir*String(va).localeCompare(String(vb));});
  const maxV=Math.max(...data.map(x=>x.ventes),1);
 
  document.getElementById('main-content').innerHTML=`
    <div class="page-header">
      <div><div class="page-title" style="color:${eventColor}">${eventLabel}</div><div class="page-sub">${data.length} ambassadeurs · Cliquer sur les ventes pour modifier</div></div>
      <button class="add-btn" onclick="openAddAmb()">+ Ajouter ambassadeur</button>
    </div>
    <div class="kpi-grid">
      <div class="kpi gold"><div class="kpi-n">${data.length}</div><div class="kpi-l">Ambassadeurs</div></div>
      <div class="kpi green"><div class="kpi-n">${totalV}</div><div class="kpi-l">Tickets vendus</div><div class="kpi-sub">objectif : ${data.length*8}</div></div>
      <div class="kpi blue"><div class="kpi-n">${actifs}</div><div class="kpi-l">Ont vendu</div><div class="kpi-sub">${data.length-actifs} à relancer</div></div>
      <div class="kpi purple"><div class="kpi-n">${ecoles||'—'}</div><div class="kpi-l">Écoles</div></div>
    </div>
    <div class="controls">
      <div class="search-box"><span class="search-icon">🔍</span><input type="text" id="srch" placeholder="Rechercher..." value="${searchVal}" oninput="renderAmb()"></div>
      <button class="filter-btn ${filterStatus==='tous'?'active':''}" onclick="setFilter('tous')">Tous (${data.length})</button>
      <button class="filter-btn ${filterStatus==='actifs'?'active':''}" onclick="setFilter('actifs')">✅ Actifs (${actifs})</button>
      <button class="filter-btn ${filterStatus==='zero'?'active':''}" onclick="setFilter('zero')">🔴 0 ventes (${data.length-actifs})</button>
      <button class="filter-btn ${filterStatus==='top'?'active':''}" onclick="setFilter('top')">⭐ Top (${data.filter(a=>a.ventes>=10).length})</button>
    </div>
    <div class="table-wrap">
      <table>
        <thead><tr>
          <th onclick="setSortAmb('nom')" class="${sortKey==='nom'?'sort-'+(sortDir>0?'asc':'desc'):''}">Ambassadeur</th>
          <th onclick="setSortAmb('code')" class="${sortKey==='code'?'sort-'+(sortDir>0?'asc':'desc'):''}">Code</th>
          <th onclick="setSortAmb('ventes')" class="${sortKey==='ventes'?'sort-'+(sortDir>0?'asc':'desc'):''}">Ventes</th>
          <th>Contact</th>
          <th onclick="setSortAmb('ecole')" class="${sortKey==='ecole'?'sort-'+(sortDir>0?'asc':'desc'):''}">École</th>
          <th>Statut</th>
          <th></th>
        </tr></thead>
        <tbody>
        ${list.length===0?`<tr><td colspan="7"><div class="empty"><div class="empty-icon">🔍</div>Aucun résultat</div></td></tr>`:
        list.map((a,i)=>{
          const[bg,fg]=AVT[i%AVT.length];
          const st=getStatus(a.ventes);
          const inBoth=bc.has(a.code);
          const barW=Math.round(a.ventes/maxV*100);
          const barColor=a.ventes>=10?'#22c55e':a.ventes>0?'#f97316':'#333';
          const tel=cleanTel(a.tel);
          return`<tr>
            <td><div class="name-cell"><div class="avatar" style="background:${bg};color:${fg}">${initials(a.nom)}</div><div><div class="amb-name">${a.nom}${inBoth&&currentPage!=='both'?'<span class="both-tag">2 events</span>':''}</div>${a.ecole?`<div class="amb-school">${a.ecole}</div>`:''}</div></div></td>
            <td><span class="code-badge">${a.code}</span></td>
            <td><div class="ventes-wrap"><span class="ventes-num" onclick="editVentes('${a.id}')" title="Modifier">${a.ventes}</span><div class="vbar-bg"><div class="vbar" style="width:${barW}%;background:${barColor}"></div></div></div></td>
            <td style="white-space:nowrap">
              ${tel?`<a href="https://wa.me/${tel.replace(/^0/,'33')}" target="_blank" style="font-size:18px;text-decoration:none;margin-right:6px" title="WhatsApp">💬</a>`:''}
              ${a.ig?`<a href="https://instagram.com/${a.ig}" target="_blank" style="font-size:18px;text-decoration:none" title="Instagram">📸</a>`:''}
            </td>
            <td style="font-size:11px;color:var(--text3)">${a.ecole||'—'}</td>
            <td><span class="status-pill ${st.cls}">${st.label}</span></td>
            <td><button class="act-btn" onclick="openEditAmb('${a.id}')">✏️</button></td>
          </tr>`;
        }).join('')}
        </tbody>
      </table>
    </div>
  `;
}
 
function setFilter(f){filterStatus=f;renderAmb();}
function setSortAmb(k){if(sortKey===k)sortDir*=-1;else{sortKey=k;sortDir=k==='ventes'?-1:1;}renderAmb();}
 
function editVentes(id){
  const data=getData();
  const a=data.find(x=>x.id===id);
  if(!a)return;
  const v=prompt(`Ventes pour ${a.nom} :`,a.ventes);
  if(v!==null&&!isNaN(parseInt(v))){a.ventes=parseInt(v);saveStorage();renderAmb();toast(`✅ ${a.nom} → ${v} ventes`);}
}
 
function openAddAmb(){
  editAmbId=null;editAmbEvent=currentPage==='wildside'?'wildside':'commerciale';
  document.getElementById('modal-amb-title').textContent='+ Ajouter un ambassadeur';
  ['nom','code','ig','tel','ecole'].forEach(f=>document.getElementById('fa-'+f).value='');
  document.getElementById('fa-ventes').value=0;
  ['fa-ev-c','fa-ev-w','fa-ev-wl','fa-ev-f','fa-ev-s'].forEach(id=>document.getElementById(id).checked=false);
  if(currentPage==='wildside')document.getElementById('fa-ev-w').checked=true;
  else document.getElementById('fa-ev-c').checked=true;
  document.getElementById('modal-amb').classList.remove('hidden');
}
 
function openEditAmb(id){
  const data=getData();
  const a=data.find(x=>x.id===id);
  if(!a)return;
  editAmbId=id;editAmbEvent=currentPage;
  document.getElementById('modal-amb-title').textContent='Modifier '+a.nom;
  document.getElementById('fa-nom').value=a.nom;
  document.getElementById('fa-code').value=a.code;
  document.getElementById('fa-ig').value=a.ig;
  document.getElementById('fa-tel').value=a.tel;
  document.getElementById('fa-ecole').value=a.ecole;
  document.getElementById('fa-ventes').value=a.ventes;
  document.getElementById('modal-amb').classList.remove('hidden');
}
 
function saveAmb(){
  const nom=document.getElementById('fa-nom').value.trim();
  const code=document.getElementById('fa-code').value.trim().toUpperCase();
  if(!nom||!code){alert('Nom et code requis');return;}
  const obj={nom,code,ig:document.getElementById('fa-ig').value.trim().replace('@',''),tel:document.getElementById('fa-tel').value.trim(),ecole:document.getElementById('fa-ecole').value.trim(),ventes:parseInt(document.getElementById('fa-ventes').value)||0};
  const useWild=document.getElementById('fa-ev-w').checked;
  const useComm=document.getElementById('fa-ev-c').checked;
  if(editAmbId){
    const data=getData();
    const a=data.find(x=>x.id===editAmbId);
    if(a)Object.assign(a,obj);
    toast('✅ Ambassadeur modifié');
  } else {
    const newId='x'+Date.now();
    if(useComm||(!useWild&&editAmbEvent==='commerciale'))COMMERCIALE.push({id:newId,...obj});
    if(useWild)WILDSIDE.push({id:newId+'w',...obj});
    updateBadges();
    toast('✅ Ambassadeur ajouté');
  }
  saveStorage();closeModals();renderAmb();
}
 
// ─── CONTACTS ──────────────────────────────────────────────────────────────────
function renderContacts(){
  const searchVal=document.getElementById('csrch')?document.getElementById('csrch').value:'';
  let list=[...CONTACTS];
  if(contactFilter!=='tous')list=list.filter(c=>c.role===contactFilter);
  if(searchVal){const s=searchVal.toLowerCase();list=list.filter(c=>(c.prenom+' '+c.nom).toLowerCase().includes(s)||(c.bde||'').toLowerCase().includes(s)||(c.ig||'').toLowerCase().includes(s)||(c.tel||'').includes(s));}
 
  // Grouper les BDE
  const bdeGroups={};
  list.filter(c=>c.role==='bde').forEach(c=>{
    const key=c.bde||'Inconnu';
    if(!bdeGroups[key])bdeGroups[key]=[];
    bdeGroups[key].push(c);
  });
 
  document.getElementById('main-content').innerHTML=`
    <div class="page-header">
      <div><div class="page-title" style="color:var(--blue)">Contacts</div><div class="page-sub">${CONTACTS.length} contacts · Ambassadeurs · Promoteurs · BDE · Partenaires</div></div>
      <button class="add-btn" onclick="openAddContact()">+ Ajouter un contact</button>
    </div>
    <div class="controls">
      <div class="search-box"><span class="search-icon">🔍</span><input type="text" id="csrch" placeholder="Rechercher nom, BDE, Instagram..." value="${searchVal}" oninput="renderContacts()"></div>
      <button class="filter-btn ${contactFilter==='tous'?'active':''}" onclick="setContactFilter('tous')">Tous (${CONTACTS.length})</button>
      <button class="filter-btn ${contactFilter==='ambassadeur'?'active':''}" onclick="setContactFilter('ambassadeur')">Ambassadeurs</button>
      <button class="filter-btn ${contactFilter==='promoteur'?'active':''}" onclick="setContactFilter('promoteur')">Promoteurs</button>
      <button class="filter-btn ${contactFilter==='bde'?'active':''}" onclick="setContactFilter('bde')">BDE</button>
      <button class="filter-btn ${contactFilter==='club'?'active':''}" onclick="setContactFilter('club')">Clubs</button>
    </div>
    ${list.length===0?`<div class="empty"><div class="empty-icon">👥</div><div>Aucun contact</div><div style="margin-top:8px"><button class="add-btn" onclick="openAddContact()">+ Ajouter le premier contact</button></div></div>`:
    `<div class="contacts-grid">${list.map((c,i)=>renderContactCard(c,i)).join('')}</div>`}
  `;
}
 
function renderContactCard(c,i){
  const[bg,fg]=AVT[i%AVT.length];
  const fullName=(c.prenom+' '+c.nom).trim();
  const tel=cleanTel(c.tel);
  const waLink=tel?`https://wa.me/${tel.startsWith('+')?tel.replace('+',''):tel.startsWith('0')?'33'+tel.slice(1):'33'+tel}`:'';
  const roleLabel={ambassadeur:'Ambassadeur',promoteur:'Promoteur',bde:'BDE',club:'Club / Partenaire',autre:'Autre'}[c.role]||c.role;
  const events=(c.events||[]);
  const tagClass={commerciale:'tag-commerciale',wildside:'tag-wildside',wonderland:'tag-wonderland',fomo:'tag-fomo',savage:'tag-savage'};
 
  return`<div class="contact-card">
    <div class="contact-header">
      <div class="contact-avatar" style="background:${bg};color:${fg}">${initials(fullName||'?')}</div>
      <div>
        <div class="contact-name">${fullName||'Sans nom'}</div>
        <div class="contact-role">${roleLabel}${c.post?' · '+c.post:''}${c.bde?' · '+c.bde:''}</div>
      </div>
    </div>
    <div class="contact-body">
      ${tel?`<div class="contact-field"><span class="contact-field-icon">💬</span><a href="${waLink}" target="_blank" class="wa">${c.tel}</a></div>`:''}
      ${c.ig?`<div class="contact-field"><span class="contact-field-icon">📸</span><a href="https://instagram.com/${c.ig.replace('@','')}" target="_blank" class="ig">@${c.ig.replace('@','')}</a></div>`:''}
      ${c.code?`<div class="contact-field"><span class="contact-field-icon">🎟️</span><span class="code-badge">${c.code}</span></div>`:''}
      ${c.notes?`<div class="contact-field"><span class="contact-field-icon">📝</span>${c.notes}</div>`:''}
    </div>
    ${events.length>0?`<div class="contact-tags">${events.map(e=>`<span class="contact-tag ${tagClass[e]||'tag-ambassadeur'}">${e.charAt(0).toUpperCase()+e.slice(1)}</span>`).join('')}</div>`:''}
    <div class="contact-tags" style="padding-top:0">
      <span class="contact-tag ${c.role==='bde'?'tag-bde':c.role==='promoteur'?'tag-promoteur':'tag-ambassadeur'}">${roleLabel}</span>
    </div>
    <div class="contact-actions">
      ${waLink?`<a href="${waLink}" target="_blank" class="c-btn" style="text-decoration:none;color:var(--green);border-color:rgba(37,211,102,.3)">💬 WhatsApp</a>`:''}
      ${c.ig?`<a href="https://instagram.com/${c.ig.replace('@','')}" target="_blank" class="c-btn" style="text-decoration:none;color:var(--pink);border-color:rgba(236,72,153,.3)">📸 Instagram</a>`:''}
      <button class="c-btn edit" onclick="openEditContact('${c.id}')">✏️</button>
    </div>
  </div>`;
}
 
function setContactFilter(f){contactFilter=f;renderContacts();}
 
function openAddContact(){
  editContactId=null;
  document.getElementById('modal-contact-title').textContent='+ Ajouter un contact';
  ['prenom','nom','tel','ig','post','bde','code','notes'].forEach(f=>document.getElementById('fc-'+f).value='');
  document.getElementById('fc-role').value='ambassadeur';
  ['fc-ev-c','fc-ev-w','fc-ev-wl','fc-ev-f','fc-ev-s'].forEach(id=>document.getElementById(id).checked=false);
  document.getElementById('modal-contact').classList.remove('hidden');
}
 
function openEditContact(id){
  const c=CONTACTS.find(x=>x.id===id);
  if(!c)return;
  editContactId=id;
  document.getElementById('modal-contact-title').textContent='Modifier '+c.prenom;
  document.getElementById('fc-prenom').value=c.prenom||'';
  document.getElementById('fc-nom').value=c.nom||'';
  document.getElementById('fc-tel').value=c.tel||'';
  document.getElementById('fc-ig').value=c.ig||'';
  document.getElementById('fc-role').value=c.role||'ambassadeur';
  document.getElementById('fc-post').value=c.post||'';
  document.getElementById('fc-bde').value=c.bde||'';
  document.getElementById('fc-code').value=c.code||'';
  document.getElementById('fc-notes').value=c.notes||'';
  const evMap={commerciale:'fc-ev-c',wildside:'fc-ev-w',wonderland:'fc-ev-wl',fomo:'fc-ev-f',savage:'fc-ev-s'};
  Object.entries(evMap).forEach(([ev,elId])=>document.getElementById(elId).checked=(c.events||[]).includes(ev));
  document.getElementById('modal-contact').classList.remove('hidden');
}
 
function saveContact(){
  const prenom=document.getElementById('fc-prenom').value.trim();
  if(!prenom){alert('Prénom requis');return;}
  const events=['commerciale','wildside','wonderland','fomo','savage'].filter((_,i)=>document.getElementById(['fc-ev-c','fc-ev-w','fc-ev-wl','fc-ev-f','fc-ev-s'][i]).checked);
  const obj={prenom,nom:document.getElementById('fc-nom').value.trim(),tel:document.getElementById('fc-tel').value.trim(),ig:document.getElementById('fc-ig').value.trim(),role:document.getElementById('fc-role').value,post:document.getElementById('fc-post').value.trim(),bde:document.getElementById('fc-bde').value.trim(),code:document.getElementById('fc-code').value.trim().toUpperCase(),notes:document.getElementById('fc-notes').value.trim(),events};
  if(editContactId){
    const idx=CONTACTS.findIndex(c=>c.id===editContactId);
    if(idx>=0)CONTACTS[idx]={...CONTACTS[idx],...obj};
    toast('✅ Contact modifié');
  } else {
    CONTACTS.push({id:'ct'+Date.now(),...obj});
    updateBadges();
    toast('✅ Contact ajouté');
  }
  saveStorage();closeModals();renderContacts();
}
 
function closeModals(){document.getElementById('modal-amb').classList.add('hidden');document.getElementById('modal-contact').classList.add('hidden');}
 
function refreshData(){toast('✅ Données actualisées');renderPage();}
function renderPage(){if(currentPage==='dashboard')renderDashboard();else if(currentPage==='contacts')renderContacts();else renderAmb();}
 
function exportCSV(){
  const data=currentPage==='contacts'?CONTACTS:getData();
  if(!data.length){toast('Aucune donnée');return;}
  const keys=Object.keys(data[0]).filter(k=>k!=='id');
  const rows=[keys,...data.map(d=>keys.map(k=>JSON.stringify(d[k]||'')))];
  const csv=rows.map(r=>r.join(',')).join('\n');
  const a=document.createElement('a');
  a.href='data:text/csv;charset=utf-8,\uFEFF'+encodeURIComponent(csv);
  a.download=`LNG_${currentPage}_${new Date().toISOString().slice(0,10)}.csv`;
  a.click();toast('📥 Export téléchargé');
}
 
function toast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),3000);}
 
// ─── INIT ──────────────────────────────────────────────────────────────────────
loadStorage();
updateBadges();
renderDashboard();
</script>
</body>
</html>
