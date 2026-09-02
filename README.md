[index.html](https://github.com/user-attachments/files/31738269/index.html)
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

  /* Topbar & Search */
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
  .topbar .who{font-size:12.5px; opacity:.92;}
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
  .backup-options{display:grid; grid-template-columns:repeat(auto-fit, minmax(260px, 1fr)); gap:16px; margin-top:14px;}
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
// Error Boundary to prevent any blank screen
window.onerror = function(msg, url, line){
  console.error("System Error caught:", msg, "line:", line);
  const app = document.getElementById('app');
  if(app && app.innerHTML.trim() === ""){
    app.innerHTML = '<div class="center-screen"><div class="panel"><h1>مرحباً بك</h1><p class="sub">جارِ تجهيز السجل، اضغط على الزر التالي للدخول:</p><button class="btn btn-primary btn-block" onclick="location.reload()">تحديث وبدء التشغيل</button></div></div>';
  }
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
}catch(e){ console.warn('Firebase init notice', e); }

const COLLECTION = 'attendance_app';
async function storageGet(key){
  if(!db) return null;
  try{ const doc = await db.collection(COLLECTION).doc(key).get(); return doc.exists ? doc.data().value : null; }
  catch(e){ console.warn('get failed', key); return null; }
}
async function storageSet(key, value){
  if(!db) return;
  try{ await db.collection(COLLECTION).doc(key).set({ value }); }
  catch(e){ console.warn('set failed', key); }
}

const DEFAULT_JOB_TITLES = ['موظف', 'محاسب', 'سائق', 'إداري', 'خدمة عملاء', 'مندوب توصيل'];
const LEAVE_TYPES = { annual_leave:{label:'إجازة سنوية'}, casual_leave:{label:'إجازة عارضة'} };
const REQUEST_TYPES = {
  late:               { label:'إذن حضور متأخر', showFrom:false, showTo:false },
  early_leave:        { label:'إذن انصراف مبكر', showFrom:false, showTo:false },
  annual_leave:       { label:'إجازة سنوية', showFrom:false, showTo:false },
  casual_leave:       { label:'إجازة عارضة', showFrom:false, showTo:false },
  deduction_leave:    { label:'إجازة خصم بيوم', showFrom:false, showTo:false },
  rest_day_work:      { label:'عمل يوم راحة', showFrom:false, showTo:false },
  assignment:         { label:'تكليف', showFrom:false, showTo:false },
  no_fingerprint_in:  { label:'حضور بدون بصمة', showFrom:true, showTo:false },
  no_fingerprint_out: { label:'انصراف بدون بصمة', showFrom:false, showTo:true }
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
  view:'loginUnified', adminAuthed:false, currentPerson:null, setupTargetPerson:null, lastSubmittedReq:null,
  selectedPersonProfileId:null, topbarSearchQuery:'',
  activeTab:'departments', mgrTab:'people', error:'',
  reqFilter:'pending', penDeptFilter:'all', attDate:null, attDeptFilter:'all',
  reportMonth:null, reportDept:'all', penaltyReportMonth:null, penaltyReportDept:'all', leaveDept:'all', leaveReportMonth:null, auditFilter:'all'
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
function waLink(phone, text){
  if(!phone) return null;
  const clean = phone.replace(/[^\d]/g,'');
  return "https://wa.me/" + clean + "?text=" + encodeURIComponent(text);
}
function notifyPhoneForDept(deptId){
  const mgr = managerOf(deptId);
  if(mgr && mgr.phone) return { phone: mgr.phone, title: "مدير القسم (" + mgr.name + ")" };
  if(DB.adminConfig && DB.adminConfig.phone) return { phone: DB.adminConfig.phone, title: "الأدمن (" + DB.adminConfig.name + ")" };
  return null;
}

function generateWhatsAppMessage(r, emp){
  const info = REQUEST_TYPES[r.type] || { label: r.type };
  const dName = deptName(emp.departmentId);
  let timeDetails = '';
  if(r.type === 'no_fingerprint_in' && r.timeFrom) timeDetails = "\n⏰ وقت الحضور: " + r.timeFrom;
  else if(r.type === 'no_fingerprint_out' && r.timeTo) timeDetails = "\n⏰ وقت الانصراف: " + r.timeTo;
  else if(r.timeFrom && r.timeTo) timeDetails = "\n⏰ التوقيت: من " + r.timeFrom + " إلى " + r.timeTo;

  let dateDetails = "📅 التاريخ: " + fmtDate(r.startDate || r.date);
  if(LEAVE_TYPES[r.type] && r.durationDays > 1){
    dateDetails = "📅 الفترة: من " + fmtDate(r.startDate || r.date) + " إلى " + fmtDate(r.endDate || r.date) + " (" + r.durationDays + " أيام)";
  }

  return "السلام عليكم ورحمة الله وبركاته،\n\nطلب اعتماد جديد مرسل من الموظف:\n👤 الاسم: " + emp.name + (emp.code ? " (كود: " + emp.code + ")" : "") + "\n🏢 القسم: " + dName + "\n📋 نوع الطلب: " + info.label + "\n" + dateDetails + timeDetails + "\n📝 السبب: " + r.reason + "\n\n🔗 يرجى مراجعة الطلب واتخاذ القرار (موافقة / رفض) عبر برنامج الحضور والانصراف.";
}

function saveSession(data){
  try{ localStorage.setItem('attendance_active_session', JSON.stringify(data)); }catch(e){}
}
function clearSession(){
  try{ localStorage.removeItem('attendance_active_session'); }catch(e){}
}
function restoreSession(){
  try{
    const raw = localStorage.getItem('attendance_active_session');
    if(!raw) return false;
    const sess = JSON.parse(raw);
    if(sess.role === 'admin' && DB.adminConfig){
      UI.adminAuthed = true;
      UI.currentPerson = null;
      UI.view = 'adminDashboard';
      UI.activeTab = 'departments';
      return true;
    } else if(sess.role === 'person' && sess.id){
      const p = personById(sess.id);
      if(p && p.password){
        if(p.disabled){ clearSession(); return false; }
        UI.currentPerson = p;
        if(p.role === 'admin'){
          UI.adminAuthed = false;
          UI.view = 'adminDashboard';
          UI.activeTab = 'departments';
        } else if(p.role === 'manager'){
          UI.view = 'managerDashboard';
          UI.mgrTab = 'people';
        } else {
          UI.view = 'employeeDashboard';
        }
        return true;
      }
    }
  }catch(e){ console.warn('Session restore error', e); }
  return false;
}

function loadLocalCache(){
  try{
    const cCfg = localStorage.getItem('attendance_admin_config');
    if(cCfg) DB.adminConfig = JSON.parse(cCfg);
    const cDeps = localStorage.getItem('attendance_cache_departments');
    if(cDeps) DB.departments = JSON.parse(cDeps);
    const cPeople = localStorage.getItem('attendance_cache_people');
    if(cPeople) DB.people = JSON.parse(cPeople);
    const cReqs = localStorage.getItem('attendance_cache_requests');
    if(cReqs) DB.requests = JSON.parse(cReqs);
    const cPens = localStorage.getItem('attendance_cache_penalties');
    if(cPens) DB.penalties = JSON.parse(cPens);
    const cAtt = localStorage.getItem('attendance_cache_attendance');
    if(cAtt) DB.attendance = JSON.parse(cAtt);
    const cJobs = localStorage.getItem('attendance_cache_jobs');
    if(cJobs) DB.jobTitles = JSON.parse(cJobs);
    const cLeaves = localStorage.getItem('attendance_cache_leaves');
    if(cLeaves) DB.leaveBalances = JSON.parse(cLeaves);
    const cLogs = localStorage.getItem('attendance_cache_logs');
    if(cLogs) DB.auditLogs = JSON.parse(cLogs);
  }catch(e){ console.warn('Cache error', e); }
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

function isTyping(){
  const el = document.activeElement;
  return el && (el.tagName==='INPUT' || el.tagName==='TEXTAREA' || el.tagName==='SELECT');
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
      if(!isTyping()) render();
    }, err => console.warn('Live sync notice', err));
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

  const safeGet = (key, fallback) => Promise.race([
    storageGet(key),
    new Promise(resolve => setTimeout(() => resolve(fallback), 2500))
  ]).catch(() => fallback);

  try {
    const [cfg, deps, people, reqs, pens, att, jobs, leaves, logs] = await Promise.all([
      safeGet('admin_config', DB.adminConfig),
      safeGet('departments', DB.departments),
      safeGet('people', DB.people),
      safeGet('requests', DB.requests),
      safeGet('penalties', DB.penalties),
      safeGet('attendance', DB.attendance),
      safeGet('job_titles', DB.jobTitles && DB.jobTitles.length ? DB.jobTitles : DEFAULT_JOB_TITLES.slice()),
      safeGet('leave_balances', DB.leaveBalances),
      safeGet('audit_logs', DB.auditLogs)
    ]);

    if(cfg) DB.adminConfig = cfg;
    DB.departments = Array.isArray(deps) ? deps : DB.departments;
    DB.people = Array.isArray(people) ? people : DB.people;
    DB.requests = Array.isArray(reqs) ? reqs : DB.requests;
    DB.penalties = Array.isArray(pens) ? pens : DB.penalties;
    DB.attendance = att && typeof att === 'object' ? att : DB.attendance;
    DB.jobTitles = Array.isArray(jobs) && jobs.length ? jobs : (DB.jobTitles.length ? DB.jobTitles : DEFAULT_JOB_TITLES.slice());
    DB.leaveBalances = leaves && typeof leaves === 'object' ? leaves : DB.leaveBalances;
    DB.auditLogs = Array.isArray(logs) ? logs : DB.auditLogs;

    saveLocalCache();

    if(!restoreSession()) {
      UI.view = DB.adminConfig ? 'loginUnified' : 'adminSetup';
    }
    render();
    watchLiveUpdates();
  } catch(e) {
    console.warn('Sync notice', e);
  }
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
  if(typeof XLSX === 'undefined'){
    alert('مكتبة الإكسل غير جاهزة حالياً، يرجى التأكد من اتصال الإنترنت لتنزيل ملفات Excel.');
    return;
  }
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
  if(typeof XLSX === 'undefined'){
    alert('مكتبة الإكسل غير جاهزة، تأكد من الاتصال بالإنترنت.');
    return;
  }
  const reader = new FileReader();
  reader.onload = async (e) => {
    try {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, { type: 'array' });
      const firstSheetName = workbook.SheetNames[0];
      const worksheet = workbook.Sheets[firstSheetName];
      const rows = XLSX.utils.sheet_to_json(worksheet);

      if(!rows || rows.length === 0){
        alert('الملف فارغ أو لا يحتوي على صفوف بيانات صالحة.');
        return;
      }

      let addedCount = 0, skippedCount = 0, errors = [];

      for(let i = 0; i < rows.length; i++){
        const row = rows[i];
        const name = (row['الاسم'] || row['اسم الموظف'] || row['Name'] || '') + '';
        const code = (row['الكود'] || row['الكود الوظيفي'] || row['Code'] || '') + '';
        const job = (row['الوظيفة'] || row['المسمى الوظيفي'] || row['Job'] || 'موظف') + '';
        const dept = (row['القسم'] || row['اسم القسم'] || row['Department'] || 'عام') + '';
        const roleStr = (row['نوع الصلاحية'] || row['الصلاحية'] || row['Role'] || 'موظف') + '';
        const phone = (row['رقم الواتساب'] || row['الواتساب'] || row['Phone'] || '') + '';
        const annualBal = parseInt(row['رصيد سنوية'] || row['سنوية'] || 0, 10) || 0;
        const casualBal = parseInt(row['رصيد عارضة'] || row['عارضة'] || 0, 10) || 0;

        const cleanName = name.trim(), cleanCode = code.trim(), cleanDept = dept.trim(), cleanJob = job.trim();

        if(!cleanName){ skippedCount++; continue; }

        if(cleanCode && DB.people.some(p => p.code && p.code.toLowerCase() === cleanCode.toLowerCase())){
          errors.push(`تم تخطي الموظف (${cleanName}) لأن الكود (${cleanCode}) مستخدم مسبقاً.`);
          skippedCount++;
          continue;
        }

        let department = DB.departments.find(d => d.name.trim().toLowerCase() === cleanDept.toLowerCase());
        if(!department && cleanDept){
          department = { id: uid(), name: cleanDept };
          DB.departments.push(department);
        }
        const departmentId = department ? department.id : '';

        if(cleanJob && !DB.jobTitles.includes(cleanJob)){
          DB.jobTitles.push(cleanJob);
        }

        let role = 'employee';
        if(roleStr.includes('أدمن') || roleStr.includes('ادمن') || roleStr.toLowerCase().includes('admin')){
          role = 'admin';
        } else if(roleStr.includes('مدير') || roleStr.toLowerCase().includes('manager')){
          role = 'manager';
        }

        const newPersonId = uid();
        const newPerson = {
          id: newPersonId,
          name: cleanName,
          jobTitle: cleanJob || 'موظف',
          role: role,
          departmentId: departmentId,
          password: '',
          phone: phone.trim(),
          code: cleanCode,
          disabled: false,
          createdAt: new Date().toISOString()
        };

        DB.people.push(newPerson);

        if(annualBal > 0 || casualBal > 0){
          DB.leaveBalances[newPersonId] = {
            annual_leave: { allocated: annualBal, used: 0 },
            casual_leave: { allocated: casualBal, used: 0 }
          };
        }

        addedCount++;
      }

      await Promise.all([
        saveDepartments(),
        savePeople(),
        saveJobTitles(),
        saveLeaveBalances()
      ]);

      await audit('استيراد موظفين من إكسل', `تم استيراد (${addedCount}) موظف بنجاح من ملف الإكسل`);

      let msg = `✅ تم استيراد ${addedCount} موظف بنجاح!`;
      if(skippedCount > 0) msg += `\n⚠️ تم تخطي ${skippedCount} صف.`;
      if(errors.length > 0) msg += `\n\nالتفاصيل:\n` + errors.slice(0, 5).join('\n');
      alert(msg);

      render();
    } catch(err) {
      console.error(err);
      alert('حدث خطأ أثناء قراءة ملف الإكسل: ' + err.message);
    }
  };
  reader.readAsArrayBuffer(file);
}

function exportFullJsonBackup(){
  const fullBackupData = {
    exportDate: new Date().toISOString(),
    appName: "سجل الحضور والانصراف",
    version: "2.0",
    data: {
      adminConfig: DB.adminConfig,
      departments: DB.departments,
      people: DB.people,
      requests: DB.requests,
      penalties: DB.penalties,
      attendance: DB.attendance,
      jobTitles: DB.jobTitles,
      leaveBalances: DB.leaveBalances,
      auditLogs: DB.auditLogs
    }
  };

  const blob = new Blob([JSON.stringify(fullBackupData, null, 2)], { type: "application/json;charset=utf-8" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `نسخة_احتياطية_كاملة_${todayStr()}.json`;
  a.click();
  URL.revokeObjectURL(url);
  audit('تصدير نسخة احتياطية', 'قام الأدمن بتنزيل نسخة احتياطية كاملة من قاعدة البيانات (JSON)');
}

async function restoreFromJsonFile(file){
  if(!file) return;
  const reader = new FileReader();
  reader.onload = async (e)=>{
    try{
      const parsed = JSON.parse(e.target.result);
      if(!parsed || !parsed.data){
        alert('الملف غير صالح، يرجى اختيار ملف نسخة احتياطية صحيح بصيغة .json');
        return;
      }

      if(!confirm('⚠️ تحذير هـام: استرجاع النسخة الاحتياطية سيقوم باستبدال كافة البيانات الحالية بالبيانات الموجودة داخل الملف. هل أنت متأكد من المتابعة؟')) return;

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
        saveDepartments(),
        savePeople(),
        saveRequests(),
        savePenalties(),
        saveAttendance(),
        saveJobTitles(),
        saveLeaveBalances(),
        saveAdminConfig(),
        saveAuditLogs()
      ]);

      await audit('استرجاع نسخة احتياطية', `تم استرجاع قاعدة البيانات بنجاح من ملف بتاريخ: ${parsed.exportDate || '—'}`);
      alert('✅ تم استرجاع كافة البيانات بنجاح ومزامنتها سحابياً!');
      render();
    } catch(err){
      console.error(err);
      alert('حدث خطأ أثناء قراءة ملف النسخة الاحتياطية: ' + err.message);
    }
  };
  reader.readAsText(file);
}

function exportFullExcelBackup(){
  if(typeof XLSX === 'undefined'){
    alert('مكتبة الإكسل غير متصلة.');
    return;
  }
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
    'حالة الحساب': p.disabled ? 'معطل' : 'نشط', 'رقم الواتساب': p.phone || '—', 'تاريخ التسجيل': p.createdAt ? p.createdAt.slice(0,10) : '—'
  })));

  addArabicSheet('الأقسام', DB.departments.map(d => ({
    'اسم القسم': d.name, 'مدير القسم': managerOf(d.id)?.name || 'بدون مدير',
    'عدد الموظفين': DB.people.filter(p => p.departmentId === d.id && p.role === 'employee').length
  })));

  const attendanceRows = [];
  Object.keys(DB.attendance).forEach(key => {
    const splitIdx = key.indexOf('_');
    if(splitIdx > 0){
      const empId = key.slice(0, splitIdx);
      const date = key.slice(splitIdx + 1);
      const emp = personById(empId);
      const record = DB.attendance[key];
      if(emp && (record.checkIn || record.checkOut)){
        attendanceRows.push({
          'التاريخ': date, 'اليوم': fmtDate(date), 'الموظف': emp.name, 'الكود': emp.code || '—',
          'القسم': deptName(emp.departmentId), 'وقت الحضور': record.checkIn || '—', 'وقت الانصراف': record.checkOut || '—'
        });
      }
    }
  });
  attendanceRows.sort((a,b) => b['التاريخ'].localeCompare(a['التاريخ']));
  addArabicSheet('سجل الحضور', attendanceRows);

  addArabicSheet('الطلبات والإجازات', DB.requests.map(r => ({
    'الموظف': r.empName, 'القسم': deptName(r.departmentId), 'نوع الطلب': REQUEST_TYPES[r.type]?.label || r.type,
    'عدد الأيام': r.durationDays || 1, 'من تاريخ': r.startDate || r.date, 'إلى تاريخ': r.endDate || r.date,
    'تاريخ يوم الإجازة المفصل': requestDateLabel(r), 'الحالة': r.status === 'approved' ? 'معتمد' : r.status === 'rejected' ? 'مرفوض' : 'قيد المراجعة',
    'السبب': r.reason, 'وقت الحضور': r.timeFrom || '—', 'وقت الانصراف': r.timeTo || '—'
  })).sort((a,b) => b['من تاريخ'].localeCompare(a['من تاريخ'])));

  addArabicSheet('الجزاءات', DB.penalties.map(pn => ({
    'التاريخ': pn.createdAt.slice(0,10), 'اليوم': fmtDate(pn.createdAt.slice(0,10)), 'الموظف': pn.empName,
    'القسم': deptName(pn.departmentId), 'نوع الجزاء': PENALTY_TYPES[pn.type]?.label || pn.type,
    'قيمة الخصم (أيام)': PENALTY_TYPES[pn.type]?.weight || 0, 'السبب': pn.reason, 'بواسطة': pn.appliedByName || '—'
  })).sort((a,b) => b['التاريخ'].localeCompare(a['التاريخ'])));

  addArabicSheet('أرصدة الإجازات', DB.people.filter(p => p.role === 'employee').map(p => {
    const a = getLeaveBalance(p, 'annual_leave');
    const c = getLeaveBalance(p, 'casual_leave');
    return {
      'الموظف': p.name, 'الكود': p.code || '—', 'القسم': deptName(p.departmentId),
      'سنوية (المخصص)': a.isSet ? a.allocated : 'غير محدد', 'سنوية (المستخدم)': a.used, 'سنوية (المتبقي)': a.isSet ? a.remaining : '—',
      'عارضة (المخصص)': c.isSet ? c.allocated : 'غير محدد', 'عارضة (المستخدم)': c.used, 'عارضة (المتبقي)': c.isSet ? c.remaining : '—'
    };
  }));

  XLSX.writeFile(wb, `نسخة_شاملة_للنظام_${todayStr()}.xlsx`);
  audit('تصدير نسخة إكسل شاملة', 'قام الأدمن بتنزيل مصنف إكسل يحتوي على كل بيانات النظام');
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

function getDeviceId(){
  let id = localStorage.getItem('attendance_device_id');
  if(!id){ id = uid()+'-'+uid(); localStorage.setItem('attendance_device_id', id); }
  return id;
}
async function claimDevice(person){
  if(person.role === 'admin') return true;
  const deviceId = getDeviceId();
  if(person.activeDeviceId && person.activeDeviceId !== deviceId) return false;
  if(!person.activeDeviceId){ person.activeDeviceId=deviceId; person.deviceLinkedAt=new Date().toISOString(); await savePeople(); }
  return true;
}
function resetEmployeeDevice(id){ const p=personById(id); if(p){ delete p.activeDeviceId; delete p.deviceLinkedAt; } }
function requestDateLabel(r){
  const start=r.startDate||r.date, end=r.endDate||r.date;
  return (!end || start===end) ? fmtDate(start) : (fmtDate(start) + " إلى " + fmtDate(end));
}
function calcDaysInclusive(a,b){
  if(!a||!b) return 1;
  const x=new Date(a+'T00:00:00'), y=new Date(b+'T00:00:00');
  return Math.max(1, Math.floor((y-x)/86400000)+1);
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
    <div class="field"><label>اسم الأدمن الرئيسي</label><input id="setupName" type="text" placeholder="مثال: محمد أحمد" autocomplete="off"></div>
    <div class="field"><label>رقم واتساب (اختياري، للإشعارات)</label><input id="setupPhone" type="text" placeholder="مثال: 201001234567" autocomplete="off"></div>
    <div class="field"><label>كلمة المرور</label><input id="setupPass" type="password" placeholder="6 أحرف على الأقل" autocomplete="new-password"></div>
    <div class="field"><label>تأكيد كلمة المرور</label><input id="setupPass2" type="password" autocomplete="new-password"></div>
    <button class="btn btn-primary btn-block" id="setupSubmit">إنشاء الحساب والبدء</button>
  </div></div>`;
}
function attachAdminSetupEvents(){
  document.getElementById('setupSubmit').onclick = async ()=>{
    const name = document.getElementById('setupName').value.trim();
    const phone = document.getElementById('setupPhone').value.trim();
    const pass = document.getElementById('setupPass').value;
    const pass2 = document.getElementById('setupPass2').value;
    if(!name){ UI.error='من فضلك اكتب اسمك'; render(); return; }
    if(pass.length<6){ UI.error='كلمة المرور يجب ألا تقل عن 6 أحرف'; render(); return; }
    if(pass!==pass2){ UI.error='كلمتا المرور غير متطابقتين'; render(); return; }
    const newAdmin = { name, phone, password: pass, createdAt:new Date().toISOString() };
    DB.adminConfig = newAdmin;
    await saveAdminConfig();
    saveSession({ role:'admin' });
    UI.adminAuthed=true; UI.error=''; UI.view='adminDashboard'; UI.activeTab='departments'; render();
  };
}

function renderLoginUnified(){
  return `<div class="center-screen"><div class="panel">
    <div class="brandmark">${logoSvg(false)}<span class="brand-title" style="font-size:18px;">سجل الحضور والانصراف</span></div>
    <p class="sub">تسجيل الدخول للنظام (أدمن / مدير / موظف)</p>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <div class="field">
      <label>الاسم أو الكود الوظيفي</label>
      <input id="uInput" type="text" placeholder="اكتب اسمك أو الكود الخاص بك" autocomplete="off" autofocus>
    </div>
    <div class="field" id="uPassWrap">
      <label>كلمة المرور</label>
      <input id="uPass" type="password" placeholder="كلمة المرور (اتركها فارغة إذا كان دخولك لأول مرة)" autocomplete="current-password">
    </div>
    <button class="btn btn-primary btn-block" id="uSubmit">دخول</button>
    <div style="margin-top:14px; text-align:center;">
      <button class="link-btn" id="firstTimeLoginBtn" style="color:var(--primary); font-size:12px; border-color:var(--border);">دخول لأول مرة؟ اضغط هنا لإنشاء كلمة المرور</button>
    </div>
  </div></div>`;
}

function attachLoginUnifiedEvents(){
  const btn = document.getElementById('uSubmit');
  const firstTimeBtn = document.getElementById('firstTimeLoginBtn');

  if(firstTimeBtn){
    firstTimeBtn.onclick = ()=>{
      const inputVal = document.getElementById('uInput').value.trim();
      if(!inputVal){ UI.error = 'اكتب اسمك أو الكود الوظيفي أولاً ثم اضغط على زر إنشاء كلمة المرور'; render(); return; }
      const p = findPersonByLogin(inputVal);
      if(!p){ UI.error = 'لم يتم العثور على موظف بهذا الاسم أو الكود. تواصل مع الأدمن لإضافتك.'; render(); return; }
      if(p.disabled){ UI.error = 'تم تعطيل هذا الحساب من قبل الإدارة. يرجى مراجعة المسؤول.'; render(); return; }
      if(p.password){ UI.error = 'هذا الحساب لديه كلمة مرور بالفعل! أدخل كلمة المرور للدخول.'; render(); return; }
      UI.setupTargetPerson = p; UI.error = ''; UI.view = 'empSetPassword'; render();
    };
  }

  if(!btn) return;
  const submit = async ()=>{
    const inputVal = document.getElementById('uInput').value.trim();
    const pass = document.getElementById('uPass').value;
    if(!inputVal){ UI.error='من فضلك أدخل الاسم أو الكود الوظيفي'; render(); return; }

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
      if(p.disabled){ UI.error = 'تم تعطيل هذا الحساب من قبل الإدارة. يرجى مراجعة المسؤول.'; render(); return; }
      if(!p.password){ UI.setupTargetPerson = p; UI.error = ''; UI.view = 'empSetPassword'; render(); return; }

      if(p.password === pass){
        if(!(await claimDevice(p))){ UI.error='هذا الحساب مرتبط بجهاز آخر. تواصل مع الإدارة لفتح جهاز جديد.'; render(); return; }
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
    <p class="sub">مرحباً بك يا <strong>${esc(p?.name||'')}</strong>، قم بإنشاء كلمة المرور الخاصة بك</p>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <div class="field">
      <label>كلمة المرور الجديدة</label>
      <input id="newEmpPass" type="password" placeholder="اكتب كلمة مرور قوية (4 أحرف/أرقام على الأقل)" autofocus autocomplete="new-password">
    </div>
    <div class="field">
      <label>تأكيد كلمة المرور</label>
      <input id="newEmpPass2" type="password" placeholder="أعد كتابة كلمة المرور" autocomplete="new-password">
    </div>
    <button class="btn btn-primary btn-block" id="empPassSubmit">حفظ والدخول للبرنامج</button>
    <div style="margin-top:12px; text-align:center;">
      <button class="link-btn" id="cancelSetPassBtn" style="color:var(--ink-soft); font-size:12px;">رجوع لتسجيل الدخول</button>
    </div>
  </div></div>`;
}

function attachEmpSetPasswordEvents(){
  const cancelBtn = document.getElementById('cancelSetPassBtn');
  if(cancelBtn){
    cancelBtn.onclick = ()=>{ UI.setupTargetPerson = null; UI.error = ''; UI.view = 'loginUnified'; render(); };
  }

  const submitBtn = document.getElementById('empPassSubmit');
  if(!submitBtn) return;
  submitBtn.onclick = async ()=>{
    const pass = document.getElementById('newEmpPass').value;
    const pass2 = document.getElementById('newEmpPass2').value;
    const p = UI.setupTargetPerson;
    if(!p){ UI.view = 'loginUnified'; render(); return; }

    if(!pass || pass.length < 4){ UI.error = 'كلمة المرور يجب ألا تقل عن 4 أحرف أو أرقام'; render(); return; }
    if(pass !== pass2){ UI.error = 'كلمتا المرور غير متطابقتين'; render(); return; }

    p.password = pass;
    p.passwordSetAt = new Date().toISOString();
    await savePeople();
    await audit('إنشاء كلمة مرور', `قام ${p.name} بإنشاء كلمة المرور الخاصة به`);

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
  UI.topbarSearchQuery = '';
  render();
}

function renderPersonProfile(){
  const p = personById(UI.selectedPersonProfileId);
  if(!p){
    return `<div class="center-screen"><div class="panel"><h1>لم يتم العثور على الموظف</h1><button class="btn btn-primary btn-block" id="backFromProfile">رجوع</button></div></div>`;
  }

  const deptMgr = p.departmentId ? managerOf(p.departmentId) : null;
  const annualBal = getLeaveBalance(p, 'annual_leave');
  const casualBal = getLeaveBalance(p, 'casual_leave');

  const empReqs = DB.requests.filter(r => r.empId === p.id).sort((a,b)=> b.createdAt.localeCompare(a.createdAt));
  const empPens = DB.penalties.filter(pn => pn.empId === p.id).sort((a,b)=> b.createdAt.localeCompare(a.createdAt));
  const penaltyDaysTotal = empPens.reduce((sum, pn) => sum + (PENALTY_TYPES[pn.type]?.weight || 0), 0);

  const attEntries = Object.keys(DB.attendance).filter(k => k.startsWith(p.id + '_')).map(k => DB.attendance[k]);
  const presentCount = attEntries.filter(a => a.checkIn).length;

  const reqRows = empReqs.map(r => {
    const stampClass = r.status==='approved'?'stamp-approved':r.status==='rejected'?'stamp-rejected':'stamp-pending';
    const stampText = r.status==='approved'?'معتمد':r.status==='rejected'?'مرفوض':'قيد المراجعة';
    const info = REQUEST_TYPES[r.type] || { label: r.type };
    return `<div class="ledger-row">
      <div class="row-main"><div class="name">${esc(info.label)} — ${requestDateLabel(r)}</div><div class="meta">${esc(r.reason)}</div></div>
      <div class="row-actions"><span class="stamp ${stampClass}">${stampText}</span></div>
    </div>`;
  }).join('');

  const penRows = empPens.map(pn => {
    const info = PENALTY_TYPES[pn.type] || { label: pn.type };
    return `<div class="ledger-row">
      <div class="row-main"><div class="name">${esc(info.label)} (خصم ${info.weight || 0} يوم) — ${fmtDate(pn.createdAt.slice(0,10))}</div><div class="meta">السبب: ${esc(pn.reason)} · بواسطة: ${esc(pn.appliedByName)}</div></div>
      <div class="row-actions">
        <span class="stamp stamp-penalty">${esc(info.label)}</span>
        ${(UI.adminAuthed || UI.currentPerson?.role==='admin') ? `<button class="btn btn-danger btn-sm" data-delete-penalty="${pn.id}">حذف</button>` : ''}
      </div>
    </div>`;
  }).join('');

  const isAdmin = UI.adminAuthed || (UI.currentPerson && UI.currentPerson.role==='admin');
  const isAdminOrMgr = isAdmin || (UI.currentPerson && UI.currentPerson.role==='manager');

  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`الملف الشخصي للموظف — ${esc(p.name)}`, 'admin')}
    <div class="content">
      <div style="margin-bottom:14px; display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:8px;">
        <button class="btn btn-outline btn-sm" id="backFromProfile">← رجوع للوحة التحكم</button>
        <div style="display:flex; gap:8px;">
          ${isAdmin ? `<button class="btn ${p.disabled?'btn-success':'btn-warning'} btn-sm" data-toggle-status="${p.id}">${p.disabled?'✅ إعادة تشغيل الحساب':'⛔ تعطيل الحساب'}</button>` : ''}
          ${isAdminOrMgr ? `<button class="btn btn-outline btn-sm" data-edit-leave="${p.id}">تعديل رصيد الإجازات</button>` : ''}
        </div>
      </div>

      <div class="profile-header">
        <div style="display:flex; justify-content:space-between; align-items:flex-start; flex-wrap:wrap; gap:12px;">
          <div>
            <h1 style="font-size:22px; color:var(--primary); margin:0 0 6px;">
              ${esc(p.name)} 
              ${p.disabled ? '<span class="chip chip-disabled">الحساب معطل حالياً</span>' : '<span class="chip" style="color:var(--success); border-color:var(--success);">حساب نشط</span>'}
            </h1>
            <div style="color:var(--ink-soft); font-size:13.5px; display:flex; gap:10px; flex-wrap:wrap; align-items:center;">
              <span>الوظيفة: <strong>${esc(p.jobTitle || 'غير محدد')}</strong></span>
              <span>·</span>
              <span>القسم: <strong class="chip chip-dept">${esc(deptName(p.departmentId))}</strong></span>
              <span>·</span>
              <span>الكود: <strong class="chip chip-code">${esc(p.code || 'بدون كود')}</strong></span>
            </div>
          </div>
          <div style="text-align:left;">
            <div style="font-size:12px; color:var(--ink-soft);">المدير المباشر</div>
            <div style="font-size:14.5px; font-weight:700; color:var(--manager);">${esc(deptMgr ? deptMgr.name : 'لا يوجد')}</div>
          </div>
        </div>
      </div>

      <div class="profile-stats-grid">
        <div class="stat-card"><div class="num" style="color:var(--success);">${annualBal.isSet ? `${annualBal.remaining}/${annualBal.allocated}` : '—'}</div><div class="lbl">رصيد الإجازة السنوية المتبقي</div></div>
        <div class="stat-card"><div class="num" style="color:var(--manager);">${casualBal.isSet ? `${casualBal.remaining}/${casualBal.allocated}` : '—'}</div><div class="lbl">رصيد الإجازة العارضة المتبقي</div></div>
        <div class="stat-card"><div class="num" style="color:var(--danger);">${penaltyDaysTotal}</div><div class="lbl">إجمالي أيام الخصم والجزاءات</div></div>
        <div class="stat-card"><div class="num">${presentCount}</div><div class="lbl">إجمالي أيام الحضور المسجلة</div></div>
      </div>

      <div class="section-head"><h2>سجل الجزاءات والخصومات</h2><span class="count">${empPens.length} جزاء</span></div>
      <div class="ledger" style="margin-bottom:24px;">${penRows || '<div class="empty-illustration">لا توجد جزاءات مسجلة على هذا الموظف</div>'}</div>

      <div class="section-head"><h2>سجل الطلبات والإجازات والأذونات</h2><span class="count">${empReqs.length} طلب</span></div>
      <div class="ledger">${reqRows || '<div class="empty-illustration">لا توجد طلبات مسجلة لهذا الموظف</div>'}</div>
    </div>
  </div>`;
}

function attachPersonProfileEvents(){
  attachTopbarSearchEvents();
  attachLogout();

  const backBtn = document.getElementById('backFromProfile');
  if(backBtn){
    backBtn.onclick = ()=>{
      if(UI.adminAuthed || UI.currentPerson?.role === 'admin'){ UI.view = 'adminDashboard'; }
      else if(UI.currentPerson?.role === 'manager'){ UI.view = 'managerDashboard'; }
      else { UI.view = 'employeeDashboard'; }
      render();
    };
  }

  document.querySelectorAll('[data-toggle-status]').forEach(b=>{
    b.onclick = async ()=>{
      const p = personById(b.dataset.toggleStatus);
      if(!p) return;
      const willDisable = !p.disabled;
      const actionText = willDisable ? 'تعطيل حساب' : 'إعادة تشغيل حساب';
      if(!confirm(`هل أنت متأكد من ${actionText} الموظف (${p.name})؟`)) return;
      p.disabled = willDisable;
      await savePeople();
      await audit(`${actionText}`, `قام الأدمن بـ ${actionText} للموظف ${p.name}`);
      render();
    };
  });

  document.querySelectorAll('[data-delete-penalty]').forEach(b=>{
    b.onclick = async ()=>{
      const penId = b.dataset.deletePenalty;
      const pen = DB.penalties.find(p=>p.id===penId);
      if(!pen || !confirm(`هل أنت متأكد من حذف هذا الجزاء؟`)) return;
      DB.penalties = DB.penalties.filter(p=>p.id!==penId);
      await savePenalties();
      await audit('حذف جزاء', `قام المسؤول بحذف جزاء مسجل على ${pen.empName}`);
      render();
    };
  });

  const editLeaveBtn = document.querySelector('[data-edit-leave]');
  if(editLeaveBtn){
    editLeaveBtn.onclick = async ()=>{
      const p = personById(UI.selectedPersonProfileId);
      if(!p) return;
      const a = getLeaveBalance(p, 'annual_leave');
      const c = getLeaveBalance(p, 'casual_leave');
      const av = prompt(`حدد إجمالي رصيد الإجازة السنوية للموظف (${p.name}):`, a.isSet ? a.allocated : '');
      if(av === null) return;
      const cv = prompt(`حدد إجمالي رصيد الإجازة العارضة للموظف (${p.name}):`, c.isSet ? c.allocated : '');
      if(cv === null) return;

      DB.leaveBalances[p.id] = {
        annual_leave: { allocated: Math.max(0, parseInt(av,10)||0), used: a.used },
        casual_leave: { allocated: Math.max(0, parseInt(cv,10)||0), used: c.used }
      };
      await saveLeaveBalances();
      await audit('تعديل رصيد إجازات', `تم تعديل رصيد إجازات ${p.name}`);
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

  return `<div class="topbar ${typeClass||''}">
    <div class="topbar-right">
      <div class="brand">${logoSvg(true)}<span class="brand-title">سجل الحضور</span></div>
      ${searchBoxHtml}
    </div>
    <div class="topbar-actions">
      ${isAdmin ? `<button class="link-btn link-btn-accent" id="quickBackupBtn" title="تنزيل نسخة احتياطية فورية لقاعدة البيانات">💾 نسخة احتياطية</button>` : ''}
      <span class="who">${esc(whoText)}</span>
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
  document.getElementById('logoutBtn').onclick = ()=>{
    clearSession();
    UI.adminAuthed=false; UI.currentPerson=null; UI.view='loginUnified'; UI.error=''; render();
  };
}

function renderAdminDashboard(){
  const pending = DB.requests.filter(r=>r.status==='pending').length;
  const adminName = UI.adminAuthed ? (DB.adminConfig?.name||'الأدمن الرئيسي') : `${UI.currentPerson?.name} (أدمن)`;
  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`لوحة التحكم والإدارة — ${esc(adminName)}`, 'admin')}
    <div class="tabs">
      <button class="tab-btn ${UI.activeTab==='departments'?'active':''}" data-tab="departments">الأقسام</button>
      <button class="tab-btn ${UI.activeTab==='people'?'active':''}" data-tab="people">المستخدمين والصلاحيات</button>
      <button class="tab-btn ${UI.activeTab==='jobs'?'active':''}" data-tab="jobs">الوظائف</button>
      <button class="tab-btn ${UI.activeTab==='leaveBalances'?'active':''}" data-tab="leaveBalances">أرصدة الإجازات</button>
      <button class="tab-btn ${UI.activeTab==='leaveReport'?'active':''}" data-tab="leaveReport">تقرير الإجازات</button>
      <button class="tab-btn ${UI.activeTab==='requests'?'active':''}" data-tab="requests">${pending>0?`<span class="tab-badge">${pending}</span>`:''}الطلبات</button>
      <button class="tab-btn ${UI.activeTab==='penalties'?'active':''}" data-tab="penalties">الجزاءات</button>
      <button class="tab-btn ${UI.activeTab==='penaltyReport'?'active':''}" data-tab="penaltyReport">تقرير الجزاءات</button>
      <button class="tab-btn ${UI.activeTab==='attendance'?'active':''}" data-tab="attendance">الحضور اليومي</button>
      <button class="tab-btn ${UI.activeTab==='reports'?'active':''}" data-tab="reports">التقارير الشهرية</button>
      <button class="tab-btn ${UI.activeTab==='backup'?'active':''}" data-tab="backup">النسخ الاحتياطي</button>
      <button class="tab-btn ${UI.activeTab==='audit'?'active':''}" data-tab="audit">سجل التعديلات</button>
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
    return `<div class="ledger-row">
      <div class="row-main"><div class="name">${esc(d.name)}</div><div class="meta">${mgr? 'المدير: '+esc(mgr.name) : 'بدون مدير معيّن'} · ${empCount} موظف</div></div>
      <div class="row-actions"><button class="btn btn-danger btn-sm" data-remove-dept="${d.id}">حذف</button></div>
    </div>`;
  }).join('');
  return `
  <div class="section-head"><h2>الأقسام</h2><span class="count">${DB.departments.length} قسم</span></div>
  <div class="form-card"><h3>إضافة قسم جديد</h3>
    <div class="form-row"><div class="field"><label>اسم القسم</label><input id="newDeptName" type="text" placeholder="مثال: قسم المبيعات" autocomplete="off"></div></div>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <button class="btn btn-primary" id="addDeptBtn">إضافة القسم</button>
  </div>
  <div class="ledger">${rows || `<div class="empty-illustration">لسه ما تمّش إضافة أي قسم</div>`}</div>`;
}

function renderPeopleTab(isAdmin, deptScopeId){
  const scoped = isAdmin ? DB.people : DB.people.filter(p=>p.departmentId===deptScopeId && p.role==='employee');

  const rows = scoped.map(p=>{
    const today = todayStr();
    const att = DB.attendance[p.id+'_'+today];
    const status = att?.checkIn ? `حاضر منذ ${att.checkIn}` : 'لم يسجل حضور اليوم';
    let roleBadge = p.role === 'admin' ? '<span class="chip chip-admin">أدمن</span>' : p.role === 'manager' ? '<span class="chip chip-mgr">مدير قسم</span>' : '<span class="chip">موظف</span>';
    const disabledBadge = p.disabled ? '<span class="chip chip-disabled">معطل</span>' : '';
    const passStatus = p.password ? `<span style="color:var(--success); font-size:11px;">(تم تعيين كلمة المرور)</span>` : `<span style="color:var(--danger); font-size:11px;">(في انتظار إنشاء كلمة المرور)</span>`;

    return `<div class="ledger-row ${p.disabled ? 'disabled-emp' : ''}">
      <div class="row-main">
        <div class="name clickable" data-open-profile="${p.id}" title="اضغط لفتح الملف الشامل للموظف">
          ${esc(p.name)} 🔍 ${roleBadge} ${disabledBadge} ${passStatus}
        </div>
        <div class="meta">${esc(p.jobTitle||'')} · <span class="chip chip-dept">${esc(deptName(p.departmentId))}</span>
          ${p.code ? `<span class="chip chip-code">كود: ${esc(p.code)}</span>` : `<span class="chip" style="color:var(--danger);">بدون كود</span>`}
          ${p.role==='employee'?`· ${status}`:''}</div>
      </div>
      <div class="row-actions">
        <button class="btn btn-outline btn-sm" data-open-profile="${p.id}">الملف الشامل</button>
        ${isAdmin ? `<button class="btn ${p.disabled?'btn-success':'btn-warning'} btn-sm" data-toggle-status="${p.id}">${p.disabled?'تشغيل':'تعطيل'}</button>` : ''}
        ${isAdmin ? `<button class="btn btn-outline btn-sm" data-change-role="${p.id}">تغيير الصلاحية</button>` : ''}
        <button class="btn btn-outline btn-sm" data-edit-person="${p.id}">تعديل البيانات</button>
        ${isAdmin ? `<button class="btn btn-outline btn-sm" data-set-code="${p.id}">${p.code?'تعديل الكود':'إدخال الكود'}</button>` : ''}
        ${isAdmin ? `<button class="btn btn-outline btn-sm" data-reset-pass="${p.id}">${p.password?'تغيير كلمة المرور':'تعيين كلمة مرور'}</button>` : ''}
        ${isAdmin && p.password ? `<button class="btn btn-outline btn-sm" data-clear-pass="${p.id}" style="color:var(--accent);">إلغاء كلمة المرور</button>` : ''}
        <button class="btn btn-danger btn-sm" data-remove-person="${p.id}">حذف</button>
      </div>
    </div>`;
  }).join('');

  const deptOptions = (isAdmin ? DB.departments : DB.departments.filter(d=>d.id===deptScopeId))
    .map(d=>`<option value="${d.id}">${esc(d.name)}</option>`).join('');

  return `
  <div class="section-head">
    <h2>${isAdmin?'المستخدمين والصلاحيات':'موظفين قسمي'}</h2>
    <div style="display:flex; gap:8px; align-items:center; flex-wrap:wrap;">
      <span class="count">${scoped.length} شخص</span>
      ${isAdmin ? `
        <button class="btn btn-outline btn-sm" id="downloadEmpTemplateBtn" title="تنزيل ملف إكسل فارغ معبأ بنماذج لإدخال الموظفين">📥 تنزيل نموذج الإكسل</button>
        <input type="file" id="importEmpExcelInput" accept=".xlsx, .xls, .csv" style="display:none;">
        <button class="btn btn-success btn-sm" id="triggerImportEmpBtn">📊 استيراد موظفين من ملف Excel</button>
      ` : ''}
    </div>
  </div>

  <div class="form-card"><h3>إضافة ${isAdmin?'مستخدم / موظف جديد':'موظف جديد'}</h3>
    <div class="form-row">
      <div class="field"><label>الاسم</label><input id="newPName" type="text" placeholder="اسم الموظف" autocomplete="off"></div>
      <div class="field"><label>الكود الوظيفي (إلزامي للتعريف)</label><input id="newPCode" type="text" placeholder="مثال: 101" autocomplete="off"></div>
      <div class="field"><label>الوظيفة</label><select id="newPJob">${(DB.jobTitles||DEFAULT_JOB_TITLES).map(j=>`<option value="${esc(j)}">${esc(j)}</option>`).join('')}<option value="__custom__">وظيفة أخرى...</option></select></div>
    </div>
    <div class="form-row">
      ${isAdmin ? `
        <div class="field">
          <label>نوع الحساب والصلاحية</label>
          <select id="newPRole">
            <option value="employee">موظف عادي</option>
            <option value="manager">مدير قسم</option>
            <option value="admin">أدمن (مسؤول بصلاحية كاملة)</option>
          </select>
        </div>` : ''}
      <div class="field"><label>القسم</label><select id="newPDept">${deptOptions || '<option value="">لا يوجد أقسام بعد</option>'}</select></div>
      <div class="field"><label>رقم واتساب (اختياري للإشعارات)</label><input id="newPPhone" type="text" placeholder="201001234567" autocomplete="off"></div>
    </div>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <button class="btn btn-primary" id="addPersonBtn">إضافة الموظف</button>
  </div>
  <div class="ledger">${rows || `<div class="empty-illustration">لا يوجد موظفون مسجلون بعد</div>`}</div>`;
}

function renderJobsTab(){
  const rows=(DB.jobTitles||[]).map((j,i)=>`<div class="ledger-row"><div class="row-main"><div class="name">${esc(j)}</div></div><div class="row-actions"><button class="btn btn-danger btn-sm" data-remove-job="${i}">حذف</button></div></div>`).join('');
  return `<div class="section-head"><h2>الوظائف</h2><span class="count">${(DB.jobTitles||[]).length} وظيفة</span></div><div class="form-card"><h3>إضافة وظيفة جديدة</h3><div class="form-row"><div class="field"><label>اسم الوظيفة</label><input id="newJobTitle" type="text" placeholder="مثال: مدير حسابات" autocomplete="off"></div></div><button class="btn btn-primary" id="addJobBtn">إضافة الوظيفة</button></div><div class="ledger">${rows||'<div class="empty-illustration">لا توجد وظائف مضافة</div>'}</div>`;
}
function attachJobsEvents(){
  document.getElementById('addJobBtn').onclick=async()=>{const v=document.getElementById('newJobTitle').value.trim(); if(!v)return; if(!DB.jobTitles.includes(v))DB.jobTitles.push(v); await saveJobTitles(); render();};
  document.querySelectorAll('[data-remove-job]').forEach(b=>b.onclick=async()=>{const j=DB.jobTitles[+b.dataset.removeJob]; if(!j||!confirm('حذف الوظيفة من قائمة الاختيارات؟'))return; DB.jobTitles.splice(+b.dataset.removeJob,1); await saveJobTitles(); render();});
}

function renderRequestsTab(isAdmin, deptScopeId){
  const filters = [['pending','قيد المراجعة'],['approved','تمت الموافقة'],['rejected','مرفوضة'],['all','الكل']];
  let list = DB.requests.filter(r => isAdmin ? true : r.departmentId===deptScopeId);
  list = list.filter(r => UI.reqFilter==='all' ? true : r.status===UI.reqFilter);
  list = list.slice().sort((a,b)=> b.createdAt.localeCompare(a.createdAt));

  const rows = list.map(r=>{
    const stampClass = r.status==='approved'?'stamp-approved':r.status==='rejected'?'stamp-rejected':'stamp-pending';
    const stampText = r.status==='approved'?'معتمد':r.status==='rejected'?'مرفوض':'قيد المراجعة';
    const typeInfo = REQUEST_TYPES[r.type] || { label:r.type };

    return `<div class="ledger-row">
      <div class="row-main">
        <div class="name clickable" data-open-profile="${r.empId}" title="اضغط لفتح الملف الشامل للموظف">${esc(r.empName)} 🔍 — ${esc(typeInfo.label)} ${isAdmin?`<span class="chip chip-dept">${esc(deptName(r.departmentId))}</span>`:''}</div>
        <div class="meta">بتاريخ ${requestDateLabel(r)}${r.durationDays>1?` · ${r.durationDays} يوم`:''} · السبب: ${esc(r.reason)}</div>
      </div>
      <div class="row-actions">
        <span class="stamp ${stampClass}">${stampText}</span>
        ${r.status==='pending' ? `<button class="btn btn-success btn-sm" data-approve="${r.id}">موافقة</button><button class="btn btn-danger btn-sm" data-reject="${r.id}">رفض</button>` : ''}
      </div>
    </div>`;
  }).join('');

  return `
  <div class="section-head"><h2>طلبات الإذن</h2><span class="count">${list.length} طلب</span></div>
  <div style="display:flex; gap:8px; margin-bottom:14px; flex-wrap:wrap;">
    ${filters.map(([k,l])=>`<button class="btn ${UI.reqFilter===k?'btn-primary':'btn-outline'} btn-sm" data-filter="${k}">${l}</button>`).join('')}
  </div>
  <div class="ledger">${rows || `<div class="empty-illustration">لا توجد طلبات في هذا التصنيف</div>`}</div>`;
}

function renderPenaltiesTab(isAdmin, deptScopeId){
  const scopedPeople = isAdmin ? DB.people.filter(p=>p.role==='employee') : DB.people.filter(p=>p.departmentId===deptScopeId && p.role==='employee');
  let list = DB.penalties.filter(pn => isAdmin ? true : pn.departmentId===deptScopeId);
  list = list.slice().sort((a,b)=> b.createdAt.localeCompare(a.createdAt));

  const rows = list.map(pn=>{
    const info = PENALTY_TYPES[pn.type] || { label: pn.type };
    return `<div class="ledger-row">
      <div class="row-main">
        <div class="name clickable" data-open-profile="${pn.empId}" title="اضغط لفتح الملف الشامل للموظف">${esc(pn.empName)} 🔍 ${isAdmin?`<span class="chip chip-dept">${esc(deptName(pn.departmentId))}</span>`:''}</div>
        <div class="meta">السبب: ${esc(pn.reason)} · بواسطة ${esc(pn.appliedByName)} · ${fmtDate(pn.createdAt.slice(0,10))}</div>
      </div>
      <div class="row-actions">
        <span class="stamp stamp-penalty">${esc(info.label)}</span>
        ${isAdmin ? `<button class="btn btn-danger btn-sm" data-delete-penalty="${pn.id}">حذف الجزاء</button>` : ''}
      </div>
    </div>`;
  }).join('');

  const peopleOptions = scopedPeople.map(p=>`<option value="${p.id}">${esc(p.name)}</option>`).join('');
  const typeOptions = Object.entries(PENALTY_TYPES).map(([k,v])=>`<option value="${k}">${v.label}</option>`).join('');

  return `
  <div class="section-head"><h2>الجزاءات</h2><span class="count">${list.length} جزاء</span></div>
  <div class="form-card"><h3>إضافة جزاء</h3>
    <div class="form-row">
      <div class="field"><label>الموظف</label><select id="penEmp">${peopleOptions || '<option value="">لا يوجد موظفون</option>'}</select></div>
      <div class="field"><label>نوع الجزاء</label><select id="penType">${typeOptions}</select></div>
    </div>
    <div class="field"><label>سبب الجزاء</label><textarea id="penReason" placeholder="اكتب سبب الجزاء"></textarea></div>
    ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
    <button class="btn btn-primary" id="addPenBtn">تسجيل الجزاء</button>
  </div>
  <div class="ledger">${rows || `<div class="empty-illustration">لا توجد جزاءات مسجّلة</div>`}</div>`;
}

function renderAttendanceTab(isAdmin, deptScopeId){
  const date = UI.attDate || todayStr();
  const scoped = isAdmin
    ? DB.people.filter(p=> UI.attDeptFilter==='all' || p.departmentId===UI.attDeptFilter)
    : DB.people.filter(p=>p.departmentId===deptScopeId);
  const employees = scoped.filter(p=>p.role==='employee');

  const rows = employees.map(e=>{
    const att = DB.attendance[e.id+'_'+date];
    let stampClass, stampText, meta;
    if(att && att.checkIn){
      const isLate = att.checkIn > '09:30';
      stampClass = isLate ? 'stamp-rejected' : 'stamp-approved';
      stampText = isLate ? 'متأخر' : 'حاضر';
      meta = `دخول ${att.checkIn}${att.checkOut ? ' · انصراف '+att.checkOut : ''}`;
    } else if(date <= todayStr()){
      stampClass='stamp-rejected'; stampText='غائب'; meta='لا يوجد تسجيل حضور';
    } else { stampClass='stamp-pending'; stampText='—'; meta='لم يحن الموعد بعد'; }

    return `<div class="ledger-row">
      <div class="row-main">
        <div class="name clickable" data-open-profile="${e.id}" title="اضغط لفتح الملف الشامل للموظف">${esc(e.name)} 🔍 ${isAdmin?`<span class="chip chip-dept">${esc(deptName(e.departmentId))}</span>`:''}</div>
        <div class="meta">${meta}</div>
      </div>
      <div class="row-actions"><span class="stamp ${stampClass}">${stampText}</span></div>
    </div>`;
  }).join('');

  const deptFilterHtml = isAdmin ? `
    <select class="date-picker" id="attDeptFilter" style="margin-inline-start:8px;">
      <option value="all" ${UI.attDeptFilter==='all'?'selected':''}>كل الأقسام</option>
      ${DB.departments.map(d=>`<option value="${d.id}" ${UI.attDeptFilter===d.id?'selected':''}>${esc(d.name)}</option>`).join('')}
    </select>` : '';

  return `
  <div class="section-head"><h2>حالة الحضور</h2>
    <div><input type="date" class="date-picker" id="attDatePick" value="${date}">${deptFilterHtml}</div>
  </div>
  <div style="margin-bottom:12px; color:var(--ink-soft); font-size:13px;">${fmtDate(date)}</div>
  <div class="ledger">${employees.length ? rows : `<div class="empty-illustration">لا يوجد موظفون بعد</div>`}</div>`;
}

function computeReportRows(month, deptScopeId, isAdmin){
  const employees = DB.people.filter(p=>{
    if(p.role!=='employee') return false;
    if(!isAdmin) return p.departmentId===deptScopeId;
    return UI.reportDept==='all' || p.departmentId===UI.reportDept;
  });
  return employees.map(e=>{
    const attKeys = Object.keys(DB.attendance).filter(k=>k.startsWith(e.id+'_') && k.slice(e.id.length+1).startsWith(month));
    const presentDays = attKeys.filter(k=>DB.attendance[k].checkIn).length;
    const lateDays = attKeys.filter(k=>DB.attendance[k].checkIn > '09:30').length;
    const monthReqs = DB.requests.filter(r=>r.empId===e.id && r.date.startsWith(month) && r.status==='approved');
    const reqCounts = {};
    Object.keys(REQUEST_TYPES).forEach(t=> reqCounts[t] = monthReqs.filter(r=>r.type===t).length);
    const monthPens = DB.penalties.filter(pn=>pn.empId===e.id && pn.createdAt.slice(0,7)===month);
    const penaltyDaysTotal = monthPens.reduce((sum,pn)=> sum + (PENALTY_TYPES[pn.type]?.weight||0), 0);
    const deptMgr = e.departmentId ? managerOf(e.departmentId) : null;
    return {
      'الاسم': e.name, 'القسم': deptName(e.departmentId), 'مدير القسم': deptMgr ? deptMgr.name : '—', 'الكود': e.code||'—',
      'أيام الحضور': presentDays, 'أيام التأخير': lateDays,
      ...Object.fromEntries(Object.entries(REQUEST_TYPES).map(([k,v])=>[v.label, reqCounts[k]])),
      'عدد الجزاءات': monthPens.length, 'إجمالي أيام الجزاء': penaltyDaysTotal
    };
  });
}
function computePenaltyReportRows(month, deptScopeId, isAdmin){
  return DB.penalties.filter(p=>{
    if(!isAdmin && p.departmentId!==deptScopeId) return false;
    if(isAdmin && UI.penaltyReportDept!=='all' && p.departmentId!==UI.penaltyReportDept) return false;
    return !month || p.createdAt.slice(0,7)===month;
  }).sort((a,b)=>b.createdAt.localeCompare(a.createdAt)).map(p=>({
    '_id': p.id,
    'التاريخ':p.createdAt.slice(0,10), 'اليوم':fmtDate(p.createdAt.slice(0,10)), 'الموظف':p.empName, 'القسم':deptName(p.departmentId),
    'نوع الجزاء':PENALTY_TYPES[p.type]?.label||p.type, 'قيمة الجزاء (أيام)':PENALTY_TYPES[p.type]?.weight||0,
    'السبب':p.reason, 'بواسطة':p.appliedByName||'—'
  }));
}
function renderPenaltyReportTab(isAdmin, deptScopeId){
  const month=UI.penaltyReportMonth||monthStr(), rows=computePenaltyReportRows(month,deptScopeId,isAdmin);
  const deptFilter=isAdmin?`<select class="date-picker" id="penaltyReportDeptFilter"><option value="all">كل الأقسام</option>${DB.departments.map(d=>`<option value="${d.id}" ${UI.penaltyReportDept===d.id?'selected':''}>${esc(d.name)}</option>`).join('')}</select>`:'';
  const cols=['التاريخ','اليوم','الموظف','القسم','نوع الجزاء','قيمة الجزاء (أيام)','السبب','بواسطة'];
  return `<div class="section-head"><h2>تقرير الجزاءات التفصيلي</h2><div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap;"><input type="month" class="date-picker" id="penaltyReportMonthPick" value="${month}">${deptFilter}<button class="btn btn-outline btn-sm" id="downloadPenaltyReportBtn">تنزيل Excel (عربي RTL)</button></div></div><div class="table-scroll"><table class="report-table"><thead><tr>${cols.map(c=>`<th>${c}</th>`).join('')}${isAdmin?'<th>إجراء</th>':''}</tr></thead><tbody>${rows.length?rows.map(r=>`<tr>${cols.map(c=>`<td>${esc(r[c])}</td>`).join('')}${isAdmin?`<td><button class="btn btn-danger btn-sm" data-delete-penalty="${r._id}">حذف</button></td>`:''}</tr>`).join(''):`<tr><td colspan="${cols.length+(isAdmin?1:0)}">لا توجد جزاءات لهذا الشهر</td></tr>`} </tbody></table></div>`;
}
function attachPenaltyReportEvents(isAdmin,deptScopeId){
  document.getElementById('penaltyReportMonthPick').onchange=e=>{UI.penaltyReportMonth=e.target.value;render();};
  const df=document.getElementById('penaltyReportDeptFilter'); if(df)df.onchange=e=>{UI.penaltyReportDept=e.target.value;render();};
  document.getElementById('downloadPenaltyReportBtn').onclick=()=>{
    const rows=computePenaltyReportRows(UI.penaltyReportMonth||monthStr(),deptScopeId,isAdmin).map(({_id, ...rest})=>rest);
    if(!rows.length){alert('لا يوجد جزاءات لهذا الشهر');return;}
    const wb = XLSX.utils.book_new();
    createAndSaveArabicExcel(wb, 'تقرير الجزاءات', rows, `تقرير-الجزاءات-${UI.penaltyReportMonth||monthStr()}.xlsx`);
  };
  
  if(isAdmin){
    document.querySelectorAll('[data-delete-penalty]').forEach(b=>{
      b.onclick = async ()=>{
        const penId = b.dataset.deletePenalty;
        const pen = DB.penalties.find(p=>p.id===penId);
        if(!pen || !confirm(`هل أنت متأكد من حذف هذا الجزاء المسجل على الموظف (${pen.empName})؟`)) return;
        DB.penalties = DB.penalties.filter(p=>p.id!==penId);
        await savePenalties();
        await audit('حذف جزاء', `قام الأدمن بحذف جزاء (${PENALTY_TYPES[pen.type]?.label||pen.type}) المسجل على ${pen.empName}`);
        render();
      };
    });
  }
}

function renderReportsTab(isAdmin, deptScopeId){
  const month = UI.reportMonth || monthStr();
  const rows = computeReportRows(month, deptScopeId, isAdmin);
  const cols = rows[0] ? Object.keys(rows[0]) : ['الاسم','القسم','مدير القسم','الكود','أيام الحضور','أيام التأخير','عدد الجزاءات','إجمالي أيام الجزاء'];

  const deptFilterHtml = isAdmin ? `
    <select class="date-picker" id="reportDeptFilter" style="margin-inline-start:8px;">
      <option value="all" ${UI.reportDept==='all'?'selected':''}>كل الأقسام</option>
      ${DB.departments.map(d=>`<option value="${d.id}" ${UI.reportDept===d.id?'selected':''}>${esc(d.name)}</option>`).join('')}
    </select>` : '';

  return `
  <div class="section-head"><h2>التقرير الشهري</h2>
    <div style="display:flex; align-items:center; gap:8px;">
      <input type="month" class="date-picker" id="reportMonthPick" value="${month}">
      ${deptFilterHtml}
      <button class="btn btn-outline btn-sm" id="downloadReportBtn">تنزيل Excel (عربي RTL)</button>
    </div>
  </div>
  <div class="table-scroll">
    <table class="report-table"><thead><tr>${cols.map(c=>`<th>${esc(c)}</th>`).join('')}</tr></thead>
    <tbody>${rows.length ? rows.map(r=>`<tr>${cols.map(c=>`<td>${esc(r[c])}</td>`).join('')}</tr>`) : `<tr><td colspan="${cols.length}">لا يوجد موظفون في هذا النطاق</td></tr>`} </tbody></table>
  </div>`;
}

function attachReportsEvents(isAdmin, deptScopeId){
  document.getElementById('reportMonthPick').onchange = e=>{ UI.reportMonth=e.target.value; render(); };
  const df = document.getElementById('reportDeptFilter'); if(df) df.onchange = e=>{ UI.reportDept=e.target.value; render(); };
  document.getElementById('downloadReportBtn').onclick = ()=>{
    const rows = computeReportRows(UI.reportMonth||monthStr(), deptScopeId, isAdmin);
    if(!rows.length){ alert('لا يوجد بيانات لهذا الشهر'); return; }
    const wb = XLSX.utils.book_new();
    createAndSaveArabicExcel(wb, 'التقرير الشهري', rows, `تقرير-${UI.reportMonth||monthStr()}.xlsx`);
  };
}

function renderLeaveBalancesTab(isAdmin, deptScopeId){
  const people = DB.people.filter(p=>p.role==='employee' && (isAdmin || p.departmentId===deptScopeId) && (isAdmin ? (UI.leaveDept==='all'||p.departmentId===UI.leaveDept) : true));
  const deptFilter = isAdmin ? `<select class="date-picker" id="leaveDeptFilter"><option value="all">كل الأقسام</option>${DB.departments.map(d=>`<option value="${d.id}" ${UI.leaveDept===d.id?'selected':''}>${esc(d.name)}</option>`).join('')}</select>` : '';
  
  const rows = people.map(p=>{
    const a = getLeaveBalance(p,'annual_leave');
    const c = getLeaveBalance(p,'casual_leave');
    const aText = a.isSet ? `سنوية: ${a.remaining}/${a.allocated} متبقي` : `<span style="color:var(--danger)">سنوية: لم يحدد رصيد</span>`;
    const cText = c.isSet ? `عارضة: ${c.remaining}/${c.allocated} متبقي` : `<span style="color:var(--danger)">عارضة: لم يحدد رصيد</span>`;
    return `<div class="ledger-row ${p.disabled ? 'disabled-emp' : ''}">
      <div class="row-main">
        <div class="name clickable" data-open-profile="${p.id}" title="اضغط لفتح الملف الشامل للموظف">
          ${esc(p.name)} 🔍 ${p.disabled ? '<span class="chip chip-disabled">معطل</span>' : ''} <span class="chip chip-dept">${esc(deptName(p.departmentId))}</span>
        </div>
        <div class="meta">${aText} · ${cText}</div>
      </div>
      <div class="row-actions">
        <button class="btn btn-outline btn-sm" data-open-profile="${p.id}">الملف الشامل</button>
        <button class="btn btn-outline btn-sm" data-edit-leave="${p.id}">تحديد/تعديل الرصيد</button>
      </div>
    </div>`;
  }).join('');

  return `
  <div class="section-head"><h2>أرصدة الإجازات</h2><div>${deptFilter}</div></div>
  <div class="ledger">${rows || '<div class="empty-illustration">لا يوجد موظفون</div>'}</div>`;
}

function attachLeaveBalancesEvents(isAdmin, deptScopeId){
  const f = document.getElementById('leaveDeptFilter'); 
  if(f) f.onchange = e=>{ UI.leaveDept=e.target.value; render(); };
  
  document.querySelectorAll('[data-edit-leave]').forEach(b=>{
    b.onclick = async ()=>{
      const p = personById(b.dataset.editLeave);
      if(!p) return;
      const a = getLeaveBalance(p, 'annual_leave');
      const c = getLeaveBalance(p, 'casual_leave');
      const av = prompt(`حدد إجمالي رصيد الإجازة السنوية للموظف (${p.name}):`, a.isSet ? a.allocated : '');
      if(av === null) return;
      const cv = prompt(`حدد إجمالي رصيد الإجازة العارضة للموظف (${p.name}):`, c.isSet ? c.allocated : '');
      if(cv === null) return;

      DB.leaveBalances[p.id] = {
        annual_leave: { allocated: Math.max(0, parseInt(av, 10) || 0), used: a.used },
        casual_leave: { allocated: Math.max(0, parseInt(cv, 10) || 0), used: c.used }
      };

      await saveLeaveBalances();
      await audit('تحديد رصيد إجازات', `تم تحديد رصيد الإجازات لـ ${p.name}`);
      render();
    };
  });
}

function computeLeaveReportRows(month,deptScopeId,isAdmin){
  return DB.requests.filter(r=>r.status==='approved' && LEAVE_TYPES[r.type] && (!month||String(r.startDate||r.date).startsWith(month)) && (isAdmin?(UI.leaveDept==='all'||r.departmentId===UI.leaveDept):r.departmentId===deptScopeId))
    .map(r=>{
      const deptMgr = r.departmentId ? managerOf(r.departmentId) : null;
      return {
        '_id': r.id,
        'empId': r.empId,
        'الموظف': r.empName,
        'القسم': deptName(r.departmentId),
        'مدير القسم': deptMgr ? deptMgr.name : '—',
        'نوع الإجازة': LEAVE_TYPES[r.type].label,
        'عدد الأيام': r.durationDays || 1,
        'من تاريخ': r.startDate || r.date,
        'إلى تاريخ': r.endDate || r.date,
        'تاريخ يوم الإجازة': requestDateLabel(r),
        'السبب': r.reason
      };
    }).sort((a,b)=>b['من تاريخ'].localeCompare(a['من تاريخ']));
}

function renderLeaveReportTab(isAdmin,deptScopeId){
  const month=UI.leaveReportMonth||monthStr(), rows=computeLeaveReportRows(month,deptScopeId,isAdmin), cols=['الموظف','القسم','مدير القسم','نوع الإجازة','عدد الأيام','من تاريخ','إلى تاريخ','تاريخ يوم الإجازة','السبب'];
  const df=isAdmin?`<select class="date-picker" id="leaveReportDeptFilter"><option value="all">كل الأقسام</option>${DB.departments.map(d=>`<option value="${d.id}" ${UI.leaveDept===d.id?'selected':''}>${esc(d.name)}</option>`).join('')}</select>`:'';
  return `<div class="section-head"><h2>تقرير الإجازات المفصل</h2><div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap;"><input type="month" class="date-picker" id="leaveReportMonthPick" value="${month}">${df}<button class="btn btn-outline btn-sm" id="downloadLeaveReportBtn">تنزيل Excel (عربي RTL)</button></div></div>
  <div class="table-scroll"><table class="report-table"><thead><tr>${cols.map(c=>`<th>${c}</th>`).join('')}${isAdmin?'<th>إجراء</th>':''}</tr></thead><tbody>${rows.length?rows.map(r=>`<tr>${cols.map(c=>`<td>${c==='الموظف'?`<span class="clickable" data-open-profile="${r.empId}">${esc(r[c])} 🔍</span>`:esc(r[c])}</td>`).join('')}${isAdmin?`<td><button class="btn btn-danger btn-sm" data-delete-leave-req="${r._id}">حذف الإجازة</button></td>`:''}</tr>`).join(''):`<tr><td colspan="${cols.length+(isAdmin?1:0)}">لا توجد إجازات لهذا الشهر</td></tr>`} </tbody></table></div>`;
}

function attachLeaveReportEvents(isAdmin,deptScopeId){
  const m=document.getElementById('leaveReportMonthPick'); if(m)m.onchange=e=>{UI.leaveReportMonth=e.target.value;render();};
  const d=document.getElementById('leaveReportDeptFilter'); if(d)d.onchange=e=>{UI.leaveDept=e.target.value;render();};
  const b=document.getElementById('downloadLeaveReportBtn');
  if(b) b.onclick=()=>{
    const rows=computeLeaveReportRows(UI.leaveReportMonth||monthStr(),deptScopeId,isAdmin).map(({_id, empId, ...rest})=>rest);
    if(!rows.length){alert('لا توجد إجازات لهذا الشهر');return;}
    const wb = XLSX.utils.book_new();
    createAndSaveArabicExcel(wb, 'تقرير الإجازات', rows, `تقرير-الإجازات-${UI.leaveReportMonth||monthStr()}.xlsx`);
  };

  if(isAdmin){
    document.querySelectorAll('[data-delete-leave-req]').forEach(b=>{
      b.onclick = async ()=>{
        const reqId = b.dataset.deleteLeaveReq;
        const req = DB.requests.find(r=>r.id===reqId);
        if(!req || !confirm(`هل أنت متأكد من حذف هذه الإجازة الخاصة بالموظف (${req.empName})؟`)) return;

        if(LEAVE_TYPES[req.type] && req.empId){
          const emp = personById(req.empId);
          if(emp && DB.leaveBalances[emp.id] && DB.leaveBalances[emp.id][req.type]){
            const currentUsed = Number(DB.leaveBalances[emp.id][req.type].used || 0);
            const daysToRestore = Number(req.durationDays || 1);
            DB.leaveBalances[emp.id][req.type].used = Math.max(0, currentUsed - daysToRestore);
            await saveLeaveBalances();
          }
        }

        DB.requests = DB.requests.filter(r=>r.id!==reqId);
        await saveRequests();
        await audit('حذف إجازة من التقرير', `قام الأدمن بحذف إجازة لـ ${req.empName}`);
        render();
      };
    });
  }
}

function renderBackupTab(){
  return `
  <div class="section-head"><h2>إدارة النسخ الاحتياطي واستعادة البيانات</h2></div>
  <div class="backup-card">
    <h3 style="margin:0 0 8px; color:var(--primary); font-size:16px;">تأمين واسترجاع بيانات المنظومة</h3>
    <div class="backup-options">
      <div class="backup-box" style="border:1.5px solid var(--primary);">
        <div>
          <h4>🔄 استرجاع البيانات من نسخة احتياطية (Restore)</h4>
          <p>اختر ملف النسخة الاحتياطية (.json) الذي قمت بتنزيله سابقاً لاستعادة كافة البيانات فوراً.</p>
        </div>
        <input type="file" id="importJsonFileInput" accept=".json" style="display:none;">
        <button class="btn btn-primary btn-block" id="triggerImportBtn">📥 اختيار ملف واسترجاع البيانات الآن</button>
      </div>

      <div class="backup-box">
        <div>
          <h4>💾 تنزيل نسخة احتياطية رقمية (JSON Backup)</h4>
          <p>تنزيل ملف رقمي شامل لقاعدة البيانات.</p>
        </div>
        <button class="btn btn-outline" id="exportJsonBtn">💾 تنزيل نسخة شاملة (JSON)</button>
      </div>

      <div class="backup-box">
        <div>
          <h4>📊 تنزيل مصنف إكسل شامل (Excel RTL)</h4>
          <p>تنزيل ملف إكسل منسق عربي بالكامل.</p>
        </div>
        <button class="btn btn-success" id="exportExcelBtn">📊 تنزيل مصنف شامل (Excel)</button>
      </div>
    </div>
  </div>`;
}

function attachBackupEvents(){
  const jsonBtn = document.getElementById('exportJsonBtn');
  if(jsonBtn) jsonBtn.onclick = () => exportFullJsonBackup();

  const excelBtn = document.getElementById('exportExcelBtn');
  if(excelBtn) excelBtn.onclick = () => exportFullExcelBackup();

  const triggerBtn = document.getElementById('triggerImportBtn');
  const fileInput = document.getElementById('importJsonFileInput');

  if(triggerBtn && fileInput){
    triggerBtn.onclick = () => fileInput.click();
    fileInput.onchange = (e) => {
      const file = e.target.files[0];
      if(file) restoreFromJsonFile(file);
      fileInput.value = '';
    };
  }
}

function renderAuditTab(){
  const rows=DB.auditLogs.slice(0,200).map(x=>`<div class="ledger-row"><div class="row-main"><div class="name">${esc(x.action)}</div><div class="meta">${esc(x.details)} · بواسطة ${esc(x.actor)} · ${new Date(x.createdAt).toLocaleString('ar-EG')}</div></div></div>`).join('');
  return `<div class="section-head"><h2>سجل التعديلات والعمليات</h2><span class="count">${DB.auditLogs.length}</span></div><div class="ledger">${rows||'<div class="empty-illustration">لا توجد عمليات مسجلة</div>'}</div>`;
}

function attachAdminDashboardEvents(){
  attachTopbarSearchEvents();
  attachLogout();
  document.querySelectorAll('.tab-btn').forEach(b=> b.onclick = ()=>{ UI.activeTab=b.dataset.tab; UI.error=''; render(); });

  if(UI.activeTab==='departments'){
    document.getElementById('addDeptBtn').onclick = async ()=>{
      const name = document.getElementById('newDeptName').value.trim();
      if(!name){ UI.error='اكتب اسم القسم'; render(); return; }
      DB.departments.push({ id: uid(), name });
      await saveDepartments(); UI.error=''; render();
    };
    document.querySelectorAll('[data-remove-dept]').forEach(b=> b.onclick = async ()=>{
      if(!confirm('حذف القسم؟')) return;
      DB.departments = DB.departments.filter(d=>d.id!==b.dataset.removeDept);
      await saveDepartments(); render();
    });
  }

  if(UI.activeTab==='people'){ attachPeopleFormEvents(true, null); }
  if(UI.activeTab==='jobs'){ attachJobsEvents(); }
  if(UI.activeTab==='leaveBalances') attachLeaveBalancesEvents(true,null);
  if(UI.activeTab==='leaveReport') attachLeaveReportEvents(true,null);
  if(UI.activeTab==='requests'){ attachRequestsEvents(true, null); }
  if(UI.activeTab==='penalties'){ attachPenaltiesEvents(true, null); }
  if(UI.activeTab==='penaltyReport'){ attachPenaltyReportEvents(true, null); }
  if(UI.activeTab==='attendance'){
    document.getElementById('attDatePick').onchange = e=>{ UI.attDate=e.target.value; render(); };
    const df = document.getElementById('attDeptFilter'); if(df) df.onchange = e=>{ UI.attDeptFilter=e.target.value; render(); };
  }
  if(UI.activeTab==='reports'){ attachReportsEvents(true, null); }
  if(UI.activeTab==='backup'){ attachBackupEvents(); }
}

function attachPeopleFormEvents(isAdmin, deptScopeId){
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
  const addBtn = document.getElementById('addPersonBtn');
  if(addBtn) addBtn.onclick = async ()=>{
    const name = document.getElementById('newPName').value.trim();
    const code = document.getElementById('newPCode').value.trim();
    let job = document.getElementById('newPJob').value;
    if(job==='__custom__'){ job=prompt('اكتب اسم الوظيفة الجديدة:')?.trim()||''; if(job && isAdmin && !DB.jobTitles.includes(job)){ DB.jobTitles.push(job); await saveJobTitles(); } }
    const roleEl = document.getElementById('newPRole');
    const role = isAdmin ? (roleEl ? roleEl.value : 'employee') : 'employee';
    const deptSel = document.getElementById('newPDept').value;
    const departmentId = isAdmin ? deptSel : deptScopeId;
    const phone = document.getElementById('newPPhone').value.trim();

    if(!name || !departmentId || !code){ 
      UI.error='يرجى إدخال اسم الموظف، والكود الخاص به، وتحديد القسم'; 
      render(); return; 
    }
    
    if(DB.people.some(p=>p.code && p.code.toLowerCase()===code.toLowerCase())){
      UI.error='هذا الكود مستخدم بالفعل لموظف آخر، يرجى كتابة كود مختلف'; render(); return;
    }

    DB.people.push({ 
      id: uid(), name, jobTitle: job, role, departmentId, password: '', phone, code, disabled: false, createdAt: new Date().toISOString() 
    });

    await savePeople(); 
    await audit('إضافة موظف جديد', `تمت إضافة ${name} بالكود (${code})`); 
    UI.error=''; render();
  };

  document.querySelectorAll('[data-toggle-status]').forEach(b=>{
    b.onclick = async ()=>{
      const person = personById(b.dataset.toggleStatus);
      if(!person) return;
      const willDisable = !person.disabled;
      const actionText = willDisable ? 'تعطيل حساب' : 'إعادة تشغيل حساب';
      if(!confirm(`هل أنت متأكد من ${actionText} الموظف (${person.name})؟`)) return;
      person.disabled = willDisable;
      await savePeople();
      await audit(`${actionText}`, `قام الأدمن بـ ${actionText} للموظف ${person.name}`);
      render();
    };
  });

  document.querySelectorAll('[data-change-role]').forEach(b=> b.onclick = async ()=>{
    const person = personById(b.dataset.changeRole);
    if(!person) return;
    const current = person.role === 'admin' ? '3' : person.role === 'manager' ? '2' : '1';
    const choice = prompt(`تعديل صلاحية ${person.name}:
1: موظف عادي
2: مدير قسم
3: أدمن مسؤول`, current);
    if(!choice) return;
    const map = {'1':'employee', '2':'manager', '3':'admin'};
    if(map[choice]){
      person.role = map[choice];
      await savePeople();
      await audit('تعديل صلاحية', `تم تعديل صلاحية ${person.name} إلى ${map[choice]}`);
      render();
    }
  });

  document.querySelectorAll('[data-remove-person]').forEach(b=> b.onclick = async ()=>{
    if(!confirm('حذف هذا الحساب من السجل؟')) return;
    DB.people = DB.people.filter(p=>p.id!==b.dataset.removePerson);
    await savePeople(); render();
  });

  document.querySelectorAll('[data-reset-pass]').forEach(b=> b.onclick = async ()=>{
    const person = personById(b.dataset.resetPass);
    if(!person) return;
    const np = prompt(`تعيين كلمة مرور جديدة لـ (${person.name}):`, '');
    if(!np || np.trim().length < 4){ alert('كلمة المرور يجب أن تتكون من 4 خانات على الأقل'); return; }
    person.password = np.trim();
    await savePeople();
    await audit('تغيير كلمة مرور من الأدمن', `قام الأدمن بتعيين كلمة مرور للموظف ${person.name}`);
    render();
  });

  document.querySelectorAll('[data-clear-pass]').forEach(b=> b.onclick = async ()=>{
    const person = personById(b.dataset.clearPass);
    if(!person || !confirm(`هل تريد مسح كلمة المرور للموظف (${person.name})؟`)) return;
    person.password = '';
    await savePeople();
    await audit('إعادة ضبط كلمة المرور', `تم مسح كلمة المرور لـ ${person.name}`);
    render();
  });

  document.querySelectorAll('[data-set-code]').forEach(b=> b.onclick = async ()=>{
    const person = personById(b.dataset.setCode);
    if(!person) return;
    const nc = prompt(`أدخل الكود الوظيفي لـ (${person.name}):`, person.code||'');
    if(nc===null) return;
    const cleanCode = nc.trim();
    if(cleanCode && DB.people.some(p=>p.id!==person.id && p.code && p.code.toLowerCase()===cleanCode.toLowerCase())){
      alert('هذا الكود مستخدم بالفعل لموظف آخر!'); return;
    }
    person.code = cleanCode;
    await savePeople(); 
    await audit('تعديل كود الموظف', `تم تعديل كود الموظف ${person.name} إلى (${cleanCode})`);
    render();
  });

  document.querySelectorAll('[data-edit-person]').forEach(b=> b.onclick = async ()=>{
    const p=personById(b.dataset.editPerson); if(!p) return;
    const name=prompt('اسم الموظف:',p.name); if(name===null) return;
    const job=prompt('الوظيفة:',p.jobTitle||''); if(job===null) return;
    p.name=name.trim()||p.name; p.jobTitle=job.trim();
    await savePeople(); render();
  });
}

function attachRequestsEvents(isAdmin, deptScopeId){
  document.querySelectorAll('[data-filter]').forEach(b=> b.onclick = ()=>{ UI.reqFilter=b.dataset.filter; render(); });
  document.querySelectorAll('[data-approve]').forEach(b=> b.onclick = async ()=>{
    const r = DB.requests.find(x=>x.id===b.dataset.approve);
    if(!isAdmin && r.departmentId!==deptScopeId) return;
    r.status='approved'; r.reviewedAt=new Date().toISOString();

    if(LEAVE_TYPES[r.type] && r.empId){
      const emp = personById(r.empId);
      if(emp){
        const bal = getLeaveBalance(emp, r.type);
        const days = r.durationDays || 1;
        DB.leaveBalances[emp.id] = DB.leaveBalances[emp.id] || {};
        DB.leaveBalances[emp.id][r.type] = { allocated: bal.allocated, used: bal.used + days };
        await saveLeaveBalances();
      }
    }
    await saveRequests(); render();
  });
  document.querySelectorAll('[data-reject]').forEach(b=> b.onclick = async ()=>{
    const r = DB.requests.find(x=>x.id===b.dataset.reject);
    if(!isAdmin && r.departmentId!==deptScopeId) return;
    r.status='rejected'; r.reviewedAt=new Date().toISOString();
    await saveRequests(); render();
  });
}

function attachPenaltiesEvents(isAdmin, deptScopeId){
  const addBtn = document.getElementById('addPenBtn');
  if(addBtn) addBtn.onclick = async ()=>{
    const empId = document.getElementById('penEmp').value;
    const type = document.getElementById('penType').value;
    const reason = document.getElementById('penReason').value.trim();
    if(!empId || !reason){ UI.error='اختر الموظف واكتب سبب الجزاء'; render(); return; }
    const emp = personById(empId);
    const applier = UI.adminAuthed ? { name: DB.adminConfig.name } : UI.currentPerson;
    DB.penalties.push({
      id: uid(), empId, empName: emp.name, departmentId: emp.departmentId, type, reason,
      appliedByName: applier.name, createdAt: new Date().toISOString()
    });
    await savePenalties(); await audit('تسجيل جزاء',`تم تسجيل جزاء على ${emp.name}`); UI.error=''; render();
  };

  if(isAdmin){
    document.querySelectorAll('[data-delete-penalty]').forEach(b=>{
      b.onclick = async ()=>{
        const penId = b.dataset.deletePenalty;
        const pen = DB.penalties.find(p=>p.id===penId);
        if(!pen || !confirm(`هل أنت متأكد من حذف هذا الجزاء؟`)) return;
        DB.penalties = DB.penalties.filter(p=>p.id!==penId);
        await savePenalties();
        render();
      };
    });
  }
}

function renderManagerDashboard(){
  const mgr = UI.currentPerson;
  const pending = DB.requests.filter(r=>r.departmentId===mgr.departmentId && r.status==='pending').length;
  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`مدير قسم ${esc(deptName(mgr.departmentId))} — ${esc(mgr.name)}`, 'mgr')}
    <div class="tabs">
      <button class="tab-btn ${UI.mgrTab==='people'?'active':''}" data-mtab="people">موظفين قسمي</button>
      <button class="tab-btn ${UI.mgrTab==='requests'?'active':''}" data-mtab="requests">${pending>0?`<span class="tab-badge">${pending}</span>`:''}الطلبات</button>
      <button class="tab-btn ${UI.mgrTab==='penalties'?'active':''}" data-mtab="penalties">الجزاءات</button>
      <button class="tab-btn ${UI.mgrTab==='attendance'?'active':''}" data-mtab="attendance">الحضور اليومي</button>
      <button class="tab-btn ${UI.mgrTab==='reports'?'active':''}" data-mtab="reports">التقرير الشهري</button>
    </div>
    <div class="content">
      ${UI.mgrTab==='people' ? renderPeopleTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='requests' ? renderRequestsTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='penalties' ? renderPenaltiesTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='attendance' ? renderAttendanceTab(false, mgr.departmentId) : ''}
      ${UI.mgrTab==='reports' ? renderReportsTab(false, mgr.departmentId) : ''}
    </div>
  </div>`;
}
function attachManagerDashboardEvents(){
  attachTopbarSearchEvents();
  attachLogout();
  const mgr = UI.currentPerson;
  document.querySelectorAll('.tab-btn').forEach(b=> b.onclick = ()=>{ UI.mgrTab=b.dataset.mtab; UI.error=''; render(); });
  if(UI.mgrTab==='people') attachPeopleFormEvents(false, mgr.departmentId);
  if(UI.mgrTab==='requests') attachRequestsEvents(false, mgr.departmentId);
  if(UI.mgrTab==='penalties') attachPenaltiesEvents(false, mgr.departmentId);
  if(UI.mgrTab==='attendance'){ document.getElementById('attDatePick').onchange = e=>{ UI.attDate=e.target.value; render(); }; }
  if(UI.mgrTab==='reports') attachReportsEvents(false, mgr.departmentId);
}

function renderEmployeeDashboard(){
  const emp = UI.currentPerson;
  const date = todayStr();
  const att = DB.attendance[emp.id+'_'+date] || {};
  const myRequests = DB.requests.filter(r=>r.empId===emp.id).slice().sort((a,b)=>b.createdAt.localeCompare(a.createdAt));
  const myPenalties = DB.penalties.filter(pn=>pn.empId===emp.id).slice().sort((a,b)=>b.createdAt.localeCompare(a.createdAt));
  const annualBal = getLeaveBalance(emp, 'annual_leave');
  const casualBal = getLeaveBalance(emp, 'casual_leave');

  const historyRows = myRequests.map(r=>{
    const stampClass = r.status==='approved'?'stamp-approved':r.status==='rejected'?'stamp-rejected':'stamp-pending';
    const stampText = r.status==='approved'?'معتمد':r.status==='rejected'?'مرفوض':'قيد المراجعة';
    const info = REQUEST_TYPES[r.type] || { label:r.type };
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(info.label)} — ${requestDateLabel(r)}</div><div class="meta">${esc(r.reason)}</div></div><div class="row-actions"><span class="stamp ${stampClass}">${stampText}</span></div></div>`;
  }).join('');

  const penaltyRows = myPenalties.map(pn=>{
    const info = PENALTY_TYPES[pn.type] || { label: pn.type };
    return `<div class="ledger-row"><div class="row-main"><div class="name">${esc(info.label)}</div><div class="meta">${esc(pn.reason)} · ${fmtDate(pn.createdAt.slice(0,10))}</div></div></div>`;
  }).join('');

  return `<div style="display:flex; flex-direction:column; min-height:100vh;">
    ${topbar(`${emp.name} — ${emp.jobTitle||'موظف'}`, '')}
    <div class="content">
      <div class="dept-banner">
        <div class="item"><div class="label">القسم التابع له</div><div class="val">${esc(deptName(emp.departmentId))}</div></div>
        <div class="item"><div class="label">الكود الوظيفي</div><div class="val">${emp.code || 'لم يُحدد بعد'}</div></div>
        <div class="item"><div class="label">رصيد الإجازات</div><div class="val" style="font-size:13px; font-weight:700;">سنوي: ${annualBal.isSet?`${annualBal.remaining}/${annualBal.allocated}`:'لم يحدد'} | عارضة: ${casualBal.isSet?`${casualBal.remaining}/${casualBal.allocated}`:'لم يحدد'}</div></div>
      </div>

      <div class="today-card">
        <div class="date">${fmtDate(date)}</div>
        <div class="times">
          <div class="time-block"><strong>${att.checkIn || '—'}</strong><span>وقت الحضور</span></div>
          <div class="time-block"><strong>${att.checkOut || '—'}</strong><span>وقت الانصراف</span></div>
        </div>
        ${!att.checkIn ? `<button class="btn btn-primary" id="checkInBtn">تسجيل الحضور الآن</button>`
          : !att.checkOut ? `<button class="btn btn-outline" id="checkOutBtn">تسجيل الانصراف</button>`
          : `<div class="info-msg" style="max-width:280px; margin:0 auto;">تم تسجيل حضورك وانصرافك اليوم</div>`}
      </div>

      <div class="form-card">
        <h3>تقديم طلب إذن / إجازة</h3>
        <div class="form-row">
          <div class="field"><label>نوع الطلب</label><select id="reqType">${Object.entries(REQUEST_TYPES).map(([k,v])=>`<option value="${k}">${v.label}</option>`).join('')}</select></div>
          <div class="field"><label>التاريخ</label><input id="reqDate" type="date" value="${date}"></div>
          <div class="field" id="endDateWrap" style="display:none;"><label>إلى تاريخ</label><input id="reqEndDate" type="date" value="${date}"></div>
        </div>
        <div class="field"><label>السبب</label><textarea id="reqReason" placeholder="اكتب سبب الطلب"></textarea></div>
        ${UI.error?`<div class="error-msg">${esc(UI.error)}</div>`:''}
        <button class="btn btn-primary btn-block" id="submitReqBtn">إرسال الطلب وحفظه</button>
      </div>

      <div class="section-head"><h2>طلباتي السابقة</h2><span class="count">${myRequests.length}</span></div>
      <div class="ledger" style="margin-bottom:20px;">${historyRows || `<div class="empty-illustration">لم تقدّم أي طلبات بعد</div>`}</div>

      <div class="section-head"><h2>الجزاءات المسجّلة عليّ</h2><span class="count">${myPenalties.length}</span></div>
      <div class="ledger">${penaltyRows || `<div class="empty-illustration">لا توجد جزاءات مسجّلة</div>`}</div>
    </div>
  </div>`;
}

function attachEmployeeDashboardEvents(){
  attachLogout();
  const emp = UI.currentPerson;
  const date = todayStr();

  const inBtn = document.getElementById('checkInBtn');
  if(inBtn) inBtn.onclick = async ()=>{
    const key = emp.id+'_'+date;
    DB.attendance[key] = { ...(DB.attendance[key]||{}), checkIn: nowTime() };
    await saveAttendance(); render();
  };
  const outBtn = document.getElementById('checkOutBtn');
  if(outBtn) outBtn.onclick = async ()=>{
    const key = emp.id+'_'+date;
    DB.attendance[key] = { ...(DB.attendance[key]||{}), checkOut: nowTime() };
    await saveAttendance(); render();
  };

  const typeSel = document.getElementById('reqType');
  if(typeSel){
    typeSel.onchange = ()=>{
      const ew = document.getElementById('endDateWrap');
      if(ew) ew.style.display = LEAVE_TYPES[typeSel.value] ? 'block' : 'none';
    };
  }

  const submitBtn = document.getElementById('submitReqBtn');
  if(submitBtn) submitBtn.onclick = async ()=>{
    const type = document.getElementById('reqType').value;
    const rdate = document.getElementById('reqDate').value;
    const endDate = document.getElementById('reqEndDate')?.value || rdate;
    const reason = document.getElementById('reqReason').value.trim();

    if(!rdate || !reason){ UI.error='أكمل التاريخ والسبب'; render(); return; }

    const durationDays = LEAVE_TYPES[type] ? calcDaysInclusive(rdate, endDate) : 1;
    const newReq = {
      id: uid(), empId: emp.id, empName: emp.name, departmentId: emp.departmentId,
      type, date: rdate, startDate: rdate, endDate, durationDays, reason,
      status:'pending', createdAt:new Date().toISOString()
    };

    DB.requests.push(newReq);
    await saveRequests();
    UI.error = '';
    render();
  };
}

// Start immediately
loadAll();
</script>
</body>
</html>
