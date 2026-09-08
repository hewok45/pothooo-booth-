<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Photo Booth</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Anton&family=Caveat:wght@600&family=Manrope:wght@500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root{
    --maroon:#6B1E23;
    --maroon-dark:#45141A;
    --cream:#F3E9DC;
    --paper:#FFFDF8;
    --mustard:#E8A33D;
    --mustard-dark:#C6842A;
    --ink:#211712;
    --teal:#3F6659;
    --shadow: rgba(33,23,18,.35);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--ink);
    background-image:
      radial-gradient(circle at 20% 10%, rgba(232,163,61,.08), transparent 40%),
      radial-gradient(circle at 85% 80%, rgba(63,102,89,.12), transparent 45%);
    font-family:'Manrope',sans-serif;
    color:var(--cream);
    min-height:100vh;
    padding:28px 16px 60px;
  }
  .wrap{max-width:1100px;margin:0 auto;}

  /* ---------- Marquee header ---------- */
  header.marquee{
    text-align:center;
    margin-bottom:30px;
    position:relative;
  }
  .bulb-row{
    display:flex;
    justify-content:center;
    gap:10px;
    margin-bottom:10px;
    flex-wrap:wrap;
  }
  .bulb{
    width:9px;height:9px;border-radius:50%;
    background:var(--mustard);
    box-shadow:0 0 8px 2px rgba(232,163,61,.7);
    animation:twinkle 2.4s ease-in-out infinite;
  }
  @keyframes twinkle{
    0%,100%{opacity:.35; transform:scale(.85);}
    50%{opacity:1; transform:scale(1);}
  }
  h1.title{
    font-family:'Anton',sans-serif;
    font-size:clamp(2.4rem,7vw,4.2rem);
    letter-spacing:.03em;
    margin:0;
    color:var(--paper);
    text-shadow:0 0 18px rgba(232,163,61,.35), 3px 3px 0 var(--maroon-dark);
  }
  p.subtitle{
    font-size:.95rem;
    color:var(--mustard);
    margin:6px 0 0;
    font-weight:600;
    letter-spacing:.02em;
  }

  /* ---------- Layout ---------- */
  .layout{
    display:grid;
    grid-template-columns:1.4fr 1fr;
    gap:22px;
    align-items:start;
  }
  @media(max-width:820px){
    .layout{grid-template-columns:1fr;}
  }

  /* ---------- Stage (camera booth) ---------- */
  .booth{
    background:linear-gradient(180deg,var(--maroon) 0%, var(--maroon-dark) 100%);
    border-radius:18px;
    padding:16px;
    box-shadow:0 20px 40px var(--shadow);
    position:relative;
  }
  .booth-inner{
    position:relative;
    background:#050403;
    border-radius:10px;
    overflow:hidden;
    aspect-ratio:4/3;
    display:flex;
    align-items:center;
    justify-content:center;
  }
  video{
    width:100%;height:100%;
    object-fit:cover;
    display:block;
    transition:filter .15s ease;
  }
  video.mirror{transform:scaleX(-1);}
  .placeholder-msg{
    position:absolute;inset:0;
    display:flex;flex-direction:column;align-items:center;justify-content:center;
    gap:14px;text-align:center;padding:20px;
    color:var(--cream);
  }
  .placeholder-msg p{max-width:320px;margin:0;font-size:.9rem;opacity:.85;}
  .btn{
    font-family:'Manrope',sans-serif;
    font-weight:700;
    font-size:.92rem;
    border:none;
    cursor:pointer;
    padding:12px 22px;
    border-radius:999px;
    background:var(--mustard);
    color:var(--ink);
    box-shadow:0 6px 0 var(--mustard-dark);
    transition:transform .08s ease, box-shadow .08s ease;
  }
  .btn:active{transform:translateY(4px); box-shadow:0 2px 0 var(--mustard-dark);}
  .btn.secondary{
    background:transparent;
    color:var(--cream);
    box-shadow:none;
    border:2px solid rgba(243,233,220,.4);
  }
  .btn.secondary:active{transform:translateY(2px);}
  .btn:disabled{opacity:.45;cursor:not-allowed;box-shadow:none;transform:none;}

  /* frame overlay for live preview */
  .frame-overlay{position:absolute;inset:0;pointer-events:none;}
  .frame-classic{border:8px solid var(--ink); border-radius:6px; box-shadow: inset 0 0 0 2px rgba(255,255,255,.7);}
  .frame-polaroid{border:16px solid #fffdf8; border-bottom-width:56px; box-sizing:border-box;}
  .frame-filmreel::before, .frame-filmreel::after{
    content:'';position:absolute;top:0;bottom:0;width:34px;
    background:#0a0a0a;
    background-image:radial-gradient(circle, #050403 0 5px, transparent 6px);
    background-size:34px 34px;
  }
  .frame-filmreel::before{left:0;}
  .frame-filmreel::after{right:0;}
  .frame-confetti{
    border:10px solid transparent;
    border-image:linear-gradient(90deg,#e8a33d,#c6503f,#3f6659,#e8a33d,#c6503f) 1;
  }
  .frame-heart::before,.frame-heart::after,.frame-heart .h3,.frame-heart .h4{
    position:absolute;font-size:28px;line-height:1;
  }
  .frame-heart{border:4px solid rgba(255,255,255,.85);}
  .frame-heart::before{content:'♥';top:2px;left:6px;color:#e85d5d;}
  .frame-heart::after{content:'♥';top:2px;right:6px;color:#e85d5d;}
  .frame-neon{
    border:4px solid var(--mustard);
    box-shadow:0 0 16px 3px rgba(232,163,61,.9), inset 0 0 16px 2px rgba(63,102,89,.8);
  }

  .countdown-num{
    position:absolute;inset:0;display:none;align-items:center;justify-content:center;
    font-family:'Anton',sans-serif;font-size:7rem;color:var(--paper);
    text-shadow:0 0 30px rgba(0,0,0,.6);
    background:rgba(0,0,0,.15);
  }
  .flash{
    position:absolute;inset:0;background:#fff;opacity:0;pointer-events:none;
  }
  .flash.on{animation:flashpop .35s ease;}
  @keyframes flashpop{0%{opacity:.95;}100%{opacity:0;}}

  .stage-controls{
    display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:16px;
  }

  /* ---------- Control panel ---------- */
  .panel{
    background:var(--paper);
    color:var(--ink);
    border-radius:18px;
    padding:6px;
    box-shadow:0 20px 40px var(--shadow);
    overflow:hidden;
  }
  .tabs{display:flex;}
  .tab{
    flex:1;text-align:center;padding:14px 8px;font-weight:800;cursor:pointer;
    background:var(--cream);color:var(--ink);font-size:.88rem;
    border:none;border-bottom:3px solid transparent;
  }
  .tab.active{background:var(--paper);border-bottom:3px solid var(--maroon);color:var(--maroon);}
  .tab-panel{display:none;padding:18px;}
  .tab-panel.active{display:block;}

  .swatch-grid{
    display:grid;grid-template-columns:repeat(3,1fr);gap:10px;
  }
  .swatch{
    border:2px solid #e4dccb;
    border-radius:12px;
    padding:10px 6px;
    text-align:center;
    cursor:pointer;
    background:var(--cream);
    font-size:.78rem;
    font-weight:700;
    transition:border-color .12s ease, transform .12s ease;
  }
  .swatch:hover{transform:translateY(-2px);}
  .swatch.active{border-color:var(--maroon); background:#fceee0; color:var(--maroon);}
  .swatch .chip{
    width:100%;aspect-ratio:1;border-radius:8px;margin-bottom:6px;
    display:flex;align-items:center;justify-content:center;font-size:1.3rem;
  }

  .opt-row{
    display:flex;align-items:center;justify-content:space-between;
    padding:12px 0;border-bottom:1px solid #ece4d6;
  }
  .opt-row:last-child{border-bottom:none;}
  .opt-label{font-weight:700;font-size:.9rem;}
  .seg{display:flex;border:2px solid var(--maroon);border-radius:999px;overflow:hidden;}
  .seg button{
    border:none;background:transparent;padding:6px 12px;font-weight:700;font-size:.78rem;
    cursor:pointer;color:var(--maroon);
  }
  .seg button.active{background:var(--maroon);color:var(--paper);}
  .switch{
    position:relative;width:44px;height:24px;border-radius:999px;background:#d9cfba;cursor:pointer;
  }
  .switch.on{background:var(--teal);}
  .switch::after{
    content:'';position:absolute;top:2px;left:2px;width:20px;height:20px;border-radius:50%;
    background:#fff;transition:left .15s ease;
  }
  .switch.on::after{left:22px;}

  /* ---------- Gallery ---------- */
  .gallery{margin-top:26px;}
  .gallery h2{
    font-family:'Anton',sans-serif;font-weight:400;letter-spacing:.02em;
    font-size:1.4rem;color:var(--paper);margin-bottom:12px;
  }
  .gallery-row{display:flex;gap:14px;overflow-x:auto;padding-bottom:8px;}
  .gallery-item{
    flex:0 0 auto;background:var(--paper);border-radius:12px;padding:8px;
    box-shadow:0 10px 20px var(--shadow);
  }
  .gallery-item img{display:block;max-height:180px;border-radius:6px;}
  .gallery-actions{display:flex;gap:6px;margin-top:6px;}
  .gallery-actions button{
    flex:1;font-size:.72rem;font-weight:700;padding:6px 4px;border-radius:8px;border:none;cursor:pointer;
  }
  .gallery-actions .dl{background:var(--mustard);color:var(--ink);}
  .gallery-actions .rm{background:#ece4d6;color:var(--ink);}
  .empty-note{color:rgba(243,233,220,.6);font-size:.85rem;}

  canvas{display:none;}
</style>
</head>
<body>
<div class="wrap">

  <header class="marquee">
    <div class="bulb-row" id="bulbRowTop"></div>
    <h1 class="title">PHOTO BOOTH</h1>
    <p class="subtitle">Pilih bingkai &amp; filter — lalu senyum ke kamera</p>
    <div class="bulb-row" id="bulbRowBottom"></div>
  </header>

  <div class="layout">

    <!-- STAGE -->
    <div class="booth">
      <div class="booth-inner" id="boothInner">
        <video id="video" autoplay playsinline></video>
        <div class="frame-overlay" id="frameOverlay"></div>
        <div class="countdown-num" id="countdownNum">3</div>
        <div class="flash" id="flash"></div>
        <div class="placeholder-msg" id="placeholder">
          <p>Kamera belum aktif. Izinkan akses kamera untuk mulai berfoto.</p>
          <button class="btn" id="openCamBtn">Buka Kamera</button>
        </div>
      </div>
      <div class="stage-controls">
        <button class="btn" id="captureBtn" disabled>📸 Ambil Foto</button>
        <button class="btn secondary" id="switchBtn" disabled>🔄 Ganti Kamera</button>
      </div>
    </div>

    <!-- PANEL -->
    <div class="panel">
      <div class="tabs">
        <button class="tab active" data-tab="frame">Bingkai</button>
        <button class="tab" data-tab="filter">Filter</button>
        <button class="tab" data-tab="other">Lainnya</button>
      </div>

      <div class="tab-panel active" id="tab-frame">
        <div class="swatch-grid" id="frameGrid"></div>
      </div>

      <div class="tab-panel" id="tab-filter">
        <div class="swatch-grid" id="filterGrid"></div>
      </div>

      <div class="tab-panel" id="tab-other">
        <div class="opt-row">
          <span class="opt-label">Cermin (mirror)</span>
          <div class="switch on" id="mirrorSwitch"></div>
        </div>
        <div class="opt-row">
          <span class="opt-label">Efek kilat</span>
          <div class="switch on" id="flashSwitch"></div>
        </div>
        <div class="opt-row">
          <span class="opt-label">Hitung mundur</span>
          <div class="seg" id="countdownSeg">
            <button data-val="0">Off</button>
            <button data-val="3" class="active">3s</button>
            <button data-val="5">5s</button>
          </div>
        </div>
        <div class="opt-row">
          <span class="opt-label">Mode foto</span>
          <div class="seg" id="modeSeg">
            <button data-val="single" class="active">Tunggal</button>
            <button data-val="strip">Strip (3x)</button>
          </div>
        </div>
      </div>
    </div>

  </div>

  <div class="gallery">
    <h2>Hasil Jepretan</h2>
    <div class="gallery-row" id="galleryRow">
      <p class="empty-note">Foto yang kamu ambil akan muncul di sini.</p>
    </div>
  </div>

</div>

<canvas id="workCanvas"></canvas>

<script>
(function(){
  // ---------- Marquee bulbs ----------
  function fillBulbs(id,count){
    const row=document.getElementById(id);
    for(let i=0;i<count;i++){
      const b=document.createElement('div');
      b.className='bulb';
      b.style.animationDelay=(i*0.12)+'s';
      row.appendChild(b);
    }
  }
  fillBulbs('bulbRowTop',16);
  fillBulbs('bulbRowBottom',16);

  // ---------- Data ----------
  const FILTERS=[
    {id:'normal', name:'Normal', css:'none', emoji:'✨'},
    {id:'bw', name:'Hitam Putih', css:'grayscale(1) contrast(1.05)', emoji:'⚫'},
    {id:'sepia', name:'Sephia', css:'sepia(.75) contrast(1.05)', emoji:'🟤'},
    {id:'vintage', name:'Vintage', css:'sepia(.35) contrast(1.15) brightness(.95) saturate(1.25)', emoji:'📼'},
    {id:'cool', name:'Dingin', css:'saturate(1.2) brightness(1.05) hue-rotate(-8deg) contrast(1.05)', emoji:'❄️'},
    {id:'warm', name:'Hangat', css:'sepia(.25) saturate(1.4) hue-rotate(-6deg) brightness(1.05)', emoji:'🔥'},
    {id:'contrast', name:'Kontras', css:'contrast(1.5) saturate(1.3)', emoji:'⚡'},
    {id:'soft', name:'Lembut', css:'brightness(1.1) contrast(.9) saturate(1.1)', emoji:'🌸'}
  ];

  const FRAMES=[
    {id:'none', name:'Tanpa Bingkai', emoji:'⬜'},
    {id:'classic', name:'Klasik Hitam', emoji:'🖼️'},
    {id:'polaroid', name:'Polaroid', emoji:'📷'},
    {id:'filmreel', name:'Reel Film', emoji:'🎞️'},
    {id:'confetti', name:'Pesta Warna', emoji:'🎉'},
    {id:'heart', name:'Cinta', emoji:'💗'},
    {id:'neon', name:'Neon Glow', emoji:'🌟'}
  ];

  const state={
    filter:'normal',
    frame:'none',
    mirror:true,
    flash:true,
    countdown:3,
    mode:'single',
    stream:null,
    facing:'user'
  };

  // ---------- Elements ----------
  const video=document.getElementById('video');
  const boothInner=document.getElementById('boothInner');
  const placeholder=document.getElementById('placeholder');
  const openCamBtn=document.getElementById('openCamBtn');
  const captureBtn=document.getElementById('captureBtn');
  const switchBtn=document.getElementById('switchBtn');
  const frameOverlay=document.getElementById('frameOverlay');
  const countdownNum=document.getElementById('countdownNum');
  const flashEl=document.getElementById('flash');
  const galleryRow=document.getElementById('galleryRow');
  const workCanvas=document.getElementById('workCanvas');

  // ---------- Build swatches ----------
  function buildSwatches(list, gridId, stateKey, extraOnClick){
    const grid=document.getElementById(gridId);
    list.forEach(item=>{
      const el=document.createElement('div');
      el.className='swatch'+(state[stateKey]===item.id?' active':'');
      el.dataset.id=item.id;
      el.innerHTML='<div class="chip">'+item.emoji+'</div>'+item.name;
      el.addEventListener('click',()=>{
        state[stateKey]=item.id;
        grid.querySelectorAll('.swatch').forEach(s=>s.classList.remove('active'));
        el.classList.add('active');
        if(extraOnClick) extraOnClick(item.id);
      });
      grid.appendChild(el);
    });
  }
  buildSwatches(FRAMES,'frameGrid','frame', updateFrameOverlay);
  buildSwatches(FILTERS,'filterGrid','filter', updateVideoFilter);

  function updateVideoFilter(){
    const f=FILTERS.find(x=>x.id===state.filter);
    video.style.filter=f?f.css:'none';
  }
  function updateFrameOverlay(){
    frameOverlay.className='frame-overlay';
    if(state.frame!=='none'){
      frameOverlay.classList.add('frame-'+state.frame);
    }
  }
  updateVideoFilter();
  updateFrameOverlay();

  // ---------- Tabs ----------
  document.querySelectorAll('.tab').forEach(tab=>{
    tab.addEventListener('click',()=>{
      document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
      document.querySelectorAll('.tab-panel').forEach(p=>p.classList.remove('active'));
      tab.classList.add('active');
      document.getElementById('tab-'+tab.dataset.tab).classList.add('active');
    });
  });

  // ---------- Options ----------
  const mirrorSwitch=document.getElementById('mirrorSwitch');
  mirrorSwitch.addEventListener('click',()=>{
    state.mirror=!state.mirror;
    mirrorSwitch.classList.toggle('on',state.mirror);
    video.classList.toggle('mirror',state.mirror);
  });
  video.classList.toggle('mirror',state.mirror);

  const flashSwitch=document.getElementById('flashSwitch');
  flashSwitch.addEventListener('click',()=>{
    state.flash=!state.flash;
    flashSwitch.classList.toggle('on',state.flash);
  });

  document.getElementById('countdownSeg').addEventListener('click',(e)=>{
    if(e.target.tagName!=='BUTTON')return;
    document.querySelectorAll('#countdownSeg button').forEach(b=>b.classList.remove('active'));
    e.target.classList.add('active');
    state.countdown=parseInt(e.target.dataset.val,10);
  });
  document.getElementById('modeSeg').addEventListener('click',(e)=>{
    if(e.target.tagName!=='BUTTON')return;
    document.querySelectorAll('#modeSeg button').forEach(b=>b.classList.remove('active'));
    e.target.classList.add('active');
    state.mode=e.target.dataset.val;
  });

  // ---------- Camera ----------
  async function openCamera(){
    try{
      if(state.stream){
        state.stream.getTracks().forEach(t=>t.stop());
      }
      const stream=await navigator.mediaDevices.getUserMedia({
        video:{facingMode:state.facing, width:{ideal:1280}, height:{ideal:960}},
        audio:false
      });
      state.stream=stream;
      video.srcObject=stream;
      placeholder.style.display='none';
      captureBtn.disabled=false;
      switchBtn.disabled=false;
    }catch(err){
      placeholder.querySelector('p').textContent='Tidak dapat mengakses kamera. Pastikan izin kamera sudah diberikan pada browser, lalu coba lagi.';
      placeholder.style.display='flex';
    }
  }
  openCamBtn.addEventListener('click',openCamera);
  switchBtn.addEventListener('click',()=>{
    state.facing = state.facing==='user' ? 'environment' : 'user';
    openCamera();
  });

  // ---------- Countdown ----------
  function runCountdown(seconds){
    return new Promise(resolve=>{
      if(seconds<=0){resolve();return;}
      let n=seconds;
      countdownNum.style.display='flex';
      countdownNum.textContent=n;
      const timer=setInterval(()=>{
        n--;
        if(n<=0){
          clearInterval(timer);
          countdownNum.style.display='none';
          resolve();
        }else{
          countdownNum.textContent=n;
        }
      },1000);
    });
  }

  function doFlash(){
    if(!state.flash)return;
    flashEl.classList.remove('on');
    void flashEl.offsetWidth;
    flashEl.classList.add('on');
  }

  // ---------- Capture raw filtered frame to a canvas ----------
  function grabRawFrame(){
    const w=video.videoWidth||1280, h=video.videoHeight||960;
    const c=document.createElement('canvas');
    c.width=w; c.height=h;
    const ctx=c.getContext('2d');
    const f=FILTERS.find(x=>x.id===state.filter);
    ctx.filter=f?f.css:'none';
    ctx.save();
    if(state.mirror){
      ctx.translate(w,0);
      ctx.scale(-1,1);
    }
    ctx.drawImage(video,0,0,w,h);
    ctx.restore();
    return c;
  }

  // ---------- Frame drawing ----------
  function applyFrame(sourceCanvas, frameId){
    const w=sourceCanvas.width, h=sourceCanvas.height;
    if(frameId==='none'){
      return sourceCanvas;
    }
    if(frameId==='classic'){
      const c=document.createElement('canvas'); c.width=w; c.height=h;
      const ctx=c.getContext('2d');
      ctx.drawImage(sourceCanvas,0,0);
      ctx.strokeStyle='#211712'; ctx.lineWidth=Math.round(w*0.03);
      ctx.strokeRect(ctx.lineWidth/2,ctx.lineWidth/2,w-ctx.lineWidth,h-ctx.lineWidth);
      ctx.strokeStyle='#fffdf8'; ctx.lineWidth=Math.round(w*0.006);
      const off=Math.round(w*0.045);
      ctx.strokeRect(off,off,w-off*2,h-off*2);
      return c;
    }
    if(frameId==='polaroid'){
      const border=Math.round(w*0.045), bottom=Math.round(h*0.18);
      const c=document.createElement('canvas'); c.width=w+border*2; c.height=h+border+bottom;
      const ctx=c.getContext('2d');
      ctx.fillStyle='#fffdf8'; ctx.fillRect(0,0,c.width,c.height);
      ctx.shadowColor='rgba(0,0,0,.25)'; ctx.shadowBlur=18;
      ctx.drawImage(sourceCanvas,border,border);
      ctx.shadowBlur=0;
      ctx.fillStyle='#211712';
      ctx.font=Math.round(bottom*0.42)+'px Caveat, cursive';
      ctx.textAlign='center';
      ctx.fillText('Photo Booth ✨', c.width/2, h+border+bottom*0.62);
      return c;
    }
    if(frameId==='filmreel'){
      const strip=Math.round(w*0.09);
      const 
