[index.html](https://github.com/user-attachments/files/29157468/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>创伤分诊评分卡 · Trauma Triage Scorecard</title>
<style>
  :root{
    --ink:#15191F; --muted:#5C6672; --faint:#8A929C;
    --paper:#F4F6F8; --card:#FFFFFF; --line:#E4E8ED; --line-2:#EEF1F4;
    --primary:#1C4E6B; --primary-soft:#E9F0F4;
    --go:#1B7A45; --go-soft:#E7F3EC;
    --caution:#9A5B0B; --caution-soft:#FBF0DF;
    --alert:#B5231C; --alert-soft:#FBE8E6;
    --shadow:0 1px 2px rgba(20,30,45,.05),0 8px 24px rgba(20,30,45,.06);
    --mono:"SF Mono",ui-monospace,"DejaVu Sans Mono","Roboto Mono",Menlo,Consolas,monospace;
    --sans:system-ui,-apple-system,"Segoe UI","PingFang SC","Microsoft YaHei","Hiragino Sans GB",Roboto,Helvetica,Arial,sans-serif;
    --r:14px;
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{margin:0}
  body{
    background:var(--paper);color:var(--ink);font-family:var(--sans);
    font-size:16px;line-height:1.5;-webkit-font-smoothing:antialiased;
    padding:0 0 env(safe-area-inset-bottom);
  }
  .wrap{max-width:680px;margin:0 auto;padding:0 18px}

  /* ---------- header ---------- */
  header{padding:26px 0 18px;border-bottom:1px solid var(--line)}
  .topline{display:flex;align-items:flex-start;justify-content:space-between;gap:14px}
  .eyebrow{font-size:11.5px;font-weight:600;letter-spacing:.14em;text-transform:uppercase;color:var(--primary);margin:0 0 7px}
  h1{font-size:24px;line-height:1.18;font-weight:700;letter-spacing:-.01em;margin:0}
  h1 .en{display:block;font-size:14px;font-weight:600;color:var(--muted);letter-spacing:0;margin-top:4px}
  .purpose{font-size:13.5px;color:var(--muted);margin:11px 0 0;max-width:46ch}
  .lang{display:inline-flex;border:1px solid var(--line);border-radius:999px;overflow:hidden;flex:none;background:var(--card)}
  .lang button{font-family:inherit;font-size:12.5px;font-weight:600;border:0;background:transparent;color:var(--muted);padding:7px 13px;cursor:pointer}
  .lang button[aria-pressed="true"]{background:var(--primary);color:#fff}

  /* ---------- inputs ---------- */
  .section-label{display:flex;align-items:baseline;justify-content:space-between;margin:24px 2px 2px}
  .section-label .t{font-size:12px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--faint)}
  .progress{font-size:12px;font-weight:600;color:var(--faint);font-variant-numeric:tabular-nums}

  .field{background:var(--card);border:1px solid var(--line);border-radius:var(--r);padding:13px 14px 14px;margin-top:11px;box-shadow:var(--shadow)}
  .field.pending{border-color:#D9DEE4;border-left:3px solid #CFD6DE}
  .field.set{border-left:3px solid var(--go)}
  .field-head{display:flex;align-items:baseline;justify-content:space-between;gap:10px;margin-bottom:11px}
  .field-name{font-size:15.5px;font-weight:650}
  .field-unit{font-size:12.5px;color:var(--faint);font-weight:500}
  .opts{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
  .opts.g5{grid-template-columns:repeat(5,1fr)}
  .opt{
    font-family:inherit;cursor:pointer;border:1px solid var(--line);background:#fff;
    border-radius:10px;padding:10px 6px;text-align:center;color:var(--ink);
    transition:background .12s,border-color .12s,box-shadow .12s,color .12s;min-height:54px;
    display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;
  }
  .opt:hover{border-color:#BCC6D0}
  .opt:focus-visible{outline:2px solid var(--primary);outline-offset:2px}
  .opt .v{font-size:16px;font-weight:680;font-variant-numeric:tabular-nums;line-height:1.05}
  .opt .s{font-size:10.5px;color:var(--faint);font-weight:500}
  .opt[aria-pressed="true"]{background:var(--primary);border-color:var(--primary);color:#fff;box-shadow:0 2px 8px rgba(28,78,107,.28)}
  .opt[aria-pressed="true"] .s{color:rgba(255,255,255,.82)}

  /* ---------- results ---------- */
  .results{margin-top:24px;display:grid;gap:13px}
  .tier{border-radius:var(--r);border:1px solid var(--line);background:var(--card);overflow:hidden;box-shadow:var(--shadow);position:relative}
  .tier-top{display:flex;align-items:stretch}
  .readout{flex:none;width:118px;padding:16px 0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;border-right:1px solid var(--line-2)}
  .readout .score{font-family:var(--mono);font-size:46px;font-weight:600;line-height:.95;letter-spacing:-.02em;font-variant-numeric:tabular-nums}
  .readout .max{font-size:11px;color:var(--faint);font-weight:600;letter-spacing:.02em}
  .readout .waiting{font-size:13px;color:var(--faint);font-weight:600;text-align:center;padding:0 8px;line-height:1.35}
  .tier-body{flex:1;min-width:0;padding:14px 15px;display:flex;flex-direction:column;justify-content:center;gap:7px}
  .tier-name{font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--faint)}
  .tier-name b{color:var(--primary)}
  .badge{display:inline-flex;align-items:center;gap:7px;font-size:14.5px;font-weight:700;border-radius:8px;padding:7px 11px;align-self:flex-start;line-height:1.2}
  .badge .dot{width:9px;height:9px;border-radius:50%;flex:none}
  .badge.go{background:var(--go-soft);color:var(--go)} .badge.go .dot{background:var(--go)}
  .badge.caution{background:var(--caution-soft);color:var(--caution)} .badge.caution .dot{background:var(--caution)}
  .badge.alert{background:var(--alert-soft);color:var(--alert)} .badge.alert .dot{background:var(--alert)}
  .badge.idle{background:#F0F2F5;color:var(--faint)} .badge.idle .dot{background:var(--faint)}
  .risk{font-size:13px;color:var(--muted)}
  .risk b{color:var(--ink);font-variant-numeric:tabular-nums;font-weight:700}
  .action{font-size:13.5px;line-height:1.45;color:var(--ink);padding:12px 15px;border-top:1px solid var(--line-2);background:#FCFDFE}
  .tier.is-alert{border-color:#E7B8B4}
  .tier.is-alert .readout{background:var(--alert-soft)}
  .tier.is-alert .readout .score{color:var(--alert)}
  .tier.is-caution .readout{background:var(--caution-soft)}
  .tier.is-caution .readout .score{color:var(--caution)}
  .tier.is-go .readout .score{color:var(--go)}

  /* threshold meter for tier 1 */
  .meter{height:7px;border-radius:4px;background:linear-gradient(90deg,var(--go) 0%,var(--go) 14%,#E7E3C8 14%,#E7E3C8 19%,var(--alert) 19%,var(--alert) 100%);position:relative;margin:2px 0 1px}
  .meter .pin{position:absolute;top:-4px;width:3px;height:15px;border-radius:2px;background:var(--ink);transform:translateX(-50%);box-shadow:0 0 0 2px #fff;transition:left .18s ease}
  .meter-cap{display:flex;justify-content:space-between;font-size:10.5px;color:var(--faint);font-weight:600;margin-top:5px;font-variant-numeric:tabular-nums}

  /* ---------- actions ---------- */
  .controls{display:flex;gap:10px;margin-top:16px}
  .controls button{flex:1;font-family:inherit;font-size:14px;font-weight:650;padding:13px;border-radius:11px;cursor:pointer;border:1px solid var(--line);background:#fff;color:var(--ink)}
  .controls button:hover{border-color:#BCC6D0}
  .controls button.ghost{color:var(--muted)}
  .controls .ok{background:#E7F3EC;border-color:#BFE0CB;color:var(--go);display:none}
  .controls .ok.show{display:block}

  /* ---------- about ---------- */
  details{margin:18px 0 30px;border-top:1px solid var(--line);padding-top:6px}
  summary{cursor:pointer;font-size:13px;font-weight:650;color:var(--primary);padding:10px 2px;list-style:none}
  summary::-webkit-details-marker{display:none}
  summary::before{content:"＋ ";font-weight:700}
  details[open] summary::before{content:"－ "}
  .about{font-size:12.5px;line-height:1.6;color:var(--muted);padding:4px 2px 8px}
  .about p{margin:0 0 10px}
  .about b{color:var(--ink)}
  .about .warn{background:#FBF0DF;border-left:3px solid var(--caution);border-radius:6px;padding:10px 12px;color:#5e4413}
  .about table{width:100%;border-collapse:collapse;margin:6px 0 12px;font-size:11.5px;font-variant-numeric:tabular-nums}
  .about th,.about td{border:1px solid var(--line);padding:5px 7px;text-align:center}
  .about th{background:var(--primary-soft);font-weight:700;color:var(--ink)}
  .foot{font-size:11px;color:var(--faint);text-align:center;padding:0 0 26px;line-height:1.5}
  @media (max-width:430px){
    .readout{width:96px} .readout .score{font-size:40px}
    .opt .s{display:none}
    h1{font-size:21px}
  }
  @media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="topline">
      <div>
        <p class="eyebrow" data-i18n="eyebrow">护士主导 · 床旁 · 无需化验</p>
        <h1><span data-i18n="title">急诊创伤分诊两级评分卡</span><span class="en">Two-tier emergency trauma triage scorecard</span></h1>
      </div>
      <div class="lang" role="group" aria-label="Language">
        <button id="zh" aria-pressed="true">中文</button>
        <button id="en" aria-pressed="false">EN</button>
      </div>
    </div>
    <p class="purpose" data-i18n="purpose">用于已分流至抢救区的成年创伤患者的再分层。仅需六项床旁变量。</p>
  </header>

  <div class="section-label">
    <span class="t" data-i18n="inputs">输入</span>
    <span class="progress" id="progress">0 / 6</span>
  </div>
  <div id="fields"></div>

  <div class="results" id="results"></div>

  <div class="controls">
    <button class="ghost" id="reset" data-i18n="reset">重置</button>
    <button id="copy" data-i18n="copy">复制结果</button>
    <button class="ok" id="copied" data-i18n="copied">已复制 ✓</button>
  </div>

  <details>
    <summary data-i18n="aboutsum">说明 · 校准依据 · 重要声明</summary>
    <div class="about" id="about"></div>
  </details>
  <p class="foot" id="foot"></p>
</div>

<script>
/* ============ scorecard definition (Table 2) ============ */
const VARS=[
 {id:'gcs',g5:true,zh:'GCS 运动反应',en:'GCS motor response',uzh:'最佳运动',uen:'best motor',
  o:[{l:'6',zh:'遵嘱',en:'obeys',t1:0,t2:0},{l:'5',zh:'定位',en:'localiz.',t1:2,t2:2},
     {l:'4',zh:'回缩',en:'withdr.',t1:3,t2:6},{l:'3',zh:'屈曲',en:'flexion',t1:5,t2:8},
     {l:'1–2',zh:'过伸/无',en:'ext./none',t1:9,t2:8}]},
 {id:'age',zh:'年龄',en:'Age',uzh:'岁',uen:'yr',
  o:[{l:'<65',t1:0,t2:0},{l:'65–74',t1:2,t2:1},{l:'≥75',t1:4,t2:1}]},
 {id:'sbp',zh:'收缩压',en:'Systolic BP',uzh:'mmHg',uen:'mmHg',
  o:[{l:'≥110',t1:0,t2:0},{l:'90–109',t1:1,t2:1},{l:'<90',t1:3,t2:2}]},
 {id:'spo2',zh:'血氧 SpO₂',en:'SpO₂',uzh:'%',uen:'%',
  o:[{l:'≥95',t1:0,t2:0},{l:'90–94',t1:1,t2:1},{l:'<90',t1:1,t2:4}]},
 {id:'hr',zh:'心率',en:'Heart rate',uzh:'次/分',uen:'/min',
  o:[{l:'<100',t1:0,t2:0},{l:'100–119',t1:0,t2:1},{l:'≥120',t1:1,t2:2}]},
 {id:'rr',zh:'呼吸频率',en:'Resp. rate',uzh:'次/分',uen:'/min',
  o:[{l:'<21',t1:0,t2:0},{l:'21–29',t1:0,t2:0},{l:'≥30',t1:3,t2:4}]},
];
/* calibrated risk (%) by total score — logistic fit on development cohort (n=1514) */
const RISK1=[0.3,0.5,0.7,1.1,1.8,2.7,4.1,6.3,9.4,13.9,20.0,28.0,37.6,48.4,59.2,69.3,77.8,84.4,89.4,92.9,95.3,96.9];
const RISK2=[11.2,14.7,19.1,24.5,30.7,37.8,45.5,53.4,61.1,68.3,74.7,80.2,84.7,88.4,91.2,93.5,95.1,96.4,97.4,98.1,98.6,99.0];
const CUT1=4; // validated up-triage threshold (external sensitivity 100%)

/* ============ i18n strings ============ */
const T={
 zh:{eyebrow:'护士主导 · 床旁 · 无需化验',title:'急诊创伤分诊两级评分卡',
   purpose:'用于已分流至抢救区的成年创伤患者的再分层。仅需六项床旁变量。',
   inputs:'输入',reset:'重置',copy:'复制结果',copied:'已复制 ✓',aboutsum:'说明 · 校准依据 · 重要声明',
   t1name:'第一级 · <b>72 小时死亡</b>（濒临死亡安全网）',
   t2name:'第二级 · <b>救命性干预需求</b>（24 小时资源准备）',
   waiting:'请填全<br>六项',of:'共 21 分',
   riskL:'预测风险约',
   t1_alert:'达到上调阈值（总分 ≥ 4）',t1_go:'低于上调阈值（总分 < 4）',
   t1act_alert:'<b>立即上调。</b>请即刻请高级/创伤团队复核，给予最高优先级抢救床位。该切点在外部验证中对 72 小时死亡灵敏度 100%。',
   t1act_go:'目前未达上调阈值。这是安全网而非排除依据——临床判断可上调，但不应据此悄然降级。任何疑虑请上调。',
   t2_hi:'高 · 优先备齐救治资源',t2_mod:'中 · 预先准备救治资源',t2_low:'低 · 常规准备',
   t2act:'反映 24 小时内需救命/救器官性干预（气道、输血、外科/介入）的可能性。分值越高，越应<b>前置</b>相应资源与人员。',
   bandzh:['绿','琥珀','红'],idle:'待输入',
   warn:'本工具为决策<b>辅助</b>，不替代床旁护士与医师的判断。标记只应被上调，不应被悄然降级。',
   ab1:'两级评分均由六项床旁变量构成（见下），各项分值相加得总分（每级 0–21）。',
   ab2:'<b>第一级</b>预测到院 72 小时内死亡；总分 <b>≥ 4</b> 触发上调。该切点在时间外部验证（n=410）中对 72 小时死亡灵敏度 100%、特异度 70%、分诊不足 0%，仅标记约三分之一患者。',
   ab3:'<b>第二级</b>预测 24 小时内救命/救器官性侵入性干预（气管插管、胸腔闭式引流、输血，或急诊救命手术/介入）的需求，用于资源准备。',
   ab4:'<b>风险百分比</b>来自开发队列（n=1514）逻辑回归校准，为<b>近似估计</b>。外部验证显示轻度低估（校准斜率约 1.11–1.14），故真实风险可能略高；绝对风险宜结合临床、在本地重新校准后解读。决策应以分值与已验证切点为主，而非绝对百分比。',
   ab5:'模型表现（表观/外部）：第一级 AUROC 0.923 / 0.932；第二级 0.788 / 0.814。',
   tblh:['总分','第一级风险','第二级风险'],
   foot:'分值表见手稿表 2 · 仅供研究与教学用途 · 使用前请在本地验证'},
 en:{eyebrow:'Nurse-led · bedside · no laboratory',title:'Two-tier emergency trauma triage scorecard',
   purpose:'Re-stratifies adult trauma patients already streamed into the resuscitation area. Six bedside variables only.',
   inputs:'Inputs',reset:'Reset',copy:'Copy result',copied:'Copied ✓',aboutsum:'About · calibration · important notice',
   t1name:'Tier 1 · <b>72-hour mortality</b> (imminent-death safety net)',
   t2name:'Tier 2 · <b>need for life-saving intervention</b> (24-h resource readiness)',
   waiting:'Enter all<br>six items',of:'of 21',
   riskL:'approx. predicted risk',
   t1_alert:'Up-triage threshold reached (total ≥ 4)',t1_go:'Below up-triage threshold (total < 4)',
   t1act_alert:'<b>Up-triage now.</b> Trigger immediate senior / trauma-team review and a highest-priority resuscitation bed. At external validation this cut-off had 100% sensitivity for 72-hour death.',
   t1act_go:'Below the up-triage threshold. This is a safety net, not a rule-out — clinical judgement may up-triage, but a flag should never be silently de-escalated. When in doubt, escalate.',
   t2_hi:'High · prioritise resources',t2_mod:'Moderate · prepare resources',t2_low:'Low · routine readiness',
   t2act:'Reflects the likelihood of a life- or organ-saving intervention within 24 h (airway, transfusion, surgery / IR). The higher the score, the more these resources and staff should be <b>readied in advance</b>.',
   bandzh:['green','amber','red'],idle:'Awaiting input',
   warn:'A decision <b>aid</b> — it does not replace the bedside nurse&#39;s or physician&#39;s judgement. Flags may be escalated, never silently de-escalated.',
   ab1:'Each tier is built from six bedside variables (below); points are summed to a total (0–21 per tier).',
   ab2:'<b>Tier 1</b> predicts death within 72 hours; a total <b>≥ 4</b> triggers up-triage. At temporal external validation (n=410) this cut-off had 100% sensitivity and 70% specificity for 72-hour death, 0% under-triage, flagging about one third of patients.',
   ab3:'<b>Tier 2</b> predicts the need for a life- or organ-saving invasive intervention within 24 h (intubation, chest-tube drainage, transfusion, or emergency life-saving surgery / IR), to guide resource readiness.',
   ab4:'<b>Risk percentages</b> are from a logistic calibration on the development cohort (n=1514) and are <b>approximate</b>. External validation showed mild under-prediction (calibration slope ≈ 1.11–1.14), so true risk may be slightly higher; interpret absolute risk in clinical context and recalibrate locally. Base decisions on the score and the validated cut-off, not the absolute percentage.',
   ab5:'Model performance (apparent / external): Tier 1 AUROC 0.923 / 0.932; Tier 2 0.788 / 0.814.',
   tblh:['Total','Tier 1 risk','Tier 2 risk'],
   foot:'Point values per manuscript Table 2 · Research and education use only · Validate locally before use'},
};

let LANG='zh';
const state={gcs:null,age:null,sbp:null,spo2:null,hr:null,rr:null};

/* ---- build inputs ---- */
const fieldsEl=document.getElementById('fields');
function buildFields(){
  fieldsEl.innerHTML='';
  VARS.forEach(v=>{
    const f=document.createElement('div');f.className='field pending';f.id='f-'+v.id;
    const unit=LANG==='zh'?v.uzh:v.uen;
    f.innerHTML=`<div class="field-head"><span class="field-name">${LANG==='zh'?v.zh:v.en}</span>`+
      `<span class="field-unit">${unit||''}</span></div>`+
      `<div class="opts ${v.g5?'g5':''}" role="group"></div>`;
    const og=f.querySelector('.opts');
    v.o.forEach((op,i)=>{
      const b=document.createElement('button');b.className='opt';b.type='button';
      b.setAttribute('aria-pressed','false');
      const sub=LANG==='zh'?op.zh:op.en;
      b.innerHTML=`<span class="v">${op.l}</span>`+(sub?`<span class="s">${sub}</span>`:'');
      b.onclick=()=>{state[v.id]=i;
        og.querySelectorAll('.opt').forEach(x=>x.setAttribute('aria-pressed','false'));
        b.setAttribute('aria-pressed','true');
        f.className='field set';render();};
      og.appendChild(b);
    });
    fieldsEl.appendChild(f);
  });
  // restore selections after rebuild
  VARS.forEach(v=>{if(state[v.id]!=null){
    const f=document.getElementById('f-'+v.id);f.className='field set';
    f.querySelectorAll('.opt')[state[v.id]].setAttribute('aria-pressed','true');}});
}

/* ---- compute + render results ---- */
const resultsEl=document.getElementById('results');
function totals(){
  let t1=0,t2=0,n=0;
  VARS.forEach(v=>{const i=state[v.id];if(i!=null){t1+=v.o[i].t1;t2+=v.o[i].t2;n++;}});
  return{t1,t2,n};
}
function band2(score){return score>=10?2:score>=4?1:0;} // tier-2 readiness banding
function render(){
  const L=T[LANG];const{t1,t2,n}=totals();
  document.getElementById('progress').textContent=`${n} / 6`;
  document.getElementById('copy').style.display=n===6?'block':'block';
  const complete=n===6;

  // Tier 1
  const t1alert=complete&&t1>=CUT1;
  const t1cls=complete?(t1alert?'is-alert':'is-go'):'';
  const pinPct=Math.min(100,(t1/21)*100);
  const r1=complete?RISK1[t1]:null;
  // Tier 2
  const b2=complete?band2(t2):-1;
  const t2cls=complete?(b2===2?'is-alert':b2===1?'is-caution':'is-go'):'';
  const r2=complete?RISK2[t2]:null;
  const t2badge=['go','caution','alert'][b2]||'idle';
  const t2lab=[L.t2_low,L.t2_mod,L.t2_hi][b2]||L.idle;

  resultsEl.innerHTML=
  `<div class="tier ${t1cls}">
     <div class="tier-top">
       <div class="readout">${complete?`<div class="score">${t1}</div><div class="max">/ 21</div>`:`<div class="waiting">${L.waiting}</div>`}</div>
       <div class="tier-body">
         <div class="tier-name">${L.t1name}</div>
         ${complete?
            `<span class="badge ${t1alert?'alert':'go'}"><span class="dot"></span>${t1alert?L.t1_alert:L.t1_go}</span>
             <div class="meter"><span class="pin" style="left:${pinPct}%"></span></div>
             <div class="meter-cap"><span>0</span><span>↑ ${CUT1}</span><span>21</span></div>
             <div class="risk">${L.riskL}：<b>${r1}%</b></div>`
          :`<span class="badge idle"><span class="dot"></span>${L.idle}</span>`}
       </div>
     </div>
     ${complete?`<div class="action">${t1alert?L.t1act_alert:L.t1act_go}</div>`:''}
   </div>
   <div class="tier ${t2cls}">
     <div class="tier-top">
       <div class="readout">${complete?`<div class="score">${t2}</div><div class="max">/ 21</div>`:`<div class="waiting">${L.waiting}</div>`}</div>
       <div class="tier-body">
         <div class="tier-name">${L.t2name}</div>
         <span class="badge ${t2badge}"><span class="dot"></span>${t2lab}</span>
         ${complete?`<div class="risk">${L.riskL}：<b>${r2}%</b></div>`:''}
       </div>
     </div>
     ${complete?`<div class="action">${L.t2act}</div>`:''}
   </div>`;
}

/* ---- about + foot ---- */
function renderAbout(){
  const L=T[LANG];
  let rows='';
  [0,2,4,6,8,10,12,14,16,18,21].forEach(s=>{rows+=`<tr><td>${s}</td><td>${RISK1[s]}%</td><td>${RISK2[s]}%</td></tr>`;});
  document.getElementById('about').innerHTML=
    `<p class="warn">${L.warn}</p>
     <p>${L.ab1}</p><p>${L.ab2}</p><p>${L.ab3}</p><p>${L.ab4}</p>
     <table><tr><th>${L.tblh[0]}</th><th>${L.tblh[1]}</th><th>${L.tblh[2]}</th></tr>${rows}</table>
     <p>${L.ab5}</p>`;
  document.getElementById('foot').textContent=L.foot;
}

/* ---- i18n apply ---- */
function applyLang(){
  document.documentElement.lang=LANG==='zh'?'zh-CN':'en';
  document.querySelectorAll('[data-i18n]').forEach(el=>{el.innerHTML=T[LANG][el.getAttribute('data-i18n')];});
  document.getElementById('zh').setAttribute('aria-pressed',LANG==='zh');
  document.getElementById('en').setAttribute('aria-pressed',LANG==='en');
  buildFields();render();renderAbout();
}
document.getElementById('zh').onclick=()=>{LANG='zh';applyLang();};
document.getElementById('en').onclick=()=>{LANG='en';applyLang();};

/* ---- controls ---- */
document.getElementById('reset').onclick=()=>{
  Object.keys(state).forEach(k=>state[k]=null);buildFields();render();
};
document.getElementById('copy').onclick=()=>{
  const L=T[LANG];const{t1,t2,n}=totals();
  if(n<6){return;}
  const strip=s=>s.replace(/<[^>]+>/g,'');
  const t1alert=t1>=CUT1;const b2=band2(t2);
  const lines=[
    LANG==='zh'?'急诊创伤分诊两级评分卡':'Two-tier trauma triage scorecard',
    `${LANG==='zh'?'第一级(72h死亡)':'Tier 1 (72h mortality)'}: ${t1}/21 · ${L.riskL} ${RISK1[t1]}% · ${strip(t1alert?L.t1_alert:L.t1_go)}`,
    `${LANG==='zh'?'第二级(干预需求)':'Tier 2 (intervention)'}: ${t2}/21 · ${L.riskL} ${RISK2[t2]}% · ${strip([L.t2_low,L.t2_mod,L.t2_hi][b2])}`,
  ];
  navigator.clipboard.writeText(lines.join('\n')).then(()=>{
    const ok=document.getElementById('copied');ok.classList.add('show');
    setTimeout(()=>ok.classList.remove('show'),1600);
  });
};

applyLang();
</script>
</body>
</html>
