<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Auth Template — Masuk & Daftar</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #0A0E16;
    --bg-2: #0D1220;
    --surface: #121826;
    --surface-2: #171F30;
    --border: #232C40;
    --text: #E9ECF3;
    --text-muted: #8890A3;
    --violet: #7C5CFC;
    --violet-soft: #7C5CFC33;
    --cyan: #45E0D0;
    --danger: #FF6B6B;
    --radius: 18px;
    --ease: cubic-bezier(.65,0,.35,1);
    --bouncy: cubic-bezier(.34,1.56,.64,1);
  }

  *{ box-sizing: border-box; }

  html, body{
    margin:0; padding:0; min-height:100%;
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter', sans-serif;
    overflow-x: hidden;
    overflow-y: auto;
    -webkit-text-size-adjust: 100%;
  }

  /* ---------- Animated backdrop ---------- */
  .backdrop{
    position: fixed; inset: 0; z-index: 0;
    background:
      radial-gradient(ellipse 900px 600px at 15% 20%, #7C5CFC22, transparent 60%),
      radial-gradient(ellipse 800px 700px at 85% 80%, #45E0D01c, transparent 60%),
      var(--bg);
  }
  .orb{
    position:absolute; border-radius:50%; filter: blur(70px); opacity:.55;
    animation: drift 22s ease-in-out infinite;
    transition: transform .6s var(--ease);
    will-change: transform;
  }
  .orb.a{ width:420px; height:420px; background: radial-gradient(circle, #7C5CFC 0%, transparent 70%); top:-8%; left:-6%; animation-duration: 26s; }
  .orb.b{ width:360px; height:360px; background: radial-gradient(circle, #45E0D0 0%, transparent 70%); bottom:-10%; right:-4%; animation-duration: 30s; animation-delay: -6s; }
  .orb.c{ width:260px; height:260px; background: radial-gradient(circle, #FF6B9D 0%, transparent 70%); top:55%; left:70%; opacity:.28; animation-duration: 34s; animation-delay: -14s; }

  @keyframes drift{
    0%,100%{ transform: translate(0,0) scale(1); }
    33%{ transform: translate(30px,-40px) scale(1.08); }
    66%{ transform: translate(-25px,25px) scale(0.95); }
  }

  .grain{
    position: fixed; inset:0; z-index: 1; pointer-events:none; opacity:.03;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  }

  /* ---------- Layout ---------- */
  .stage{
    position: relative; z-index: 2;
    min-height: 100dvh; width:100%;
    display:flex; align-items:center; justify-content:center;
    padding: 24px;
    perspective: 1600px;
  }

  .panel{
    position: relative;
    width: min(900px, 100%);
    min-height: 600px;
    background: linear-gradient(180deg, var(--surface), var(--bg-2));
    border: 1px solid var(--border);
    border-radius: calc(var(--radius) + 6px);
    display: grid;
    grid-template-columns: 1fr 1fr;
    overflow: hidden;
    box-shadow: 0 40px 100px -30px #00000090;
    transform-style: preserve-3d;
    transition: transform .5s var(--ease), box-shadow .5s var(--ease);
    animation: panelIn .7s var(--bouncy) both;
  }
  @keyframes panelIn{
    from{ opacity:0; transform: translateY(26px) scale(.97); }
    to{ opacity:1; transform: translateY(0) scale(1); }
  }

  /* ---------- Brand side ---------- */
  .brand{
    position: relative;
    padding: 48px 40px;
    display:flex; flex-direction:column; justify-content:space-between;
    background:
      radial-gradient(circle at 30% 20%, #7C5CFC55, transparent 55%),
      linear-gradient(160deg, #1a1030, #0D1220 70%);
    color:#fff;
  }
  .brand .mark{
    display:flex; align-items:center; gap:10px;
    font-family:'Space Grotesk', sans-serif; font-weight:700; font-size:18px;
  }
  .mark .dot{
    width:10px; height:10px; border-radius:50%;
    background: linear-gradient(135deg, var(--violet), var(--cyan));
    box-shadow: 0 0 16px #7C5CFCaa;
    animation: pulse 2.4s ease-in-out infinite;
  }
  @keyframes pulse{
    0%,100%{ box-shadow: 0 0 10px #7C5CFCaa; }
    50%{ box-shadow: 0 0 22px #45E0D0cc; }
  }
  .brand h1{
    font-family:'Space Grotesk', sans-serif;
    font-size: clamp(26px, 3.4vw, 34px);
    line-height:1.18; font-weight:600; margin: 0;
    letter-spacing: -0.01em;
  }
  .brand h1, .brand p{ transition: opacity .35s var(--ease), transform .35s var(--ease); }
  .brand p{
    color:#C7CBE0; font-size:14.5px; line-height:1.6; max-width: 34ch; margin: 14px 0 0;
  }
  .brand .foot{
    display:flex; align-items:center; gap:10px; font-size:13px; color:#9AA1BD;
  }
  .brand .foot .avatars{ display:flex; }
  .brand .foot .avatars span{
    width:26px; height:26px; border-radius:50%; display:inline-block;
    border:2px solid #1a1030; margin-left:-8px;
    background: linear-gradient(135deg, var(--cyan), var(--violet));
  }
  .brand .foot .avatars span:first-child{ margin-left:0; }

  /* ---------- Form side ---------- */
  .formside{
    position: relative;
    padding: 36px 44px;
    display:flex; flex-direction:column; justify-content:center;
  }

  /* ---------- Mascot ---------- */
  .mascot-wrap{
    display:flex; justify-content:center; margin-bottom: 6px;
  }
  .mascot{
    width: 108px; height: 92px;
    filter: drop-shadow(0 10px 18px #00000060);
  }
  .mascot .glow{ opacity:.5; }
  .mascot .ear{ transform-box: fill-box; transform-origin: center; transition: transform .35s var(--bouncy); will-change: transform; }
  .mascot.attentive .ear-l{ transform: rotate(-8deg); }
  .mascot.attentive .ear-r{ transform: rotate(8deg); }
  .mascot .pupil{ transition: transform .12s linear; }
  .mascot .mouth{ transition: d .3s var(--ease); }
  .mascot .paw{
    transition: transform .38s var(--bouncy);
    transform-box: fill-box;
    transform-origin: center;
    will-change: transform;
  }
  .mascot .paw-l{ transform: translateY(-74px) rotate(-8deg); }
  .mascot .paw-r{ transform: translateY(-74px) rotate(8deg); }
  .mascot.covering .paw-l{ transform: translateY(0) rotate(-3deg); }
  .mascot.covering .paw-r{ transform: translateY(0) rotate(3deg); }
  .mascot .eye-lid{ transition: transform .2s var(--ease); }
  .mascot.blink .eye-lid{ transform: scaleY(1); }
  .mascot .eye-l, .mascot .eye-r{ transition: transform .1s ease; transform-box: fill-box; transform-origin: center; }
  .mascot.blinking .eye-l, .mascot.blinking .eye-r{ transform: scaleY(0.12); }

  .mode-head{ margin-bottom: 22px; text-align:center; }
  .mode-head h2{
    font-family:'Space Grotesk', sans-serif; font-size:23px; margin:0 0 6px; font-weight:600;
  }
  .mode-head p{ margin:0; color:var(--text-muted); font-size:14px; }
  .mode-head p a{ color: var(--cyan); text-decoration:none; cursor:pointer; }
  .mode-head p a:hover{ text-decoration: underline; }

  form{ display:flex; flex-direction:column; gap:13px; }

  .field{
    position: relative;
    opacity:0; transform: translateY(10px);
    animation: fieldIn .45s var(--ease) forwards;
  }
  .field:nth-child(1){ animation-delay: .05s; }
  .field:nth-child(2){ animation-delay: .11s; }
  .field:nth-child(3){ animation-delay: .17s; }
  @keyframes fieldIn{ to{ opacity:1; transform: translateY(0); } }

  .field input{
    width:100%;
    background: var(--surface-2);
    border: 1px solid var(--border);
    color: var(--text);
    font-family:'Inter', sans-serif;
    font-size:14.5px;
    padding: 14px 16px;
    border-radius: 12px;
    outline: none;
    transition: border-color .25s var(--ease), box-shadow .25s var(--ease), background .25s var(--ease);
  }
  .field input::placeholder{ color: #626B85; }
  .field input:focus{
    border-color: var(--violet);
    box-shadow: 0 0 0 4px var(--violet-soft);
    background: #141C2E;
  }
  .field.invalid{ animation: shake .4s var(--ease); }
  .field.invalid input{ border-color: var(--danger); }
  .field .msg{
    font-size:12px; color: var(--danger); margin-top:6px; min-height:14px;
    opacity:0; transform: translateY(-4px);
    transition: opacity .2s, transform .2s;
  }
  .field.invalid .msg{ opacity:1; transform: translateY(0); }
  @keyframes shake{
    0%,100%{ transform: translateX(0); }
    25%{ transform: translateX(-6px); }
    75%{ transform: translateX(6px); }
  }

  .toggle-eye{
    position:absolute; right:14px; top:16px;
    background:none; border:none; cursor:pointer; color:#7B84A0;
    padding:4px; display:flex; transition: color .2s, transform .2s;
  }
  .toggle-eye:hover{ color: var(--cyan); transform: scale(1.1); }

  .row-between{
    display:flex; align-items:center; justify-content:space-between;
    font-size: 13px; color: var(--text-muted); margin-top:-2px;
  }
  .remember{ display:flex; align-items:center; gap:8px; }
  .remember input{ accent-color: var(--violet); }
  .row-between a{ color: var(--text-muted); text-decoration:none; }
  .row-between a:hover{ color: var(--cyan); }

  .submit{
    position: relative;
    margin-top: 4px;
    padding: 15px 18px;
    border: none; border-radius: 12px;
    background: linear-gradient(135deg, var(--violet), #5A3FE0);
    color: #fff; font-family:'Inter', sans-serif; font-weight:600; font-size: 14.5px;
    cursor: pointer;
    overflow: hidden;
    transition: transform .18s var(--ease), box-shadow .18s var(--ease);
    box-shadow: 0 10px 24px -10px #7C5CFC80;
  }
  .submit:hover{ transform: translateY(-2px); box-shadow: 0 16px 30px -10px #7C5CFCa0; }
  .submit:active{ transform: translateY(0px) scale(.98); }
  .submit .spinner{
    width:16px; height:16px; border-radius:50%;
    border: 2px solid #ffffff55; border-top-color:#fff;
    display:none; animation: spin .7s linear infinite;
    position:absolute; left:50%; top:50%; margin:-8px 0 0 -8px;
  }
  .submit.loading span{ opacity:0; }
  .submit.loading .spinner{ display:block; }
  @keyframes spin{ to{ transform: rotate(360deg); } }
  .ripple{
    position:absolute; border-radius:50%; background:#ffffff55;
    transform: scale(0); animation: rippleAnim .6s ease-out forwards;
    pointer-events:none;
  }
  @keyframes rippleAnim{ to{ transform: scale(2.6); opacity:0; } }

  .divider{
    display:flex; align-items:center; gap:12px; color:#5A6280; font-size:12.5px; margin: 4px 0 2px;
  }
  .divider::before, .divider::after{
    content:''; flex:1; height:1px; background: var(--border);
  }

  .social{ display:flex; gap:10px; }
  .social button{
    flex:1; display:flex; align-items:center; justify-content:center; gap:8px;
    padding: 11px; border-radius: 12px; border:1px solid var(--border);
    background: var(--surface-2); color: var(--text); font-size:13.5px;
    cursor:pointer; transition: border-color .2s, transform .15s;
  }
  .social button:hover{ border-color:#3A4560; transform: translateY(-2px); }
  .social button:active{ transform: translateY(0) scale(.97); }

  .view{ display:none; }
  .view.active{ display:flex; flex-direction:column; animation: viewIn .45s var(--ease); }
  @keyframes viewIn{
    from{ opacity:0; transform: translateY(8px); }
    to{ opacity:1; transform: translateY(0); }
  }

  .toast{
    position: fixed; left:50%; bottom: 28px; transform: translateX(-50%) translateY(20px);
    background: var(--surface-2); border:1px solid var(--border); color: var(--text);
    padding: 12px 18px; border-radius: 12px; font-size: 13.5px;
    display:flex; align-items:center; gap:10px;
    opacity:0; pointer-events:none; transition: all .35s var(--bouncy);
    z-index: 10; box-shadow: 0 20px 40px -20px #000;
  }
  .toast.show{ opacity:1; transform: translateX(-50%) translateY(0); pointer-events:auto; }
  .toast .tick{
    width:18px; height:18px; border-radius:50%; background: var(--cyan);
    display:flex; align-items:center; justify-content:center; color:#0A0E16; font-size:12px; flex:none;
  }

  .particle{
    position: fixed; z-index: 9; pointer-events:none; border-radius:50%;
    animation: pop .8s var(--ease) forwards;
  }
  @keyframes pop{
    0%{ transform: translate(0,0) scale(1); opacity:1; }
    100%{ transform: translate(var(--dx), var(--dy)) scale(0); opacity:0; }
  }

  @media (max-width: 760px){
    .stage{ padding: 18px 14px; align-items: flex-start; }
    .panel{ grid-template-columns: 1fr; min-height: auto; transform:none !important; margin-top: 10px; }
    .brand{ display:none; }
    .formside{ padding: 28px 20px 32px; }
    .mascot{ width: 80px; height: 68px; }
    .mascot-wrap{ margin-bottom: 2px; }
    .mode-head{ margin-bottom: 16px; }
    .mode-head h2{ font-size: 20px; }
    form{ gap: 11px; }
    .field input{ padding: 13px 14px; font-size: 16px; }
    .submit{ padding: 14px 16px; }
  }

  @media (prefers-reduced-motion: reduce){
    .orb, .panel, .field, .view, .mascot *{ animation: none !important; transition: none !important; }
  }
</style>
</head>
<body>

<div class="backdrop">
  <div class="orb a" id="orbA"></div>
  <div class="orb b" id="orbB"></div>
  <div class="orb c" id="orbC"></div>
</div>
<div class="grain"></div>

<div class="stage" id="stage">
  <div class="panel" id="panel" data-mode="login">

    <div class="brand">
      <div class="mark"><span class="dot"></span> Akun.io</div>
      <div>
        <h1 id="brandHeading">Masuk dan lanjutkan dari titik terakhirmu.</h1>
        <p id="brandSub">Satu akun untuk semua project kamu — cepat, aman, dan tanpa ribet.</p>
      </div>
      <div class="foot">
        <div class="avatars"><span></span><span></span><span></span></div>
        Dipakai oleh ribuan developer
      </div>
    </div>

    <div class="formside">

      <div class="mascot-wrap">
        <svg class="mascot" id="mascot" viewBox="0 0 200 160">
          <ellipse class="glow" cx="100" cy="85" rx="80" ry="70" fill="#7C5CFC" opacity="0.12"/>
          <path class="ear ear-l" d="M55 55 C35 30 30 5 45 8 C68 12 72 45 70 60 Z" fill="#C97B4A"/>
          <path class="ear ear-r" d="M145 55 C165 30 170 5 155 8 C132 12 128 45 130 60 Z" fill="#C97B4A"/>
          <ellipse cx="100" cy="92" rx="62" ry="52" fill="#F0A868"/>
          <ellipse cx="100" cy="128" rx="34" ry="24" fill="#FBDCB6"/>
          <g class="eye-l">
            <circle cx="78" cy="82" r="15" fill="#1B1030"/>
            <circle class="pupil pupil-l" cx="78" cy="82" r="6.5" fill="#0A0E16"/>
            <circle cx="81" cy="78" r="2" fill="#fff"/>
          </g>
          <g class="eye-r">
            <circle cx="122" cy="82" r="15" fill="#1B1030"/>
            <circle class="pupil pupil-r" cx="122" cy="82" r="6.5" fill="#0A0E16"/>
            <circle cx="125" cy="78" r="2" fill="#fff"/>
          </g>
          <ellipse cx="100" cy="122" rx="10" ry="7" fill="#7A4B2E"/>
          <path class="mouth" d="M88 138 Q100 146 112 138" stroke="#7A4B2E" stroke-width="3" fill="none" stroke-linecap="round"/>
          <g class="paw paw-l">
            <ellipse cx="78" cy="84" rx="20" ry="18" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="66" cy="69" r="7" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="78" cy="64" r="7.5" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="90" cy="69" r="7" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
          </g>
          <g class="paw paw-r">
            <ellipse cx="122" cy="84" rx="20" ry="18" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="110" cy="69" r="7" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="122" cy="64" r="7.5" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
            <circle cx="134" cy="69" r="7" fill="#F0A868" stroke="#C97B4A" stroke-width="2"/>
          </g>
        </svg>
      </div>

      <div class="view active" id="loginView">
        <div class="mode-head">
          <h2>Masuk ke akunmu</h2>
          <p>Belum punya akun? <a data-switch="register">Daftar sekarang</a></p>
        </div>
        <form id="loginForm" novalidate>
          <div class="field" data-field="email">
            <input type="email" class="track-eyes" placeholder="Alamat email" autocomplete="email">
            <div class="msg">Masukkan email yang valid</div>
          </div>
          <div class="field" data-field="password">
            <input type="password" class="pw-input" placeholder="Kata sandi" autocomplete="current-password">
            <button type="button" class="toggle-eye" aria-label="Tampilkan kata sandi">👁</button>
            <div class="msg">Kata sandi minimal 6 karakter</div>
          </div>
          <div class="row-between">
            <label class="remember"><input type="checkbox"> Ingat saya</label>
            <a href="#">Lupa kata sandi?</a>
          </div>
          <button class="submit" type="submit"><span>Masuk</span><div class="spinner"></div></button>
        </form>
        <div class="divider">atau lanjutkan dengan</div>
        <div class="social">
          <button type="button">Google</button>
          <button type="button">GitHub</button>
        </div>
      </div>

      <div class="view" id="registerView">
        <div class="mode-head">
          <h2>Buat akun baru</h2>
          <p>Sudah punya akun? <a data-switch="login">Masuk di sini</a></p>
        </div>
        <form id="registerForm" novalidate>
          <div class="field" data-field="name">
            <input type="text" class="track-eyes" placeholder="Nama lengkap" autocomplete="name">
            <div class="msg">Nama tidak boleh kosong</div>
          </div>
          <div class="field" data-field="email">
            <input type="email" class="track-eyes" placeholder="Alamat email" autocomplete="email">
            <div class="msg">Masukkan email yang valid</div>
          </div>
          <div class="field" data-field="password">
            <input type="password" class="pw-input" placeholder="Buat kata sandi" autocomplete="new-password">
            <button type="button" class="toggle-eye" aria-label="Tampilkan kata sandi">👁</button>
            <div class="msg">Kata sandi minimal 6 karakter</div>
          </div>
          <button class="submit" type="submit"><span>Buat akun</span><div class="spinner"></div></button>
        </form>
        <div class="divider">atau daftar dengan</div>
        <div class="social">
          <button type="button">Google</button>
          <button type="button">GitHub</button>
        </div>
      </div>

    </div>
  </div>
</div>

<div class="toast" id="toast"><span class="tick">✓</span><span id="toastMsg">Berhasil</span></div>

<script>
  const panel = document.getElementById('panel');
  const stage = document.getElementById('stage');
  const loginView = document.getElementById('loginView');
  const registerView = document.getElementById('registerView');
  const brandHeading = document.getElementById('brandHeading');
  const brandSub = document.getElementById('brandSub');
  const mascot = document.getElementById('mascot');

  const copy = {
    login: {
      heading: 'Masuk dan lanjutkan dari titik terakhirmu.',
      sub: 'Satu akun untuk semua project kamu — cepat, aman, dan tanpa ribet.'
    },
    register: {
      heading: 'Mulai dalam hitungan detik.',
      sub: 'Buat akun sekali, pakai di semua tools dan bot yang kamu bangun.'
    }
  };

  function switchMode(mode){
    panel.dataset.mode = mode;
    const showRegister = mode === 'register';
    loginView.classList.toggle('active', !showRegister);
    registerView.classList.toggle('active', showRegister);
    brandHeading.style.opacity = 0;
    brandSub.style.opacity = 0;
    setTimeout(()=>{
      brandHeading.textContent = copy[mode].heading;
      brandSub.textContent = copy[mode].sub;
      brandHeading.style.opacity = 1;
      brandSub.style.opacity = 1;
    }, 180);
    mascot.classList.remove('covering','peeking');
  }

  document.querySelectorAll('[data-switch]').forEach(el=>{
    el.addEventListener('click', ()=> switchMode(el.dataset.switch));
  });

  /* ---------- Mascot behaviour ---------- */
  document.querySelectorAll('.track-eyes').forEach(input=>{
    input.addEventListener('focus', ()=> mascot.classList.add('attentive'));
    input.addEventListener('blur', ()=> mascot.classList.remove('attentive'));
    input.addEventListener('input', ()=>{
      const shift = Math.max(-5, Math.min(5, input.value.length * 0.6 - 2));
      mascot.querySelectorAll('.pupil').forEach(p=> p.style.transform = `translateX(${shift}px)`);
    });
  });

  document.querySelectorAll('.pw-input').forEach(input=>{
    let blurTimer;
    input.addEventListener('focus', ()=>{
      clearTimeout(blurTimer);
      mascot.classList.remove('attentive');
      mascot.classList.toggle('covering', input.type === 'password');
    });
    input.addEventListener('blur', ()=>{
      blurTimer = setTimeout(()=> mascot.classList.remove('covering'), 100);
    });
  });

  document.querySelectorAll('.toggle-eye').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const input = btn.previousElementSibling;
      const willShow = input.type === 'password';
      input.type = willShow ? 'text' : 'password';
      btn.textContent = willShow ? '🙈' : '👁';
      mascot.classList.toggle('covering', !willShow);
      input.focus();
    });
  });

  /* ---------- Idle blink (mata berkedip sendiri) ---------- */
  setInterval(()=>{
    if(mascot.classList.contains('covering')) return;
    mascot.classList.add('blinking');
    setTimeout(()=> mascot.classList.remove('blinking'), 130);
  }, 2800 + Math.random()*1800);

  /* ---------- Parallax orbs + card tilt (mouse), throttled via rAF ---------- */
  const orbA = document.getElementById('orbA');
  const orbB = document.getElementById('orbB');
  const orbC = document.getElementById('orbC');
  let rafId = null, pendingPx = 0, pendingPy = 0;

  function applyParallax(){
    rafId = null;
    if(window.innerWidth > 760){
      panel.style.transform = `rotateY(${pendingPx*4}deg) rotateX(${-pendingPy*4}deg)`;
    }
    orbA.style.transform = `translate(${pendingPx*20}px, ${pendingPy*20}px)`;
    orbB.style.transform = `translate(${-pendingPx*24}px, ${-pendingPy*24}px)`;
    orbC.style.transform = `translate(${pendingPx*14}px, ${pendingPy*14}px)`;
  }

  stage.addEventListener('mousemove', (e)=>{
    const r = stage.getBoundingClientRect();
    pendingPx = (e.clientX - r.left) / r.width - 0.5;
    pendingPy = (e.clientY - r.top) / r.height - 0.5;
    if(rafId === null) rafId = requestAnimationFrame(applyParallax);
  });
  stage.addEventListener('mouseleave', ()=>{
    panel.style.transform = 'rotateY(0) rotateX(0)';
  });

  stage.addEventListener('touchmove', (e)=>{
    const t = e.touches[0];
    const r = stage.getBoundingClientRect();
    pendingPx = ((t.clientX - r.left) / r.width - 0.5) * 0.8;
    pendingPy = ((t.clientY - r.top) / r.height - 0.5) * 0.8;
    if(rafId === null) rafId = requestAnimationFrame(applyParallax);
  }, { passive: true });

  /* ---------- Button ripple ---------- */
  document.querySelectorAll('.submit, .social button').forEach(btn=>{
    btn.addEventListener('click', function(e){
      const r = this.getBoundingClientRect();
      const ripple = document.createElement('span');
      ripple.className = 'ripple';
      ripple.style.left = (e.clientX - r.left - 10) + 'px';
      ripple.style.top = (e.clientY - r.top - 10) + 'px';
      ripple.style.width = ripple.style.height = '20px';
      this.appendChild(ripple);
      setTimeout(()=> ripple.remove(), 650);
    });
  });

  /* ---------- Validation + fake submit + confetti ---------- */
  function validateField(fieldEl, rule){
    const input = fieldEl.querySelector('input');
    const valid = rule(input.value.trim());
    fieldEl.classList.toggle('invalid', !valid);
    return valid;
  }

  function showToast(msg){
    const toast = document.getElementById('toast');
    document.getElementById('toastMsg').textContent = msg;
    toast.classList.add('show');
    setTimeout(()=> toast.classList.remove('show'), 2600);
  }

  function burstConfetti(){
    const colors = ['#7C5CFC','#45E0D0','#FF6B9D'];
    const originX = window.innerWidth/2, originY = window.innerHeight - 60;
    for(let i=0;i<18;i++){
      const p = document.createElement('div');
      p.className = 'particle';
      const size = 5 + Math.random()*5;
      p.style.width = p.style.height = size + 'px';
      p.style.left = originX + 'px';
      p.style.top = originY + 'px';
      p.style.background = colors[i % colors.length];
      const angle = Math.random()*Math.PI*2;
      const dist = 60 + Math.random()*80;
      p.style.setProperty('--dx', Math.cos(angle)*dist + 'px');
      p.style.setProperty('--dy', Math.sin(angle)*dist - 40 + 'px');
      document.body.appendChild(p);
      setTimeout(()=> p.remove(), 850);
    }
  }

  function handleSubmit(formEl, rules, successMsg){
    formEl.addEventListener('submit', (e)=>{
      e.preventDefault();
      let allValid = true;
      Object.entries(rules).forEach(([name, rule])=>{
        const fieldEl = formEl.querySelector(`[data-field="${name}"]`);
        if(!validateField(fieldEl, rule)) allValid = false;
      });
      if(!allValid) return;

      const btn = formEl.querySelector('.submit');
      btn.classList.add('loading');
      btn.disabled = true;

      // --- Ganti bagian ini dengan pemanggilan API/Supabase Auth kamu ---
      setTimeout(()=>{
        btn.classList.remove('loading');
        btn.disabled = false;
        mascot.querySelector('.mouth').setAttribute('d','M86 136 Q100 150 114 136');
        showToast(successMsg);
        burstConfetti();
        formEl.reset();
        setTimeout(()=> mascot.querySelector('.mouth').setAttribute('d','M88 138 Q100 146 112 138'), 1800);
      }, 1100);
    });
  }

  const isEmail = v => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v);
  const minLen = n => v => v.length >= n;
  const notEmpty = v => v.length > 0;

  handleSubmit(document.getElementById('loginForm'), {
    email: isEmail,
    password: minLen(6)
  }, 'Berhasil masuk!');

  handleSubmit(document.getElementById('registerForm'), {
    name: notEmpty,
    email: isEmail,
    password: minLen(6)
  }, 'Akun berhasil dibuat!');
</script>

</body>
</html>
