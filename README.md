<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Love Forever</title>
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
  <style>
    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background: #1a003a;
      background: radial-gradient(ellipse at 30% 20%, #3d0070 0%, #1a003a 40%, #0d001f 100%);
      min-height: 100vh;
      height: 100vh;
      margin: 0;
      overflow: hidden;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif;
    }

    /* Fond mauve animé avec particules */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background:
        radial-gradient(circle at 70% 80%, #6b00c4 0%, transparent 50%),
        radial-gradient(circle at 20% 60%, #9b00e8 0%, transparent 40%),
        radial-gradient(circle at 50% 10%, #4a0080 0%, transparent 45%);
      animation: bgPulse 8s ease-in-out infinite alternate;
      pointer-events: none;
    }

    body::after {
      content: '';
      position: fixed;
      inset: 0;
      background: radial-gradient(ellipse at center, transparent 30%, #0d001f88 100%);
      pointer-events: none;
    }

    @keyframes bgPulse {
      0%   { opacity: 0.6; }
      100% { opacity: 1; }
    }

    /* --- Love words --- */
    #ui .love {
      position: absolute;
      top: 50%;
      left: 50%;
      margin: -225px 0 0 -225px;
      --i: 1;
    }

    #ui .love:last-child .love_word {
      color: #f0a0cc;
      font-size: clamp(0.8rem, 1.4vw, 1.4rem);
      text-shadow: 0 0 10px #fff, 0 0 20px #c040ff;
    }
    #ui .love_word {
      color: #f0a0cc;
      font-size: clamp(0.8rem, 1.4vw, 1.4rem);
      transform: translateY(-100%) rotateZ(-30deg);
      text-shadow: 0 0 10px #fff, 0 0 20px #c040ff;
      letter-spacing: 2px;
      white-space: nowrap;
    }
    #ui .love_horizontal {
      animation: horizontal 10000ms infinite alternate ease-in-out;
      animation-delay: calc(var(--i) * -300ms);
    }
    #ui .love_vertical {
      animation: vertical 20000ms infinite linear;
      animation-delay: calc(var(--i) * -300ms);
    }

    @keyframes horizontal {
      from { transform: translateX(0px); }
      to   { transform: translateX(min(450px, 90vw)); }
    }
    @keyframes vertical {
      0%   { transform: translateY(180px); }
      10%  { transform: translateY(45px); }
      15%  { transform: translateY(4.5px); }
      18%  { transform: translateY(0px); }
      20%  { transform: translateY(4.5px); }
      22%  { transform: translateY(34.6px); }
      24%  { transform: translateY(64.3px); }
      25%  { transform: translateY(112.5px); }
      26%  { transform: translateY(64.3px); }
      28%  { transform: translateY(34.6px); }
      30%  { transform: translateY(4.5px); }
      32%  { transform: translateY(0px); }
      35%  { transform: translateY(4.5px); }
      40%  { transform: translateY(45px); }
      50%  { transform: translateY(180px); }
      71%  { transform: translateY(428.6px); }
      72.5%{ transform: translateY(441.2px); }
      75%  { transform: translateY(450px); }
      77.5%{ transform: translateY(441.2px); }
      79%  { transform: translateY(428.6px); }
      100% { transform: translateY(180px); }
    }

    /* Responsive mobile */
    @media (max-width: 500px) {
      #ui .love {
        margin: -45vw 0 0 -45vw;
      }
      @keyframes horizontal {
        from { transform: translateX(0px); }
        to   { transform: translateX(90vw); }
      }
      @keyframes vertical {
        0%   { transform: translateY(36vw); }
        10%  { transform: translateY(9vw); }
        15%  { transform: translateY(0.9vw); }
        18%  { transform: translateY(0px); }
        20%  { transform: translateY(0.9vw); }
        22%  { transform: translateY(6.9vw); }
        24%  { transform: translateY(12.9vw); }
        25%  { transform: translateY(22.5vw); }
        26%  { transform: translateY(12.9vw); }
        28%  { transform: translateY(6.9vw); }
        30%  { transform: translateY(0.9vw); }
        32%  { transform: translateY(0px); }
        35%  { transform: translateY(0.9vw); }
        40%  { transform: translateY(9vw); }
        50%  { transform: translateY(36vw); }
        71%  { transform: translateY(85.7vw); }
        72.5%{ transform: translateY(88.2vw); }
        75%  { transform: translateY(90vw); }
        77.5%{ transform: translateY(88.2vw); }
        79%  { transform: translateY(85.7vw); }
        100% { transform: translateY(36vw); }
      }
    }

    /* --- Overlay démarrage --- */
    #music-overlay {
      position: fixed;
      inset: 0;
      z-index: 9999;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 20px;
      background: rgba(10, 0, 30, 0.85);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      transition: opacity 0.8s ease;
    }
    #music-overlay.hidden {
      opacity: 0;
      pointer-events: none;
    }

    .overlay-title {
      color: #f0a0cc;
      font-size: clamp(1.2rem, 6vw, 2rem);
      letter-spacing: 4px;
      text-transform: uppercase;
      text-shadow: 0 0 20px #c040ff, 0 0 40px #ea80b0;
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
      font-size: clamp(0.85rem, 4vw, 1rem);
      letter-spacing: 3px;
      text-transform: uppercase;
      padding: clamp(12px, 4vw, 16px) clamp(28px, 10vw, 40px);
      border-radius: 50px;
      cursor: pointer;
      text-shadow: 0 0 10px #ea80b0;
      box-shadow: 0 0 20px #ea80b066, inset 0 0 20px #ea80b011;
      transition: all 0.3s ease;
      animation: pulse-btn 2s infinite;
      -webkit-appearance: none;
    }
    #start-btn:hover,
    #start-btn:active {
      background: #ea80b022;
      box-shadow: 0 0 40px #ea80b0aa, inset 0 0 30px #ea80b033;
    }
    @keyframes pulse-btn {
      0%, 100% { box-shadow: 0 0 20px #ea80b066, inset 0 0 20px #ea80b011; }
      50%       { box-shadow: 0 0 40px #ea80b0aa, inset 0 0 30px #ea80b033; }
    }

    /* --- Bouton mute --- */
    #mute-btn {
      position: fixed;
      bottom: max(20px, env(safe-area-inset-bottom, 20px));
      right: 20px;
      z-index: 1000;
      background: rgba(106, 0, 180, 0.3);
      border: 1px solid #ea80b055;
      color: #ea80b0;
      font-size: 18px;
      width: 44px;
      height: 44px;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      align-items: center;
      justify-content: center;
      transition: all 0.3s ease;
      opacity: 0.7;
      -webkit-appearance: none;
    }
    #mute-btn:hover,
    #mute-btn:active { opacity: 1; border-color: #ea80b0; background: rgba(106, 0, 180, 0.5); }
    #mute-btn.visible { display: flex; }

    /* Petites étoiles décoratives */
    .star {
      position: fixed;
      background: #fff;
      border-radius: 50%;
      animation: twinkle var(--dur, 3s) ease-in-out infinite;
      animation-delay: var(--del, 0s);
      pointer-events: none;
    }
    @keyframes twinkle {
      0%, 100% { opacity: 0.1; transform: scale(1); }
      50%       { opacity: 0.8; transform: scale(1.5); }
    }
  </style>
</head>
<body>

  <!-- Étoiles -->
  <script>
    const starCount = 40;
    for (let i = 0; i < starCount; i++) {
      const s = document.createElement('div');
      s.className = 'star';
      const size = Math.random() * 2.5 + 0.5;
      s.style.cssText = `
        width: ${size}px; height: ${size}px;
        left: ${Math.random() * 100}vw;
        top: ${Math.random() * 100}vh;
        --dur: ${2 + Math.random() * 4}s;
        --del: ${-Math.random() * 4}s;
      `;
      document.body.appendChild(s);
    }
  </script>

  <!-- Overlay de démarrage -->
  <div id="music-overlay">
    <div class="overlay-title">My Love For You</div>
    <button id="start-btn">♥ Entrer ♥</button>
  </div>

  <!-- Bouton mute/unmute -->
  <button id="mute-btn" title="Mute / Unmute">🔊</button>

  <audio id="bg-music" loop preload="auto">
    <source src="inmark.mp3" type="audio/mpeg">
  </audio>

  <div id="ui"></div>

  <script>
    // Génération des "I love you" flottants
    const N = 100;
    const ui = document.getElementById('ui');
    for (let i = 1; i <= N; i++) {
      const love = document.createElement('div');
      love.className = 'love';
      love.style.setProperty('--i', i);
      const h = document.createElement('div');
      h.className = 'love_horizontal';
      const v = document.createElement('div');
      v.className = 'love_vertical';
      const word = document.createElement('div');
      word.className = 'love_word';
      word.textContent = 'I love you';
      v.appendChild(word);
      h.appendChild(v);
      love.appendChild(h);
      ui.appendChild(love);
    }

    // Musique
    const audio    = document.getElementById('bg-music');
    const overlay  = document.getElementById('music-overlay');
    const startBtn = document.getElementById('start-btn');
    const muteBtn  = document.getElementById('mute-btn');

    audio.loop = true;
    audio.addEventListener('ended', () => audio.play());

    startBtn.addEventListener('click', () => {
      audio.play().then(() => {
        overlay.classList.add('hidden');
        setTimeout(() => overlay.style.display = 'none', 900);
        muteBtn.classList.add('visible');
      }).catch(err => {
        console.warn('Lecture audio refusée :', err);
        overlay.classList.add('hidden');
        setTimeout(() => overlay.style.display = 'none', 900);
      });
    });

    muteBtn.addEventListener('click', () => {
      audio.muted = !audio.muted;
      muteBtn.textContent = audio.muted ? '🔇' : '🔊';
    });
  </script>
</body>
</html>
