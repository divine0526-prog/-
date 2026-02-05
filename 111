<!doctype html>
<html lang="zh-Hant">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>尾牙轉盤抽獎（動畫版）</title>
  <style>
    :root{
      --bg:#0b1020;
      --card:#121a33;
      --text:#eaf0ff;
      --muted:#a9b5d6;
      --gold:#ffd166;
      --accent:#ff4d6d;
      --ok:#00d4ff;
    }
    *{box-sizing:border-box;}
    body{
      margin:0;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans TC", Arial, sans-serif;
      background: radial-gradient(1200px 600px at 50% -10%, #2a3b88 0%, var(--bg) 55%, #070a12 100%);
      color:var(--text);
      min-height:100vh;
      display:flex;
      justify-content:center;
      padding:20px 14px 26px;
      overflow-x:hidden;
    }
    .wrap{ width:min(560px, 100%); display:flex; flex-direction:column; gap:14px; }
    .title{ text-align:center; font-weight:900; letter-spacing:.06em; font-size:28px; margin-top:6px; }
    .sub{ text-align:center; color:var(--muted); font-size:14px; margin-top:-6px; }

    .card{
      background: rgba(18,26,51,.72);
      border: 1px solid rgba(255,255,255,.08);
      box-shadow: 0 18px 60px rgba(0,0,0,.35);
      border-radius:22px;
      padding:16px;
      position:relative;
      overflow:hidden;
    }

    /* 霓虹邊框光暈 */
    .card::before{
      content:"";
      position:absolute; inset:-2px;
      border-radius:24px;
      background: conic-gradient(from 0deg,
        rgba(255,209,102,.0), rgba(255,209,102,.9), rgba(0,212,255,.9),
        rgba(255,77,109,.9), rgba(255,209,102,.9), rgba(255,209,102,.0));
      filter: blur(10px);
      opacity:.55;
      animation: glow 4s linear infinite;
      pointer-events:none;
    }
    @keyframes glow{ to { transform: rotate(360deg); } }

    .stage{ display:flex; justify-content:center; padding:10px 0 6px; }
    .wheelBox{
      position:relative;
      width:min(380px, 88vw);
      aspect-ratio:1/1;
      display:flex;
      align-items:center;
      justify-content:center;
      margin: 0 auto;
      user-select:none;
    }
    .pointer{
      width:0;height:0;
      border-left:14px solid transparent;
      border-right:14px solid transparent;
      border-bottom:26px solid var(--gold);
      filter: drop-shadow(0 8px 10px rgba(0,0,0,.35));
      position:absolute;
      top: -6px;
      left: 50%;
      transform: translateX(-50%);
      z-index: 6;
    }

    canvas#wheel{
      width:100%;
      height:100%;
      border-radius:50%;
      background:rgba(255,255,255,.04);
      box-shadow: inset 0 0 0 10px rgba(255,255,255,.05),
                  0 24px 60px rgba(0,0,0,.35);
      position:relative;
      z-index:2;
    }
    .center{
      position:absolute;
      width:34%;
      aspect-ratio:1/1;
      border-radius:50%;
      background: radial-gradient(circle at 30% 30%, #fff 0%, #ffe9a8 18%, #ffcf58 45%, #b37a00 100%);
      box-shadow: 0 14px 30px rgba(0,0,0,.35);
      display:flex;
      align-items:center;
      justify-content:center;
      color:#2a1a00;
      font-weight:1000;
      letter-spacing:.10em;
      text-align:center;
      padding:8px;
      z-index:5;
      transform: translateZ(0);
    }

    .controls{
      display:flex;
      flex-direction:column;
      gap:10px;
      margin-top:10px;
      position:relative;
      z-index:3;
    }
    .btn{
      width:100%;
      border:none;
      border-radius:16px;
      padding:14px 14px;
      font-size:20px;
      font-weight:900;
      letter-spacing:.06em;
      color:#071018;
      background: linear-gradient(135deg, var(--gold), #ff8fab);
      box-shadow: 0 12px 30px rgba(0,0,0,.35);
      cursor:pointer;
      -webkit-tap-highlight-color: transparent;
      transform: translateZ(0);
    }
    .btn:active{ transform: scale(.985); }
    .btn:disabled{
      opacity:.45;
      cursor:not-allowed;
      filter: grayscale(.2);
    }
    .btn.secondary{
      background: linear-gradient(135deg, rgba(255,255,255,.18), rgba(255,255,255,.08));
      color: var(--text);
      border: 1px solid rgba(255,255,255,.14);
      box-shadow:none;
      font-weight:800;
      font-size:16px;
      padding:12px 14px;
    }
    .row{ display:flex; gap:10px; }
    .row .secondary{ flex:1; }

    .status{
      text-align:center;
      color:var(--muted);
      font-size:14px;
      line-height:1.4;
      margin-top:2px;
    }

    .result{
      text-align:center;
      font-size:30px;
      font-weight:1000;
      margin-top:6px;
      color: var(--ok);
      text-shadow: 0 10px 24px rgba(0,212,255,.15);
      min-height: 46px;
    }
    .result.pop{
      animation: pop .65s cubic-bezier(.2,.9,.2,1) both;
    }
    @keyframes pop{
      0%{ transform: translateY(10px) scale(.92); opacity:0; }
      60%{ transform: translateY(-2px) scale(1.06); opacity:1; }
      100%{ transform: translateY(0) scale(1); }
    }

    .list{ margin-top:10px; display:grid; grid-template-columns: 1fr; gap:8px; }
    .item{
      background: rgba(255,255,255,.06);
      border: 1px solid rgba(255,255,255,.08);
      border-radius:14px;
      padding:10px 12px;
      display:flex;
      justify-content:space-between;
      align-items:center;
      font-size:14px;
      color:var(--text);
    }
    .tag{
      font-weight:1000;
      color:#071018;
      background: rgba(255,209,102,.95);
      padding:4px 10px;
      border-radius:999px;
      letter-spacing:.04em;
      min-width: 72px;
      text-align:center;
    }

    /* 倒數遮罩 */
    .overlay{
      position:fixed;
      inset:0;
      display:none;
      align-items:center;
      justify-content:center;
      background: rgba(0,0,0,.45);
      backdrop-filter: blur(4px);
      z-index: 999;
    }
    .overlay.show{ display:flex; }
    .count{
      font-size:88px;
      font-weight:1000;
      letter-spacing:.08em;
      color: #fff;
      text-shadow: 0 20px 60px rgba(0,0,0,.5);
      animation: countPop .9s cubic-bezier(.2,.9,.2,1) both;
    }
    @keyframes countPop{
      0%{ transform: scale(.7); opacity:0; }
      55%{ transform: scale(1.08); opacity:1; }
      100%{ transform: scale(1); opacity:1; }
    }

    /* confetti 層 */
    canvas#fx{
      position:fixed;
      inset:0;
      width:100%;
      height:100%;
      pointer-events:none;
      z-index: 1000;
    }
    .hint{
      font-size:12px;
      color:rgba(233,240,255,.72);
      text-align:center;
      margin-top:10px;
    }
  </style>
</head>
<body>
  <canvas id="fx"></canvas>

  <div class="overlay" id="overlay">
    <div class="count" id="count">3</div>
  </div>

  <div class="wrap">
    <div class="title">尾牙轉盤抽獎 🎡（動畫版）</div>
    <div class="sub">7 人輪流按「開始抽」｜抽過不重複｜倒數＋彩帶＋音效</div>

    <div class="card">
      <div class="stage">
        <div class="wheelBox">
          <div class="pointer"></div>
          <canvas id="wheel" width="900" height="900"></canvas>
          <div class="center">按<br>開<br>始</div>
        </div>
      </div>

      <div class="controls">
        <button id="spinBtn" class="btn">開始抽 🎉</button>
        <div class="row">
          <button id="resetBtn" class="btn secondary">重置</button>
          <button id="soundBtn" class="btn secondary">音效：開</button>
        </div>
        <div class="status" id="status">剩餘獎項：7 ｜ 下一位：第 <b id="turn">1</b> 位</div>
        <div class="result" id="result"></div>

        <div class="list" id="history"></div>
        <div class="hint">手機：建議用瀏覽器「加入主畫面」→ 看起來像 App</div>
      </div>
    </div>
  </div>

<script>
(() => {
  // ====== 獎項池（抽過不重複） ======
  const initialPool = ["頭獎","二獎","二獎","三獎","三獎","四獎","四獎"];

  let pool = [...initialPool];
  let turn = 1;
  let spinning = false;
  let soundOn = true;

  const canvas = document.getElementById('wheel');
  const ctx = canvas.getContext('2d');
  const spinBtn = document.getElementById('spinBtn');
  const resetBtn = document.getElementById('resetBtn');
  const soundBtn = document.getElementById('soundBtn');
  const statusEl = document.getElementById('status');
  const turnEl = document.getElementById('turn');
  const resultEl = document.getElementById('result');
  const historyEl = document.getElementById('history');

  // overlay countdown
  const overlay = document.getElementById('overlay');
  const countEl = document.getElementById('count');

  // ====== WebAudio 音效 ======
  let audioCtx = null;
  const ensureAudio = () => {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  };

  const beep = (freq=880, dur=0.06, type='sine', gain=0.03) => {
    if (!soundOn) return;
    ensureAudio();
    const o = audioCtx.createOscillator();
    const g = audioCtx.createGain();
    o.type = type;
    o.frequency.value = freq;
    g.gain.value = gain;
    o.connect(g); g.connect(audioCtx.destination);
    o.start();
    o.stop(audioCtx.currentTime + dur);
  };

  const fanfare = () => {
    if (!soundOn) return;
    const seq = [523, 659, 784, 988];
    seq.forEach((f, i) => setTimeout(()=>beep(f, 0.09, 'triangle', 0.045), i*95));
    setTimeout(()=>beep(1319, 0.20, 'sawtooth', 0.035), 430);
  };

  const vibrate = (ms) => {
    try { if (navigator.vibrate) navigator.vibrate(ms); } catch {}
  };

  // ====== Confetti（無外部套件） ======
  const fx = document.getElementById('fx');
  const fctx = fx.getContext('2d');
  const DPR = Math.max(1, Math.min(2, window.devicePixelRatio || 1));
  let confetti = [];

  const resizeFx = () => {
    fx.width = Math.floor(window.innerWidth * DPR);
    fx.height = Math.floor(window.innerHeight * DPR);
  };
  window.addEventListener('resize', resizeFx);
  resizeFx();

  const rand = (a,b)=>a+Math.random()*(b-a);
  const fxColors = [
    "rgba(255,209,102,.95)","rgba(0,212,255,.90)","rgba(255,77,109,.88)",
    "rgba(166,255,195,.78)","rgba(170,160,255,.78)","rgba(255,180,245,.78)"
  ];

  const burstConfetti = (power=170) => {
    const cx = fx.width/2, cy = fx.height*0.18;
    const count = Math.floor(power);
    for (let i=0;i<count;i++){
      confetti.push({
        x: cx + rand(-80,80),
        y: cy + rand(-20,20),
        vx: rand(-4.2,4.2) * DPR,
        vy: rand(2.0,7.0) * DPR,
        g: rand(0.06,0.12) * DPR,
        s: rand(6,14) * DPR,
        r: rand(0,Math.PI),
        vr: rand(-0.18,0.18),
        life: rand(70,130),
        c: fxColors[i % fxColors.length],
        shape: Math.random() < 0.55 ? "rect" : "circle"
      });
    }
  };

  const stepFx = () => {
    fctx.clearRect(0,0,fx.width,fx.height);
    confetti = confetti.filter(p => p.life > 0);
    for (const p of confetti){
      p.life -= 1;
      p.x += p.vx;
      p.y += p.vy;
      p.vy += p.g;
      p.r += p.vr;

      fctx.save();
      fctx.translate(p.x, p.y);
      fctx.rotate(p.r);
      fctx.fillStyle = p.c;

      if (p.shape === "rect"){
        fctx.fillRect(-p.s/2, -p.s/2, p.s, p.s*0.6);
      }else{
        fctx.beginPath();
        fctx.arc(0,0,p.s*0.35,0,Math.PI*2);
        fctx.fill();
      }
      fctx.restore();
    }
    requestAnimationFrame(stepFx);
  };
  stepFx();

  // ====== 轉盤繪製 ======
  const colors = [
    "rgba(255,209,102,.95)",
    "rgba(0,212,255,.90)",
    "rgba(255,77,109,.88)",
    "rgba(166,255,195,.75)",
    "rgba(170,160,255,.78)",
    "rgba(255,180,245,.75)",
  ];

  const displayLabels = () => {
    const counts = {};
    return pool.map(p => {
      counts[p] = (counts[p] || 0) + 1;
      return `${p} #${counts[p]}`;
    });
  };

  let wheelAngle = 0;
  let targetAngle = 0;
  let lastTickAt = 0;

  const drawWheel = (labels, angle) => {
    const W = canvas.width, H = canvas.height;
    const cx = W/2, cy = H/2;
    const r = Math.min(W,H)*0.46;

    ctx.clearRect(0,0,W,H);
    ctx.save();
    ctx.translate(cx, cy);
    ctx.rotate(angle);

    const n = labels.length || 1;
    const arc = (Math.PI*2)/n;

    for (let i=0;i<n;i++){
      const start = i*arc;
      const end = start + arc;

      ctx.beginPath();
      ctx.moveTo(0,0);
      ctx.arc(0,0,r,start,end);
      ctx.closePath();
      ctx.fillStyle = colors[i % colors.length];
      ctx.fill();

      ctx.strokeStyle = "rgba(7,16,24,.18)";
      ctx.lineWidth = 10;
      ctx.stroke();

      ctx.save();
      ctx.rotate(start + arc/2);
      ctx.textAlign = "right";
      ctx.fillStyle = "rgba(7,16,24,.9)";
      ctx.font = "1000 48px system-ui, -apple-system, Segoe UI, Roboto, Noto Sans TC, Arial";
      ctx.fillText(labels[i], r - 30, 16);
      ctx.restore();
    }

    ctx.beginPath();
    ctx.arc(0,0,r+8,0,Math.PI*2);
    ctx.strokeStyle = "rgba(255,255,255,.18)";
    ctx.lineWidth = 14;
    ctx.stroke();

    ctx.restore();
  };

  const updateStatus = () => {
    turnEl.textContent = String(turn);
    statusEl.innerHTML = `剩餘獎項：${pool.length} ｜ 下一位：第 <b id="turn">${turn}</b> 位`;
  };

  const addHistory = (who, prize) => {
    const div = document.createElement('div');
    div.className = 'item';
    div.innerHTML = `<div>第 <b>${who}</b> 位</div><div class="tag">${prize}</div>`;
    historyEl.prepend(div);
  };

  // ====== 公平抽獎 ======
  const pickPrize = () => {
    const idx = Math.floor(Math.random() * pool.length);
    const prize = pool[idx];
    pool.splice(idx, 1);
    return prize;
  };

  // ====== 倒數動畫 ======
  const sleep = (ms) => new Promise(r => setTimeout(r, ms));

  const countdown = async () => {
    overlay.classList.add('show');
    const seq = ["3","2","1","抽！"];
    for (let i=0;i<seq.length;i++){
      countEl.textContent = seq[i];
      countEl.style.animation = 'none';
      // reflow
      void countEl.offsetHeight;
      countEl.style.animation = '';
      beep(440 + i*110, 0.08, 'triangle', 0.05);
      vibrate(20);
      await sleep(650);
    }
    overlay.classList.remove('show');
  };

  // ====== 轉動動畫（更順） ======
  const easeOutQuint = (t) => 1 - Math.pow(1 - t, 5);

  const spinToPrize = (prize) => {
    const labels = displayLabels(); // 轉動當下顯示
    const n = labels.length;
    const arc = (Math.PI*2)/n;

    const idx = labels.findIndex(x => x.startsWith(prize + " #"));
    const pointerAngle = -Math.PI/2;
    const segmentCenter = idx*arc + arc/2;
    const desired = pointerAngle - segmentCenter;

    const spins = 7 + Math.floor(Math.random()*3); // 7~9圈更嗨
    const current = wheelAngle;

    targetAngle = desired + spins*Math.PI*2;
    while (targetAngle < current + Math.PI*2) targetAngle += Math.PI*2;

    const start = performance.now();
    const duration = 3600 + Math.floor(Math.random()*900); // 3.6~4.5s

    const tick = (now) => {
      const t = Math.min(1, (now - start) / duration);
      const eased = easeOutQuint(t);
      const ang = current + (targetAngle - current) * eased;

      // tick 音效
      if (soundOn) {
        const delta = Math.abs(ang - lastTickAt);
        if (delta > 0.20 && t < 0.985) {
          lastTickAt = ang;
          beep(380 + Math.random()*160, 0.03, 'square', 0.02);
        }
      }

      wheelAngle = ang;
      drawWheel(labels, wheelAngle);

      if (t < 1) requestAnimationFrame(tick);
      else {
        spinning = false;
        fanfare();
        burstConfetti(220);
        vibrate([30, 40, 30]);
        finish(prize);
      }
    };
    requestAnimationFrame(tick);
  };

  const finish = (prize) => {
    resultEl.classList.remove('pop');
    resultEl.textContent = `恭喜抽中：${prize}！`;
    // 觸發彈跳動畫
    void resultEl.offsetHeight;
    resultEl.classList.add('pop');

    addHistory(turn, prize);
    turn += 1;
    updateStatus();

    if (pool.length === 0) {
      spinBtn.disabled = true;
      spinBtn.textContent = "已抽完 ✅";
    } else {
      spinBtn.disabled = false;
      spinBtn.textContent = "開始抽 🎉";
    }
  };

  // ====== 事件 ======
  spinBtn.addEventListener('click', async () => {
    if (spinning) return;

    if (soundOn) {
      ensureAudio();
      if (audioCtx.state === 'suspended') {
        try { await audioCtx.resume(); } catch {}
      }
    }

    if (pool.length === 0) return;

    spinning = true;
    spinBtn.disabled = true;
    resultEl.textContent = "";

    await countdown();

    // 先抽出來確保公平
    const prize = pickPrize();

    // 讓轉盤顯示時仍包含該獎（避免 label 對不到），用「暫放回→找位置→再移除」技巧
    pool.push(prize);
    const tempPrize = prize;
    const removeIdx = pool.findIndex(p => p === tempPrize);
    pool.splice(removeIdx, 1);

    spinToPrize(tempPrize);
  });

  resetBtn.addEventListener('click', () => {
    pool = [...initialPool];
    turn = 1;
    spinning = false;
    spinBtn.disabled = false;
    spinBtn.textContent = "開始抽 🎉";
    resultEl.textContent = "";
    historyEl.innerHTML = "";
    wheelAngle = 0;
    lastTickAt = 0;
    drawWheel(displayLabels(), wheelAngle);
    updateStatus();
    beep(660, 0.06, 'triangle', 0.04);
  });

  soundBtn.addEventListener('click', async () => {
    soundOn = !soundOn;
    soundBtn.textContent = `音效：${soundOn ? "開" : "關"}`;
    if (soundOn) {
      ensureAudio();
      if (audioCtx.state === 'suspended') {
        try { await audioCtx.resume(); } catch {}
      }
      beep(880, 0.06, 'triangle', 0.04);
    }
  });

  // 初始
  drawWheel(displayLabels(), wheelAngle);
  updateStatus();
})();
</script>
</body>
</html>
