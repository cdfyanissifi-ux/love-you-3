<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Love Forever</title>
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
    }
    body {
      background: #1a003a;
      font-family: system-ui, -apple-system, sans-serif;
    }
    #bg {
      position: fixed;
      inset: 0;
      background:
        radial-gradient(ellipse at 30% 20%, #3d0070 0%, #1a003a 40%, #0d001f 100%),
        radial-gradient(circle at 70% 80%, #6b00c4 0%, transparent 50%),
        radial-gradient(circle at 20% 60%, #9b00e8 0%, transparent 40%);
      animation: bgPulse 8s ease-in-out infinite alternate;
      z-index: 0;
    }
    @keyframes bgPulse {
      0%   { filter: brightness(0.8); }
      100% { filter: brightness(1.15); }
    }
    #stars-canvas {
      position: fixed;
      inset: 0;
      z-index: 1;
      pointer-events: none;
    }
    #ui {
      position: fixed;
      inset: 0;
      z-index: 2;
      pointer-events: none;
    }
    .love {
      position: absolute;
    }
    .love_word {
      color: #f0a0cc;
      transform: translateY(-100%) rotateZ(-30deg);
      text-shadow: 0 0 10px #fff, 0 0 20px #c040ff;
      letter-spacing: 2px;
      white-space: nowrap;
    }
    .love_horizontal {
      animation: loveH 10000ms infinite alternate ease-in-out;
    }
    .love_vertical {
      animation: loveV 20000ms infinite linear;
    }
    #music-overlay {
      position: fixed;
      inset: 0;
      z-index: 9999;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 24px;
      background: rgba(10, 0, 30, 0.88);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      transition: opacity 0.8s ease;
    }
    #music-overlay.hidden {
      opacity: 0;
      pointer-events: none;
    }
    .overlay-title {
      color: #f0a0cc;
      font-size: clamp(1.4rem, 5vw, 2.2rem);
      letter-spacing: 5px;
      text-transform: uppercase;
      text-align: center;
      animation: titleGlow 2s ease-in-out infinite alternate;
    }
    @keyframes titleGlow {
      from { text-shadow: 0 0 10px #c040ff, 0 0 20px #ea80b0; }
      to   { text-shadow: 0 0 30px #c040ff, 0 0 60px #ea80b0, 0 0 80px #ff80cc; }
    }
    #start-btn {
      background: transparent;
      border: 2px solid #ea80b0;
      color: #ea80b0;
      font-size: clamp(0.85rem, 3vw, 1rem);
      letter-spacing: 3px;
      text-transform: uppercase;
      padding: 14px 38px;
      border-radius: 50px;
      cursor: pointer;
      text-shadow: 0 0 10px #ea80b0;
      animation: pulseBtn 2s infinite;
      -webkit-appearance: none;
      transition: background 0.3s;
    }
    #start-btn:hover, #start-btn:active { background: #ea80b022; }
    @keyframes pulseBtn {
      0%, 100% { box-shadow: 0 0 20px #ea80b066, inset 0 0 20px #ea80b011; }
      50%       { box-shadow: 0 0 40px #ea80b0aa, inset 0 0 30px #ea80b033; }
    }
    #mute-btn {
      position: fixed;
      bottom: 24px;
      right: 20px;
      z-index: 1000;
      background: rgba(80, 0, 140, 0.4);
      border: 1px solid #ea80b066;
      color: #ea80b0;
      font-size: 18px;
      width: 46px;
      height: 46px;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      align-items: center;
      justify-content: center;
      opacity: 0.75;
      transition: opacity 0.3s;
      -webkit-appearance: none;
    }
    #mute-btn:hover, #mute-btn:active { opacity: 1; }
    #mute-btn.visible { display: flex; }
  </style>
</head>
<body>

<div id="bg"></div>
<canvas id="stars-canvas"></canvas>
<div id="ui"></div>

<div id="music-overlay">
  <div class="overlay-title">My Love For You</div>
  <button id="start-btn">&#9829; Entrer &#9829;</button>
</div>

<button id="mute-btn" title="Mute / Unmute">&#128266;</button>

<audio id="bg-music" loop preload="auto">
  <source src="inmark.mp3" type="audio/mpeg">
</audio>

<script>
// ── Étoiles canvas ──
(function() {
  var canvas = document.getElementById('stars-canvas');
  var ctx = canvas.getContext('2d');
  var stars = [];

  function resize() {
    canvas.width  = window.innerWidth;
    canvas.height = window.innerHeight;
    stars = [];
    for (var i = 0; i < 60; i++) {
      stars.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        r: Math.random() * 1.5 + 0.3,
        speed: Math.random() * 0.015 + 0.005,
        phase: Math.random() * Math.PI * 2
      });
    }
  }

  function draw(t) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (var i = 0; i < stars.length; i++) {
      var s = stars[i];
      var alpha = 0.1 + 0.7 * (0.5 + 0.5 * Math.sin(t * s.speed + s.phase));
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
      ctx.fillStyle = 'rgba(255,255,255,' + alpha + ')';
      ctx.fill();
    }
    requestAnimationFrame(draw);
  }

  resize();
  requestAnimationFrame(draw);
  window.addEventListener('resize', resize);
})();

// ── Amours flottants ──
(function() {
  var ui = document.getElementById('ui');
  var N  = 100;

  function rebuild() {
    var W = window.innerWidth;
    var H = window.innerHeight;
    // span = taille du coeur, adapté à la plus petite dimension
    var span = Math.min(W, H) * 0.88;
    var originX = (W - span) / 2;
    var originY = (H - span) / 2;
    var fs = Math.max(10, Math.min(18, span * 0.030)) + 'px';

    // Keyframes dynamiques
    var old = document.getElementById('dkf');
    if (old) old.remove();
    var style = document.createElement('style');
    style.id = 'dkf';
    var s = span;
    style.textContent = [
      '@keyframes loveH {',
      '  from { transform: translateX(0px); }',
      '  to   { transform: translateX(' + s + 'px); }',
      '}',
      '@keyframes loveV {',
      '  0%    { transform: translateY(' + (s*0.40) + 'px); }',
      '  10%   { transform: translateY(' + (s*0.10) + 'px); }',
      '  15%   { transform: translateY(' + (s*0.01) + 'px); }',
      '  18%   { transform: translateY(0px); }',
      '  20%   { transform: translateY(' + (s*0.01) + 'px); }',
      '  22%   { transform: translateY(' + (s*0.077) + 'px); }',
      '  24%   { transform: translateY(' + (s*0.143) + 'px); }',
      '  25%   { transform: translateY(' + (s*0.25) + 'px); }',
      '  26%   { transform: translateY(' + (s*0.143) + 'px); }',
      '  28%   { transform: translateY(' + (s*0.077) + 'px); }',
      '  30%   { transform: translateY(' + (s*0.01) + 'px); }',
      '  32%   { transform: translateY(0px); }',
      '  35%   { transform: translateY(' + (s*0.01) + 'px); }',
      '  40%   { transform: translateY(' + (s*0.10) + 'px); }',
      '  50%   { transform: translateY(' + (s*0.40) + 'px); }',
      '  71%   { transform: translateY(' + (s*0.953) + 'px); }',
      '  72.5% { transform: translateY(' + (s*0.980) + 'px); }',
      '  75%   { transform: translateY(' + s + 'px); }',
      '  77.5% { transform: translateY(' + (s*0.980) + 'px); }',
      '  79%   { transform: translateY(' + (s*0.953) + 'px); }',
      '  100%  { transform: translateY(' + (s*0.40) + 'px); }',
      '}'
    ].join('\n');
    document.head.appendChild(style);

    ui.innerHTML = '';
    for (var i = 1; i <= N; i++) {
      var love = document.createElement('div');
      love.className = 'love';
      love.style.left = originX + 'px';
      love.style.top  = originY + 'px';

      var delay = (i * -300) + 'ms';

      var h = document.createElement('div');
      h.className = 'love_horizontal';
      h.style.animationDelay = delay;

      var v = document.createElement('div');
      v.className = 'love_vertical';
      v.style.animationDelay = delay;

      var word = document.createElement('div');
      word.className = 'love_word';
      word.style.fontSize = fs;
      word.textContent = 'I love you';

      v.appendChild(word);
      h.appendChild(v);
      love.appendChild(h);
      ui.appendChild(love);
    }
  }

  rebuild();
  var t;
  window.addEventListener('resize', function() {
    clearTimeout(t);
    t = setTimeout(rebuild, 200);
  });
})();

// ── Musique ──
var audio    = document.getElementById('bg-music');
var overlay  = document.getElementById('music-overlay');
var startBtn = document.getElementById('start-btn');
var muteBtn  = document.getElementById('mute-btn');

audio.loop = true;

startBtn.addEventListener('click', function() {
  audio.play().catch(function(){});
  overlay.classList.add('hidden');
  setTimeout(function(){ overlay.style.display = 'none'; }, 900);
  muteBtn.classList.add('visible');
});

muteBtn.addEventListener('click', function() {
  audio.muted = !audio.muted;
  muteBtn.innerHTML = audio.muted ? '&#128263;' : '&#128266;';
});
</script>
</body>
</html>
