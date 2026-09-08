[index.html](https://github.com/user-attachments/files/31945330/index.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل الحضور والانصراف</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#F3F6F9; --surface:#FFFFFF; --surface-alt:#F7F9FC;
    --ink:#17212B; --ink-soft:#64748B;
    --primary:#173B5E; --primary-dark:#0E2942;
    --manager:#237A68;
    --admin-role:#8E24AA;
    --accent:#C58A2B;
    --success:#21865B; --success-bg:#E8F6EF;
    --danger:#C23B32; --danger-bg:#FCEBE9;
    --pending-bg:#FFF6DF;
    --border:#D9E1EA;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{font-family:'Cairo',Tahoma,sans-serif; background:var(--bg); color:var(--ink); min-height:100vh;}
  #app{min-height:100vh; display:flex; flex-direction:column;}
  .center-screen{flex:1; display:flex; align-items:center; justify-content:center; padding:24px;}
  .brandmark{display:flex; align-items:center; gap:10px; justify-content:center; margin-bottom:6px;}
  .brand-title{font-size:15px; color:var(--primary); font-weight:700;}

  .panel{background:var(--surface); border:1px solid var(--border); border-radius:14px; padding:34px 30px; width:100%; max-width:410px; position:relative; box-shadow:0 12px 35px rgba(23,59,94,.08);}
  .panel h1{font-size:20px; margin:0 0 4px; font-weight:800; color:var(--primary); text-align:center;}
  .panel .sub{text-align:center; color:var(--ink-soft); font-size:13.5px; margin:0 0 24px;}

  .field{margin-bottom:15px;}
  .field label{display:block; font-size:13px; color:var(--ink-soft); margin-bottom:6px; font-weight:600;}
  .field input, .field select, .field textarea{
    width:100%; padding:10px 12px; border:1px solid var(--border); border-radius:7px;
    font-family:inherit; font-size:14.5px; background:var(--surface-alt); color:var(--ink);
  }
  .field textarea{resize:vertical; min-height:64px;}
  .field input:disabled{opacity:.6;}
  .field input:focus, .field select:focus, .field textarea:focus{outline:2px solid var(--primary); outline-offset:1px; background:var(--surface);}
  .field-hint{font-size:11.5px; color:var(--ink-soft); margin-top:4px;}

  .btn{display:inline-flex; align-items:center; justify-content:center; gap:6px; padding:10px 16px; border-radius:7px; border:1px solid transparent; font-family:inherit; font-weight:700; font-size:13.5px; cursor:pointer;}
  .btn:active{transform:translateY(1px);}
  .btn-primary{background:var(--primary); color:#fff;} .btn-primary:hover{filter:brightness(1.12);}
  .btn-outline{background:transparent; color:var(--primary); border-color:var(--primary);} .btn-outline:hover{background:var(--surface-alt);}
  .btn-block{width:100%;}
  .btn-success{background:var(--success); color:#fff;} .btn-success:hover{filter:brightness(1.1);}
  .btn-warning{background:var(--accent); color:#fff;} .btn-warning:hover{filter:brightness(1.1);}
  .btn-danger{background:transparent; color:var(--danger); border-color:var(--danger);} .btn-danger:hover{background:var(--danger-bg);}
  .btn-wa{background:#25D366; color:#fff; text-decoration:none;} .btn-wa:hover{filter:brightness(1.08);}
  .btn-sm{padding:6px 11px; font-size:12.5px;}
  .btn[disabled]{opacity:.4; cursor:not-allowed;}

  .error-msg{background:var(--danger-bg); color:var(--danger); border:1px solid var(--danger); padding:9px 12px; border-radius:7px; font-size:13px; margin-bottom:14px;}
  .success-msg{background:var(--success-bg); color:var(--success); border:1px solid var(--success); padding:9px 12px; border-radius:7px; font-size:13px; margin-bottom:14px;}
  .info-msg{background:var(--surface-alt); color:var(--ink-soft); border:1px dashed var(--border); padding:14px; border-radius:7px; font-size:13.5px; text-align:center;}

  .topbar{background:linear-gradient(135deg,var(--primary),var(--primary-dark)); color:#fff; padding:11px 22px; display:flex; align-items:center; justify-content:space-between; gap:14px; border-bottom:3px solid var(--accent); flex-wrap:wrap;}
  .topbar.mgr{border-bottom-color:var(--manager);}
  .topbar.admin{border-bottom-color:var(--admin-role);}
  .topbar-right{display:flex; align-items:center; gap:16px; flex:1; min-width:260px;}
  .topbar .brand{display:flex; align-items:center; gap:10px; flex-shrink:0;}
  .topbar .brand .brand-title{color:#fff; font-size:15.5px;}

  .topbar-search-wrap{position:relative; flex:1; max-width:320px; display:flex; align-items:center;}
  .topbar-search-wrap svg{position:absolute; right:11px; color:rgba(255,255,255,0.7); pointer-events:none;}
  .topbar-search-input{
    width:100%; padding:7px 34px 7px 12px; border-radius:20px; border:1px solid rgba(255,255,255,0.25);
    background:rgba(255,255,255,0.15); color:#fff; font-family:inherit; font-size:13px;
    transition:all .2s; outline:none;
  }
  .topbar-search-input::placeholder{color:rgba(255,255,255,0.7);}
  .topbar-search-input:focus{background:rgba(255,255,255,0.25); border-color:#fff;}
  .topbar-search-results{
    position:absolute; top:calc(100% + 6px); right:0; width:100%; min-width:280px;
    background:var(--surface); border:1px solid var(--border); border-radius:10px;
    box-shadow:0 10px 30px rgba(0,0,0,0.18); color:var(--ink); z-index:1000; overflow:hidden;
  }
  .topbar-search-item{padding:9px 12px; border-bottom:1px solid var(--border); cursor:pointer; display:flex; align-items:center; justify-content:space-between; font-size:13px;}
  .topbar-search-item:last-child{border-bottom:none;}
  .topbar-search-item:hover{background:var(--surface-alt); color:var(--primary);}

  .topbar-actions{display:flex; align-items:center; gap:10px; flex-wrap:wrap;}
  .topbar .who-wrap{display:inline-flex; align-items:center; gap:6px; font-size:12.5px; opacity:.96;}
  .topbar .who-code-badge{
    background:rgba(255,255,255,0.2); border:1px solid rgba(255,255,255,0.35);
    border-radius:10px; padding:1px 7px; font-size:11px; font-family:monospace; direction:ltr;
  }
  .link-btn{background:transparent; border:1px solid rgba(255,255,255,.5); color:#fff; padding:6px 12px; border-radius:7px; font-family:inherit; font-size:12.5px; cursor:pointer; font-weight:600;}
  .link-btn:hover{background:rgba(255,255,255,.12);}
  .link-btn-accent{background:rgba(197,138,43,0.3); border-color:var(--accent); color:#fff;}
  .link-btn-accent:hover{background:rgba(197,138,43,0.5);}

  .tabs{display:flex; gap:4px; padding:0 22px; background:var(--bg); border-bottom:1px solid var(--border); overflow-x:auto;}
  .tab-btn{padding:11px 16px; background:var(--surface-alt); border:1px solid var(--border); border-bottom:none; border-radius:6px 6px 0 0; font-family:inherit; font-size:13.5px; font-weight:600; color:var(--ink-soft); cursor:pointer; position:relative; top:1px; white-space:nowrap;}
  .tab-btn.active{background:var(--surface); color:var(--primary); border-bottom:1px solid var(--surface);}
  .tab-badge{display:inline-block; background:var(--accent); color:#fff; font-size:10.5px; font-weight:700; border-radius:10px; padding:1px 6px; margin-right:5px;}

  .content{flex:1; padding:24px 22px 60px; max-width:1080px; width:100%; margin:0 auto;}
  .section-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:14px; flex-wrap:wrap; gap:10px;}
  .section-head h2{font-size:17px; color:var(--primary); margin:0; font-weight:800;}
  .section-head .count{color:var(--ink-soft); font-size:12.5px;}

  .ledger{background:var(--surface); border:1px solid var(--border); border-radius:12px; overflow:hidden; box-shadow:0 4px 16px rgba(23,59,94,.04);}
  .ledger-row{display:flex; align-items:center; gap:12px; padding:13px 16px; border-bottom:1px solid var(--border); flex-wrap:wrap;}
  .ledger-row.disabled-emp{background:#FAF0F0; opacity:0.85;}
  .ledger-row:last-child{border-bottom:none;}
  .ledger-row:nth-child(even):not(.disabled-emp){background:var(--surface-alt);}
  .row-main{flex:1; min-width:180px;}
  .row-main .name{font-weight:700; font-size:14px; color:var(--ink);}
  .row-main .name.clickable{cursor:pointer; color:var(--primary); text-decoration:underline; text-underline-offset:3px;}
  .row-main .name.clickable:hover{color:var(--accent);}
  .row-main .meta{font-size:12px; color:var(--ink-soft); margin-top:2px;}
  .row-actions{display:flex; gap:7px; flex-shrink:0; flex-wrap:wrap; align-items:center;}

  .chip{display:inline-flex; align-items:center; padding:2px 9px; border-radius:12px; font-size:11px; font-weight:700; border:1px solid var(--border); color:var(--ink-soft); background:var(--surface-alt);}
  .chip-dept{color:var(--primary); border-color:var(--primary);}
  .chip-mgr{color:var(--manager); border-color:var(--manager);}
  .chip-admin{color:var(--admin-role); border-color:var(--admin-role);}
  .chip-disabled{color:var(--danger); border-color:var(--danger); background:var(--danger-bg);}
  .chip-code{font-family:monospace; letter-spacing:.5px;}

  .stamp{display:inline-flex; align-items:center; justify-content:center; padding:4px 11px; border-radius:20px; font-size:11px; font-weight:800; transform:rotate(-2deg); border:1.5px dashed;}
  .stamp-approved{color:var(--success); border-color:var(--success); background:var(--success-bg);}
  .stamp-rejected{color:var(--danger); border-color:var(--danger); background:var(--danger-bg);}
  .stamp-pending{color:var(--accent); border-color:var(--accent); background:var(--pending-bg);}
  .stamp-penalty{color:var(--danger); border-color:var(--danger); background:var(--danger-bg); transform:rotate(2deg);}

  .form-card{background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:18px; margin-bottom:18px; box-shadow:0 5px 18px rgba(23,59,94,.05);}
  .form-card h3{margin:0 0 14px; font-size:14.5px; color:var(--primary); font-weight:700;}
  .form-row{display:flex; gap:12px; flex-wrap:wrap;}
  .form-row .field{flex:1; min-width:150px;}

  .today-card{background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:22px; text-align:center; margin-bottom:20px;}
  .today-card .date{color:var(--ink-soft); font-size:13px; margin-bottom:10px;}
  .today-card .times{display:flex; justify-content:center; gap:32px; margin:14px 0 18px;}
  .today-card .time-block strong{display:block; font-size:21px; color:var(--primary); font-weight:800;}
  .today-card .time-block span{font-size:11.5px; color:var(--ink-soft);}

  .dept-banner{background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:14px 18px; margin-bottom:18px; display:flex; align-items:center; justify-content:space-between; flex-wrap:wrap; gap:14px;}
  .dept-banner .item{display:flex; flex-direction:column;}
  .dept-banner .label{font-size:12px; color:var(--ink-soft); margin-bottom:2px;}
  .dept-banner .val{font-size:14.5px; font-weight:800; color:var(--primary);}

  .report-table{width:100%; border-collapse:collapse; font-size:12.5px;}
  .report-table th, .report-table td{border:1px solid var(--border); padding:8px 10px; text-align:center;}
  .report-table th{background:var(--surface-alt); color:var(--primary); font-weight:700;}
  .report-table td:first-child, .report-table th:first-child{text-align:right;}
  .table-scroll{overflow-x:auto; background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:4px;}

  .empty-illustration{text-align:center; padding:36px 20px; color:var(--ink-soft);}
  select.date-picker{max-width:220px;}

  .profile-header{background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:22px; margin-bottom:18px; box-shadow:0 4px 16px rgba(23,59,94,.04);}
  .profile-stats-grid{display:grid; grid-template-columns:repeat(auto-fit, minmax(180px, 1fr)); gap:12px; margin-bottom:18px;}
  .stat-card{background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:14px 16px; text-align:center;}
  .stat-card .num{font-size:22px; font-weight:800; color:var(--primary); margin-bottom:2px;}
  .stat-card .lbl{font-size:12px; color:var(--ink-soft);}

  .backup-card{background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:22px; margin-bottom:20px; box-shadow:0 4px 16px rgba(23,59,94,.04);}
  .backup-options{display:grid; grid-template-columns:repeat(auto-fit, minmax(240px, 1fr)); gap:16px;}
  .backup-box{background:var(--surface-alt); border:1px solid var(--border); border-radius:10px; padding:18px; display:flex; flex-direction:column; justify-content:space-between; gap:12px;}
  .backup-box h4{margin:0; font-size:15px; color:var(--primary); font-weight:700;}
  .backup-box p{margin:0; font-size:12.5px; color:var(--ink-soft); line-height:1.6;}

  @media (max-width:640px){
    .topbar{padding:11px 14px;} .content{padding:16px 14px 50px;}
    .today-card .times{gap:18px;} .ledger-row{flex-direction:column; align-items:flex-start;}
    .row-actions{width:100%;}
    .topbar-right{flex-direction:column; align-items:flex-start; gap:8px;}
    .topbar-search-wrap{max-width:100%; width:100%;}
  }
</style>
</head>
<body>
<div id="app"></div>

<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore-compat.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<script>
window.onerror = function(msg, url, line){
  console.error("System Error caught:", msg, "line:", line);
};

const firebaseConfig = {
  apiKey: "AIzaSyDZm5t35loiHyhz3YApURB3lrNy-6J8VTE",
  authDomain: "hr-inout-98c16.firebaseapp.com",
  projectId: "hr-inout-98c16",
  storageBucket: "hr-inout-98c16.firebasestorage.app",
  messagingSenderId: "558598041502",
  appId: "1:558598041502:web:a5fa963a891dcc9500f08f",
  measurementId: "G-TNB1HBYEY8"
};

let db = null, firebaseReady = false;
try{ 
  if(typeof firebase !== 'undefined'){
    firebase.initializeApp(firebaseConfig); 
    db = firebase.firestore(); 
    firebaseReady = true; 
  }
}catch(e){}

const COLLECTION = 'attendance_app';
async function storageGet(key){
  if(!db) return null;
  try{ const doc = await db.collection(COLLECTION).doc(key).get(); return doc.exists ? doc.data().value : null; }
  catch(e){ return null; }
}
async function storageSet(key, value){
  if(!db) return;
  try{ await db.collection(COLLECTION).doc(key).set({ value }); }catch(e){}
}

const DEFAULT_JOB_TITLES = ['موظف', 'محاسب', 'سائق', 'إداري', 'خدمة عملاء', 'مندوب توصيل'];
const LEAVE_TYPES = { annual_leave:{label:'إجازة سنوية'}, casual_leave:{label:'إجازة عارضة'} };
const REQUEST_TYPES = {
  late:               { label:'إذن حضور متأخر' },
  early_leave:        { label:'إذن انصراف مبكر' },
  annual_leave:       { label:'إجازة سنوية' },
  casual_leave:       { label:'إجازة عارضة' },
  deduction_leave:    { label:'إجازة خصم بيوم' },
  rest_day_work:      { label:'عمل يوم راحة' },
  assignment:         { label:'تكليف' },
  no_fingerprint_in:  { label:'حضور بدون بصمة' },
  no_fingerprint_out: { label:'انصراف بدون بصمة' }
};
const PENALTY_TYPES = {
  quarter_day:{ label:'جزاء ربع يوم', weight:0.25 },
  half_day:   { label:'جزاء نص يوم', weight:0.5 },
  one_day:    { label:'جزاء يوم', weight:1 },
  two_days:   { label:'جزاء يومين', weight:2 },
  three_days: { label:'جزاء 3 أيام', weight:3 }
};

let DB = { adminConfig:null, departments:[], people:[], requests:[], penalties:[], attendance:{}, jobTitles:[], leaveBalances:{}, auditLogs:[] };
let UI = {
  view:'loginUnified', adminAuthed:false, currentPerson:null, setupTargetPerson:null,
  selectedPersonProfileId:null, topbarSearchQuery:'',
  activeTab:'departments', mgrTab:'people', error:'',
  reqFilter:'pending', attDate:null, attDeptFilter:'all',
  reportMonth:null, reportDept:'all', penaltyReportMonth:null, penaltyReportDept:'all', leaveDept:'all', leaveReportMonth:null
};

function uid(){ return Date.now().toString(36) + Math.random().toString(36).slice(2,7); }
function todayStr(){ return new Date().toISOString().slice(0,10); }
function monthStr(){ return new Date().toISOString().slice(0,7); }
function nowTime(){ const d=new Date(); return String(d.getHours()).padStart(2,'0')+':'+String(d.getMinutes()).padStart(2,'0'); }
function fmtDate(s){
  if(!s) return '—';
  try{
    const d = new Date(s+'T00:00:00');
    const days=['الأحد','الاثنين','الثلاثاء','الأربعاء','الخميس','الجمعة','السبت'];
    const months=['يناير','فبراير','مارس','أبريل','مايو','يونيو','يوليو','أغسطس','سبتمبر','أكتوبر','نوفمبر','ديسمبر'];
    return days[d.getDay()] + ' ' + d.getDate() + ' ' + months[d.getMonth()];
  }catch(e){ return s; }
}
function esc(s){ const d=document.createElement('div'); d.innerText = s==null?'':String(s); return d.innerHTML; }
function deptName(id){ const d = DB.departments.find(x=>x.id===id); return d ? d.name : 'بدون قسم'; }
function personById(id){ return DB.people.find(p=>p.id===id); }
function findPersonByLogin(identifier){
  if(!identifier) return null;
  const clean = identifier.trim().toLowerCase();
  return DB.people.find(p => 
    (p.code && p.code.trim().toLowerCase() === clean) || 
    (p.name && p.name.trim().toLowerCase() === clean)
  );
}
function managerOf(deptId){ return DB.people.find(p=>p.role==='manager' && p.departmentId===deptId); }

function saveSession(data){ try{ localStorage.setItem('attendance_active_session', JSON.stringify(data)); }catch(e){} }
function clearSession(){ try{ localStorage.removeItem('attendance_active_session'); }catch(e){} }
function restoreSession(){
  try{
    const raw = localStorage.getItem('attendance_active_session');
    if(!raw) return false;
    const sess = JSON.parse(raw);
    if(sess.role === 'admin' && DB.adminConfig){
      UI.adminAuthed = true; UI.currentPerson = null; UI.view = 'adminDashboard'; UI.activeTab = 'departments'; return true;
    } else if(sess.role === 'person' && sess.id){
      const p = personById(sess.id);
      if(p && p.password){
        if(p.disabled){ clearSession(); return false; }
        UI.currentPerson = p;
        if(p.role === 'admin'){ UI.adminAuthed = false; UI.view = 'adminDashboard'; UI.activeTab = 'departments'; }
        else if(p.role === 'manager'){ UI.view = 'managerDashboard'; UI.mgrTab = 'people'; }
        else { UI.view = 'employeeDashboard'; }
        return true;
      }
    }
  }catch(e){}
  return false;
}

function loadLocalCache(){
  try{
    const cCfg = localStorage.getItem('attendance_admin_config'); if(cCfg) DB.adminConfig = JSON.parse(cCfg);
    const cDeps = localStorage.getItem('attendance_cache_departments'); if(cDeps) DB.departments = JSON.parse(cDeps);
    const cPeople = localStorage.getItem('attendance_cache_people'); if(cPeople) DB.people = JSON.parse(cPeople);
    const cReqs = localStorage.getItem('attendance_cache_requests'); if(cReqs) DB.requests = JSON.parse(cReqs);
    const cPens = localStorage.getItem('attendance_cache_penalties'); if(cPens) DB.penalties = JSON.parse(cPens);
    const cAtt = localStorage.getItem('attendance_cache_attendance'); if(cAtt) DB.attendance = JSON.parse(cAtt);
    const cJobs = localStorage.getItem('attendance_cache_jobs'); if(cJobs) DB.jobTitles = JSON.parse(cJobs);
    const cLeaves = localStorage.getItem('attendance_cache_leaves'); if(cLeaves) DB.leaveBalances = JSON.parse(cLeaves);
    const cLogs = localStorage.getItem('attendance_cache_logs'); if(cLogs) DB.auditLogs = JSON.parse(cLogs);
  }catch(e){}
}

function saveLocalCache(){
  try{
    if(DB.adminConfig) localStorage.setItem('attendance_admin_config', JSON.stringify(DB.adminConfig));
    localStorage.setItem('attendance_cache_departments', JSON.stringify(DB.departments));
    localStorage.setItem('attendance_cache_people', JSON.stringify(DB.people));
    localStorage.setItem('attendance_cache_requests', JSON.stringify(DB.requests));
    localStorage.setItem('attendance_cache_penalties', JSON.stringify(DB.penalties));
    localStorage.setItem('attendance_cache_attendance', JSON.stringify(DB.attendance));
    localStorage.setItem('attendance_cache_jobs', JSON.stringify(DB.jobTitles));
    localStorage.setItem('attendance_cache_leaves', JSON.stringify(DB.leaveBalances));
    localStorage.setItem('attendance_cache_logs', JSON.stringify(DB.auditLogs));
  }catch(e){}
}

function watchLiveUpdates(){
  if(!db) return;
  const keys = ['departments','people','requests','penalties','attendance','admin_config','leave_balances'];
  keys.forEach(k=>{
    db.collection(COLLECTION).doc(k).onSnapshot(doc=>{
      if(!doc.exists) return;
      if(k==='admin_config') DB.adminConfig = doc.data().value;
      else if(k==='leave_balances') DB.leaveBalances = doc.data().value ?? {};
      else DB[k] = doc.data().value ?? (k==='attendance' ? {} : []);
      saveLocalCache();
      render();
    }, err => {});
  });
}

async function loadAll(){
  UI.attDate = todayStr();
  UI.reportMonth = monthStr();
  loadLocalCache();
  if(!restoreSession()) {
    UI.view = DB.adminConfig ? 'loginUnified' : 'adminSetup';
  }
  render();

  if(!firebaseReady) return;

  try {
    const [cfg, deps, people, reqs, pens, att, jobs, leaves, logs] = await Promise.all([
      storageGet('admin_config'), storageGet('departments'), storageGet('people'),
      storageGet('requests'), storageGet('penalties'), storageGet('attendance'),
      storageGet('job_titles'), storageGet('leave_balances'), storageGet('audit_logs')
    ]);

    if(cfg) DB.adminConfig = cfg;
    if(deps) DB.departments = deps;
    if(people) DB.people = people;
    if(reqs) DB.requests = reqs;
    if(pens) DB.penalties = pens;
    if(att) DB.attendance = att;
    if(jobs && jobs.length) DB.jobTitles = jobs;
    if(leaves) DB.leaveBalances = leaves;
    if(logs) DB.auditLogs = logs;

    if(!DB.jobTitles || !DB.jobTitles.length) DB.jobTitles = DEFAULT_JOB_TITLES.slice();

    saveLocalCache();
    if(!restoreSession()) {
      UI.view = DB.adminConfig ? 'loginUnified' : 'adminSetup';
    }
    render();
    watchLiveUpdates();
  } catch(e) {}
}

function saveDepartments(){ saveLocalCache(); return storageSet('departments', DB.departments); }
function savePeople(){ saveLocalCache(); return storageSet('people', DB.people); }
function saveRequests(){ saveLocalCache(); return storageSet('requests', DB.requests); }
function savePenalties(){ saveLocalCache(); return storageSet('penalties', DB.penalties); }
function saveAttendance(){ saveLocalCache(); return storageSet('attendance', DB.attendance); }
function saveAdminConfig(){ saveLocalCache(); return storageSet('admin_config', DB.adminConfig); }
function saveJobTitles(){ saveLocalCache(); return storageSet('job_titles', DB.jobTitles); }
function saveLeaveBalances(){ saveLocalCache(); return storageSet('leave_balances', DB.leaveBalances); }
function saveAuditLogs(){ saveLocalCache(); return storageSet('audit_logs', DB.auditLogs); }

async function audit(action, details){
  const actor = UI.adminAuthed ? (DB.adminConfig?.name||'الأدمن الرئيسي') : (UI.currentPerson?.name||'النظام');
  DB.auditLogs.unshift({id:uid(), action, details, actor, createdAt:new Date().toISOString()});
  if(DB.auditLogs.length>500) DB.auditLogs=DB.auditLogs.slice(0,500);
  await saveAuditLogs();
}

function createAndSaveArabicExcel(wb, sheetName, rows, fileName){
  if(typeof XLSX === 'undefined'){ alert('مكتبة الإكسل غير جاهزة حالياً'); return; }
  const ws = XLSX.utils.json_to_sheet(rows && rows.length ? rows : [{'تنبيه': 'لا توجد بيانات'}]);
  ws['!views'] = [{ rightToLeft: true, RTL: true }];
  if(!wb.Workbook) wb.Workbook = {};
  if(!wb.Workbook.Views) wb.Workbook.Views = [];
  wb.Workbook.Views[0] = { ...(wb.Workbook.Views[0] || {}), RTL: true };

  if(rows && rows.length > 0){
    const keys = Object.keys(rows[0]);
    ws['!cols'] = keys.map(k => {
      let maxLen = k.length;
      rows.forEach(r => {
        const valStr = r[k] ? String(r[k]) : '';
        if(valStr.length > maxLen) maxLen = valStr.length;
      });
      return { wch: Math.max(maxLen + 6, 15) };
    });
  }
  XLSX.utils.book_append_sheet(wb, ws, sheetName);
  XLSX.writeFile(wb, fileName);
}

function downloadEmployeeExcelTemplate(){
  const wb = XLSX.utils.book_new();
  const sampleData = [
    { 'الاسم': 'أحمد محمود علي', 'الكود': '101', 'الوظيفة': 'محاسب', 'القسم': 'الحسابات', 'نوع الصلاحية': 'موظف', 'رقم الواتساب': '201001234567', 'رصيد سنوية': 21, 'رصيد عارضة': 7 },
    { 'الاسم': 'سارة إبراهيم حسن', 'الكود': '102', 'الوظيفة': 'مدير المبيعات', 'القسم': 'المبيعات', 'نوع الصلاحية': 'مدير قسم', 'رقم الواتساب': '201009876543', 'رصيد سنوية': 21, 'رصيد عارضة': 7 },
    { 'الاسم': 'محمود كمال عادل', 'الكود': '103', 'الوظيفة': 'مسؤول تقني', 'القسم': 'الإدارة', 'نوع الصلاحية': 'أدمن', 'رقم الواتساب': '201112233445', 'رصيد سنوية': 21, 'رصيد عارضة': 7 }
  ];
  createAndSaveArabicExcel(wb, 'بيانات الموظفين', sampleData, 'نموذج_إدخال_الموظفين.xlsx');
}

async function importEmployeesFromExcel(file){
  if(!file) return;
  if(typeof XLSX === 'undefined'){ alert('مكتبة الإكسل غير جاهزة'); return; }
  const reader = new FileReader();
  reader.onload = async (e) => {
    try {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, { type: 'array' });
      const firstSheetName = workbook.SheetNames[0];
      const worksheet = workbook.Sheets[firstSheetName];
      const rows = XLSX.utils.sheet_to_json(worksheet);

      if(!rows || rows.length === 0){ alert('الملف فارغ'); return; }

      let addedCount = 0;
      for(let i = 0; i < rows.length; i++){
        const row = rows[i];
        const name = (row['الاسم'] || row['اسم الموظف'] || row['Name'] || '').toString().trim();
        const code = (row['الكود'] || row['الكود الوظيفي'] || row['Code'] || '').toString().trim();
        const job = (row['الوظيفة'] || row['المسمى الوظيفي'] || 'موظف').toString().trim();
        const dept = (row['القسم'] || row['اسم القسم'] || 'عام').toString().trim();
        const roleStr = (row['نوع الصلاحية'] || row['الصلاحية'] || 'موظف').toString().trim();
        const phone = (row['رقم الواتساب'] || row['الواتساب'] || '').toString().trim();
        const annualBal = parseInt(row['رصيد سنوية'] || row['سنوية'] || 0, 10) || 0;
        const casualBal = parseInt(row['رصيد عارضة'] || row['عارضة'] || 0, 10) || 0;

        if(!name) continue;

        let department = DB.departments.find(d => d.name.trim().toLowerCase() === dept.toLowerCase());
        if(!department && dept){
          department = { id: uid(), name: dept };
          DB.departments.push(department);
        }
        const departmentId = department ? department.id : '';

        if(job && !DB.jobTitles.includes(job)) DB.jobTitles.push(job);

        let role = 'employee';
        if(roleStr.includes('أدمن') || roleStr.toLowerCase().includes('admin')) role = 'admin';
        else if(roleStr.includes('مدير') || roleStr.toLowerCase().includes('manager')) role = 'manager';

        const newPersonId = uid();
        DB.people.push({
          id: newPersonId, name, jobTitle: job, role, departmentId,
          password: '', phone, code, disabled: false, createdAt: new Date().toISOString()
        });

        if(annualBal > 0 || casualBal > 0){
          DB.leaveBalances[newPersonId] = {
            annual_leave: { allocated: annualBal, used: 0 },
            casual_leave: { allocated: casualBal, used: 0 }
          };
        }
        addedCount++;
      }

      await Promise.all([saveDepartments(), savePeople(), saveJobTitles(), saveLeaveBalances()]);
      alert(`✅ تم استيراد ${addedCount} موظف بنجاح!`);
      render();
    } catch(err) { alert('حدث خطأ أثناء قراءة ملف الإكسل: ' + err.message); }
  };
  reader.readAsArrayBuffer(file);
}

function exportFullJsonBackup(){
  const fullBackupData = {
    exportDate: new Date().toISOString(),
    appName: "سجل الحضور والانصراف",
    version: "2.0",
    data: {
      adminConfig: DB.adminConfig, departments: DB.departments, people: DB.people,
      requests: DB.requests, penalties: DB.penalties, attendance: DB.attendance,
      jobTitles: DB.jobTitles, leaveBalances: DB.leaveBalances, auditLogs: DB.auditLogs
    }
  };
  const blob = new Blob([JSON.stringify(fullBackupData, null, 2)], { type: "application/json;charset=utf-8" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `نسخة_احتياطية_${todayStr()}.json`;
  a.click();
  URL.revokeObjectURL(url);
}

async function restoreFromJsonFile(file){
  if(!file) return;
  const reader = new FileReader();
  reader.onload = async (e)=>{
    try{
      const parsed = JSON.parse(e.target.result);
      if(!parsed || !parsed.data){ alert('الملف غير صالح'); return; }
      if(!confirm('استرجاع النسخة سيقوم باستبدال كافة البيانات الحالية بالبيانات الموجودة في الملف. هل تود المتابعة؟')) return;
      const d = parsed.data;
      if(d.departments) DB.departments = d.departments;
      if(d.people) DB.people = d.people;
      if(d.requests) DB.requests = d.requests;
      if(d.penalties) DB.penalties = d.penalties;
      if(d.attendance) DB.attendance = d.attendance;
      if(d.jobTitles) DB.jobTitles = d.jobTitles;
      if(d.leaveBalances) DB.leaveBalances = d.leaveBalances;
      if(d.adminConfig) DB.adminConfig = d.adminConfig;
      if(d.auditLogs) DB.auditLogs = d.auditLogs;

      await Promise.all([
        saveDepartments(), savePeople(), saveRequests(), savePenalties(),
        saveAttendance(), saveJobTitles(), saveLeaveBalances(), saveAdminConfig(), saveAuditLogs()
      ]);
      alert('✅ تم استرجاع كافة البيانات بنجاح!');
      render();
    } catch(err){ alert('حدث خطأ أثناء قراءة الملف: ' + err.message); }
  };
  reader.readAsText(file);
}

function exportFullExcelBackup(){
  if(typeof XLSX === 'undefined'){ alert('مكتبة الإكسل غير متصلة.'); return; }
  const wb = XLSX.utils.book_new();
  wb.Workbook = { Views: [{ RTL: true }] };

  function addArabicSheet(sheetName, rows){
    const ws = XLSX.utils.json_to_sheet(rows && rows.length ? rows : [{'تنبيه': 'لا توجد بيانات'}]);
    ws['!views'] = [{ rightToLeft: true, RTL: true }];
    if(rows && rows.length > 0){
      const keys = Object.keys(rows[0]);
      ws['!cols'] = keys.map(k => {
        let maxLen = k.length;
        rows.forEach(r => {
          const valStr = r[k] ? String(r[k]) : '';
          if(valStr.length > maxLen) maxLen = valStr.length;
        });
        return { wch: Math.max(maxLen + 6, 15) };
      });
    }
    XLSX.utils.book_append_sheet(wb, ws, sheetName);
  }

  addArabicSheet('الموظفون', DB.people.map(p => ({
    'الكود': p.code || '—', 'الاسم': p.name, 'الوظيفة': p.jobTitle || '—', 'القسم': deptName(p.departmentId),
    'نوع الصلاحية': p.role === 'admin' ? 'أدمن' : p.role === 'manager' ? 'مدير قسم' : 'موظف',
    'حالة الحساب': p.disabled ? 'معطل' : 'نشط', 'رقم الواتساب': p.phone || '—'
  })));

  addArabicSheet('الأقسام', DB.departments.map(d => ({
    'اسم القسم': d.name, 'مدير القسم': managerOf(d.id)?.name || 'بدون مدير'
  })));

  XLSX.writeFile(wb, `نسخة_شاملة_للنظام_${todayStr()}.xlsx`);
}

function getLeaveBalance(emp, type){
  if(!emp) return {allocated:0, used:0, remaining:0, isSet:false};
  const key=emp.id; const b=DB.leaveBalances[key]||{};
  const entry = b[type];
  if(!entry || entry.allocated === undefined || entry.allocated === null){
    return {allocated:0, used:Number(entry?.used||0), remaining:0, isSet:false};
  }
  const allocated = Number(entry.allocated||0);
  const used = Number(entry.used||0);
  return {allocated, used, remaining:Math.max(0, allocated - used), isSet:true};
}

function requestDateLabel(r){
  const start=r.startDate||r.date, end=r.endDate||r.date;
  return (!end || start===end) ? fmtDate(start) : (fmtDate(start) + " إلى " + fmtDate(end));
}

function logoSvg(light){
  const c = light ? '#fff' : 'var(--primary)';
  return '<svg width="28" height="28" viewBox="0 0 30 30" fill="none"><rect x="1.5" y="1.5" width="27" height="27" rx="4" stroke="'+c+'" stroke-width="1.6"/><path d="M8 11h14M8 15.5h9" stroke="'+c+'" stroke-width="1.6" stroke-linecap="round"/><path d="M8 20.5l2.4 2.4L15.5 18" stroke="var(--accent)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>';
}

function render(){
  const app = document.getElementById('app');
  if(!app) return;
  if(UI.view==='adminSetup'){ app.innerHTML = renderAdminSetup(); attachAdminSetupEvents(); return; }
  if(UI.view==='loginUnified'){ app.innerHTML = renderLoginUnified(); attachLoginUnifiedEvents(); return; }
  if(UI.view==='empSetPassword'){ app.innerHTML = renderEmpSetPassword(); attachEmpSetPasswordEvents(); return; }
  if(UI.view==='personProfile'){ app.innerHTML = renderPersonProfile(); attachPersonProfileEvents(); return; }
  if(UI.view==='adminDashboard'){ app.innerHTML = renderAdminDashboard(); attachAdminDashboardEvents(); return; }
  if(UI.view==='managerDashboard'){ app.innerHTML = renderManagerDashboard(); attachManagerDashboardEvents(); return; }
  if(UI.view==='employeeDashboard'){ app.innerHTML = renderEmployeeDashboard(); attachEmployeeDashboardEvents(); return; }
}

function renderAdminSetup(){
  return `<div class="center-screen"><div class="panel">
    <h1>إعداد الأدمن الرئيسي</h1><p class="sub">أول مرة يتم فيها فتح السجل</p>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <div class="field"><label>اسم الأدمن الرئيسي</label><input id="setupName" type="text" placeholder="اسم الأدمن"></div>
    <div class="field"><label>كلمة المرور</label><input id="setupPass" type="password" placeholder="6 أحرف على الأقل"></div>
    <div class="field"><label>تأكيد كلمة المرور</label><input id="setupPass2" type="password"></div>
    <button class="btn btn-primary btn-block" id="setupSubmit">إنشاء الحساب والبدء</button>
  </div></div>`;
}
function attachAdminSetupEvents(){
  const sub = document.getElementById('setupSubmit');
  if(sub) sub.onclick = async ()=>{
    const name = document.getElementById('setupName').value.trim();
    const pass = document.getElementById('setupPass').value;
    const pass2 = document.getElementById('setupPass2').value;
    if(!name){ UI.error='اكتب اسم الأدمن'; render(); return; }
    if(pass.length<6){ UI.error='كلمة المرور يجب ألا تقل عن 6 أحرف'; render(); return; }
    if(pass!==pass2){ UI.error='كلمتا المرور غير متطابقتين'; render(); return; }
    DB.adminConfig = { name, password: pass, createdAt:new Date().toISOString() };
    await saveAdminConfig();
    saveSession({ role:'admin' });
    UI.adminAuthed=true; UI.error=''; UI.view='adminDashboard'; UI.activeTab='departments'; render();
  };
}

function renderLoginUnified(){
  return `<div class="center-screen"><div class="panel">
    <div class="brandmark">${logoSvg(false)}<span class="brand-title" style="font-size:18px;">سجل الحضور والانصراف</span></div>
    <p class="sub">تسجيل الدخول</p>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <div class="field">
      <label>الاسم أو الكود الوظيفي</label>
      <input id="uInput" type="text" placeholder="اكتب اسمك أو الكود الخاص بك" autocomplete="off" autofocus>
    </div>
    <div class="field" id="uPassWrap">
      <label>كلمة المرور</label>
      <input id="uPass" type="password" placeholder="كلمة المرور (اتركها فارغة إذا كان دخولك لأول مرة)">
    </div>
    <button class="btn btn-primary btn-block" id="uSubmit">دخول</button>
    <div style="margin-top:14px; text-align:center;">
      <button class="link-btn" id="firstTimeLoginBtn" style="color:var(--primary); font-size:12px; border:none; background:transparent; cursor:pointer; text-decoration:underline;">دخول لأول مرة؟ اضغط هنا لإنشاء كلمة المرور</button>
    </div>
  </div></div>`;
}

function attachLoginUnifiedEvents(){
  const btn = document.getElementById('uSubmit');
  const firstTimeBtn = document.getElementById('firstTimeLoginBtn');

  if(firstTimeBtn){
    firstTimeBtn.onclick = ()=>{
      const inputVal = document.getElementById('uInput').value.trim();
      if(!inputVal){ UI.error = 'اكتب اسمك أو الكود أولاً'; render(); return; }
      const p = findPersonByLogin(inputVal);
      if(!p){ UI.error = 'لم يتم العثور على هذا الحساب.'; render(); return; }
      if(p.disabled){ UI.error = 'تم تعطيل هذا الحساب من قبل الإدارة.'; render(); return; }
      if(p.password){ UI.error = 'هذا الحساب لديه كلمة مرور بالفعل! أدخلها للدخول.'; render(); return; }
      UI.setupTargetPerson = p; UI.error = ''; UI.view = 'empSetPassword'; render();
    };
  }

  if(!btn) return;
  const submit = async ()=>{
    const inputVal = document.getElementById('uInput').value.trim();
    const pass = document.getElementById('uPass').value;
    if(!inputVal){ UI.error='من فضلك أدخل الاسم أو الكود'; render(); return; }

    const mainAdminName = DB.adminConfig?.name?.trim().toLowerCase();
    if((mainAdminName && inputVal.toLowerCase() === mainAdminName) || inputVal.toLowerCase() === 'admin'){
      if(pass === DB.adminConfig?.password){
        saveSession({ role:'admin' });
        UI.adminAuthed = true;
        UI.currentPerson = null;
        UI.error = '';
        UI.view = 'adminDashboard';
        UI.activeTab = 'departments';
        render();
        return;
      }
    }

    const p = findPersonByLogin(inputVal);
    if(p){
      if(p.disabled){ UI.error = 'تم تعطيل هذا الحساب من قبل الإدارة.'; render(); return; }
      if(!p.password){ UI.setupTargetPerson = p; UI.error = ''; UI.view = 'empSetPassword'; render(); return; }

      if(p.password === pass){
        saveSession({ role:'person', id: p.id });
        UI.currentPerson = p;
        UI.error = '';
        if(p.role === 'admin'){ UI.adminAuthed = false; UI.view = 'adminDashboard'; UI.activeTab = 'departments'; }
        else if(p.role === 'manager'){ UI.view = 'managerDashboard'; UI.mgrTab = 'people'; }
        else { UI.view = 'employeeDashboard'; }
        render();
        return;
      }
    }

    UI.error = 'بيانات الدخول غير صحيحة، تأكد من الاسم/الكود وكلمة المرور';
    render();
  };
  btn.onclick = submit;
  document.getElementById('uPass').addEventListener('keydown', e=>{ if(e.key==='Enter') submit(); });
  document.getElementById('uInput').addEventListener('keydown', e=>{ if(e.key==='Enter') submit(); });
}

function renderEmpSetPassword(){
  const p = UI.setupTargetPerson;
  return `<div class="center-screen"><div class="panel">
    <div class="brandmark">${logoSvg(false)}<span class="brand-title" style="font-size:18px;">إنشاء كلمة مرور جديدة</span></div>
    <p class="sub">مرحباً بك يا <strong>${esc(p?.name||'')}</strong></p>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <div class="field"><label>كلمة المرور الجديدة</label><input id="newEmpPass" type="password" placeholder="4 خانات على الأقل" autofocus></div>
    <div class="field"><label>تأكيد كلمة المرور</label><input id="newEmpPass2" type="password"></div>
    <button class="btn btn-primary btn-block" id="empPassSubmit">حفظ والدخول</button>
  </div></div>`;
}

function attachEmpSetPasswordEvents(){
  const submitBtn = document.getElementById('empPassSubmit');
  if(!submitBtn) return;
  submitBtn.onclick = async ()=>{
    const pass = document.getElementById('newEmpPass').value;
    const pass2 = document.getElementById('newEmpPass2').value;
    const p = UI.setupTargetPerson;
    if(!p){ UI.view = 'loginUnified'; render(); return; }
    if(!pass || pass.length < 4){ UI.error = 'كلمة المرور لا تقل عن 4 خانات'; render(); return; }
    if(pass !== pass2){ UI.error = 'كلمتا المرور غير متطابقتين'; render(); return; }

    p.password = pass;
    await savePeople();
    saveSession({ role:'person', id: p.id });
    UI.currentPerson = p; UI.setupTargetPerson = null; UI.error = '';
    if(p.role === 'admin'){ UI.adminAuthed = false; UI.view = 'adminDashboard'; UI.activeTab = 'departments'; }
    else if(p.role === 'manager'){ UI.view = 'managerDashboard'; UI.mgrTab = 'people'; }
    else { UI.view = 'employeeDashboard'; }
    render();
  };
}

function openPersonProfile(personId){
  UI.selectedPersonProfileId = personId;
  UI.view = 'personProfile';
  render();
}

function renderPersonProfile(){
  const p = personById(UI.selectedPersonProfileId);
  if(!p) return `<div class="center-screen"><div class="panel"><h1>لم يتم العثور على الموظف</h1><button class="btn btn-primary btn-block" id="backFromProfile">رجوع</button></div></div>`;

  const annualBal = getLeaveBalance(p, 'annual_leave');
  const casualBal = getLeaveBalance(p, 'casual_leave');
  const isAdmin = UI.adminAuthed || (UI.currentPerson && UI.currentPerson.role==='admin');

  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`الملف الشخصي — ${esc(p.name)}`, 'admin')}
    <div class="content">
      <div style="margin-bottom:14px; display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:8px;">
        <button class="btn btn-outline btn-sm" id="backFromProfile">← رجوع</button>
        <div style="display:flex; gap:8px;">
          ${isAdmin ? `<button class="btn ${p.disabled?'btn-success':'btn-warning'} btn-sm" data-toggle-status="${p.id}">${p.disabled?'إعادة تشغيل الحساب':'تعطيل الحساب'}</button>` : ''}
          ${isAdmin ? `<button class="btn btn-outline btn-sm" data-edit-leave="${p.id}">تعديل رصيد الإجازات (أدمن فقط)</button>` : ''}
          ${isAdmin ? `<button class="btn btn-outline btn-sm" data-reset-pass="${p.id}">تغيير كلمة المرور</button>` : ''}
        </div>
      </div>

      <div class="today-card" style="text-align:right;">
        <h1 style="font-size:20px; color:var(--primary); margin:0 0 6px;">${esc(p.name)} ${p.disabled ? '<span class="chip chip-disabled">معطل</span>' : '<span class="chip" style="color:var(--success);">نشط</span>'}</h1>
        <div style="color:var(--ink-soft); font-size:13px; display:flex; gap:10px; flex-wrap:wrap;">
          <span>الوظيفة: <strong>${esc(p.jobTitle || 'موظف')}</strong></span>
          <span>القسم: <strong class="chip chip-dept">${esc(deptName(p.departmentId))}</strong></span>
          <span>الكود: <strong class="chip chip-code">${esc(p.code || 'بدون')}</strong></span>
        </div>
      </div>

      <div class="profile-stats-grid">
        <div class="stat-card"><div class="num" style="color:var(--success);">${annualBal.isSet ? `${annualBal.remaining}/${annualBal.allocated}` : '—'}</div><div class="lbl">رصيد السنوية المتبقي</div></div>
        <div class="stat-card"><div class="num" style="color:var(--manager);">${casualBal.isSet ? `${casualBal.remaining}/${casualBal.allocated}` : '—'}</div><div class="lbl">رصيد العارضة المتبقي</div></div>
      </div>
    </div>
  </div>`;
}

function attachPersonProfileEvents(){
  attachTopbarSearchEvents();
  attachLogout();
  const backBtn = document.getElementById('backFromProfile');
  if(backBtn){
    backBtn.onclick = ()=>{
      if(UI.adminAuthed || UI.currentPerson?.role === 'admin') UI.view = 'adminDashboard';
      else if(UI.currentPerson?.role === 'manager') UI.view = 'managerDashboard';
      else UI.view = 'employeeDashboard';
      render();
    };
  }

  const isAdmin = UI.adminAuthed || (UI.currentPerson && UI.currentPerson.role==='admin');

  document.querySelectorAll('[data-toggle-status]').forEach(b=>{
    b.onclick = async ()=>{
      if(!isAdmin) return;
      const p = personById(b.dataset.toggleStatus);
      if(!p) return;
      p.disabled = !p.disabled;
      await savePeople();
      render();
    };
  });

  document.querySelectorAll('[data-reset-pass]').forEach(b=>{
    b.onclick = async ()=>{
      if(!isAdmin) return;
      const p = personById(UI.selectedPersonProfileId);
      if(!p) return;
      const np = prompt(`أدخل كلمة المرور الجديدة للموظف (${p.name}):`, '');
      if(!np || np.trim().length < 4){ alert('يجب ألا تقل كلمة المرور عن 4 خانات'); return; }
      p.password = np.trim();
      await savePeople();
      alert('✅ تم تحديث كلمة المرور بنجاح');
      render();
    };
  });

  const editLeaveBtn = document.querySelector('[data-edit-leave]');
  if(editLeaveBtn){
    editLeaveBtn.onclick = async ()=>{
      if(!isAdmin) return;
      const p = personById(UI.selectedPersonProfileId);
      if(!p) return;
      const a = getLeaveBalance(p, 'annual_leave');
      const c = getLeaveBalance(p, 'casual_leave');
      const av = prompt(`رصيد السنوية لـ (${p.name}):`, a.isSet ? a.allocated : '');
      if(av === null) return;
      const cv = prompt(`رصيد العارضة لـ (${p.name}):`, c.isSet ? c.allocated : '');
      if(cv === null) return;

      DB.leaveBalances[p.id] = {
        annual_leave: { allocated: Math.max(0, parseInt(av,10)||0), used: a.used },
        casual_leave: { allocated: Math.max(0, parseInt(cv,10)||0), used: c.used }
      };
      await saveLeaveBalances();
      render();
    };
  }
}

function topbar(whoText, typeClass){
  const isAdmin = UI.adminAuthed || (UI.currentPerson && UI.currentPerson.role==='admin');
  const showSearch = isAdmin || (UI.currentPerson && UI.currentPerson.role==='manager');

  let searchDropdownHtml = '';
  if(UI.topbarSearchQuery && UI.topbarSearchQuery.trim().length >= 1){
    const q = UI.topbarSearchQuery.trim().toLowerCase();
    const results = DB.people.filter(p => 
      (p.name && p.name.toLowerCase().includes(q)) || 
      (p.code && p.code.toLowerCase().includes(q))
    ).slice(0, 6);

    if(results.length > 0){
      const items = results.map(p => `
        <div class="topbar-search-item" data-open-profile="${p.id}">
          <div>
            <strong>${esc(p.name)}</strong>
            ${p.disabled ? '<span class="chip chip-disabled" style="font-size:10px; margin-right:4px;">معطل</span>' : ''}
            <span style="font-size:11.5px; color:var(--ink-soft); margin-right:6px;">(${esc(deptName(p.departmentId))})</span>
          </div>
          <span class="chip chip-code">${esc(p.code || 'بدون كود')}</span>
        </div>
      `).join('');
      searchDropdownHtml = `<div class="topbar-search-results">${items}</div>`;
    } else {
      searchDropdownHtml = `<div class="topbar-search-results"><div style="padding:12px; text-align:center; font-size:12.5px; color:var(--ink-soft);">لا توجد نتائج مطابقة</div></div>`;
    }
  }

  const searchBoxHtml = showSearch ? `
    <div class="topbar-search-wrap">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
      <input type="text" class="topbar-search-input" id="topbarSearchInput" placeholder="بحث سريع عن موظف بالاسم أو الكود..." value="${esc(UI.topbarSearchQuery || '')}" autocomplete="off">
      ${searchDropdownHtml}
    </div>
  ` : '';

  const userObj = UI.currentPerson;
  const userCodeBadge = userObj && userObj.code ? ` <span class="who-code-badge">#${esc(userObj.code)}</span>` : '';

  return `<div class="topbar ${typeClass||''}">
    <div class="topbar-right">
      <div class="brand">${logoSvg(true)}<span class="brand-title">سجل الحضور</span></div>
      ${searchBoxHtml}
    </div>
    <div class="topbar-actions">
      ${isAdmin ? `<button class="link-btn link-btn-accent" id="quickBackupBtn" title="تنزيل نسخة احتياطية فورية لقاعدة البيانات">💾 نسخة احتياطية</button>` : ''}
      <span class="who">${esc(whoText)}${userCodeBadge}</span>
      <button class="link-btn" id="logoutBtn">تسجيل الخروج</button>
    </div>
  </div>`;
}

function attachTopbarSearchEvents(){
  const searchInput = document.getElementById('topbarSearchInput');
  if(searchInput){
    searchInput.oninput = (e)=>{
      UI.topbarSearchQuery = e.target.value;
      render();
      const el = document.getElementById('topbarSearchInput');
      if(el){ el.focus(); el.setSelectionRange(el.value.length, el.value.length); }
    };
  }

  document.querySelectorAll('[data-open-profile]').forEach(b=>{
    b.onclick = ()=>{ openPersonProfile(b.dataset.openProfile); };
  });

  const quickBackup = document.getElementById('quickBackupBtn');
  if(quickBackup){
    quickBackup.onclick = ()=>{ exportFullJsonBackup(); };
  }
}

function attachLogout(){
  const logoutBtn = document.getElementById('logoutBtn');
  if(logoutBtn){
    logoutBtn.onclick = ()=>{
      clearSession();
      UI.adminAuthed = false;
      UI.currentPerson = null;
      UI.view = 'loginUnified';
      UI.error = '';
      render();
    };
  }
}

function renderAdminDashboard(){
  const pending = DB.requests.filter(r=>r.status==='pending').length;
  const adminName = UI.adminAuthed ? (DB.adminConfig?.name||'الأدمن الرئيسي') : `${UI.currentPerson?.name} (أدمن)`;
  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`لوحة التحكم — ${esc(adminName)}`, 'admin')}
    <div class="tabs">
      <button class="tab-btn ${UI.activeTab==='departments'?'active':''}" data-tab="departments">الأقسام</button>
      <button class="tab-btn ${UI.activeTab==='people'?'active':''}" data-tab="people">الموظفين والصلاحيات</button>
      <button class="tab-btn ${UI.activeTab==='jobs'?'active':''}" data-tab="jobs">الوظائف</button>
      <button class="tab-btn ${UI.activeTab==='leaveBalances'?'active':''}" data-tab="leaveBalances">أرصدة الإجازات</button>
      <button class="tab-btn ${UI.activeTab==='leaveReport'?'active':''}" data-tab="leaveReport">تقرير الإجازات</button>
      <button class="tab-btn ${UI.activeTab==='requests'?'active':''}" data-tab="requests">${pending>0?`<span class="tab-badge">${pending}</span>`:''}الطلبات</button>
      <button class="tab-btn ${UI.activeTab==='penalties'?'active':''}" data-tab="penalties">الجزاءات</button>
      <button class="tab-btn ${UI.activeTab==='penaltyReport'?'active':''}" data-tab="penaltyReport">تقرير الجزاءات</button>
      <button class="tab-btn ${UI.activeTab==='attendance'?'active':''}" data-tab="attendance">الحضور اليومي</button>
      <button class="tab-btn ${UI.activeTab==='reports'?'active':''}" data-tab="reports">التقرير الشهري</button>
      <button class="tab-btn ${UI.activeTab==='backup'?'active':''}" data-tab="backup">النسخ الاحتياطي</button>
      <button class="tab-btn ${UI.activeTab==='audit'?'active':''}" data-tab="audit">سجل العمليات</button>
    </div>
    <div class="content">
      ${UI.activeTab==='departments' ? renderDepartmentsTab() : ''}
      ${UI.activeTab==='people' ? renderPeopleTab(true, null) : ''}
      ${UI.activeTab==='jobs' ? renderJobsTab() : ''}
      ${UI.activeTab==='leaveBalances' ? renderLeaveBalancesTab(true, null) : ''}
      ${UI.activeTab==='leaveReport' ? renderLeaveReportTab(true, null) : ''}
      ${UI.activeTab==='requests' ? renderRequestsTab(true, null) : ''}
      ${UI.activeTab==='penalties' ? renderPenaltiesTab(true, null) : ''}
      ${UI.activeTab==='penaltyReport' ? renderPenaltyReportTab(true, null) : ''}
      ${UI.activeTab==='attendance' ? renderAttendanceTab(true, null) : ''}
      ${UI.activeTab==='reports' ? renderReportsTab(true, null) : ''}
      ${UI.activeTab==='backup' ? renderBackupTab() : ''}
      ${UI.activeTab==='audit' ? renderAuditTab() : ''}
    </div>
  </div>`;
}

function renderDepartmentsTab(){
  const rows = DB.departments.map(d=>{
    const mgr = managerOf(d.id);
    const empCount = DB.people.filter(p=>p.departmentId===d.id && p.role==='employee').length;
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(d.name)}</div><div class="meta">${mgr? 'المدير: '+esc(mgr.name) : 'بدون مدير'} · ${empCount} موظف</div></div><div class="row-actions"><button class="btn btn-danger btn-sm" data-remove-dept="${d.id}">حذف</button></div></div>`;
  }).join('');
  return `<div class="section-head"><h2>الأقسام</h2><span class="count">${DB.departments.length}</span></div>
  <div class="form-card"><h3>إضافة قسم جديد</h3><div class="form-row"><div class="field"><input id="newDeptName" type="text" placeholder="اسم القسم" autocomplete="off"></div></div><button class="btn btn-primary" id="addDeptBtn">إضافة القسم</button></div>
  <div class="ledger">${rows || '<div class="empty-illustration">لا توجد أقسام</div>'}</div>`;
}

function renderPeopleTab(isAdmin, deptScopeId){
  const scoped = isAdmin ? DB.people : DB.people.filter(p=>p.departmentId===deptScopeId && p.role==='employee');
  const rows = scoped.map(p=>{
    let roleBadge = p.role === 'admin' ? '<span class="chip chip-admin">أدمن</span>' : p.role === 'manager' ? '<span class="chip chip-mgr">مدير</span>' : '<span class="chip">موظف</span>';
    return `<div class="ledger-row ${p.disabled?'disabled-emp':''}"><div class="row-main"><div class="name clickable" data-open-profile="${p.id}">${esc(p.name)} 🔍 ${roleBadge}</div><div class="meta">${esc(p.jobTitle||'')} · ${esc(deptName(p.departmentId))} · كود: ${esc(p.code||'بدون')}</div></div>
    <div class="row-actions">
      <button class="btn btn-outline btn-sm" data-open-profile="${p.id}">الملف</button>
      ${isAdmin ? `<button class="btn btn-outline btn-sm" data-set-code="${p.id}">${p.code?'تعديل الكود':'إدخال الكود'}</button>` : ''}
      ${isAdmin ? `<button class="btn btn-outline btn-sm" data-reset-pass-quick="${p.id}">كلمة المرور</button>` : ''}
      ${isAdmin ? `<button class="btn btn-outline btn-sm" data-change-role="${p.id}">الصلاحية</button>` : ''}
      ${isAdmin ? `<button class="btn btn-danger btn-sm" data-remove-person="${p.id}">حذف</button>` : ''}
    </div></div>`;
  }).join('');
  const deptOptions = DB.departments.map(d=>`<option value="${d.id}">${esc(d.name)}</option>`).join('');

  return `
  <div class="section-head">
    <h2>الموظفون</h2>
    <div style="display:flex; gap:8px; align-items:center; flex-wrap:wrap;">
      <span class="count">${scoped.length} شخص</span>
      ${isAdmin ? `
        <button class="btn btn-outline btn-sm" id="downloadEmpTemplateBtn" title="تنزيل نموذج ملف Excel">📥 تنزيل نموذج الإكسل</button>
        <input type="file" id="importEmpExcelInput" accept=".xlsx, .xls, .csv" style="display:none;">
        <button class="btn btn-success btn-sm" id="triggerImportEmpBtn">📊 استيراد موظفين من ملف Excel</button>
      ` : ''}
    </div>
  </div>
  ${isAdmin ? `
  <div class="form-card"><h3>إضافة موظف جديد يدوياً</h3>
    <div class="form-row">
      <div class="field"><label>الاسم</label><input id="newPName" type="text" placeholder="اسم الموظف" autocomplete="off"></div>
      <div class="field"><label>الكود</label><input id="newPCode" type="text" placeholder="مثال: 101" autocomplete="off"></div>
      <div class="field"><label>الوظيفة</label><select id="newPJob">${DB.jobTitles.map(j=>`<option value="${esc(j)}">${esc(j)}</option>`).join('')}</select></div>
    </div>
    <div class="form-row">
      <div class="field"><label>الصلاحية</label><select id="newPRole"><option value="employee">موظف</option><option value="manager">مدير قسم</option><option value="admin">أدمن</option></select></div>
      <div class="field"><label>القسم</label><select id="newPDept">${deptOptions || '<option value="">لا توجد أقسام</option>'}</select></div>
    </div>
    <button class="btn btn-primary" id="addPersonBtn">إضافة الموظف</button>
  </div>` : ''}
  <div class="ledger">${rows || '<div class="empty-illustration">لا يوجد موظفون</div>'}</div>`;
}

function renderJobsTab(){
  const rows = DB.jobTitles.map((j,i)=>`<div class="ledger-row"><div class="row-main"><div class="name">${esc(j)}</div></div><div class="row-actions"><button class="btn btn-danger btn-sm" data-remove-job="${i}">حذف</button></div></div>`).join('');
  return `<div class="section-head"><h2>الوظائف</h2></div><div class="form-card"><h3>إضافة وظيفة</h3><div class="form-row"><div class="field"><input id="newJobTitle" type="text" placeholder="اسم الوظيفة"></div></div><button class="btn btn-primary" id="addJobBtn">إضافة</button></div><div class="ledger">${rows}</div>`;
}

function renderLeaveBalancesTab(isAdmin, deptScopeId){
  const people = DB.people.filter(p=>p.role==='employee' && (isAdmin || p.departmentId===deptScopeId));
  const rows = people.map(p=>{
    const a = getLeaveBalance(p, 'annual_leave');
    const c = getLeaveBalance(p, 'casual_leave');
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(p.name)}</div><div class="meta">سنوية: ${a.remaining}/${a.allocated} | عارضة: ${c.remaining}/${c.allocated}</div></div>
    <div class="row-actions">${isAdmin ? `<button class="btn btn-outline btn-sm" data-edit-leave="${p.id}">تعديل الرصيد (أدمن)</button>` : ''}</div></div>`;
  }).join('');
  return `<div class="section-head"><h2>أرصدة الإجازات</h2></div><div class="ledger">${rows || '<div class="empty-illustration">لا توجد بيانات</div>'}</div>`;
}

function renderRequestsTab(isAdmin, deptScopeId){
  const list = DB.requests.filter(r=>isAdmin ? true : (r.departmentId===deptScopeId && r.empId!==UI.currentPerson?.id));
  const rows = list.map(r=>{
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(r.empName)} — ${REQUEST_TYPES[r.type]?.label||r.type}</div><div class="meta">${r.reason}</div></div><div class="row-actions"><span class="stamp ${r.status==='approved'?'stamp-approved':'stamp-pending'}">${r.status==='approved'?'معتمد':'قيد المراجعة'}</span>${r.status==='pending'?`<button class="btn btn-success btn-sm" data-approve="${r.id}">موافقة</button><button class="btn btn-danger btn-sm" data-reject="${r.id}">رفض</button>`:''}</div></div>`;
  }).join('');
  return `<div class="section-head"><h2>الطلبات</h2></div><div class="ledger">${rows || '<div class="empty-illustration">لا توجد طلبات</div>'}</div>`;
}

function renderPenaltiesTab(isAdmin, deptScopeId){
  const scopedPeople = isAdmin ? DB.people.filter(p=>p.role==='employee') : DB.people.filter(p=>p.departmentId===deptScopeId && p.role==='employee');
  const list = DB.penalties.filter(pn=>isAdmin ? true : pn.departmentId===deptScopeId);
  const rows = list.map(pn=>`<div class="ledger-row"><div class="row-main"><div class="name">${esc(pn.empName)} — ${PENALTY_TYPES[pn.type]?.label}</div><div class="meta">${pn.reason} · بواسطة: ${esc(pn.appliedByName)}</div></div></div>`).join('');
  return `<div class="section-head"><h2>الجزاءات</h2><span class="count">${list.length}</span></div>
  <div class="form-card"><h3>إضافة جزاء جديد</h3>
    <div class="form-row">
      <div class="field"><label>الموظف</label><select id="penEmp">${scopedPeople.map(p=>`<option value="${p.id}">${p.name}</option>`).join('')}</select></div>
      <div class="field"><label>نوع الجزاء</label><select id="penType">${Object.entries(PENALTY_TYPES).map(([k,v])=>`<option value="${k}">${v.label}</option>`).join('')}</select></div>
    </div>
    <div class="field"><label>سبب الجزاء</label><textarea id="penReason" placeholder="اكتب سبب الجزاء"></textarea></div>
    <button class="btn btn-primary" id="addPenBtn">تسجيل الجزاء</button>
  </div>
  <div class="ledger">${rows || '<div class="empty-illustration">لا توجد جزاءات مسجلة</div>'}</div>`;
}

function renderAttendanceTab(isAdmin, deptScopeId){
  const date = UI.attDate || todayStr();
  const employees = DB.people.filter(p=>p.role==='employee' && (isAdmin || p.departmentId===deptScopeId));
  const rows = employees.map(e=>{
    const att = DB.attendance[e.id+'_'+date] || {};
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(e.name)}</div><div class="meta">دخول: ${att.checkIn||'—'} | انصراف: ${att.checkOut||'—'}</div></div></div>`;
  }).join('');
  return `<div class="section-head"><h2>الحضور اليومي</h2><input type="date" class="date-picker" id="attDatePick" value="${date}"></div><div class="ledger">${rows}</div>`;
}

function renderReportsTab(isAdmin, deptScopeId){
  const month = UI.reportMonth || monthStr();
  return `<div class="section-head"><h2>التقرير الشهري</h2>
    <div style="display:flex; align-items:center; gap:8px;">
      <input type="month" class="date-picker" id="reportMonthPick" value="${month}">
      <button class="btn btn-outline btn-sm" id="downloadReportBtn">تنزيل Excel</button>
    </div>
  </div><div class="info-msg">تقارير الحضور والإجازات والجزاءات.</div>`;
}

function renderPenaltyReportTab(){
  return `<div class="section-head"><h2>تقرير الجزاءات التفصيلي</h2><button class="btn btn-outline btn-sm" id="downloadPenaltyReportBtn">تنزيل Excel</button></div><div class="info-msg">بيان تفصيلي بخصومات وجزاءات الموظفين.</div>`;
}

function renderLeaveReportTab(){
  return `<div class="section-head"><h2>تقرير الإجازات التفصيلي</h2><button class="btn btn-outline btn-sm" id="downloadLeaveReportBtn">تنزيل Excel</button></div><div class="info-msg">بيان تفصيلي بإجازات الموظفين المعتمدة.</div>`;
}

function renderBackupTab(){
  return `<div class="section-head"><h2>إدارة النسخ الاحتياطي</h2></div>
  <div class="backup-card">
    <div class="backup-options">
      <div class="backup-box">
        <h4>استرجاع البيانات من نسخة (Restore)</h4>
        <input type="file" id="importJsonFileInput" accept=".json" style="display:none;">
        <button class="btn btn-primary btn-block" id="triggerImportBtn">📥 استرجاع ملف JSON</button>
      </div>
      <div class="backup-box">
        <h4>تنزيل نسخة احتياطية رقمية</h4>
        <button class="btn btn-outline btn-block" id="exportJsonBtn">💾 تنزيل ملف JSON</button>
      </div>
      <div class="backup-box">
        <h4>تنزيل ملف إكسل شامل للنظام</h4>
        <button class="btn btn-success btn-block" id="exportFullExcelBtn">📊 تنزيل مصنف Excel</button>
      </div>
    </div>
  </div>`;
}

function renderAuditTab(){
  const rows = DB.auditLogs.slice(0, 100).map(x=>`<div class="ledger-row"><div class="row-main"><div class="name">${esc(x.action)}</div><div class="meta">${esc(x.details)} · بواسطة: ${esc(x.actor)} · ${new Date(x.createdAt).toLocaleString('ar-EG')}</div></div></div>`).join('');
  return `<div class="section-head"><h2>سجل العمليات والتعديلات</h2></div><div class="ledger">${rows || '<div class="empty-illustration">لا توجد عمليات مسجلة</div>'}</div>`;
}

function attachAdminDashboardEvents(){
  attachTopbarSearchEvents();
  attachLogout();
  document.querySelectorAll('.tab-btn').forEach(b=> {
    b.onclick = ()=>{ UI.activeTab = b.dataset.tab; render(); };
  });

  const templateBtn = document.getElementById('downloadEmpTemplateBtn');
  if(templateBtn) templateBtn.onclick = () => downloadEmployeeExcelTemplate();

  const triggerImportBtn = document.getElementById('triggerImportEmpBtn');
  const importInput = document.getElementById('importEmpExcelInput');
  if(triggerImportBtn && importInput){
    triggerImportBtn.onclick = () => importInput.click();
    importInput.onchange = (e) => {
      const file = e.target.files[0];
      if(file) importEmployeesFromExcel(file);
      importInput.value = '';
    };
  }

  if(UI.activeTab==='departments'){
    const addBtn = document.getElementById('addDeptBtn');
    if(addBtn) addBtn.onclick = async ()=>{
      const name = document.getElementById('newDeptName').value.trim();
      if(!name) return;
      DB.departments.push({ id: uid(), name });
      await saveDepartments(); render();
    };
    document.querySelectorAll('[data-remove-dept]').forEach(b=> {
      b.onclick = async ()=>{
        DB.departments = DB.departments.filter(d=>d.id!==b.dataset.removeDept);
        await saveDepartments(); render();
      };
    });
  }

  if(UI.activeTab==='people'){
    const addP = document.getElementById('addPersonBtn');
    if(addP) addP.onclick = async ()=>{
      const name = document.getElementById('newPName').value.trim();
      const code = document.getElementById('newPCode').value.trim();
      const job = document.getElementById('newPJob').value;
      const role = document.getElementById('newPRole').value;
      const departmentId = document.getElementById('newPDept').value;
      if(!name || !departmentId || !code) return;
      DB.people.push({ id: uid(), name, code, jobTitle: job, role, departmentId, password:'', disabled:false, createdAt:new Date().toISOString() });
      await savePeople(); render();
    };
    document.querySelectorAll('[data-remove-person]').forEach(b=> {
      b.onclick = async ()=>{
        DB.people = DB.people.filter(p=>p.id!==b.dataset.removePerson);
        await savePeople(); render();
      };
    });
    document.querySelectorAll('[data-set-code]').forEach(b=> {
      b.onclick = async ()=>{
        const p = personById(b.dataset.setCode);
        if(!p) return;
        const code = prompt('أدخل الكود الوظيفي الجديد:', p.code||'');
        if(code===null) return;
        p.code = code.trim();
        await savePeople(); render();
      };
    });
    document.querySelectorAll('[data-reset-pass-quick]').forEach(b=> {
      b.onclick = async ()=>{
        const p = personById(b.dataset.resetPassQuick);
        if(!p) return;
        const np = prompt(`أدخل كلمة المرور الجديدة للموظف (${p.name}):`, '');
        if(!np || np.trim().length < 4){ alert('يجب ألا تقل كلمة المرور عن 4 خانات'); return; }
        p.password = np.trim();
        await savePeople();
        alert('✅ تم تحديث كلمة المرور بنجاح');
        render();
      };
    });
    document.querySelectorAll('[data-change-role]').forEach(b=> {
      b.onclick = async ()=>{
        const p = personById(b.dataset.changeRole);
        if(!p) return;
        const choice = prompt(`تعديل صلاحية ${p.name}:\n1: موظف\n2: مدير قسم\n3: أدمن`, p.role==='admin'?'3':p.role==='manager'?'2':'1');
        const map = {'1':'employee', '2':'manager', '3':'admin'};
        if(map[choice]){ p.role = map[choice]; await savePeople(); render(); }
      };
    });
    document.querySelectorAll('[data-edit-person]').forEach(b=> {
      b.onclick = async ()=>{
        const p = personById(b.dataset.editPerson);
        if(!p) return;
        const name = prompt('تعديل اسم الموظف:', p.name);
        if(!name) return;
        p.name = name.trim();
        await savePeople(); render();
      };
    });
  }

  if(UI.activeTab==='jobs'){
    const addJ = document.getElementById('addJobBtn');
    if(addJ) addJ.onclick = async ()=>{
      const v = document.getElementById('newJobTitle').value.trim();
      if(!v) return;
      if(!DB.jobTitles.includes(v)) DB.jobTitles.push(v);
      await saveJobTitles(); render();
    };
    document.querySelectorAll('[data-remove-job]').forEach(b=> {
      b.onclick = async ()=>{
        DB.jobTitles.splice(+b.dataset.removeJob, 1);
        await saveJobTitles(); render();
      };
    });
  }

  if(UI.activeTab==='leaveBalances'){
    document.querySelectorAll('[data-edit-leave]').forEach(b=>{
      b.onclick = async ()=>{
        const p = personById(b.dataset.editLeave);
        if(!p) return;
        const av = prompt(`رصيد السنوية لـ (${p.name}):`, '21');
        const cv = prompt(`رصيد العارضة لـ (${p.name}):`, '7');
        if(av===null || cv===null) return;
        DB.leaveBalances[p.id] = { annual_leave:{ allocated:parseInt(av,10)||0, used:0 }, casual_leave:{ allocated:parseInt(cv,10)||0, used:0 } };
        await saveLeaveBalances(); render();
      };
    });
  }

  if(UI.activeTab==='requests'){
    document.querySelectorAll('[data-approve]').forEach(b=> {
      b.onclick = async ()=>{
        const r = DB.requests.find(x=>x.id===b.dataset.approve);
        if(!r) return;
        r.status = 'approved';
        await saveRequests(); render();
      };
    });
    document.querySelectorAll('[data-reject]').forEach(b=> {
      b.onclick = async ()=>{
        const r = DB.requests.find(x=>x.id===b.dataset.reject);
        if(!r) return;
        r.status = 'rejected';
        await saveRequests(); render();
      };
    });
  }

  if(UI.activeTab==='penalties'){
    const addPen = document.getElementById('addPenBtn');
    if(addPen) addPen.onclick = async ()=>{
      const empid = document.getElementById('penEmp').value;
      const type = document.getElementById('penType').value;
      const reason = document.getElementById('penReason').value.trim();
      if(!empid || !reason) return;
      const emp = personById(empid);
      DB.penalties.push({ id:uid(), empId:empid, empName:emp.name, departmentId:emp.departmentId, type, reason, appliedByName:'الأدمن', createdAt:new Date().toISOString() });
      await savePenalties(); render();
    };
  }

  if(UI.activeTab==='attendance'){
    const dp = document.getElementById('attDatePick');
    if(dp) dp.onchange = e=>{ UI.attDate = e.target.value; render(); };
  }

  if(UI.activeTab==='reports'){
    const rb = document.getElementById('downloadReportBtn');
    if(rb) rb.onclick = ()=>{
      const rows = DB.people.filter(p=>p.role==='employee').map(e=>({
        'الاسم': e.name, 'القسم': deptName(e.departmentId), 'الكود': e.code||'—'
      }));
      const wb = XLSX.utils.book_new();
      createAndSaveArabicExcel(wb, 'تقرير', rows, `تقرير-${UI.reportMonth||monthStr()}.xlsx`);
    };
  }

  if(UI.activeTab==='penaltyReport'){
    const pb = document.getElementById('downloadPenaltyReportBtn');
    if(pb) pb.onclick = ()=>{
      const rows = DB.penalties.map(p=>({
        'الموظف': p.empName, 'القسم': deptName(p.departmentId), 'نوع الجزاء': PENALTY_TYPES[p.type]?.label||p.type, 'السبب': p.reason, 'بواسطة': p.appliedByName
      }));
      const wb = XLSX.utils.book_new();
      createAndSaveArabicExcel(wb, 'تقرير الجزاءات', rows, `تقرير-الجزاءات.xlsx`);
    };
  }

  if(UI.activeTab==='leaveReport'){
    const lb = document.getElementById('downloadLeaveReportBtn');
    if(lb) lb.onclick = ()=>{
      const rows = DB.requests.filter(r=>r.status==='approved' && LEAVE_TYPES[r.type]).map(r=>({
        'الموظف': r.empName, 'القسم': deptName(r.departmentId), 'نوع الإجازة': LEAVE_TYPES[r.type]?.label, 'السبب': r.reason
      }));
      const wb = XLSX.utils.book_new();
      createAndSaveArabicExcel(wb, 'تقرير الإجازات', rows, `تقرير-الإجازات.xlsx`);
    };
  }

  if(UI.activeTab==='backup'){
    const eb = document.getElementById('exportJsonBtn');
    if(eb) eb.onclick = ()=> exportFullJsonBackup();
    const exb = document.getElementById('exportFullExcelBtn');
    if(exb) exb.onclick = ()=> exportFullExcelBackup();
    const triggerBtn = document.getElementById('triggerImportBtn');
    const fileInput = document.getElementById('importJsonFileInput');
    if(triggerBtn && fileInput){
      triggerBtn.onclick = ()=> fileInput.click();
      fileInput.onchange = (e)=>{
        const file = e.target.files[0];
        if(file) restoreFromJsonFile(file);
        fileInput.value = '';
      };
    }
  }
}

function renderManagerDashboard(){
  const mgr = UI.currentPerson;
  const pending = DB.requests.filter(r=>r.departmentId===mgr.departmentId && r.status==='pending' && r.empId!==mgr.id).length;
  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`مدير قسم ${esc(deptName(mgr.departmentId))} — ${esc(mgr.name)}`, 'mgr')}
    <div class="tabs">
      <button class="tab-btn ${UI.mgrTab==='people'?'active':''}" data-mtab="people">موظفي القسم</button>
      <button class="tab-btn ${UI.mgrTab==='requests'?'active':''}" data-mtab="requests">${pending>0?`<span class="tab-badge">${pending}</span>`:''}الطلبات</button>
      <button class="tab-btn ${UI.mgrTab==='penalties'?'active':''}" data-mtab="penalties">الجزاءات</button>
      <button class="tab-btn ${UI.mgrTab==='attendance'?'active':''}" data-mtab="attendance">الحضور</button>
      <button class="tab-btn ${UI.mgrTab==='reports'?'active':''}" data-mtab="reports">التقرير الشهري</button>
      <button class="tab-btn ${UI.mgrTab==='mySelf'?'active':''}" data-mtab="mySelf">👤 صفحتي وطلباتي</button>
    </div>
    <div class="content">
      ${UI.mgrTab==='people' ? renderPeopleTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='requests' ? renderRequestsTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='penalties' ? renderPenaltiesTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='attendance' ? renderAttendanceTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='reports' ? renderReportsTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='mySelf' ? renderEmployeeDashboardContent(mgr) : ''}
    </div>
  </div>`;
}

function attachManagerDashboardEvents(){
  attachTopbarSearchEvents();
  attachLogout();
  document.querySelectorAll('.tab-btn').forEach(b=> {
    b.onclick = ()=>{ UI.mgrTab = b.dataset.mtab; render(); };
  });

  if(UI.mgrTab==='requests'){
    document.querySelectorAll('[data-approve]').forEach(b=> {
      b.onclick = async ()=>{
        const r = DB.requests.find(x=>x.id===b.dataset.approve);
        if(!r) return;
        r.status = 'approved';
        await saveRequests(); render();
      };
    });
    document.querySelectorAll('[data-reject]').forEach(b=> {
      b.onclick = async ()=>{
        const r = DB.requests.find(x=>x.id===b.dataset.reject);
        if(!r) return;
        r.status = 'rejected';
        await saveRequests(); render();
      };
    });
  }

  if(UI.mgrTab==='penalties'){
    const addPen = document.getElementById('addPenBtn');
    if(addPen) addPen.onclick = async ()=>{
      const empid = document.getElementById('penEmp').value;
      const type = document.getElementById('penType').value;
      const reason = document.getElementById('penReason').value.trim();
      if(!empid || !reason){ alert('يرجى اختيار الموظف وكتابة السبب'); return; }
      const emp = personById(empid);
      DB.penalties.push({ id:uid(), empId:empid, empName:emp.name, departmentId:emp.departmentId, type, reason, appliedByName:UI.currentPerson.name, createdAt:new Date().toISOString() });
      await savePenalties(); render();
    };
  }

  if(UI.mgrTab==='attendance'){
    const dp = document.getElementById('attDatePick');
    if(dp) dp.onchange = e=>{ UI.attDate = e.target.value; render(); };
  }

  if(UI.mgrTab==='reports'){
    const rb = document.getElementById('downloadReportBtn');
    if(rb) rb.onclick = ()=>{
      const rows = DB.people.filter(p=>p.role==='employee' && p.departmentId===UI.currentPerson.departmentId).map(e=>({
        'الاسم': e.name, 'القسم': deptName(e.departmentId), 'الكود': e.code||'—'
      }));
      const wb = XLSX.utils.book_new();
      createAndSaveArabicExcel(wb, 'تقرير القسم', rows, `تقرير-${deptName(UI.currentPerson.departmentId)}.xlsx`);
    };
  }

  if(UI.mgrTab==='mySelf'){
    attachAttendanceAndReqActions(UI.currentPerson);
  }
}

function renderEmployeeDashboardContent(emp){
  const date = todayStr();
  const att = DB.attendance[emp.id+'_'+date] || {};
  const annualBal = getLeaveBalance(emp, 'annual_leave');
  const casualBal = getLeaveBalance(emp, 'casual_leave');
  const myRequests = DB.requests.filter(r=>r.empId===emp.id).slice().sort((a,b)=>b.createdAt.localeCompare(a.createdAt));

  const historyRows = myRequests.map(r=>`<div class="ledger-row"><div class="row-main"><div class="name">${REQUEST_TYPES[r.type]?.label||r.type}</div><div class="meta">${requestDateLabel(r)} - ${r.reason}</div></div><div class="row-actions"><span class="stamp ${r.status==='approved'?'stamp-approved':'stamp-pending'}">${r.status==='approved'?'معتمد':'قيد المراجعة'}</span></div></div>`).join('');

  return `
    <div class="dept-banner">
      <div class="item"><div class="label">القسم</div><div class="val">${esc(deptName(emp.departmentId))}</div></div>
      <div class="item"><div class="label">رصيد الإجازات</div><div class="val">سنوي: ${annualBal.isSet?annualBal.remaining:'لم يحدد'} | عارضة: ${casualBal.isSet?casualBal.remaining:'لم يحدد'}</div></div>
    </div>
    <div class="today-card">
      <div class="date">${fmtDate(date)}</div>
      <div class="times">
        <div class="time-block"><strong>${att.checkIn || '—'}</strong><span>دخول</span></div>
        <div class="time-block"><strong>${att.checkOut || '—'}</strong><span>انصراف</span></div>
      </div>
      ${!att.checkIn ? `<button class="btn btn-primary" id="checkInBtn">تسجيل الحضور الآن</button>` : !att.checkOut ? `<button class="btn btn-outline" id="checkOutBtn">تسجيل الانصراف</button>` : `<div class="info-msg">تم تسجيل حضورك وانصرافك اليوم بنجاح</div>`}
    </div>
    <div class="form-card">
      <h3>تقديم طلب إجازة / إذن</h3>
      <div class="form-row">
        <div class="field"><label>نوع الطلب</label><select id="reqType">${Object.entries(REQUEST_TYPES).map(([k,v])=>`<option value="${k}">${v.label}</option>`).join('')}</select></div>
        <div class="field"><label>التاريخ</label><input id="reqDate" type="date" value="${date}"></div>
      </div>
      <div class="field"><label>السبب</label><textarea id="reqReason" placeholder="اكتب سبب الطلب"></textarea></div>
      <button class="btn btn-primary btn-block" id="submitReqBtn">إرسال الطلب</button>
    </div>
    <div class="section-head"><h2>طلباتي السابقة</h2></div>
    <div class="ledger">${historyRows || '<div class="empty-illustration">لا توجد طلبات سابقة</div>'}</div>
  `;
}

function renderEmployeeDashboard(){
  const emp = UI.currentPerson;
  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`${emp.name} — ${emp.jobTitle||'موظف'}`, '')}
    <div class="content">${renderEmployeeDashboardContent(emp)}</div>
  </div>`;
}

function attachAttendanceAndReqActions(emp){
  const inBtn = document.getElementById('checkInBtn');
  if(inBtn) inBtn.onclick = async ()=>{
    const key = emp.id+'_'+todayStr();
    DB.attendance[key] = { ...(DB.attendance[key]||{}), checkIn: nowTime() };
    await saveAttendance(); render();
  };
  const outBtn = document.getElementById('checkOutBtn');
  if(outBtn) outBtn.onclick = async ()=>{
    const key = emp.id+'_'+todayStr();
    DB.attendance[key] = { ...(DB.attendance[key]||{}), checkOut: nowTime() };
    await saveAttendance(); render();
  };

  const submitBtn = document.getElementById('submitReqBtn');
  if(submitBtn) submitBtn.onclick = async ()=>{
    const type = document.getElementById('reqType').value;
    const rdate = document.getElementById('reqDate').value;
    const reason = document.getElementById('reqReason').value.trim();
    if(!rdate || !reason){ alert('يرجى كتابة التاريخ والسبب'); return; }
    DB.requests.push({ id: uid(), empId: emp.id, empName: emp.name, departmentId: emp.departmentId, type, date: rdate, startDate: rdate, endDate: rdate, durationDays: 1, reason, status:'pending', createdAt:new Date().toISOString() });
    await saveRequests(); render();
  };
}

function attachEmployeeDashboardEvents(){
  attachLogout();
  attachAttendanceAndReqActions(UI.currentPerson);
}

loadAll();
</script>
</body>
</html>
