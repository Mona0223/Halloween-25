<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Mona’s Halloween Movie Trivia — Blood & Score Edition</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0a0a0f; --card:#12121a; --text:#f5f6fb;
    --accent:#ff7a00;        /* candle orange */
    --slime:#40ff8c;         /* toxic green for “correct” */
    --violet:#6c4cff;        /* moonlit violet */
    --line:rgba(255,255,255,.12);
    --shadow:0 10px 30px rgba(0,0,0,.45);
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    background:
      radial-gradient(1200px 500px at 50% -20%, #151523, transparent),
      radial-gradient(800px 400px at 120% 10%, #1b1330, transparent),
      var(--bg);
    color:var(--text);
    font-family:Montserrat,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;
    line-height:1.5; overflow-x:hidden;
  }

  /* Atmospherics */
  .fog{position:fixed;inset:-10% -10% auto -10%;height:60vh;pointer-events:none;mix-blend-lighten;
       background:radial-gradient(60% 40% at 50% 50%, rgba(255,255,255,.06), transparent 70%),
                  radial-gradient(50% 35% at 20% 60%, rgba(255,255,255,.05), transparent 70%),
                  radial-gradient(55% 35% at 80% 45%, rgba(255,255,255,.05), transparent 70%);
       filter:blur(2px); animation:fogdrift 28s linear infinite;}
  @keyframes fogdrift{0%{transform:translateX(0)}50%{transform:translateX(2%)}100%{transform:translateX(0)}}

  .bat{
    position:fixed; top:12vh; left:-10vw; width:50px; height:20px; transform:scale(.9);
    background:linear-gradient(90deg,#000 30%,transparent 0),linear-gradient(-90deg,#000 30%,transparent 0);
    background-size:50% 100%; background-repeat:no-repeat; border-radius:50%;
    filter:drop-shadow(0 0 6px rgba(0,0,0,.6));
    animation:fly 12s linear infinite, flap .4s ease-in-out infinite; opacity:.8;
  }
  .bat::before,.bat::after{content:""; position:absolute; top:50%; width:14px; height:14px; background:#000; border-radius:50%; transform:translateY(-50%)}
  .bat::before{left:10px} .bat::after{right:10px}
  .bat.b2{top:22vh; animation-duration:16s; animation-delay:-4s; transform:scale(.7)}
  .bat.b3{top:35vh; animation-duration:20s; animation-delay:-7s; transform:scale(1.1)}
  @keyframes flap{0%,100%{transform:translateY(0) scale(var(--s,1))}50%{transform:translateY(-3px) scale(var(--s,1))}}
  @keyframes fly{0%{left:-12vw}100%{left:112vw}}

  .web{position:fixed; right:0; top:0; width:220px; height:220px; opacity:.18; pointer-events:none}

  /* Blood splatter canvas */
  #blood{
    position:fixed; inset:0; pointer-events:none; z-index:0; opacity:.85;
    mix-blend-mode:normal; /* keeps red vivid */
  }

  .page{position:relative; z-index:1; max-width:980px; margin:0 auto; padding:20px 16px 80px}
  header.brand{
    display:flex; justify-content:space-between; align-items:center; gap:12px; padding:10px 0;
    border-bottom:1px solid var(--line);
  }
  .title{font-weight:800; letter-spacing:.3px}
  .sig{color:var(--accent); font-weight:700}

  .bar{
    display:flex; align-items:center; justify-content:space-between; gap:12px; margin-top:12px;
    padding:12px 14px; background:linear-gradient(180deg,#161625,#101018); border:1px solid var(--line);
    border-radius:14px; box-shadow:var(--shadow);
  }
  .score{font-weight:800; color:var(--slime)}
  .reset{appearance:none; border:none; border-radius:999px; padding:8px 12px; font-weight:700; cursor:pointer;
         color:var(--accent); background:transparent; border:1px solid rgba(255,122,0,.5)}

  .quiz{
    margin:16px 0 0; background:linear-gradient(180deg,#12121a,#0f0f16); border:1px solid var(--line);
    border-radius:16px; box-shadow:var(--shadow); padding:18px;
  }
  .quiz h2{margin:0 0 8px; color:var(--violet); text-shadow:0 0 14px rgba(108,76,255,.25)}
  .sub{opacity:.85; margin:0 0 8px}
  .qa{display:grid; gap:14px; margin-top:12px}
  .card{background:#0e0e15; border:1px solid var(--line); border-radius:14px; padding:14px}
  .qrow{display:flex; justify-content:space-between; align-items:flex-start; gap:10px}
  .q{font-weight:600}
  .sound{
    display:inline-flex; align-items:center; gap:8px; cursor:pointer; color:var(--accent); text-decoration:none;
    position:relative; padding:6px 10px; border:1px solid rgba(255,122,0,.45); border-radius:999px;
    box-shadow:inset 0 0 0 1px rgba(255,122,0,.2);
  }
  .sound::after{content:""; position:absolute; inset:-3px; border-radius:inherit; box-shadow:0 0 12px 2px rgba(255,122,0,.35); opacity:0; transition:opacity .25s}
  .sound:hover::after{opacity:.9}

  .opt{display:grid; gap:8px; margin:10px 0 0}
  .opt label{
    display:flex; gap:10px; align-items:flex-start; background:#0b0b12; border:1px solid rgba(255,255,255,.08);
    padding:10px 12px; border-radius:10px; cursor:pointer;
  }
  .opt input{margin-top:3px}
  .ctrls{display:flex; gap:10px; flex-wrap:wrap; margin-top:10px}
  .btn{
    appearance:none; border:none; border-radius:999px; padding:10px 14px; font-weight:700; cursor:pointer;
    color:#0b0b0f; background:linear-gradient(180deg,var(--slime), #1be173);
    box-shadow:0 6px 18px rgba(64,255,140,.25);
  }
  .result{margin-top:8px; font-weight:700}
  .correct{color:var(--slime)}
  .wrong{color:#ff4d6d}
  footer{margin-top:22px; padding-top:16px; border-top:1px solid var(--line); display:flex; justify-content:space-between; align-items:center; gap:10px}
  .foot-sig{color:var(--accent); font-weight:800; letter-spacing:.3px}

  /* Print */
  @page{size:8.5in 11in; margin:.6in}
  @media print{ .bat,.fog,.web,#blood{display:none !important} }
</style>
</head>
<body>
<!-- Blood splatter canvas -->
<canvas id="blood"></canvas>

<!-- Atmospherics -->
<div class="fog" aria-hidden="true"></div>
<div class="bat" style="--s:0.9" aria-hidden="true"></div>
<div class="bat b2" style="--s:0.7" aria-hidden="true"></div>
<div class="bat b3" style="--s:1.1" aria-hidden="true"></div>
<svg class="web" viewBox="0 0 200 200" aria-hidden="true">
  <g fill="none" stroke="#ffffff" stroke-opacity=".6" stroke-width=".8">
    <path d="M200,0 L0,200" />
    <path d="M200,0 L60,200" />
    <path d="M200,0 L120,200" />
    <path d="M200,0 L180,200" />
    <path d="M200,0 C140,40 100,80 80,120 C60,160 40,180 0,200" />
    <path d="M200,0 C160,30 130,70 110,110 C90,150 60,180 0,200" />
  </g>
</svg>

<div class="page">
  <header class="brand">
    <div class="title">💀Halloween Trivia💀 — <span style="color:var(--accent)">Multiple Choice</span>
  <div class="bar">
    <div>Answer all 5.</div>
    <div class="score" id="score">Score: 0/5</div>
    <button class="reset" id="resetBtn">Reset Quiz</button>
  </div>

  <section class="quiz" id="quiz">
    <h2>🩸Do You Want To Play A Game!🩸— 5 Questions</h2>
    <p class="sub">Choose an option, then **Check Answer**. Correct answers add to your score. You can retry until you nail it — but you only score once per question.</p>
    <div class="qa" id="qa"></div>
  </section>

  <footer>
    <div class="foot-sig">Mona’s Productivity Suite — Halloween Edition</div>
    <div style="opacity:.75">Print → Save as PDF (Background graphics on) for a slick host copy.</div>
  </footer>
</div>

<!-- Audio -->
<audio id="audio-reveal" preload="none" crossorigin="anonymous"></audio>
<audio id="audio-whisper" preload="none" crossorigin="anonymous"></audio>

<script>
/* ========= CONFIG ========= */
/* Direct-stream Google Drive links */
const URL_REVEAL  = "https://drive.google.com/uc?export=download&id=1yk0VwCqfEkG-EuDHI54sd_y0tAaqutdL";
const URL_WHISPER = "https://drive.google.com/uc?export=download&id=1ogEMxFGAcKcJAC4iI2lPB0J0bMnkiUoi";

/* ========= QUIZ DATA ========= */
const quiz = [
  {
    q:"In *Halloween* (1978), what was Michael Myers’ iconic mask originally made from?",
    choices:["A blank mannequin mold","A William Shatner mask","A clown mask from a thrift store","A custom latex cast"],
    correct:1,
    explain:"It was a spray-painted Captain Kirk (William Shatner) mask, bought cheap and altered."
  },
  {
    q:"In *The Shining* (1980), what reads on Jack’s typewritten pages?",
    choices:["“All work and no play makes Jack a dull boy”","“REDRUM” repeated","“Come play with us”","“Here’s Johnny!” on loop"],
    correct:0,
    explain:"Stacks of pages repeat “All work and no play makes Jack a dull boy.”"
  },
  {
    q:"In *The Exorcist* (1973), what’s the possessed child’s name?",
    choices:["Carrie","Regan","Carol Anne","Samara"],
    correct:1,
    explain:"Regan MacNeil."
  },
  {
    q:"In *Scream* (1996), the killer’s phone voice asks the opening victim about which movie?",
    choices:["*A Nightmare on Elm Street*","*Friday the 13th*","*Halloween*","*Psycho*"],
    correct:0,
    explain:"Ghostface goes meta about *A Nightmare on Elm Street*."
  },
  {
    q:"In *The Nightmare Before Christmas* (1993), Jack Skellington is the Pumpkin King of which town?",
    choices:["Spookyville","Halloweentown","Graveglade","Nightshade Hollow"],
    correct:1,
    explain:"Halloweentown."
  }
];

/* ========= RENDER ========= */
const qa = document.getElementById('qa');
const scoreEl = document.getElementById('score');
const resetBtn = document.getElementById('resetBtn');

let score = 0;
let scored = Array(quiz.length).fill(false); // prevent double-scoring

function updateScore(){ scoreEl.textContent = `Score: ${score}/${quiz.length}`; }

function renderQuiz(){
  qa.innerHTML = quiz.map((item,idx)=>{
    const opts = item.choices.map((txt,i)=>`
      <label>
        <input type="radio" name="q${idx}" value="${i}"/>
        <span>${String.fromCharCode(65+i)}. ${txt}</span>
      </label>
    `).join('');
    return `
      <div class="card" id="q-${idx}">
        <div class="qrow">
          <div class="q">${idx+1}) ${item.q}</div>
          <a class="sound" data-sound="reveal" role="button" aria-label="Play chime"><span>🎧</span></a>
        </div>
        <div class="opt">${opts}</div>
        <div class="ctrls">
          <button class="btn" data-check="${idx}">Check Answer</button>
        </div>
        <div class="result" id="res-${idx}"></div>
      </div>
    `;
  }).join('');
}

/* ========= AUDIO: gesture-anywhere unlock + ambient ========= */
const audioReveal  = document.getElementById('audio-reveal');
const audioWhisper = document.getElementById('audio-whisper');
audioReveal.src = URL_REVEAL;
audioWhisper.src = URL_WHISPER;
audioWhisper.loop = true; audioWhisper.volume = 0.0;
(async()=>{ try{ await audioWhisper.play(); }catch(_){} })(); // desktop may autoplay

let unlocked = false;
function unlockAudio(){
  if(unlocked) return; unlocked = true;
  audioWhisper.play().catch(()=>{});
  let v = 0, target = .22;
  const step = ()=>{ v = Math.min(target, v+.02); audioWhisper.volume = v; if(v<target) requestAnimationFrame(step); };
  requestAnimationFrame(step);
}
['pointerdown','pointermove','touchstart','keydown','wheel','scroll'].forEach(evt=>{
  window.addEventListener(evt, unlockAudio, {once:true, passive:true});
});

/* ========= INTERACTIONS ========= */
function bindInteractions(){
  document.querySelectorAll('.sound').forEach(btn=>{
    btn.addEventListener('click', e=>{
      e.preventDefault(); unlockAudio();
      audioReveal.currentTime = 0; audioReveal.play().catch(()=>{});
    });
  });

  document.querySelectorAll('[data-check]').forEach(btn=>{
    btn.addEventListener('click', e=>{
      unlockAudio();
      const idx = +btn.getAttribute('data-check');
      const sel = document.querySelector(`input[name="q${idx}"]:checked`);
      const res = document.getElementById(`res-${idx}`);
      if(!sel){ res.innerHTML = `<span class="wrong">Select an option.</span>`; return; }

      const val = +sel.value;
      const isCorrect = (val === quiz[idx].correct);

      if(isCorrect){
        res.innerHTML = `<span class="correct">Correct!</span> ${quiz[idx].explain ? `<span style="opacity:.85"> ${quiz[idx].explain}</span>` : ""}`;
        // lock options
        document.querySelectorAll(`input[name="q${idx}"]`).forEach(r=>r.disabled=true);
        // score once
        if(!scored[idx]){ scored[idx] = true; score++; updateScore(); }
        audioReveal.currentTime = 0; audioReveal.play().catch(()=>{});
      } else {
        res.innerHTML = `<span class="wrong">Not quite. Try again.</span>`;
      }
    });
  });

  resetBtn.addEventListener('click',()=>{
    score = 0; scored = Array(quiz.length).fill(false); updateScore();
    renderQuiz(); bindInteractions();
    window.scrollTo({top:0, behavior:'smooth'});
  });
}

/* ========= BLOOD SPLATTER ========= */
const blood = document.getElementById('blood');
const ctx = blood.getContext('2d');

function resizeCanvas(){
  const dpr = Math.max(1, window.devicePixelRatio || 1);
  blood.width  = Math.floor(window.innerWidth  * dpr);
  blood.height = Math.floor(window.innerHeight * dpr);
  blood.style.width  = window.innerWidth + 'px';
  blood.style.height = window.innerHeight + 'px';
  ctx.setTransform(dpr,0,0,dpr,0,0);
}
function drawSplatter(){
  ctx.clearRect(0,0,window.innerWidth,window.innerHeight);
  const W = window.innerWidth, H = window.innerHeight;
  const splats = Math.floor(6 + Math.random()*6); // 6–12 splats
  for(let s=0; s<splats; s++){
    const x = Math.random()*W, y = Math.random()*H*0.8 + 10; // avoid very bottom
    const drops = 10 + Math.floor(Math.random()*20);
    const baseR = 6 + Math.random()*18;
    // central blot
    blob(x,y, baseR, rgbaRed(0.95));
    // surrounding dots
    for(let i=0;i<drops;i++){
      const ang = Math.random()*Math.PI*2;
      const dist = baseR + Math.random()*46;
      const r = Math.max(1, baseR*0.25*Math.random());
      blob(x + Math.cos(ang)*dist, y + Math.sin(ang)*dist, r, rgbaRed(0.8));
    }
    // drips
    const drips = 1 + Math.floor(Math.random()*3);
    for(let d=0; d<drips; d++){
      const len = 20 + Math.random()*60;
      const dx = (Math.random()-.5)*8;
      drip(x+dx, y+baseR*0.6, len);
    }
  }
}
function rgbaRed(a){ return `rgba(180,0,10,${a})`; }
function blob(x,y,r,color){
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.ellipse(x,y,r*(1+Math.random()*0.4), r*(0.8+Math.random()*0.5), Math.random()*Math.PI, 0, Math.PI*2);
  ctx.fill();
}
function drip(x,y,len){
  ctx.strokeStyle = rgbaRed(0.85);
  ctx.lineWidth = 2 + Math.random()*2;
  ctx.lineCap = 'round';
  ctx.beginPath();
  ctx.moveTo(x,y);
  ctx.quadraticCurveTo(x + (Math.random()-.5)*10, y + len*0.6, x + (Math.random()-.5)*6, y+len);
  ctx.stroke();
  // drop at end
  blob(x + (Math.random()-.5)*6, y+len, 3+Math.random()*3, rgbaRed(0.9));
}

/* Redraw on resize; add a tiny jitter every 20s for randomness */
let bloodTimer;
function refreshBlood(){
  resizeCanvas(); drawSplatter();
  clearInterval(bloodTimer);
  bloodTimer = setInterval(()=>{ drawSplatter(); }, 20000);
}
window.addEventListener('resize', refreshBlood);

/* ========= INIT ========= */
updateScore();
renderQuiz();
bindInteractions();
refreshBlood();
</script>
