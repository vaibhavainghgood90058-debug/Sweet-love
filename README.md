# Sweet-love
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sorry, Love — a little letter for you</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Caveat:wght@500;600;700&family=Quicksand:wght@500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --blush:#FCEEF3;
    --blush-deep:#F7DEE9;
    --card:#FFFDFB;
    --ink:#3A2233;
    --ink-soft:#7A5568;
    --rose:#C6425E;
    --rose-deep:#A32C48;
    --gold:#E8B75E;
    --muted:#EAD9DD;
    --shadow: 0 18px 40px -20px rgba(58,34,51,.35);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    min-height:100vh;
    background: radial-gradient(circle at 20% 0%, var(--blush-deep), var(--blush) 60%);
    font-family:'Quicksand',sans-serif;
    color:var(--ink);
    display:flex;
    align-items:center;
    justify-content:center;
    overflow-x:hidden;
    position:relative;
  }
  h1,h2,.script{
    font-family:'Caveat',cursive;
    font-weight:700;
  }

  /* floating ambient hearts */
  #ambient{
    position:fixed; inset:0; pointer-events:none; z-index:0; overflow:hidden;
  }
  .drift{
    position:absolute;
    bottom:-10%;
    font-size:20px;
    opacity:.55;
    animation: rise linear infinite;
    filter: saturate(120%);
  }
  @keyframes rise{
    from{ transform: translateY(0) rotate(0deg); opacity:0; }
    10%{ opacity:.6; }
    to{ transform: translateY(-115vh) rotate(25deg); opacity:0; }
  }

  .stage{
    position:relative;
    z-index:1;
    width:min(430px, 92vw);
    margin:24px auto;
  }

  .dots{
    display:flex; gap:6px; justify-content:center; margin-bottom:14px;
  }
  .dots span{
    width:6px;height:6px;border-radius:50%;background:var(--muted);
    transition:background .3s, transform .3s;
  }
  .dots span.active{ background:var(--rose); transform:scale(1.3); }

  .screen{
    display:none;
    background:var(--card);
    border-radius:26px;
    padding:34px 26px 28px;
    box-shadow:var(--shadow);
    text-align:center;
    animation: pop .45s ease;
  }
  .screen.active{ display:block; }
  @keyframes pop{
    from{ opacity:0; transform: translateY(14px) scale(.98); }
    to{ opacity:1; transform: translateY(0) scale(1); }
  }

  /* mascot */
  .mascot{ width:150px; height:150px; margin:0 auto 8px; }
  .mascot svg{ width:100%; height:100%; }

  h1.title{ font-size:2.5rem; margin:4px 0 4px; color:var(--rose-deep); line-height:1; }
  h2.subtitle{ font-size:1.1rem; margin:0 0 18px; color:var(--ink-soft); font-family:'Quicksand'; font-weight:600; }

  .bubble{
    background:var(--blush);
    border-radius:18px;
    padding:18px 20px;
    font-size:1.02rem;
    line-height:1.5;
    color:var(--ink);
    margin-bottom:22px;
  }

  .letter{
    background: repeating-linear-gradient(var(--card), var(--card) 33px, #F3E6D9 34px);
    border-radius:18px;
    padding:24px 22px 18px;
    text-align:left;
    font-family:'Caveat',cursive;
    font-size:1.45rem;
    line-height:34px;
    color:#4a3428;
    margin-bottom:22px;
    box-shadow: inset 0 0 0 1px #f0e3d3;
  }
  .letter .signoff{ display:block; text-align:right; margin-top:6px; }

  .row{ display:flex; gap:10px; justify-content:center; }
  button{
    font-family:'Quicksand',sans-serif;
    font-weight:700;
    font-size:.98rem;
    border:none;
    border-radius:999px;
    padding:14px 22px;
    cursor:pointer;
    transition: transform .15s ease, box-shadow .15s ease, background .2s;
  }
  button:active{ transform: scale(.96); }
  .btn-primary{ background:var(--rose); color:#fff; box-shadow:0 10px 20px -8px rgba(198,66,94,.6); }
  .btn-primary:hover{ background:var(--rose-deep); }
  .btn-secondary{ background:var(--muted); color:var(--ink); }
  .btn-block{ width:100%; }

  /* envelope */
  .envelope-wrap{ cursor:pointer; user-select:none; }
  .envelope{ width:220px; height:150px; margin:20px auto 26px; position:relative; }
  .env-body{ position:absolute; inset:0; background:var(--card); border-radius:8px; box-shadow:var(--shadow); }
  .env-flap{
    position:absolute; top:0; left:0; width:100%; height:75px;
    background:var(--rose);
    clip-path: polygon(0 0, 100% 0, 50% 100%);
    transform-origin:top center;
    transition: transform .6s cubic-bezier(.6,-0.28,.74,.05);
    border-bottom:3px solid var(--gold);
  }
  .envelope.open .env-flap{ transform: rotateX(180deg); }
  .env-heart{
    position:absolute; left:50%; top:56%; transform:translate(-50%,-50%);
    font-size:26px; opacity:0; transition:opacity .4s .3s;
  }
  .envelope.open .env-heart{ opacity:1; }
  .tap-hint{ margin-top:4px; font-family:'Caveat',cursive; font-size:1.4rem; color:var(--rose-deep); }

  /* forgiveness meter */
  .meter-track{ height:16px; background:var(--muted); border-radius:999px; overflow:hidden; margin:16px 0 6px; }
  .meter-fill{ height:100%; width:0%; background:linear-gradient(90deg, var(--rose), var(--gold)); transition:width .35s ease; }
  .meter-label{ font-weight:700; color:var(--rose-deep); margin-bottom:18px; }
  .heart-tap{
    width:88px; height:88px; border-radius:50%;
    background:var(--rose); border:none; font-size:34px; color:#fff;
    box-shadow:0 14px 26px -10px rgba(198,66,94,.7);
    margin:6px auto 20px; display:block; cursor:pointer;
  }
  .heart-tap:active{ transform:scale(.9); }

  /* bribe cards */
  .cards{ display:flex; gap:10px; margin-bottom:20px; }
  .flipcard{ flex:1; perspective:800px; height:190px; }
  .flipcard-inner{
    position:relative; width:100%; height:100%; transition:transform .6s;
    transform-style:preserve-3d;
  }
  .flipcard.flipped .flipcard-inner{ transform: rotateY(180deg); }
  .face{
    position:absolute; inset:0; border-radius:16px; padding:12px 8px;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    backface-visibility:hidden; background:var(--blush);
  }
  .face .emoji{ font-size:2.2rem; }
  .face .label{ font-weight:700; font-size:.82rem; color:var(--rose-deep); margin-top:6px; }
  .face .desc{ font-size:.68rem; color:var(--ink-soft); margin-top:4px; line-height:1.3; }
  .face-back{ transform: rotateY(180deg); background:var(--rose); color:#fff; }
  .face-back .label{ color:#fff; font-size:1rem; }

  /* memories carousel */
  .carousel{ position:relative; height:300px; margin-bottom:14px; }
  .memory-slide{
    position:absolute; inset:0; display:none;
    flex-direction:column; align-items:center; justify-content:center;
  }
  .memory-slide.active{ display:flex; }
  .photo-slot{
    width:100%; height:220px; border-radius:16px;
    background:var(--blush) center/cover no-repeat;
    border:2px dashed var(--gold);
    display:flex; align-items:center; justify-content:center;
    color:var(--ink-soft); font-size:.9rem; cursor:pointer; text-align:center; padding:10px;
  }
  .photo-slot.filled{ border-style:solid; color:transparent; }
  .memory-caption{
    margin-top:10px; font-family:'Caveat',cursive; font-size:1.3rem; color:var(--rose-deep);
    background:transparent; border:none; text-align:center; width:100%; outline:none;
  }
  .carousel-nav{ display:flex; justify-content:space-between; align-items:center; margin-top:2px; }
  .nav-btn{ background:var(--muted); border:none; width:38px; height:38px; border-radius:50%; font-size:1rem; cursor:pointer; }
  .carousel-dots{ display:flex; gap:6px; }
  .carousel-dots span{ width:7px; height:7px; border-radius:50%; background:var(--muted); }
  .carousel-dots span.active{ background:var(--rose); }

  /* confetti finale */
  #confetti-layer{ position:fixed; inset:0; pointer-events:none; z-index:5; }
  .confetti-piece{ position:absolute; top:-5%; font-size:18px; animation: fall linear forwards; }
  @keyframes fall{
    to{ transform: translateY(110vh) rotate(360deg); opacity:.2; }
  }

  .brand{ margin-top:16px; text-align:center; font-size:.72rem; color:var(--ink-soft); letter-spacing:.02em; }

  input[type=file]{ display:none; }

  @media (prefers-reduced-motion: reduce){
    .drift, .confetti-piece{ animation:none !important; display:none; }
  }
</style>
</head>
<body>

<div id="ambient"></div>
<div id="confetti-layer"></div>

<div class="stage">

  <div class="dots" id="dots"></div>

  <!-- SCREEN 0: envelope -->
  <section class="screen active" data-screen="0">
    <div class="envelope-wrap" id="envelopeWrap">
      <div class="envelope" id="envelope">
        <div class="env-body"></div>
        <div class="env-flap"></div>
        <div class="env-heart">💌</div>
      </div>
      <div class="tap-hint">tap to open ❤️</div>
    </div>
  </section>

  <!-- SCREEN 1 -->
  <section class="screen" data-screen="1">
    <div class="mascot">
      <svg viewBox="0 0 160 160"><g>
        <ellipse cx="80" cy="95" rx="55" ry="50" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="35" cy="45" r="17" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="125" cy="45" r="17" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="55" cy="88" r="4" fill="#3A2233"/>
        <circle cx="105" cy="88" r="4" fill="#3A2233"/>
        <path d="M68 108 q12 8 24 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <ellipse cx="45" cy="98" rx="8" ry="5" fill="#F3B6C6" opacity=".8"/>
        <ellipse cx="115" cy="98" rx="8" ry="5" fill="#F3B6C6" opacity=".8"/>
      </g></svg>
    </div>
    <h1 class="title">I Know I Hurt You</h1>
    <div class="bubble">I messed up… and I'm really sorry for that.</div>
    <div class="row">
      <button class="btn-secondary" onclick="go(0)">← Back</button>
      <button class="btn-primary" onclick="go(2)">Next →</button>
    </div>
  </section>

  <!-- SCREEN 2 -->
  <section class="screen" data-screen="2">
    <div class="mascot">
      <svg viewBox="0 0 160 160"><g>
        <ellipse cx="80" cy="95" rx="55" ry="50" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="35" cy="45" r="17" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="125" cy="45" r="17" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <path d="M48 84 q7 -6 14 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <path d="M98 84 q7 -6 14 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <ellipse cx="80" cy="112" rx="10" ry="7" fill="#3A2233"/>
        <path d="M42 92 q-6 10 -2 20" stroke="#8ecbef" stroke-width="3" fill="none" stroke-linecap="round"/>
        <path d="M118 92 q6 10 2 20" stroke="#8ecbef" stroke-width="3" fill="none" stroke-linecap="round"/>
      </g></svg>
    </div>
    <h1 class="title">I'm Truly Sorry</h1>
    <div class="bubble">Please forgive me… you mean so much to me.</div>
    <div class="row">
      <button class="btn-secondary" onclick="go(1)">← Back</button>
      <button class="btn-primary" onclick="go(3)">Next →</button>
    </div>
  </section>

  <!-- SCREEN 3: letter -->
  <section class="screen" data-screen="3">
    <h1 class="title" style="font-size:2rem;">My Dearest <span id="nameSpan">Love</span>,</h1>
    <div class="letter" id="letterBody">
      I'm really sorry, my love. I accidentally upset the most precious and
      adorable person in my life — you. 🥺<br><br>
      Please forgive me if I hurt you or wasted even a little of your
      precious time. I promise I didn't mean to. 🥺💕
      <span class="signoff">— yours, always</span>
    </div>
    <button class="btn-primary btn-block" onclick="go(4)">Continue →</button>
  </section>

  <!-- SCREEN 4 -->
  <section class="screen" data-screen="4">
    <div class="mascot">
      <svg viewBox="0 0 160 160"><g>
        <ellipse cx="80" cy="100" rx="52" ry="48" fill="#F6D9B0" stroke="#3A2233" stroke-width="3"/>
        <circle cx="60" cy="80" r="5" fill="#3A2233"/>
        <circle cx="100" cy="80" r="5" fill="#3A2233"/>
        <path d="M65 108 q15 10 30 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <circle cx="126" cy="52" r="12" fill="var(--rose)"/>
        <path d="M120 52 l4 4 8 -8" stroke="#fff" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
      </g></svg>
    </div>
    <h1 class="title">I Messed Up<span id="nameSpan2">, Love</span></h1>
    <div class="bubble">But I'm ready to make it right. Will you hear me out?</div>
    <button class="btn-primary btn-block" onclick="go(5)">Listen to my heart 💌</button>
  </section>

  <!-- SCREEN 5: forgiveness meter -->
  <section class="screen" data-screen="5">
    <h1 class="title" style="font-size:1.9rem;">Forgiveness Meter ❤️</h1>
    <div class="mascot" style="width:110px;height:110px;">
      <svg viewBox="0 0 160 160"><g>
        <ellipse cx="80" cy="95" rx="52" ry="48" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="40" cy="48" r="15" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <circle cx="120" cy="48" r="15" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <path d="M55 90 q7 -6 14 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <path d="M91 90 q7 -6 14 0" stroke="#3A2233" stroke-width="3" fill="none" stroke-linecap="round"/>
        <ellipse cx="80" cy="112" rx="7" ry="5" fill="#3A2233"/>
      </g></svg>
    </div>
    <div class="bubble" style="margin-bottom:8px;">sorry na 🥺</div>
    <div class="meter-track"><div class="meter-fill" id="meterFill"></div></div>
    <div class="meter-label" id="meterLabel">0% forgiven</div>
    <p style="font-family:'Caveat',cursive; font-size:1.3rem; color:var(--ink-soft); margin:0 0 4px;">tap the heart to heal my heart!</p>
    <button class="heart-tap" id="heartTap" onclick="tapHeart()">🤍</button>
    <button class="btn-primary btn-block" id="meterNext" style="display:none;" onclick="go(6)">Next →</button>
  </section>

  <!-- SCREEN 6: bribes -->
  <section class="screen" data-screen="6">
    <h1 class="title" style="font-size:2rem;">Just In Case…</h1>
    <div class="cards">
      <div class="flipcard" onclick="this.classList.toggle('flipped')">
        <div class="flipcard-inner">
          <div class="face">
            <div class="emoji">💐</div>
            <div class="label">Fresh Blooms</div>
            <div class="desc">1 bouquet delivery + unlimited forehead kisses</div>
          </div>
          <div class="face face-back"><div class="label">Redeemed! ✅</div></div>
        </div>
      </div>
      <div class="flipcard" onclick="this.classList.toggle('flipped')">
        <div class="flipcard-inner">
          <div class="face">
            <div class="emoji">🧸</div>
            <div class="label">Cuddle Pass</div>
            <div class="desc">Unlimited hugs, cuddles & no drama day</div>
          </div>
          <div class="face face-back"><div class="label">Redeemed! ✅</div></div>
        </div>
      </div>
      <div class="flipcard" onclick="this.classList.toggle('flipped')">
        <div class="flipcard-inner">
          <div class="face">
            <div class="emoji">🍫</div>
            <div class="label">Sweet Treat</div>
            <div class="desc">Your favourite chocolate + my full attention</div>
          </div>
          <div class="face face-back"><div class="label">Redeemed! ✅</div></div>
        </div>
      </div>
    </div>
    <button class="btn-primary btn-block" onclick="go(7)">Enough bribes, let's go →</button>
  </section>

  <!-- SCREEN 7: sweet memories -->
  <section class="screen" data-screen="7">
    <h1 class="title" style="font-size:2rem;">Some Sweet Moments</h1>
    <h2 class="subtitle">(swipe or tap the arrows)</h2>
    <div class="carousel" id="carousel">
      <div class="memory-slide active" data-slide="0">
        <div class="photo-slot" data-slot="0" onclick="pickPhoto(0)">tap to add a photo 📷</div>
        <input class="memory-caption" data-cap="0" value="that one perfect evening 💕" />
      </div>
      <div class="memory-slide" data-slide="1">
        <div class="photo-slot" data-slot="1" onclick="pickPhoto(1)">tap to add a photo 📷</div>
        <input class="memory-caption" data-cap="1" value="us, being ridiculous 😄" />
      </div>
      <div class="memory-slide" data-slide="2">
        <div class="photo-slot" data-slot="2" onclick="pickPhoto(2)">tap to add a photo 📷</div>
        <input class="memory-caption" data-cap="2" value="my favourite kind of quiet" />
      </div>
      <div class="memory-slide" data-slide="3">
        <div class="photo-slot" data-slot="3" onclick="pickPhoto(3)">tap to add a photo 📷</div>
        <input class="memory-caption" data-cap="3" value="you mean everything 💕" />
      </div>
    </div>
    <div class="carousel-nav">
      <button class="nav-btn" onclick="shiftSlide(-1)">←</button>
      <div class="carousel-dots" id="carouselDots"></div>
      <button class="nav-btn" onclick="shiftSlide(1)">→</button>
    </div>
    <input type="file" id="fileInput" accept="image/*">
    <button class="btn-primary btn-block" style="margin-top:18px;" onclick="go(8)">Next →</button>
  </section>

  <!-- SCREEN 8 -->
  <section class="screen" data-screen="8">
    <div class="mascot">
      <svg viewBox="0 0 160 160"><g>
        <ellipse cx="55" cy="105" rx="35" ry="40" fill="#fff" stroke="#3A2233" stroke-width="3"/>
        <ellipse cx="110" cy="100" rx="38" ry="42" fill="#C98A4E" stroke="#3A2233" stroke-width="3"/>
        <circle cx="45" cy="90" r="3.5" fill="#3A2233"/>
        <circle cx="100" cy="86" r="3.5" fill="#3A2233"/>
        <path d="M40 108 q6 4 12 0" stroke="#3A2233" stroke-width="2.5" fill="none" stroke-linecap="round"/>
        <path d="M95 106 q6 4 12 0" stroke="#3A2233" stroke-width="2.5" fill="none" stroke-linecap="round"/>
        <rect x="90" y="120" width="26" height="16" rx="3" fill="#E8748C"/>
      </g></svg>
    </div>
    <h1 class="title">You Mean Everything</h1>
    <div class="bubble">I promise I'll be better for you.</div>
    <div class="row">
      <button class="btn-secondary" onclick="go(7)">← Back</button>
      <button class="btn-primary" onclick="finale()">Next →</button>
    </div>
  </section>

  <!-- SCREEN 9: finale -->
  <section class="screen" data-screen="9">
    <div style="font-size:2.6rem;">🕊️</div>
    <h1 class="title">Peace Treaty Signed!</h1>
    <h2 class="subtitle">new beginnings ahead</h2>
    <div class="bubble">
      Thank you for being the most forgiving and wonderful person.
      I love you to the moon and back and I'm never letting go! 💚🙏
    </div>
    <div style="font-size:1.6rem; margin-bottom:18px;">❤️❤️❤️❤️❤️</div>
    <button class="btn-secondary btn-block" onclick="replay()">↻ Replay</button>
    <div class="brand">made with love, just for you</div>
  </section>

</div>

<script>
  /* ---------- personalize here ---------- */
  const RECIPIENT_NAME = "Love"; // change to her/his name
  document.getElementById('nameSpan').textContent = RECIPIENT_NAME;
  document.getElementById('nameSpan2').textContent = ', ' + RECIPIENT_NAME;

  /* ---------- ambient floating hearts ---------- */
  const ambient = document.getElementById('ambient');
  const heartEmojis = ['💗','💕','💖','💓','❤️'];
  for(let i=0;i<16;i++){
    const el = document.createElement('div');
    el.className='drift';
    el.textContent = heartEmojis[Math.floor(Math.random()*heartEmojis.length)];
    el.style.left = Math.random()*100+'vw';
    el.style.fontSize = (14+Math.random()*18)+'px';
    el.style.animationDuration = (9+Math.random()*10)+'s';
    el.style.animationDelay = (Math.random()*10)+'s';
    ambient.appendChild(el);
  }

  /* ---------- screen navigation ---------- */
  const totalScreens = 10;
  const dotsWrap = document.getElementById('dots');
  for(let i=1;i<totalScreens-1;i++){ // dots for screens 1..8 (skip envelope & finale)
    const d = document.createElement('span');
    d.dataset.for = i;
    dotsWrap.appendChild(d);
  }
  function updateDots(current){
    document.querySelectorAll('#dots span').forEach(d=>{
      d.classList.toggle('active', Number(d.dataset.for)===current);
    });
    dotsWrap.style.visibility = (current===0 || current===9) ? 'hidden' : 'visible';
  }
  function go(n){
    document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
    document.querySelector('.screen[data-screen="'+n+'"]').classList.add('active');
    updateDots(n);
    window.scrollTo({top:0,behavior:'smooth'});
  }
  updateDots(0);

  /* ---------- envelope ---------- */
  document.getElementById('envelopeWrap').addEventListener('click', function(){
    const env = document.getElementById('envelope');
    env.classList.add('open');
    setTimeout(()=>go(1), 750);
  });

  /* ---------- forgiveness meter ---------- */
  let forgiven = 0;
  const milestones = {
    0:'0% forgiven', 30:'30% forgiven — keep going 🥺', 60:'60% forgiven — almost there 💕',
    90:'90% forgiven — one more tap!', 100:'100% forgiven 🎉'
  };
  function tapHeart(){
    if(forgiven>=100) return;
    forgiven = Math.min(100, forgiven+10);
    document.getElementById('meterFill').style.width = forgiven+'%';
    let label = '0% forgiven';
    Object.keys(milestones).map(Number).sort((a,b)=>a-b).forEach(k=>{
      if(forgiven>=k) label = milestones[k];
    });
    document.getElementById('meterLabel').textContent = label;
    const btn = document.getElementById('heartTap');
    btn.textContent = forgiven>=100 ? '💗' : (forgiven>=50 ? '🩷' : '🤍');
    if(forgiven>=100){
      document.getElementById('meterNext').style.display='block';
    }
  }

  /* ---------- sweet memories carousel ---------- */
  let slide = 0;
  const carDots = document.getElementById('carouselDots');
  for(let i=0;i<4;i++){ const d=document.createElement('span'); d.dataset.i=i; carDots.appendChild(d); }
  function renderCarousel(){
    document.querySelectorAll('.memory-slide').forEach(s=>{
      s.classList.toggle('active', Number(s.dataset.slide)===slide);
    });
    document.querySelectorAll('#carouselDots span').forEach(d=>{
      d.classList.toggle('active', Number(d.dataset.i)===slide);
    });
  }
  function shiftSlide(dir){
    slide = (slide + dir + 4) % 4;
    renderCarousel();
  }
  renderCarousel();

  /* swipe support */
  const carEl = document.getElementById('carousel');
  let touchStartX = null;
  carEl.addEventListener('touchstart', e=>{ touchStartX = e.touches[0].clientX; });
  carEl.addEventListener('touchend', e=>{
    if(touchStartX===null) return;
    const dx = e.changedTouches[0].clientX - touchStartX;
    if(Math.abs(dx) > 40) shiftSlide(dx < 0 ? 1 : -1);
    touchStartX = null;
  });

  /* photo upload, saved locally in this browser */
  let activeSlot = null;
  const fileInput = document.getElementById('fileInput');
  function pickPhoto(i){
    activeSlot = i;
    fileInput.click();
  }
  fileInput.addEventListener('change', function(){
    const file = this.files[0];
    if(!file || activeSlot===null) return;
    const reader = new FileReader();
    reader.onload = function(e){
      localStorage.setItem('memoryPhoto_'+activeSlot, e.target.result);
      applyPhoto(activeSlot, e.target.result);
    };
    reader.readAsDataURL(file);
  });
  function applyPhoto(i, dataUrl){
    const slot = document.querySelector('.photo-slot[data-slot="'+i+'"]');
    slot.style.backgroundImage = 'url(' + dataUrl + ')';
    slot.classList.add('filled');
  }
  // restore saved photos + captions
  for(let i=0;i<4;i++){
    const saved = localStorage.getItem('memoryPhoto_'+i);
    if(saved) applyPhoto(i, saved);
    const capSaved = localStorage.getItem('memoryCaption_'+i);
    const capEl = document.querySelector('.memory-caption[data-cap="'+i+'"]');
    if(capSaved) capEl.value = capSaved;
    capEl.addEventListener('input', function(){
      localStorage.setItem('memoryCaption_'+i, this.value);
    });
  }

  /* ---------- finale + confetti ---------- */
  function finale(){
    go(9);
    burstConfetti();
  }
  function burstConfetti(){
    const layer = document.getElementById('confetti-layer');
    const pieces = ['❤️','💛','💕','🎉','🕊️'];
    for(let i=0;i<28;i++){
      const p = document.createElement('div');
      p.className='confetti-piece';
      p.textContent = pieces[Math.floor(Math.random()*pieces.length)];
      p.style.left = Math.random()*100+'vw';
      p.style.fontSize = (14+Math.random()*16)+'px';
      p.style.animationDuration = (2.2+Math.random()*1.8)+'s';
      layer.appendChild(p);
      setTimeout(()=>p.remove(), 4200);
    }
  }
  function replay(){
    forgiven = 0;
    document.getElementById('meterFill').style.width='0%';
    document.getElementById('meterLabel').textContent='0% forgiven';
    document.getElementById('heartTap').textContent='🤍';
    document.getElementById('meterNext').style.display='none';
    document.getElementById('envelope').classList.remove('open');
    go(0);
  }
</script>
</body>
</html>
