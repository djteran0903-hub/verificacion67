<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="theme-color" content="#090909" />
  <title>67 — Acceso</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@400;500;600;700&family=Orbitron:wght@500;600;700;800;900&display=swap" rel="stylesheet" />
  <style>
    :root {
      --display-font: "Orbitron", "Arial Black", "Segoe UI", sans-serif;
      --body-font: "Chakra Petch", "Trebuchet MS", "Segoe UI", sans-serif;
      --black: #050505;
      --black-soft: #0e0e0e;
      --red: #e01822;
      --red-deep: #75000a;
      --green: #38db70;
      --white: #f6f6f6;
      --muted: #a9a9a9;
    }

    * { box-sizing: border-box; }

    html, body { min-height: 100%; width: 100%; }

    body {
      margin: 0;
      min-height: 100svh;
      min-height: 100dvh; /* Se adapta cuando cambia la altura visible del navegador. */
      overflow-x: hidden;
      background: var(--black);
      color: var(--white);
      font-family: var(--body-font);
      letter-spacing: .012em;
    }

    /* Fondo negro con acentos rojos integrados */
    .app {
      position: relative;
      isolation: isolate;
      min-height: 100svh;
      min-height: 100dvh;
      display: grid;
      place-items: center;
      padding: clamp(24px, 5vw, 64px) 20px;
      overflow: hidden;
      background:
        radial-gradient(circle at 7% 10%, rgba(190, 0, 13, .46) 0 1px, transparent 2px),
        radial-gradient(ellipse 75% 50% at 105% 102%, rgba(224, 24, 34, .24), transparent 58%),
        linear-gradient(125deg, #040404 0%, #0b0607 55%, #130304 100%);
      background-size: 160% 160%;
      animation: background-drift 6s ease-in-out infinite alternate;
    }

    .app::before,
    .app::after {
      content: "";
      position: absolute;
      z-index: -1;
      pointer-events: none;
    }

    .app::before {
      width: min(85vw, 1000px);
      height: min(85vw, 1000px);
      top: -58%;
      left: -40%;
      border: clamp(22px, 5vw, 58px) solid rgba(224, 24, 34, .15);
      transform: rotate(-24deg);
      border-radius: 50%;
      animation: ring-drift 4s ease-in-out infinite alternate;
      box-shadow: 0 0 100px rgba(220, 0, 15, .13);
    }

    .app::after {
      width: 180vw;
      height: 1px;
      bottom: 14%;
      left: -36vw;
      background: linear-gradient(90deg, transparent, rgba(224, 24, 34, .9), transparent);
      transform: rotate(-12deg);
      opacity: .72;
      animation: line-drift 3s ease-in-out infinite alternate;
    }

    .noise {
      position: absolute;
      inset: 0;
      z-index: -1;
      opacity: .25;
      pointer-events: none;
      background-image: radial-gradient(rgba(255,255,255,.33) .55px, transparent .6px);
      background-size: 7px 7px;
      mask-image: linear-gradient(to bottom, black, transparent 88%);
    }

    .panel {
      width: min(100%, 530px);
      animation: enter .5s ease both;
    }

    .brand {
      position: relative;
      display: inline-flex;
      align-items: baseline;
      justify-content: center;
      margin: 0 0 clamp(22px, 4vw, 38px);
      line-height: .7;
      font-size: clamp(10.5rem, 42vw, 21rem);
      font-family: Arial, Helvetica, sans-serif;
      font-weight: 900;
      letter-spacing: -.14em;
      color: var(--red);
      text-shadow: 0 0 28px rgba(224, 24, 34, .28);
      user-select: none;
      animation: brand-glitch 6s steps(1, end) infinite;
    }

    .brand-number { display: inline-block; }

    /* El apóstrofo queda cercano al 7, con una separación mínima controlada. */
    .brand-apostrophe {
      display: inline-block;
      margin-left: .055em;
      letter-spacing: 0;
    }

    /* Cortes negros sobre el número, para dar el efecto solicitado */
    .brand::before,
    .brand::after {
      content: "";
      position: absolute;
      z-index: 1;
      display: block;
      height: clamp(11px, 2.6vw, 19px);
      background: var(--black);
      transform: rotate(-7deg);
    }

    .brand::before {
      width: 47%;
      top: 43%;
      left: -3%;
    }

    .brand::after {
      width: 41%;
      top: 68%;
      right: -3%;
    }

    .form-card {
      position: relative;
      overflow: hidden;
      padding: clamp(26px, 6vw, 46px);
      border: 1px solid rgba(255,255,255,.13);
      border-top-color: rgba(224,24,34,.72);
      border-radius: 7px;
      background: linear-gradient(135deg, rgba(21,21,21,.96), rgba(5,5,5,.9));
      box-shadow: 16px 20px 0 rgba(115, 0, 10, .38), 0 20px 60px rgba(0,0,0,.48);
    }

    /* Línea de progreso: izquierda en paso 01, centro en paso 02 y derecha en paso 03. */
    .form-card::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 44%;
      height: 5px;
      background: var(--red);
      box-shadow: 0 0 15px rgba(224, 24, 34, .68);
      transition: left .42s ease;
    }

    .form-card.step-1::before { left: 0; }
    .form-card.step-2::before { left: 28%; }
    .form-card.step-3::before { left: 56%; }

    .step-label {
      margin: 0 0 12px;
      font-family: var(--display-font);
      color: var(--red);
      font-size: .68rem;
      font-weight: 800;
      letter-spacing: .19em;
      text-transform: uppercase;
    }

    h1 {
      margin: 0;
      font-family: var(--display-font);
      font-size: clamp(1.46rem, 6.1vw, 2.2rem);
      font-weight: 800;
      line-height: 1.18;
      letter-spacing: -.035em;
    }

    .question-mark { color: var(--red); }

    .instruction {
      margin: 12px 0 26px;
      color: var(--muted);
      line-height: 1.5;
      font-size: .95rem;
    }

    label { display: block; }

    .field-label {
      display: block;
      font-family: var(--display-font);
      margin-bottom: 9px;
      font-size: .73rem;
      font-weight: 700;
      letter-spacing: .12em;
      text-transform: uppercase;
      color: #d7d7d7;
    }

    input {
      width: 100%;
      min-height: 56px;
      padding: 0 15px;
      border: 1px solid #555;
      border-radius: 3px;
      outline: 0;
      background: #050505;
      color: var(--white);
      font: inherit;
      font-size: 1rem;
      transition: border-color .18s, box-shadow .18s;
    }

    input::placeholder { color: #737373; }

    input:focus {
      border-color: var(--red);
      box-shadow: 0 0 0 3px rgba(224,24,34,.18);
    }

    button {
      width: 100%;
      font-family: var(--display-font);
      min-height: 55px;
      margin-top: 18px;
      border: 0;
      border-radius: 3px;
      background: var(--green);
      color: #050505;
      cursor: pointer;
      font: inherit;
      font-size: .91rem;
      font-weight: 900;
      letter-spacing: .12em;
      text-transform: uppercase;
      transition: transform .16s ease, filter .16s ease, box-shadow .16s ease;
    }

    button:hover { filter: brightness(1.11); box-shadow: 0 7px 23px rgba(56,219,112,.26); }
    button:active { transform: translateY(2px); }
    button:focus-visible { outline: 3px solid white; outline-offset: 3px; }

    .notice {
      min-height: 1.3em;
      margin: 14px 0 0;
      font-size: .88rem;
      font-weight: 700;
      line-height: 1.4;
    }

    .notice.error { color: #ff4a52; }
    .notice.success { color: #68e991; }
    .notice.searching { color: #f4f4f4; }

    .locked input,
    .locked button {
      opacity: .42;
      cursor: not-allowed;
    }

    .locked::after {
      content: "PÁGINA BLOQUEADA";
      display: block;
      margin-top: 17px;
      color: #777;
      font-size: .66rem;
      font-weight: 800;
      letter-spacing: .16em;
      text-align: center;
    }

    .back {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      margin-top: 19px;
      padding: 0;
      border: 0;
      min-height: auto;
      width: auto;
      background: transparent;
      color: #bcbcbc;
      font-size: .76rem;
      font-weight: 700;
      letter-spacing: .08em;
      text-transform: uppercase;
      box-shadow: none;
    }

    .back:hover { color: white; box-shadow: none; }

    .screen { display: none; }
    .screen.active { display: block; animation: screen-in .33s ease both; }

    /* Contenido de la pantalla final, incluido en este mismo archivo HTML. */
    .link-label {
      display: block;
      font-family: var(--display-font);
      margin-bottom: 9px;
      color: #d7d7d7;
      font-size: .73rem;
      font-weight: 700;
      letter-spacing: .12em;
      text-transform: uppercase;
    }

    .copy-row { display: flex; align-items: stretch; gap: 8px; }

    .url-box {
      flex: 1;
      min-width: 0;
      display: flex;
      align-items: center;
      padding: 13px 12px;
      border: 1px solid #555;
      border-radius: 3px;
      background: #050505;
      color: #dedede;
      font-size: .78rem;
      line-height: 1.45;
      overflow-wrap: anywhere;
      user-select: all;
    }

    .copy-button {
      flex: 0 0 55px;
      width: 55px;
      min-height: 56px;
      margin: 0;
      display: grid;
      place-items: center;
      padding: 0;
      border: 1px solid var(--red);
      border-radius: 3px;
      background: #1b0608;
      color: var(--white);
    }

    .copy-button:hover { background: var(--red); box-shadow: none; }
    .copy-button svg { width: 21px; height: 21px; }

    .copy-status {
      min-height: 1.3em;
      margin: 9px 0 0;
      color: #68e991;
      font-size: .8rem;
      font-weight: 700;
    }

    .link-rule { margin: 27px 0 23px; border: 0; border-top: 1px solid rgba(255,255,255,.12); }

    .open-link {
      display: flex;
      font-family: var(--display-font);
      width: 100%;
      min-height: 56px;
      align-items: center;
      justify-content: center;
      padding: 12px 18px;
      border-radius: 3px;
      background: var(--green);
      color: #050505;
      font-size: .86rem;
      font-weight: 900;
      letter-spacing: .07em;
      text-align: center;
      text-decoration: none;
      text-transform: uppercase;
      transition: transform .16s, filter .16s, box-shadow .16s;
    }

    .open-link:hover { filter: brightness(1.1); box-shadow: 0 7px 23px rgba(56,219,112,.26); }
    .open-link:active { transform: translateY(2px); }
    .open-link:focus-visible { outline: 3px solid white; outline-offset: 3px; }
    .small-note { margin: 16px 0 0; color: #828282; font-size: .74rem; text-align: center; }

    .device-note {
      margin: 18px 2px 0;
      color: #858585;
      font-size: .72rem;
      text-align: center;
    }

    @keyframes background-drift {
      0% { background-position: 0% 0%, 0% 0%, 0% 0%; }
      100% { background-position: 18% 18%, 100% 88%, 72% 100%; }
    }

    @keyframes ring-drift {
      from { transform: rotate(-24deg) translate3d(0, 0, 0) scale(1); }
      to { transform: rotate(-17deg) translate3d(7vw, 4vh, 0) scale(1.08); }
    }

    @keyframes line-drift {
      from { transform: rotate(-12deg) translateX(-3vw); }
      to { transform: rotate(-8deg) translateX(8vw); }
    }

    @keyframes enter {
      from { opacity: 0; transform: translateY(18px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes screen-in {
      from { opacity: 0; transform: translateX(14px); }
      to { opacity: 1; transform: translateX(0); }
    }

    /* Micro-glitch: aparece brevemente, sin afectar la lectura del 67 '. */
    @keyframes brand-glitch {
      0%, 89%, 100% {
        transform: translate(0, 0) skewX(0);
        text-shadow: 0 0 28px rgba(224, 24, 34, .28);
      }
      90% {
        transform: translate(-1px, 0) skewX(-.35deg);
        text-shadow: 2px 0 rgba(255, 255, 255, .23), -2px 0 rgba(80, 0, 0, .7), 0 0 28px rgba(224, 24, 34, .28);
      }
      91% {
        transform: translate(1px, -1px) skewX(.28deg);
        text-shadow: -2px 0 rgba(255, 255, 255, .18), 2px 0 rgba(80, 0, 0, .66), 0 0 28px rgba(224, 24, 34, .28);
      }
      92% {
        transform: translate(0, 1px) skewX(0);
        text-shadow: 1px 0 rgba(255, 255, 255, .16), 0 0 28px rgba(224, 24, 34, .28);
      }
    }

    /* Reduce los elementos cuando la ventana es baja, por ejemplo en móvil horizontal. */
    @media (max-height: 700px) {
      .app { padding-top: 18px; padding-bottom: 18px; }
      .brand { font-size: clamp(5.2rem, 18vh, 8.5rem); margin-bottom: 16px; }
      .form-card { padding-top: 22px; padding-bottom: 22px; }
      .instruction { margin: 8px 0 16px; }
      input { min-height: 48px; }
      button { min-height: 48px; margin-top: 13px; }
      .device-note { margin-top: 11px; }
    }

    /* En móvil se mueve solo el fondo, de forma suave; las líneas grandes quedan estables. */
    @media (max-width: 700px) {
      .app {
        background-size: 108% 108%;
        background-position: 0 0;
        animation: background-drift-mobile 12s ease-in-out infinite alternate;
      }

      .app::before,
      .app::after {
        animation: none;
        will-change: auto;
      }

      .app::before {
        width: 115vw;
        height: 115vw;
        top: -38vw;
        left: -53vw;
      }

      .app::after {
        width: 140vw;
        left: -20vw;
        bottom: 17%;
      }

      /* El 67 ' conserva su estilo, pero ocupa menos espacio en móviles. */
      .brand {
        font-size: clamp(6.2rem, 25vw, 8.4rem);
        letter-spacing: -.12em;
        animation: brand-glitch-mobile 3.6s linear infinite;
      }
      .brand::before { animation: cut-glitch-top 3.6s linear infinite; }
      .brand::after { animation: cut-glitch-bottom 3.6s linear infinite; }
    }

    @keyframes background-drift-mobile {
      from { background-position: 0% 0%, 0% 0%, 0% 0%; }
      to { background-position: 3% 4%, 8% 7%, 5% 6%; }
    }

    @keyframes brand-glitch-mobile {
      0%, 78%, 100% {
        transform: translate(0, 0) skewX(0);
        text-shadow: 0 0 28px rgba(224, 24, 34, .28);
      }
      80% {
        transform: translate(-2px, 0) skewX(-.6deg);
        text-shadow: 2px 0 rgba(255, 255, 255, .26), -2px 0 rgba(80, 0, 0, .75), 0 0 28px rgba(224, 24, 34, .28);
      }
      83% {
        transform: translate(2px, -1px) skewX(.5deg);
        text-shadow: -2px 0 rgba(255, 255, 255, .2), 2px 0 rgba(80, 0, 0, .72), 0 0 28px rgba(224, 24, 34, .28);
      }
      86% {
        transform: translate(0, 1px) skewX(0);
        text-shadow: 1px 0 rgba(255, 255, 255, .18), 0 0 28px rgba(224, 24, 34, .28);
      }
    }

    @keyframes cut-glitch-top {
      0%, 78%, 100% { transform: rotate(-7deg) translateX(0); }
      80% { transform: rotate(-7deg) translateX(5px); }
      83% { transform: rotate(-7deg) translateX(-4px); }
      86% { transform: rotate(-7deg) translateX(1px); }
    }

    @keyframes cut-glitch-bottom {
      0%, 78%, 100% { transform: rotate(-7deg) translateX(0); }
      80% { transform: rotate(-7deg) translateX(-4px); }
      83% { transform: rotate(-7deg) translateX(5px); }
      86% { transform: rotate(-7deg) translateX(-1px); }
    }

    @media (max-width: 390px) {
      .app { padding: 22px 16px; }
      .form-card { padding: 25px 21px; box-shadow: 9px 12px 0 rgba(115,0,10,.35); }
      .brand { margin-bottom: 26px; }
    }

    @media (min-width: 800px) {
      .panel { transform: translateX(-6vw); }
      .brand { margin-left: 3px; }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; }
    }
  </style>
</head>
<body>
  <main class="app">
    <div class="noise" aria-hidden="true"></div>

    <section class="panel" aria-label="Acceso al juego">
      <div class="brand" aria-label="67 apóstrofo"><span class="brand-number">67</span><span class="brand-apostrophe">'</span></div>

      <div class="form-card step-1">
        <section class="screen active" id="game-screen" aria-labelledby="game-title">
          <p class="step-label">Paso 01 / Acceso</p>
          <h1 id="game-title">Nombre Del Juego<span class="question-mark">?</span></h1>
          <p class="instruction">Pon Tu Nombre Exacto Del Juego Para Continuar.</p>

          <form id="game-form" novalidate>
            <label for="game-name">
              <span class="field-label">Nombre</span>
              <input id="game-name" name="game-name" type="text" maxlength="80" autocomplete="off" placeholder="Escribe aquí..." required />
            </label>
            <button type="submit">Enviar</button>
            <p class="notice" id="name-notice" aria-live="polite"></p>
          </form>
        </section>

        <section class="screen" id="level-screen" aria-labelledby="level-title">
          <p class="step-label">Paso 02 / Verificación</p>
          <h1 id="level-title">Nivel De Jugador<span class="question-mark">?</span></h1>
          <p class="instruction">Ingresa tu nivel para finalizar.</p>

          <form id="level-form" novalidate>
            <label for="player-level">
              <span class="field-label">Nivel</span>
              <input id="player-level" name="player-level" type="text" inputmode="numeric" pattern="[0-9]*" autocomplete="off" maxlength="6" placeholder="Solo números" required />
            </label>
            <button type="submit">Enviar</button>
            <p class="notice" id="level-notice" aria-live="polite"></p>
          </form>
          <button class="back" id="back-button" type="button">← Cambiar nombre</button>
        </section>

        <section class="screen" id="whatsapp-screen" aria-labelledby="whatsapp-title">
          <p class="step-label">Paso 03 / Finalizado</p>
          <h1 id="whatsapp-title">Enlace De WhatsApp<span class="question-mark">.</span></h1>
          <p class="instruction">Copia el enlace o ábrelo directamente para entrar al grupo.</p>

          <span class="link-label">Enlace del grupo</span>
          <div class="copy-row">
            <div class="url-box" id="whatsapp-url">https://chat.whatsapp.com/F9DLNSAcI9yGGUJkbDTI4A?s=cl&amp;p=a&amp;mlu=4&amp;ilr=4</div>
            <button class="copy-button" id="copy-button" type="button" aria-label="Copiar enlace de WhatsApp" title="Copiar enlace">
              <svg id="copy-icon" viewBox="0 0 24 24" aria-hidden="true" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="12" height="12" rx="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg>
            </button>
          </div>
          <p class="copy-status" id="copy-status" aria-live="polite"></p>

          <hr class="link-rule" />
          <a class="open-link" href="https://chat.whatsapp.com/F9DLNSAcI9yGGUJkbDTI4A?s=cl&amp;p=a&amp;mlu=4&amp;ilr=4" target="_blank" rel="noopener noreferrer">Toca Aqui Para Abrir WhasApp Aqui</a>
          <p class="small-note">El enlace se abrirá en una nueva pestaña.</p>
        </section>
      </div>
      <p class="device-note" id="device-note"></p>
    </section>
  </main>

  <script>
    (() => {
      const gameScreen = document.getElementById('game-screen');
      const levelScreen = document.getElementById('level-screen');
      const whatsappScreen = document.getElementById('whatsapp-screen');
      const formCard = document.querySelector('.form-card');
      const gameName = document.getElementById('game-name');
      const level = document.getElementById('player-level');
      const nameNotice = document.getElementById('name-notice');
      const levelNotice = document.getElementById('level-notice');
      const deviceNote = document.getElementById('device-note');

      // Detecta el tipo de dispositivo y conserva el diseño adaptado a móvil o PC.
      const agent = navigator.userAgent || '';
      const isMobile = Boolean(navigator.userAgentData?.mobile) ||
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(agent) ||
        (navigator.maxTouchPoints > 1 && window.matchMedia('(max-width: 768px)').matches);
      document.documentElement.dataset.device = isMobile ? 'mobile' : 'desktop';
      deviceNote.textContent = isMobile
        ? 'Dispositivo detectado: Móvil'
        : 'Dispositivo detectado: PC';

      function showScreen(screenToShow) {
        [gameScreen, levelScreen, whatsappScreen].forEach(screen => screen.classList.remove('active'));
        screenToShow.classList.add('active');
        formCard.classList.remove('step-1', 'step-2', 'step-3');
        const stepClass = screenToShow === gameScreen
          ? 'step-1'
          : screenToShow === levelScreen
            ? 'step-2'
            : 'step-3';
        formCard.classList.add(stepClass);
      }

      const gameForm = document.getElementById('game-form');
      const nameSubmitButton = gameForm.querySelector('button[type="submit"]');
      let isSearchingPlayer = false;

      gameForm.addEventListener('submit', event => {
        event.preventDefault();
        if (isSearchingPlayer) return;

        if (!gameName.value.trim()) {
          nameNotice.textContent = 'Escribe un nombre para continuar.';
          nameNotice.className = 'notice error';
          gameName.focus();
          return;
        }

        isSearchingPlayer = true;
        gameName.disabled = true;
        nameSubmitButton.disabled = true;
        nameNotice.textContent = 'Buscando Jugador';
        nameNotice.className = 'notice searching';

        // Mantiene el mensaje de búsqueda durante exactamente 3 segundos.
        setTimeout(() => {
          nameNotice.textContent = 'Jugador encontrado';
          nameNotice.className = 'notice success';

          // Deja visible la confirmación antes de continuar al paso 02.
          setTimeout(() => {
            gameName.disabled = false;
            nameSubmitButton.disabled = false;
            isSearchingPlayer = false;
            nameNotice.textContent = '';
            nameNotice.className = 'notice';
            showScreen(levelScreen);
            level.focus();
          }, 900);
        }, 3000);
      });

      document.getElementById('back-button').addEventListener('click', () => {
        levelNotice.textContent = '';
        levelNotice.className = 'notice';
        showScreen(gameScreen);
        gameName.focus();
      });

      // Acepta únicamente dígitos, incluso si se pega texto en el campo.
      level.addEventListener('beforeinput', event => {
        if (event.data && /[^0-9]/.test(event.data)) event.preventDefault();
      });
      level.addEventListener('input', () => {
        level.value = level.value.replace(/[^0-9]/g, '');
      });
      level.addEventListener('paste', event => {
        event.preventDefault();
        const digits = (event.clipboardData || window.clipboardData)
          .getData('text')
          .replace(/[^0-9]/g, '');
        const start = level.selectionStart;
        const end = level.selectionEnd;
        level.value = (level.value.slice(0, start) + digits + level.value.slice(end)).slice(0, 6);
      });

      document.getElementById('level-form').addEventListener('submit', event => {
        event.preventDefault();
        const playerLevel = Number(level.value);

        if (level.value === '' || !Number.isFinite(playerLevel)) {
          levelNotice.textContent = 'Ingresa un número válido.';
          levelNotice.className = 'notice error';
          level.focus();
          return;
        }

        if (playerLevel < 50) {
          levelNotice.textContent = 'Solo se aceptan Niveles Mayores';
          levelNotice.className = 'notice error';
          // El paso queda bloqueado y solo se podrá empezar de nuevo al recargar la página.
          levelScreen.classList.add('locked');
          levelScreen.querySelectorAll('input, button').forEach(control => {
            control.disabled = true;
          });
          return;
        }

        // El paso final está dentro de este mismo archivo HTML.
        showScreen(whatsappScreen);
        document.getElementById('copy-button').focus();
      });

      const whatsappUrl = 'https://chat.whatsapp.com/F9DLNSAcI9yGGUJkbDTI4A?s=cl&p=a&mlu=4&ilr=4';
      const copyButton = document.getElementById('copy-button');
      const copyStatus = document.getElementById('copy-status');
      const copyIcon = document.getElementById('copy-icon');

      copyButton.addEventListener('click', async () => {
        try {
          if (navigator.clipboard && window.isSecureContext) {
            await navigator.clipboard.writeText(whatsappUrl);
          } else {
            const textArea = document.createElement('textarea');
            textArea.value = whatsappUrl;
            textArea.setAttribute('readonly', '');
            textArea.style.position = 'fixed';
            textArea.style.opacity = '0';
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand('copy');
            textArea.remove();
          }
          copyStatus.textContent = '¡Enlace copiado al portapapeles!';
          copyIcon.innerHTML = '<path d="m5 12 4 4L19 6"></path>';
          setTimeout(() => {
            copyStatus.textContent = '';
            copyIcon.innerHTML = '<rect x="9" y="9" width="12" height="12" rx="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path>';
          }, 2500);
        } catch (error) {
          copyStatus.textContent = 'No se pudo copiar. Selecciona el enlace y cópialo manualmente.';
        }
      });
    })();
  </script>
</body>
</html>
