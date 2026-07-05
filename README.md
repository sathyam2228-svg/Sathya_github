<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Blackboard — Smart Classroom &amp; Timetable Scheduler</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Caveat:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #1E2B23;
    --panel: #24352B;
    --panel-2: #2B3E32;
    --chalk: #EFEAE0;
    --chalk-dim: #B9C2B7;
    --yellow: #E8C468;
    --pink: #D98A8A;
    --blue: #8FB8C9;
    --green: #9CC29B;
    --lilac: #B9A6D9;
    --rule: rgba(239,234,224,0.12);
    --rule-strong: rgba(239,234,224,0.22);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(ellipse at 20% -10%, rgba(255,255,255,0.04), transparent 45%),
      radial-gradient(ellipse at 90% 110%, rgba(255,255,255,0.03), transparent 40%),
      var(--bg);
    color:var(--chalk);
    font-family:'IBM Plex Sans', sans-serif;
    min-height:100vh;
  }
  h1,h2,h3,.chalk-font{font-family:'Caveat', cursive;}
  ::selection{ background: var(--yellow); color:#1E2B23; }

  /* layout */
  .app{ display:flex; min-height:100vh; }
  .sidebar{
    width:340px; flex-shrink:0;
    background: var(--panel);
    border-right: 1px dashed var(--rule-strong);
    padding: 22px 18px 40px;
    overflow-y:auto;
    max-height:100vh;
    position:sticky; top:0;
  }
  .main{ flex:1; padding: 28px 32px 60px; min-width:0; }

  .brand{ display:flex; align-items:center; gap:10px; margin-bottom: 4px; }
  .brand svg{ width:30px; height:30px; flex-shrink:0; }
  .brand h1{ font-size:32px; margin:0; line-height:1; color:var(--yellow); }
  .tagline{ color:var(--chalk-dim); font-size:12.5px; margin: 2px 0 22px; letter-spacing:.02em; }

  .tabbar{ display:flex; gap:6px; margin-bottom:16px; flex-wrap:wrap; }
  .tabbar button{
    background:transparent; border:1px solid var(--rule-strong); color:var(--chalk-dim);
    padding:6px 12px; border-radius:20px; font-size:12.5px; cursor:pointer; font-family:'IBM Plex Sans';
    transition:.15s;
  }
  .tabbar button.active{ background:var(--yellow); color:#1E2B23; border-color:var(--yellow); font-weight:600; }
  .tabbar button:hover:not(.active){ border-color: var(--chalk-dim); color:var(--chalk); }

  .card{
    background: var(--panel-2);
    border:1px solid var(--rule);
    border-radius:10px;
    padding:14px 14px 16px;
    margin-bottom:14px;
  }
  .card h3{ margin:0 0 10px; font-size:22px; color:var(--chalk); }
  label{ display:block; font-size:11.5px; text-transform:uppercase; letter-spacing:.07em; color:var(--chalk-dim); margin: 10px 0 4px; }
  label:first-child{margin-top:0;}
  input[type=text], input[type=number], select{
    width:100%; background:#1C2820; border:1px solid var(--rule-strong); color:var(--chalk);
    padding:8px 10px; border-radius:6px; font-family:'IBM Plex Sans'; font-size:13.5px;
  }
  input:focus, select:focus{ outline:none; border-color: var(--yellow); }
  .row{ display:flex; gap:8px; }
  .row > *{ flex:1; }
  .btn{
    display:inline-flex; align-items:center; justify-content:center; gap:6px;
    background: var(--yellow); color:#1E2B23; border:none; font-weight:600;
    padding:9px 14px; border-radius:7px; cursor:pointer; font-size:13.5px; font-family:'IBM Plex Sans';
    transition:.15s; width:100%; margin-top:10px;
  }
  .btn:hover{ filter:brightness(1.08); transform:translateY(-1px); }
  .btn.secondary{ background:transparent; color:var(--chalk); border:1px solid var(--rule-strong); }
  .btn.secondary:hover{ border-color:var(--chalk); }
  .btn.danger{ background:transparent; color:var(--pink); border:1px solid rgba(217,138,138,.4); width:auto; margin:0; padding:4px 8px; font-size:12px; }
  .btn.small{ width:auto; padding:6px 10px; font-size:12.5px; margin-top:8px; }

  .list-item{
    display:flex; align-items:center; justify-content:space-between; gap:8px;
    padding:7px 9px; border-radius:6px; background:#1C2820; margin-bottom:6px; font-size:13px;
  }
  .list-item .meta{ color:var(--chalk-dim); font-size:11.5px; }
  .chip{ display:inline-block; padding:1px 7px; border-radius:10px; font-size:10.5px; margin-left:6px; }
  .chip.lab{ background:rgba(143,184,201,.18); color:var(--blue); }
  .chip.room{ background:rgba(156,194,155,.18); color:var(--green); }

  .subject-row{ border-left:3px solid var(--rule-strong); padding-left:8px; margin-bottom:8px; }

  /* main header */
  .topbar{ display:flex; align-items:center; justify-content:space-between; flex-wrap:wrap; gap:14px; margin-bottom: 18px; }
  .topbar h2{ font-size:34px; margin:0; color:var(--chalk); }
  .topbar .sub{ color:var(--chalk-dim); font-size:13px; margin-top:2px; }
  .bell-btn{
    display:flex; align-items:center; gap:9px; background:var(--yellow); color:#1E2B23;
    border:none; padding:12px 20px; border-radius:9px; font-weight:700; font-size:14.5px; cursor:pointer;
    box-shadow:0 4px 14px rgba(232,196,104,.18);
  }
  .bell-btn:hover{ filter:brightness(1.06); }
  .bell-btn svg{ width:20px; height:20px; }
  .bell-btn.ring svg{ animation: ring .5s ease 2; }
  @keyframes ring{
    0%,100%{ transform:rotate(0); } 20%{ transform:rotate(14deg);} 40%{ transform:rotate(-12deg);} 60%{ transform:rotate(8deg);} 80%{ transform:rotate(-6deg);}
  }

  .view-tabs{ display:flex; gap:6px; margin-bottom:18px; flex-wrap:wrap; }
  .view-tabs button{
    background:transparent; border:1px solid var(--rule-strong); color:var(--chalk-dim);
    padding:7px 14px; border-radius:7px; font-size:13px; cursor:pointer; font-family:'IBM Plex Sans'; font-weight:500;
  }
  .view-tabs button.active{ background:var(--panel-2); color:var(--yellow); border-color:var(--yellow); }

  .section-pills{ display:flex; gap:6px; flex-wrap:wrap; margin-bottom:16px; }
  .section-pills button{
    background:var(--panel-2); border:1px solid var(--rule); color:var(--chalk-dim);
    padding:6px 13px; border-radius:20px; font-size:12.5px; cursor:pointer; font-family:'IBM Plex Mono';
  }
  .section-pills button.active{ background:var(--blue); color:#1E2B23; border-color:var(--blue); font-weight:600; }

  /* timetable grid */
  .grid-wrap{ overflow-x:auto; border-radius:12px; border:1px dashed var(--rule-strong); background: var(--panel); padding:16px; }
  table.tt{ border-collapse:collapse; width:100%; min-width:760px; font-family:'IBM Plex Mono', monospace; }
  table.tt th{
    font-family:'IBM Plex Sans'; font-weight:600; font-size:11.5px; text-transform:uppercase; letter-spacing:.06em;
    color:var(--chalk-dim); padding:8px 6px; border-bottom:1px solid var(--rule-strong); text-align:center;
  }
  table.tt td{
    border:1px solid var(--rule); vertical-align:top; padding:5px; height:64px; width:13%;
    position:relative; transition:.12s;
  }
  table.tt td.periodcol{
    font-family:'IBM Plex Sans'; color:var(--chalk-dim); font-size:11.5px; width:90px; text-align:right; padding-right:10px; border:none; vertical-align:middle;
  }
  table.tt td.break{ background: repeating-linear-gradient(135deg, rgba(239,234,224,.04) 0 8px, transparent 8px 16px); color:var(--chalk-dim); text-align:center; font-size:11.5px; font-style:italic; vertical-align:middle;}
  table.tt td.slot{ cursor:pointer; }
  table.tt td.slot:hover{ background: rgba(239,234,224,0.05); }
  .cell-card{
    border-left:3px solid var(--yellow); background: rgba(239,234,224,0.05);
    border-radius:5px; padding:5px 7px; font-size:12px; line-height:1.3; height:100%;
  }
  .cell-card .sub{ font-weight:600; font-family:'IBM Plex Sans'; font-size:12.5px; }
  .cell-card .teach{ color:var(--chalk-dim); font-size:10.8px; }
  .cell-card .room{ color:var(--chalk-dim); font-size:10.2px; }
  .cell-card.clash{ border-left-color:var(--pink); background: rgba(217,138,138,0.1); }

  .empty-state{ color:var(--chalk-dim); text-align:center; padding:60px 20px; font-size:15px; }
  .empty-state svg{ width:46px; height:46px; opacity:.5; margin-bottom:10px; }

  .warn-banner{
    background: rgba(217,138,138,.12); border:1px solid rgba(217,138,138,.4); color:var(--pink);
    padding:10px 14px; border-radius:8px; font-size:13px; margin-bottom:16px;
  }
  .ok-banner{
    background: rgba(156,194,155,.12); border:1px solid rgba(156,194,155,.4); color:var(--green);
    padding:10px 14px; border-radius:8px; font-size:13px; margin-bottom:16px;
  }

  /* modal */
  .overlay{ position:fixed; inset:0; background:rgba(10,15,12,.6); display:flex; align-items:center; justify-content:center; z-index:50; }
  .modal{ background:var(--panel-2); border:1px solid var(--rule-strong); border-radius:12px; padding:20px; width:340px; max-width:90vw; }
  .modal h3{ margin:0 0 4px; font-size:24px; }
  .modal .sub{ color:var(--chalk-dim); font-size:12.5px; margin-bottom:14px; }
  .modal-actions{ display:flex; gap:8px; margin-top:14px; }
  .modal-actions .btn{ margin-top:0; }
  .conflict-note{ font-size:12px; color:var(--pink); margin-top:6px; }

  ::-webkit-scrollbar{ height:10px; width:10px; }
  ::-webkit-scrollbar-thumb{ background: var(--rule-strong); border-radius:6px; }

  @media print{
    .sidebar, .topbar .bell-btn, .view-tabs, .section-pills, .tagline{ display:none !important; }
    body{ background:white; color:black; }
    .grid-wrap{ border:1px solid #ccc; background:white; }
    table.tt td{ border:1px solid #999; color:black; }
    .cell-card{ background:#f2f2f2; border-left-color:#333; color:black; }
    .cell-card .teach, .cell-card .room{ color:#444; }
  }

  @media (max-width: 880px){
    .app{ flex-direction:column; }
    .sidebar{ width:100%; max-height:none; position:relative; border-right:none; border-bottom:1px dashed var(--rule-strong); }
    .main{ padding:20px 16px 50px; }
  }
</style>
</head>
<body>
<div class="app">

  <aside class="sidebar">
    <div class="brand">
      <svg viewBox="0 0 24 24" fill="none" stroke="var(--yellow)" stroke-width="1.6"><path d="M12 2v4M8 4h8M6 20h12M7 20V9a5 5 0 0110 0v11"/></svg>
      <h1>Blackboard</h1>
    </div>
    <div class="tagline">Smart Classroom &amp; Timetable Scheduler</div>

    <div class="tabbar" id="setupTabs">
      <button data-tab="config" class="active">Schedule</button>
      <button data-tab="teachers">Teachers</button>
      <button data-tab="rooms">Rooms</button>
      <button data-tab="sections">Sections</button>
    </div>

    <div id="tab-config">
      <div class="card">
        <h3>Week setup</h3>
        <label><input type="checkbox" id="includeSat" style="width:auto;margin-right:6px;">Include Saturday</label>
        <label>Periods per day</label>
        <input type="number" id="periodsPerDay" value="7" min="3" max="10">
        <label>Break after period # (0 = none)</label>
        <input type="number" id="breakPeriod" value="4" min="0" max="10">
        <button class="btn secondary" id="loadSampleBtn">Load sample data</button>
        <button class="btn secondary" id="resetBtn">Clear everything</button>
      </div>
    </div>

    <div id="tab-teachers" style="display:none">
      <div class="card">
        <h3>Add teacher</h3>
        <label>Name</label>
        <input type="text" id="teacherName" placeholder="e.g. Mrs. Iyer">
        <button class="btn" id="addTeacherBtn">Add teacher</button>
      </div>
      <div class="card"><h3>Faculty</h3><div id="teacherList"></div></div>
    </div>

    <div id="tab-rooms" style="display:none">
      <div class="card">
        <h3>Add room</h3>
        <label>Name</label>
        <input type="text" id="roomName" placeholder="e.g. Physics Lab">
        <label>Type</label>
        <select id="roomType"><option value="classroom">Classroom</option><option value="lab">Lab / Special</option></select>
        <button class="btn" id="addRoomBtn">Add room</button>
      </div>
      <div class="card"><h3>Rooms</h3><div id="roomList"></div></div>
    </div>

    <div id="tab-sections" style="display:none">
      <div class="card">
        <h3>Add section</h3>
        <label>Name</label>
        <input type="text" id="sectionName" placeholder="e.g. Grade 9 - A">
        <label>Home classroom</label>
        <select id="sectionHomeRoom"></select>
        <button class="btn" id="addSectionBtn">Add section</button>
      </div>
      <div class="card">
        <h3>Add subject to a section</h3>
        <label>Section</label>
        <select id="subjSection"></select>
        <label>Subject name</label>
        <input type="text" id="subjName" placeholder="e.g. Chemistry">
        <label>Teacher</label>
        <select id="subjTeacher"></select>
        <div class="row">
          <div><label>Periods / week</label><input type="number" id="subjPeriods" value="4" min="1" max="15"></div>
        </div>
        <label><input type="checkbox" id="subjSpecialRoom" style="width:auto;margin-right:6px;">Needs a special room</label>
        <select id="subjRoomSelect" style="display:none;margin-top:6px;"></select>
        <button class="btn" id="addSubjectBtn">Add subject</button>
      </div>
      <div class="card"><h3>Sections</h3><div id="sectionList"></div></div>
    </div>
  </aside>

  <main class="main">
    <div class="topbar">
      <div>
        <h2>This week's timetable</h2>
        <div class="sub" id="statusLine">Set up teachers, rooms and sections, then generate.</div>
      </div>
      <button class="bell-btn" id="generateBtn">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 8a6 6 0 10-12 0c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 01-3.46 0"/></svg>
        Generate timetable
      </button>
    </div>

    <div id="banner"></div>

    <div class="view-tabs" id="viewTabs">
      <button data-view="section" class="active">By section</button>
      <button data-view="teacher">By teacher</button>
      <button data-view="room">By room</button>
    </div>
    <div class="section-pills" id="pillBar"></div>

    <div class="grid-wrap" id="gridWrap">
      <div class="empty-state">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M3 9h18M9 4v16"/></svg>
        <div>No timetable yet — add some data on the left, or load the sample, then hit Generate.</div>
      </div>
    </div>

    <button class="btn secondary small" id="printBtn" style="display:none">Print / save as PDF</button>
  </main>

</div>

<div id="modalRoot"></div>

<script>
(function(){
  "use strict";

  const DAY_NAMES = ["Mon","Tue","Wed","Thu","Fri","Sat"];
  const COLORS = ["#E8C468","#8FB8C9","#D98A8A","#9CC29B","#B9A6D9","#E0A96D"];

  let state = {
    includeSat:false, periodsPerDay:7, breakPeriod:4,
    teachers:[], rooms:[], sections:[],
    timetable:null // { sectionGrid, unplaced }
  };
  let uid = 1;
  const nextId = () => 'id' + (uid++);
  let currentView = 'section';
  let activePillId = null;

  // ---------- storage ----------
  async function save(){
    try{ await window.storage.set('blackboard-scheduler-state', JSON.stringify(state), false); }
    catch(e){ /* storage unavailable, continue in-memory only */ }
  }
  async function load(){
    try{
      const res = await window.storage.get('blackboard-scheduler-state', false);
      if(res && res.value){
        const parsed = JSON.parse(res.value);
        state = Object.assign(state, parsed);
        let maxNum = 0;
        [...state.teachers,...state.rooms,...state.sections].forEach(o=>{
          const n = parseInt((o.id||'').replace('id',''),10); if(!isNaN(n)) maxNum = Math.max(maxNum,n);
        });
        state.sections.forEach(s=> s.subjects.forEach(sub=>{
          const n = parseInt((sub.id||'').replace('id',''),10); if(!isNaN(n)) maxNum = Math.max(maxNum,n);
        }));
        uid = maxNum + 1;
      }
    }catch(e){ /* first run, nothing stored */ }
  }

  function subjColor(name){
    let h = 0; for(let i=0;i<name.length;i++) h = (h*31 + name.charCodeAt(i)) >>> 0;
    return COLORS[h % COLORS.length];
  }

  function days(){ return state.includeSat ? DAY_NAMES : DAY_NAMES.slice(0,5); }
  function periodsList(){
    const arr=[]; for(let p=1;p<=state.periodsPerDay;p++) if(p!==state.breakPeriod) arr.push(p);
    return arr;
  }

  // ---------- setup tabs ----------
  document.getElementById('setupTabs').addEventListener('click', e=>{
    const btn = e.target.closest('button'); if(!btn) return;
    document.querySelectorAll('#setupTabs button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    ['config','teachers','rooms','sections'].forEach(t=>{
      document.getElementById('tab-'+t).style.display = (t===btn.dataset.tab)?'block':'none';
    });
  });

  document.getElementById('includeSat').addEventListener('change', e=>{ state.includeSat = e.target.checked; save(); renderAll(); });
  document.getElementById('periodsPerDay').addEventListener('change', e=>{ state.periodsPerDay = parseInt(e.target.value)||7; save(); });
  document.getElementById('breakPeriod').addEventListener('change', e=>{ state.breakPeriod = parseInt(e.target.value)||0; save(); });

  // ---------- teachers ----------
  document.getElementById('addTeacherBtn').addEventListener('click', ()=>{
    const input = document.getElementById('teacherName');
    const name = input.value.trim(); if(!name) return;
    state.teachers.push({id: nextId(), name});
    input.value=''; save(); renderTeachers(); renderSectionFormOptions();
  });
  function renderTeachers(){
    const el = document.getElementById('teacherList');
    if(state.teachers.length===0){ el.innerHTML = '<div class="meta">No teachers yet.</div>'; return; }
    el.innerHTML = state.teachers.map(t=>`
      <div class="list-item"><span>${escapeHtml(t.name)}</span>
      <button class="btn danger" data-remove-teacher="${t.id}">Remove</button></div>`).join('');
    el.querySelectorAll('[data-remove-teacher]').forEach(b=> b.addEventListener('click', ()=>{
      const id = b.dataset.removeTeacher;
      state.teachers = state.teachers.filter(t=>t.id!==id);
      state.sections.forEach(s=> s.subjects = s.subjects.filter(sub=>sub.teacher!==id));
      save(); renderTeachers(); renderSections(); renderSectionFormOptions();
    }));
  }

  // ---------- rooms ----------
  document.getElementById('addRoomBtn').addEventListener('click', ()=>{
    const input = document.getElementById('roomName');
    const name = input.value.trim(); if(!name) return;
    const type = document.getElementById('roomType').value;
    state.rooms.push({id: nextId(), name, type});
    input.value=''; save(); renderRooms(); renderSectionFormOptions();
  });
  function renderRooms(){
    const el = document.getElementById('roomList');
    if(state.rooms.length===0){ el.innerHTML = '<div class="meta">No rooms yet.</div>'; return; }
    el.innerHTML = state.rooms.map(r=>`
      <div class="list-item"><span>${escapeHtml(r.name)} <span class="chip ${r.type==='lab'?'lab':'room'}">${r.type}</span></span>
      <button class="btn danger" data-remove-room="${r.id}">Remove</button></div>`).join('');
    el.querySelectorAll('[data-remove-room]').forEach(b=> b.addEventListener('click', ()=>{
      const id = b.dataset.removeRoom;
      state.rooms = state.rooms.filter(r=>r.id!==id);
      state.sections.forEach(s=>{ if(s.homeRoom===id) s.homeRoom=null; s.subjects.forEach(sub=>{ if(sub.specialRoom===id) sub.specialRoom=null; }); });
      save(); renderRooms(); renderSections(); renderSectionFormOptions();
    }));
  }

  // ---------- sections ----------
  document.getElementById('addSectionBtn').addEventListener('click', ()=>{
    const input = document.getElementById('sectionName');
    const name = input.value.trim(); if(!name) return;
    const homeRoom = document.getElementById('sectionHomeRoom').value || null;
    state.sections.push({id: nextId(), name, homeRoom, subjects:[]});
    input.value=''; save(); renderSections(); renderSectionFormOptions();
  });

  document.getElementById('subjSpecialRoom').addEventListener('change', e=>{
    document.getElementById('subjRoomSelect').style.display = e.target.checked ? 'block' : 'none';
  });

  document.getElementById('addSubjectBtn').addEventListener('click', ()=>{
    const secId = document.getElementById('subjSection').value;
    const name = document.getElementById('subjName').value.trim();
    const teacher = document.getElementById('subjTeacher').value;
    const periods = parseInt(document.getElementById('subjPeriods').value)||1;
    const needsRoom = document.getElementById('subjSpecialRoom').checked;
    const specialRoom = needsRoom ? (document.getElementById('subjRoomSelect').value||null) : null;
    if(!secId || !name || !teacher) return;
    const sec = state.sections.find(s=>s.id===secId); if(!sec) return;
    sec.subjects.push({id: nextId(), name, teacher, periodsPerWeek: periods, specialRoom});
    document.getElementById('subjName').value='';
    save(); renderSections();
  });

  function renderSections(){
    const el = document.getElementById('sectionList');
    if(state.sections.length===0){ el.innerHTML = '<div class="meta">No sections yet.</div>'; return; }
    el.innerHTML = state.sections.map(s=>{
      const room = state.rooms.find(r=>r.id===s.homeRoom);
      const subjHtml = s.subjects.map(sub=>{
        const t = state.teachers.find(tt=>tt.id===sub.teacher);
        const rm = state.rooms.find(r=>r.id===sub.specialRoom);
        return `<div class="subject-row">
          <div><b>${escapeHtml(sub.name)}</b> — ${sub.periodsPerWeek}/wk</div>
          <div class="meta">${t?escapeHtml(t.name):'No teacher'}${rm?(' · '+escapeHtml(rm.name)):''}</div>
          <button class="btn danger" data-remove-subject="${s.id}|${sub.id}" style="margin-top:4px;">Remove subject</button>
        </div>`;
      }).join('') || '<div class="meta">No subjects added.</div>';
      return `<div class="card" style="background:#1C2820;">
        <h3 style="font-size:19px;">${escapeHtml(s.name)} <span class="meta">(${room?escapeHtml(room.name):'no room'})</span></h3>
        ${subjHtml}
        <button class="btn danger" data-remove-section="${s.id}" style="margin-top:8px;">Remove section</button>
      </div>`;
    }).join('');
    el.querySelectorAll('[data-remove-section]').forEach(b=> b.addEventListener('click', ()=>{
      state.sections = state.sections.filter(s=>s.id!==b.dataset.removeSection);
      save(); renderSections(); renderSectionFormOptions();
    }));
    el.querySelectorAll('[data-remove-subject]').forEach(b=> b.addEventListener('click', ()=>{
      const [secId, subId] = b.dataset.removeSubject.split('|');
      const sec = state.sections.find(s=>s.id===secId);
      if(sec) sec.subjects = sec.subjects.filter(su=>su.id!==subId);
      save(); renderSections();
    }));
  }

  function renderSectionFormOptions(){
    const roomOpts = '<option value="">— none —</option>' + state.rooms.map(r=>`<option value="${r.id}">${escapeHtml(r.name)} (${r.type})</option>`).join('');
    document.getElementById('sectionHomeRoom').innerHTML = roomOpts;
    document.getElementById('subjRoomSelect').innerHTML = state.rooms.filter(r=>r.type==='lab').map(r=>`<option value="${r.id}">${escapeHtml(r.name)}</option>`).join('') || '<option value="">No labs added</option>';
    document.getElementById('subjSection').innerHTML = state.sections.map(s=>`<option value="${s.id}">${escapeHtml(s.name)}</option>`).join('') || '<option value="">Add a section first</option>';
    document.getElementById('subjTeacher').innerHTML = state.teachers.map(t=>`<option value="${t.id}">${escapeHtml(t.name)}</option>`).join('') || '<option value="">Add a teacher first</option>';
  }

  function escapeHtml(s){ return (s||'').replace(/[&<>"']/g, c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c])); }

  // ---------- sample data ----------
  function loadSample(){
    uid = 1;
    const t = n => ({id: nextId(), name:n});
    const teachers = [t('Mrs. Rao'), t('Mr. Fernandes'), t('Ms. Sheikh'), t('Mr. Thomas'), t('Mrs. Iyer'), t('Mr. Batra')];
    const rooms = [
      {id: nextId(), name:'Room 101', type:'classroom'},
      {id: nextId(), name:'Room 102', type:'classroom'},
      {id: nextId(), name:'Science Lab', type:'lab'},
      {id: nextId(), name:'Computer Lab', type:'lab'},
    ];
    const secA = {id: nextId(), name:'Grade 9 - A', homeRoom: rooms[0].id, subjects:[]};
    const secB = {id: nextId(), name:'Grade 9 - B', homeRoom: rooms[1].id, subjects:[]};
    secA.subjects = [
      {id:nextId(), name:'Mathematics', teacher:teachers[0].id, periodsPerWeek:6, specialRoom:null},
      {id:nextId(), name:'English', teacher:teachers[1].id, periodsPerWeek:5, specialRoom:null},
      {id:nextId(), name:'Science', teacher:teachers[2].id, periodsPerWeek:5, specialRoom:rooms[2].id},
      {id:nextId(), name:'Social Studies', teacher:teachers[3].id, periodsPerWeek:4, specialRoom:null},
      {id:nextId(), name:'Computer Science', teacher:teachers[5].id, periodsPerWeek:3, specialRoom:rooms[3].id},
      {id:nextId(), name:'Art', teacher:teachers[4].id, periodsPerWeek:2, specialRoom:null},
    ];
    secB.subjects = [
      {id:nextId(), name:'Mathematics', teacher:teachers[0].id, periodsPerWeek:6, specialRoom:null},
      {id:nextId(), name:'English', teacher:teachers[1].id, periodsPerWeek:5, specialRoom:null},
      {id:nextId(), name:'Science', teacher:teachers[2].id, periodsPerWeek:5, specialRoom:rooms[2].id},
      {id:nextId(), name:'Social Studies', teacher:teachers[3].id, periodsPerWeek:4, specialRoom:null},
      {id:nextId(), name:'Computer Science', teacher:teachers[5].id, periodsPerWeek:3, specialRoom:rooms[3].id},
      {id:nextId(), name:'Music', teacher:teachers[4].id, periodsPerWeek:2, specialRoom:null},
    ];
    state.teachers = teachers; state.rooms = rooms; state.sections = [secA, secB];
    state.includeSat = false; state.periodsPerDay = 7; state.breakPeriod = 4;
    document.getElementById('includeSat').checked = false;
    document.getElementById('periodsPerDay').value = 7;
    document.getElementById('breakPeriod').value = 4;
    state.timetable = null;
    save(); renderAll();
  }
  document.getElementById('loadSampleBtn').addEventListener('click', loadSample);
  document.getElementById('resetBtn').addEventListener('click', ()=>{
    if(!confirm('Clear all teachers, rooms, sections and the generated timetable?')) return;
    state = {includeSat:false, periodsPerDay:7, breakPeriod:4, teachers:[], rooms:[], sections:[], timetable:null};
    uid = 1;
    document.getElementById('includeSat').checked = false;
    document.getElementById('periodsPerDay').value = 7;
    document.getElementById('breakPeriod').value = 4;
    save(); renderAll();
  });

  // ---------- generation algorithm ----------
  function shuffle(arr){
    for(let i=arr.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [arr[i],arr[j]]=[arr[j],arr[i]]; }
    return arr;
  }

  function generate(){
    const D = days(); const P = periodsList();
    if(state.sections.length===0 || D.length===0 || P.length===0) return {sectionGrid:{}, unplaced:[]};

    const allTasks = [];
    state.sections.forEach(sec=>{
      sec.subjects.forEach(sub=>{
        for(let i=0;i<sub.periodsPerWeek;i++){
          allTasks.push({
            sectionId: sec.id, sectionName: sec.name,
            subjectId: sub.id, subjectName: sub.name,
            teacherId: sub.teacher, room: sub.specialRoom || sec.homeRoom
          });
        }
      });
    });

    let best = null, bestUnplaced = Infinity;
    const ATTEMPTS = 250;
    for(let attempt=0; attempt<ATTEMPTS; attempt++){
      const sectionGrid = {}; const teacherGrid = {}; const roomGrid = {};
      state.sections.forEach(s=>{ sectionGrid[s.id]={}; D.forEach(d=> sectionGrid[s.id][d]={}); });

      const tasks = shuffle(allTasks.slice());
      const unplaced = [];
      tasks.forEach(task=>{
        const slotOrder = shuffle(D.flatMap(d=>P.map(p=>[d,p])));
        let placed = false;
        for(const [d,p] of slotOrder){
          if(sectionGrid[task.sectionId][d][p]) continue;
          if(teacherGrid[task.teacherId]?.[d]?.[p]) continue;
          if(task.room && roomGrid[task.room]?.[d]?.[p]) continue;
          sectionGrid[task.sectionId][d][p] = task;
          teacherGrid[task.teacherId] = teacherGrid[task.teacherId]||{};
          teacherGrid[task.teacherId][d] = teacherGrid[task.teacherId][d]||{};
          teacherGrid[task.teacherId][d][p] = true;
          if(task.room){
            roomGrid[task.room] = roomGrid[task.room]||{};
            roomGrid[task.room][d] = roomGrid[task.room][d]||{};
            roomGrid[task.room][d][p] = true;
          }
          placed = true; break;
        }
        if(!placed) unplaced.push(task);
      });
      if(unplaced.length < bestUnplaced){
        bestUnplaced = unplaced.length;
        best = {sectionGrid, unplaced};
        if(bestUnplaced===0) break;
      }
    }
    return best || {sectionGrid:{}, unplaced: allTasks};
  }

  document.getElementById('generateBtn').addEventListener('click', ()=>{
    if(state.sections.length===0){ alert('Add at least one section with subjects first (or load the sample data).'); return; }
    const btn = document.getElementById('generateBtn');
    btn.classList.add('ring');
    setTimeout(()=>btn.classList.remove('ring'), 1000);
    state.timetable = generate();
    activePillId = state.sections[0]?.id || null;
    save();
    renderAll();
  });

  document.getElementById('printBtn').addEventListener('click', ()=> window.print());

  // ---------- views ----------
  document.getElementById('viewTabs').addEventListener('click', e=>{
    const btn = e.target.closest('button'); if(!btn) return;
    currentView = btn.dataset.view;
    document.querySelectorAll('#viewTabs button').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    activePillId = null;
    renderPills(); renderGrid();
  });

  function renderPills(){
    const bar = document.getElementById('pillBar');
    let items = [];
    if(currentView==='section') items = state.sections.map(s=>({id:s.id, label:s.name}));
    if(currentView==='teacher') items = state.teachers.map(t=>({id:t.id, label:t.name}));
    if(currentView==='room') items = state.rooms.map(r=>({id:r.id, label:r.name}));
    if(items.length===0){ bar.innerHTML=''; return; }
    if(!activePillId || !items.find(i=>i.id===activePillId)) activePillId = items[0].id;
    bar.innerHTML = items.map(i=>`<button data-pill="${i.id}" class="${i.id===activePillId?'active':''}">${escapeHtml(i.label)}</button>`).join('');
    bar.querySelectorAll('[data-pill]').forEach(b=> b.addEventListener('click', ()=>{
      activePillId = b.dataset.pill; renderPills(); renderGrid();
    }));
  }

  function taskAt(d,p){
    // returns list of {task, sectionName} depending on view
    const tt = state.timetable; if(!tt) return null;
    if(currentView==='section'){
      const t = tt.sectionGrid[activePillId]?.[d]?.[p];
      return t ? [t] : null;
    }
    if(currentView==='teacher'){
      const found = [];
      Object.keys(tt.sectionGrid).forEach(secId=>{
        const t = tt.sectionGrid[secId][d]?.[p];
        if(t && t.teacherId===activePillId) found.push(t);
      });
      return found.length? found : null;
    }
    if(currentView==='room'){
      const found = [];
      Object.keys(tt.sectionGrid).forEach(secId=>{
        const t = tt.sectionGrid[secId][d]?.[p];
        if(t && t.room===activePillId) found.push(t);
      });
      return found.length? found: null;
    }
    return null;
  }

  function renderGrid(){
    const wrap = document.getElementById('gridWrap');
    const printBtn = document.getElementById('printBtn');
    if(!state.timetable || state.sections.length===0){
      wrap.innerHTML = `<div class="empty-state">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M3 9h18M9 4v16"/></svg>
        <div>No timetable yet — add some data on the left, or load the sample, then hit Generate.</div></div>`;
      printBtn.style.display='none';
      return;
    }
    printBtn.style.display='inline-block';
    const D = days(); const P = [];
    for(let p=1;p<=state.periodsPerDay;p++) P.push(p);

    let html = '<table class="tt"><thead><tr><th></th>';
    D.forEach(d=> html += `<th>${d}</th>`);
    html += '</tr></thead><tbody>';

    P.forEach(p=>{
      html += `<tr><td class="periodcol">Period ${p}${p===state.breakPeriod?' · Break':''}</td>`;
      D.forEach(d=>{
        if(p===state.breakPeriod){
          html += `<td class="break">Break</td>`;
        } else {
          const found = taskAt(d,p);
          if(!found){
            html += `<td class="slot" data-day="${d}" data-period="${p}"></td>`;
          } else if(found.length===1 && currentView==='section'){
            const task = found[0];
            const room = state.rooms.find(r=>r.id===task.room);
            const teacher = state.teachers.find(t=>t.id===task.teacherId);
            html += `<td class="slot" data-day="${d}" data-period="${p}">
              <div class="cell-card" style="border-left-color:${subjColor(task.subjectName)}">
                <div class="sub">${escapeHtml(task.subjectName)}</div>
                <div class="teach">${teacher?escapeHtml(teacher.name):''}</div>
                <div class="room">${room?escapeHtml(room.name):''}</div>
              </div></td>`;
          } else {
            // teacher/room view: show which section(s)
            const clash = found.length>1;
            html += `<td class="slot" data-day="${d}" data-period="${p}">` + found.map(task=>{
              const sec = state.sections.find(s=>s.id===task.sectionId);
              return `<div class="cell-card ${clash?'clash':''}" style="border-left-color:${subjColor(task.subjectName)};margin-bottom:3px;">
                <div class="sub">${escapeHtml(task.subjectName)}</div>
                <div class="teach">${sec?escapeHtml(sec.name):''}</div>
              </div>`;
            }).join('') + `</td>`;
          }
        }
      });
      html += '</tr>';
    });
    html += '</tbody></table>';
    wrap.innerHTML = html;

    if(currentView==='section'){
      wrap.querySelectorAll('td.slot').forEach(td=>{
        td.addEventListener('click', ()=> openCellModal(td.dataset.day, parseInt(td.dataset.period)));
      });
    }
  }

  // ---------- manual override modal (section view only) ----------
  function openCellModal(day, period){
    const sec = state.sections.find(s=>s.id===activePillId);
    if(!sec) return;
    const root = document.getElementById('modalRoot');
    const current = state.timetable.sectionGrid[sec.id][day][period];
    const options = sec.subjects.map(sub=>{
      const t = state.teachers.find(tt=>tt.id===sub.teacher);
      return `<option value="${sub.id}" ${current&&current.subjectId===sub.id?'selected':''}>${escapeHtml(sub.name)} (${t?escapeHtml(t.name):'—'})</option>`;
    }).join('');
    root.innerHTML = `<div class="overlay" id="ovl">
      <div class="modal">
        <h3>${day}, Period ${period}</h3>
        <div class="sub">${escapeHtml(sec.name)}</div>
        <label>Subject</label>
        <select id="modalSubjSelect">
          <option value="">— clear slot —</option>
          ${options}
        </select>
        <div class="conflict-note" id="conflictNote"></div>
        <div class="modal-actions">
          <button class="btn secondary" id="modalCancel">Cancel</button>
          <button class="btn" id="modalSave">Save</button>
        </div>
      </div>
    </div>`;
    const note = document.getElementById('conflictNote');
    function checkConflict(){
      const subId = document.getElementById('modalSubjSelect').value;
      note.textContent = '';
      if(!subId) return;
      const sub = sec.subjects.find(s=>s.id===subId);
      const conflicts = [];
      Object.keys(state.timetable.sectionGrid).forEach(otherSecId=>{
        if(otherSecId===sec.id) return;
        const t = state.timetable.sectionGrid[otherSecId][day]?.[period];
        if(t && t.teacherId===sub.teacher) conflicts.push('teacher double-booked with '+ (state.sections.find(s=>s.id===otherSecId)||{}).name);
        const room = sub.specialRoom || sec.homeRoom;
        if(t && room && t.room===room) conflicts.push('room clash with '+ (state.sections.find(s=>s.id===otherSecId)||{}).name);
      });
      note.textContent = conflicts.length ? '⚠ ' + conflicts.join('; ') : '';
    }
    document.getElementById('modalSubjSelect').addEventListener('change', checkConflict);
    document.getElementById('modalCancel').addEventListener('click', ()=> root.innerHTML='');
    document.getElementById('ovl').addEventListener('click', e=>{ if(e.target.id==='ovl') root.innerHTML=''; });
    document.getElementById('modalSave').addEventListener('click', ()=>{
      const subId = document.getElementById('modalSubjSelect').value;
      if(!subId){
        delete state.timetable.sectionGrid[sec.id][day][period];
      } else {
        const sub = sec.subjects.find(s=>s.id===subId);
        state.timetable.sectionGrid[sec.id][day][period] = {
          sectionId: sec.id, sectionName: sec.name, subjectId: sub.id, subjectName: sub.name,
          teacherId: sub.teacher, room: sub.specialRoom || sec.homeRoom
        };
      }
      root.innerHTML='';
      save(); renderGrid(); renderBanner();
    });
  }

  function renderBanner(){
    const el = document.getElementById('banner');
    const status = document.getElementById('statusLine');
    if(!state.timetable){ el.innerHTML=''; status.textContent = 'Set up teachers, rooms and sections, then generate.'; return; }
    const unplaced = state.timetable.unplaced || [];
    if(unplaced.length===0){
      el.innerHTML = '<div class="ok-banner">All classes placed with no clashes. ✓</div>';
      status.textContent = 'Timetable generated — click any cell to fine-tune it.';
    } else {
      const bySubj = {};
      unplaced.forEach(t=> bySubj[t.subjectName] = (bySubj[t.subjectName]||0)+1);
      const detail = Object.entries(bySubj).map(([n,c])=>`${n} ×${c}`).join(', ');
      el.innerHTML = `<div class="warn-banner">${unplaced.length} period(s) couldn't be auto-placed without a clash: ${escapeHtml(detail)}. Try adding more periods/day, more rooms, or reduce weekly load — or place them manually by clicking an empty cell.</div>`;
      status.textContent = 'Timetable generated with a few unresolved periods — see note above.';
    }
  }

  function renderAll(){
    renderTeachers(); renderRooms(); renderSections(); renderSectionFormOptions();
    renderPills(); renderGrid(); renderBanner();
  }

  load().then(renderAll);
})();
</script>
</body>
</html>

