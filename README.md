<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Para ti 🌼 — Flores Amarillas</title>
<meta name="description" content="Sorpresa interactiva con mensaje, cuenta regresiva y paisaje de flores amarillas que crecen."/>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
  :root{ --bg1:#0b1220; --bg2:#141b2e; --text:#eef2ff; --muted:#9aa4b2; --yellow:#fcd34d; --yellow2:#f59e0b; --rose:#fb7185; }
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;background:linear-gradient(160deg,var(--bg1),var(--bg2));color:var(--text)}
  body{font-family:"Plus Jakarta Sans",system-ui,Segoe UI,Roboto,Arial}
  .wrap{min-height:100%;display:grid;place-items:center;padding:22px}
  .card{width:min(980px,94vw);border-radius:22px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.12);box-shadow:0 20px 70px rgba(0,0,0,.45);overflow:hidden}
  header{display:flex;align-items:center;justify-content:space-between;padding:16px 18px;border-bottom:1px solid rgba(255,255,255,.08)}
  .heart{width:18px;height:18px;background:radial-gradient(circle at 30% 30%,var(--rose),transparent 62%),radial-gradient(circle at 70% 30%,var(--rose),transparent 62%),linear-gradient(45deg,transparent 49%,var(--rose) 49% 51%,transparent 51%),linear-gradient(-45deg,transparent 49%,var(--rose) 49% 51%,transparent 51%);clip-path:polygon(50% 84%, 4% 45%, 11% 14%, 35% 8%, 50% 22%, 65% 8%, 89% 14%, 96% 45%);display:inline-block;margin-right:8px}
  h1{font-family:"Playfair Display",serif;margin:0;font-size:clamp(24px,3.6vw,40px)}
  main{position:relative}
  .screen{display:none}
  .screen.active{display:grid}
  .intro{padding:22px;display:grid;grid-template-columns:1.1fr .9fr;gap:16px}
  @media (max-width:880px){.intro{grid-template-columns:1fr}}
  .panel{background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.1);border-radius:18px;padding:16px}
  .field{display:grid;gap:6px;margin:8px 0}
  label{font-size:12px;color:var(--muted)}
  input,textarea{width:100%;padding:12px;border-radius:12px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.14);color:var(--text);font-size:14px}
  textarea{min-height:104px;resize:vertical}
  .btn{appearance:none;border:0;border-radius:14px;padding:12px 16px;font-weight:800;letter-spacing:.2px;cursor:pointer}
  .btn.primary{background:linear-gradient(180deg,var(--yellow),var(--yellow2));color:#111827;box-shadow:0 10px 28px rgba(245,158,11,.35)}
  .btn.secondary{background:transparent;color:var(--text);border:1px solid rgba(255,255,255,.2)}
  .kbd{font-family:ui-monospace,Menlo,Consolas,monospace;background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);padding:2px 8px;border-radius:8px}
  footer{padding:14px 18px;border-top:1px solid rgba(255,255,255,.08);opacity:.8;font-size:12px}

  /* Countdown overlay */
  .overlay{position:absolute;inset:0;display:none;place-items:center;background:rgba(0,0,0,.6);backdrop-filter:blur(2px);z-index:20}
  .overlay.show{display:grid}
  .count{font-family:"Playfair Display",serif;font-size:min(20vw,180px);line-height:1;animation:pop .9s ease both}
  @keyframes pop{0%{transform:scale(.6);opacity:0}60%{transform:scale(1.1);opacity:1}100%{transform:scale(1)}}
  .hint{margin-top:8px;color:var(--muted);font-size:14px;text-align:center}

  /* Scene reveal */
  .scene{position:relative;height:min(78vh,720px)}
  .sky{position:absolute;inset:0;background:linear-gradient(#87ceeb 0%, #bfe7ff 60%, #e6f6ff 100%)}
  .sun{position:absolute;top:6%;left:8%;width:120px;height:120px;border-radius:50%;background:radial-gradient(circle at 30% 30%, #fff6, #fff0 50%), #fde047;box-shadow:0 0 80px 20px #fde04766}
  .cloud{position:absolute;top:0;width:200px;height:70px;filter:blur(.2px);background:#fff;border-radius:999px}
  .cloud:before,.cloud:after{content:"";position:absolute;background:#fff;border-radius:999px}
  .cloud:before{width:120px;height:120px;left:20px;top:-40px}
  .cloud:after{width:140px;height:140px;left:80px;top:-60px}
  .ground{position:absolute;inset:auto 0 0 0;height:48%;background:linear-gradient(#2f7d32,#1f5f29)}
  .hill{position:absolute;bottom:0;width:60vw;height:45vh;border-top-left-radius:50% 100%;border-top-right-radius:50% 100%;filter:brightness(.95)}
  .hill.a{left:-10vw;background:linear-gradient(#2d7c2f,#276a2a)}
  .hill.b{right:-12vw;background:linear-gradient(#2a6e2c,#225925)}
  canvas#flowers{position:absolute;inset:0}
  .titleBox{position:absolute;left:50%;transform:translateX(-50%);bottom:18px;text-align:center;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);backdrop-filter:blur(6px);padding:10px 14px;border-radius:14px}
  .titleBox h2{margin:0;font-family:"Playfair Display",serif;font-weight:700}
  .spark{position:absolute;width:6px;height:6px;border-radius:50%;background:#fff;box-shadow:0 0 12px #fff}

  .tw{border-right:2px solid var(--yellow);animation:blink .8s step-end infinite;white-space:pre-wrap}
  @keyframes blink{50%{border-color:transparent}}
</style>
</head>
<body>
<div class="wrap">
  <div class="card">
    <header>
      <div><span class="heart"></span><h1 style="display:inline">Flores Amarillas para <span id="toName">Mi Amor</span></h1></div>
      <div style="font-size:12px;color:var(--muted)">Hecho con cariño y JavaScript</div>
    </header>
    <main>
      <!-- SCREEN 1: Mini mensaje + botón siguiente -->
      <section class="screen intro active" id="screenIntro">
        <div class="panel">
          <h3 style="margin:0 0 8px 0">Tu mini mensaje</h3>
          <div class="field"><label>Para</label><input id="inpName" placeholder="Ej.: Ana"/></div>
          <div class="field"><label>Mensaje breve</label><textarea id="inpMsg" placeholder="Escribe algo lindo y sencillo…"></textarea></div>
          <div class="field"><label>Fecha especial (opcional)</label><input id="inpDate" type="date"/></div>
          <div class="field"><label>URL de canción (.mp3 directo, opcional)</label><input id="inpAudio" placeholder="https://.../cancion.mp3"/></div>
          <div style="display:flex;gap:10px;flex-wrap:wrap;margin-top:10px">
            <button class="btn primary" id="btnNext">Siguiente →</button>
            <button class="btn secondary" id="btnShare">Compartir enlace</button>
          </div>
          <p style="color:var(--muted);font-size:12px;margin-top:10px">Tip: parámetros: <span class="kbd">?n=Ana&m=Te%20amo&d=2025-09-21&a=https://.../song.mp3&auto=1&t=5&grow=1&gs=1200</span></p>
        </div>
        <div class="panel">
          <h3 style="margin:0 0 6px 0">Vista previa</h3>
          <p id="preview" class="tw"></p>
        </div>
      </section>

      <div class="overlay" id="overlay">
        <div>
          <div id="count" class="count">5</div>
          <div class="hint">cerrando los ojos en... ✨</div>
        </div>
      </div>

      <!-- SCREEN 2: Paisaje -->
      <section class="screen scene" id="screenScene" aria-hidden="true">
        <div class="sky" id="sky">
          <div class="sun"></div>
          <div class="cloud" style="left:10%; top:12%" data-speed="8"></div>
          <div class="cloud" style="left:45%; top:8%" data-speed="12"></div>
          <div class="cloud" style="left:72%; top:18%" data-speed="10"></div>
        </div>
        <div class="ground"></div>
        <div class="hill a"></div>
        <div class="hill b"></div>
        <canvas id="flowers"></canvas>
        <div class="titleBox">
          <h2 id="sceneTitle">Para ti</h2>
          <div id="sceneSub" style="font-size:13px;color:#fef9c3"></div>
        </div>
      </section>
    </main>
    <footer>
      Presiona <span class="kbd">Espacio</span> para más pétalos • <a id="dlImg" href="#" style="color:#fde68a;text-decoration:underline">Guardar imagen</a>
    </footer>
  </div>
</div>

<script>
const $ = s=>document.querySelector(s);
const qs = new URLSearchParams(location.search);
const state = {
  name: qs.get('n') || 'Mi Amor',
  msg: qs.get('m') ? decodeURIComponent(qs.get('m')) : 'Hoy te regalo un pedacito de cielo con un campo de flores amarillas. 💛',
  date: qs.get('d') || '',
  audio: qs.get('a') || '',
  auto: qs.get('auto') === '1',
  seconds: parseInt(qs.get('t')||'5',10),
  grow: qs.get('grow') !== '0',      // por defecto activado
  gs: parseInt(qs.get('gs')||'1200',10) // duración del crecimiento por flor (ms)
};

// Inicializar inputs
$('#toName').textContent = state.name; $('#inpName').value = state.name; $('#inpMsg').value = state.msg; $('#inpDate').value = state.date; $('#inpAudio').value = state.audio;
function typePreview(){ const node=$('#preview'); const text=$('#inpMsg').value || state.msg; node.textContent=''; node.classList.add('tw'); let i=0; const id=setInterval(()=>{ node.textContent=text.slice(0,++i); if(i>=text.length){ clearInterval(id); node.classList.remove('tw'); }},18);} typePreview();
$('#inpMsg').addEventListener('input', typePreview);

// Compartir
$('#btnShare').addEventListener('click',()=>{
  const url = new URL(location.href);
  url.searchParams.set('n',$('#inpName').value||state.name);
  url.searchParams.set('m', encodeURIComponent($('#inpMsg').value||state.msg));
  const d=$('#inpDate').value; if(d) url.searchParams.set('d',d); else url.searchParams.delete('d');
  const a=$('#inpAudio').value.trim(); if(a) url.searchParams.set('a',a); else url.searchParams.delete('a');
  url.searchParams.set('auto','1'); url.searchParams.set('t', String(state.seconds||5)); url.searchParams.set('grow', state.grow?'1':'0'); url.searchParams.set('gs', String(state.gs));
  navigator.clipboard.writeText(url.toString()).then(()=>alert('Enlace copiado. Pruébalo o envíalo por WhatsApp.'));
});

// Siguiente → cuenta regresiva → escena
$('#btnNext').addEventListener('click',()=>{ state.name=$('#inpName').value.trim()||state.name; state.msg=$('#inpMsg').value.trim()||state.msg; state.date=$('#inpDate').value||state.date; state.audio=$('#inpAudio').value.trim()||state.audio; $('#toName').textContent=state.name; startCountdown(state.seconds||5); });

function startCountdown(sec){ const ov=$('#overlay'); const cnt=$('#count'); ov.classList.add('show'); let n=sec; cnt.textContent=n; cnt.style.animation='none'; cnt.offsetHeight; cnt.style.animation='pop .9s ease both'; const t=setInterval(()=>{ n--; cnt.textContent=n; cnt.style.animation='none'; cnt.offsetHeight; cnt.style.animation='pop .9s ease both'; if(n<=0){ clearInterval(t); ov.classList.remove('show'); revealScene(); } },1000); }

let audioEl=null; function ensureAudio(){ if(!state.audio) return; if(!audioEl){ audioEl=document.createElement('audio'); audioEl.src=state.audio; audioEl.loop=true; audioEl.controls=false; audioEl.style.display='none'; document.body.appendChild(audioEl);} audioEl.play().catch(()=>{ const btn=document.createElement('button'); btn.textContent='🔊 Activar música'; btn.className='btn primary'; btn.style.position='fixed'; btn.style.zIndex='60'; btn.style.bottom='18px'; btn.style.right='18px'; document.body.appendChild(btn); btn.addEventListener('click',()=>{ audioEl.play().finally(()=>btn.remove()); }); }); }

function revealScene(){ $('#screenIntro').classList.remove('active'); const scene=$('#screenScene'); scene.classList.add('active'); scene.setAttribute('aria-hidden','false'); $('#sceneTitle').textContent=`Para ti, ${state.name}`; $('#sceneSub').textContent= state.date ? new Date(state.date).toLocaleDateString('es-CO',{year:'numeric',month:'long',day:'numeric'}) : 'Con todo mi cariño'; startFlowers(); animateSky(); sprinkleSparks(); ensureAudio(); }

// ===== Flores que crecen =====
const cvs=$('#flowers'); const ctx=cvs.getContext('2d');
let raf; let flowersData=[]; let sproutDone=false;
function resize(){ const box=$('#screenScene'); const dpr=Math.max(1,devicePixelRatio||1); cvs.width=box.clientWidth*dpr; cvs.height=box.clientHeight*dpr; cvs.style.width=box.clientWidth+'px'; cvs.style.height=box.clientHeight+'px'; ctx.setTransform(dpr,0,0,dpr,0,0); }
function buildFlowers(){ resize(); sproutDone=false; flowersData=[]; const w=cvs.width/devicePixelRatio, h=cvs.height/devicePixelRatio; const rows=7, cols=36; const baseY=h*0.58; const spreadX=w*1.1; const now=performance.now(); for(let r=0;r<rows;r++){ for(let c=0;c<cols;c++){ const x=(c/cols)*spreadX - (spreadX*.05) + (Math.random()*8-4) + w*0.02; const yTop=baseY + r*18 - 40 + Math.sin((c+r)*.3)*2; const yBase=baseY + r*18 + Math.random()*2; const size=10 + r*1.2 + Math.random()*3; const delay=r*140 + Math.random()*220; flowersData.push({x,yTop,yBase,size,delay,start:now}); } } }
function easeOutBack(x){ const c1=1.70158, c3=c1+1; return 1 + c3*Math.pow(x-1,3) + c1*Math.pow(x-1,2); }
function drawStemProgress(x,yBase,yTop,t){ const y=yBase + (yTop - yBase)*t; ctx.strokeStyle='#1e7a3a'; ctx.lineWidth=1.5; ctx.beginPath(); ctx.moveTo(x,yBase); ctx.quadraticCurveTo(x-6, yBase - 20*t, x, y); ctx.stroke(); }
function drawFlower(x,y,r,scale=1){ ctx.save(); ctx.translate(x,y); ctx.scale(scale,scale); ctx.rotate(Math.random()*Math.PI); for(let i=0;i<10;i++){ ctx.rotate((Math.PI*2)/10); const gr=ctx.createRadialGradient(r*.1,0,0, r*.1,0,r); gr.addColorStop(0,'#fde047'); gr.addColorStop(.7,'#facc15'); gr.addColorStop(1,'#f59e0b'); ctx.fillStyle=gr; ctx.beginPath(); ctx.ellipse(0,r*.35,r*.7,r*.4,0,0,Math.PI*2); ctx.fill(); } ctx.fillStyle='#915f1d'; ctx.beginPath(); ctx.arc(0,0,r*.33,0,Math.PI*2); ctx.fill(); ctx.restore(); }
function animateSprout(){ const w=cvs.width/devicePixelRatio, h=cvs.height/devicePixelRatio; const dur=state.gs || 1200; function frame(){ const now=performance.now(); ctx.clearRect(0,0,w,h); let allDone=true; flowersData.forEach((f,i)=>{ const elapsed=now - f.start - f.delay; let t=Math.max(0, Math.min(1, elapsed/dur)); if(t<1) allDone=false; const grow=easeOutBack(t); drawStemProgress(f.x, f.yBase, f.yTop, grow); const sway = sproutDone ? Math.sin(now*0.001 + i*0.2)*0.6 : 0; drawFlower(f.x + sway, f.yTop, f.size, Math.max(0.05, grow)); }); if(!allDone){ raf=requestAnimationFrame(frame);} else { sproutDone=true; swayLoop(); } } cancelAnimationFrame(raf); raf=requestAnimationFrame(frame); }
function swayLoop(){ const w=cvs.width/devicePixelRatio, h=cvs.height/devicePixelRatio; function step(){ const now=performance.now(); ctx.clearRect(0,0,w,h); flowersData.forEach((f,i)=>{ const wind=Math.sin(now*0.0006 + Math.floor(i/36)*0.4)*4; drawStemProgress(f.x, f.yBase, f.yTop, 1); drawFlower(f.x + wind*0.15, f.yTop + Math.sin(now*0.001 + i)*0.8, f.size, 1); }); raf=requestAnimationFrame(step); } cancelAnimationFrame(raf); raf=requestAnimationFrame(step); }
function startFlowers(){ buildFlowers(); if(state.grow){ animateSprout(); } else { flowersData.forEach(f=>{ f.delay=0; f.start=performance.now()-99999; }); sproutDone=true; swayLoop(); } }
addEventListener('resize', ()=>{ if($('#screenScene').classList.contains('active')){ buildFlowers(); sproutDone=false; if(state.grow) animateSprout(); else swayLoop(); } });

// Cielo
function animateSky(){ const clouds=Array.from(document.querySelectorAll('.cloud')); let t=0; (function move(){ t+=1; clouds.forEach(cl=>{ const speed=parseFloat(cl.dataset.speed||8); const x=(parseFloat(cl.style.left)||0) + (speed*0.02); cl.style.left=(x % 110) + '%'; }); requestAnimationFrame(move); })(); }

// Brillitos
function sprinkleSparks(){ const scene=$('#screenScene'); for(let i=0;i<36;i++){ const s=document.createElement('span'); s.className='spark'; s.style.left=Math.random()*100+'%'; s.style.top=(10+Math.random()*70)+'%'; s.style.opacity=0; scene.appendChild(s); setTimeout(()=>{ s.style.transition='opacity 1.2s ease, transform 2.4s ease'; s.style.opacity=.9; s.style.transform='translateY(-6px)';}, 200+i*40); } }

// Guardar imagen rápida
$('#dlImg').addEventListener('click', (e)=>{ e.preventDefault(); const cnv=document.createElement('canvas'); const box=$('#screenScene'); const w=Math.floor(box.clientWidth), h=Math.floor(box.clientHeight); cnv.width=w; cnv.height=h; const c=cnv.getContext('2d'); const g=c.createLinearGradient(0,0,0,h); g.addColorStop(0,'#87ceeb'); g.addColorStop(1,'#e6f6ff'); c.fillStyle=g; c.fillRect(0,0,w,h); c.drawImage(cvs,0,0,w,h); c.fillStyle='#11182788'; c.fillRect(0,h-64,w,64); c.fillStyle='#fef9c3'; c.font='700 20px Playfair Display'; c.fillText(`Para ti, ${state.name}`, 16,h-38); c.font='400 14px Plus Jakarta Sans'; c.fillText(state.msg.slice(0,80)+(state.msg.length>80?'…':''), 16,h-16); const a=document.createElement('a'); a.download=`flores-amarillas-${state.name}.png`; a.href=cnv.toDataURL('image/png'); a.click(); });

// Tecla espacio → nuevas flores (reinicia brote)
addEventListener('keydown', e=>{ if(e.code==='Space' && $('#screenScene').classList.contains('active')){ startFlowers(); }});

// Auto mode
window.addEventListener('DOMContentLoaded',()=>{ const hasData=!!(qs.get('n') && qs.get('m')); if(state.auto || hasData){ $('#inpName').value=state.name; $('#inpMsg').value=state.msg; $('#inpDate').value=state.date; $('#inpAudio').value=state.audio; $('#screenIntro').classList.add('active'); setTimeout(()=> startCountdown(state.seconds||5), 300); } });
</script>
</body>
</html>
