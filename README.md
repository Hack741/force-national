# <!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>FORCE NATIONAL - Sistema Secreto</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');
  body {
    margin:0; background:#08131a; color:#00d0a3; font-family: 'Share Tech Mono', monospace;
    display:flex; justify-content:center; align-items:center; height:100vh;
    user-select:none;
  }
  #loginScreen, #mainScreen, #accessDenied {
    position:absolute; top:0; left:0; width:100vw; height:100vh;
    background:#08131a; display:flex; flex-direction:column; justify-content:center; align-items:center;
  }
  #loginScreen input[type=password] {
    background:#0f1720; border:2px solid #00d0a3; color:#00d0a3;
    font-size:1.5rem; padding:0.5rem 1rem; letter-spacing: 0.2em;
    font-family: 'Share Tech Mono', monospace;
    outline:none;
    width:220px;
    text-align:center;
  }
  #loginScreen button {
    margin-top:1rem; background:#00d0a3; border:none; padding:0.6rem 1.2rem;
    font-size:1.2rem; font-weight:bold; cursor:pointer; color:#08131a;
    transition: background 0.3s ease;
  }
  #loginScreen button:hover {
    background:#00b389;
  }
  h1 {
    margin-bottom: 1rem;
  }
  .hidden {
    display:none !important;
  }
  #mainScreen {
    padding: 2rem;
    flex-direction: column;
    align-items: flex-start;
  }
  #folders {
    display: flex;
    gap: 1rem;
    margin-bottom: 2rem;
  }
  .folder {
    background:#0f1720;
    border: 2px solid #00d0a3;
    border-radius: 6px;
    width: 160px;
    height: 140px;
    cursor:pointer;
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transition: background 0.3s ease;
  }
  .folder:hover {
    background:#004d3d;
  }
  .folder img {
    width: 64px;
    margin-bottom: 0.5rem;
  }
  #contentArea {
    background:#0f1720;
    border: 2px solid #00d0a3;
    border-radius: 8px;
    padding: 1rem;
    width: 100%;
    max-width: 720px;
    height: 360px;
    overflow-y: auto;
    font-size: 1rem;
    color:#00d0a3;
  }
  #backBtn {
    margin-bottom: 1rem;
    background:#00d0a3;
    color:#08131a;
    border:none;
    padding: 0.4rem 0.8rem;
    font-family: 'Share Tech Mono', monospace;
    cursor:pointer;
  }
  #backBtn:hover {
    background:#00b389;
  }
  /* Loading bars animation for quebra de senha */
  .loading-bar {
    height: 20px;
    background: #003d2a;
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 0.8rem;
    box-shadow: inset 0 0 5px #00d0a3;
  }
  .loading-bar > div {
    height: 100%;
    background: #00d0a3;
    width: 0;
    animation-fill-mode: forwards;
  }
  .loading-anim-1 {
    animation: load1 3s linear forwards;
  }
  .loading-anim-2 {
    animation: load2 3.5s linear forwards;
  }
  .loading-anim-3 {
    animation: load3 4s linear forwards;
  }
  @keyframes load1 {
    0% { width: 0%; }
    100% { width: 75%; }
  }
  @keyframes load2 {
    0% { width: 0%; }
    100% { width: 60%; }
  }
  @keyframes load3 {
    0% { width: 0%; }
    100% { width: 90%; }
  }
  /* IP Satelite map style */
  #mapFake {
    width: 100%;
    height: 300px;
    background: radial-gradient(circle at center, #002d1f 0%, #000c05 80%);
    border-radius: 6px;
    position: relative;
    box-shadow: 0 0 10px #00d0a3 inset;
  }
  .satellite-dot {
    position: absolute;
    width: 16px;
    height: 16px;
    background: #00d0a3;
    border-radius: 50%;
    box-shadow: 0 0 8px #00d0a3;
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.4); opacity: 0.6; }
  }
  .satellite-label {
    position: absolute;
    color: #00d0a3;
    font-size: 0.8rem;
    font-family: 'Share Tech Mono', monospace;
    pointer-events:none;
  }
  /* Cameras simulation */
  #cameraGrid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
  }
  .camera-feed {
    background:#00221a;
    border: 1px solid #00d0a3;
    height: 110px;
    border-radius: 6px;
    position: relative;
    overflow: hidden;
  }
  .camera-feed img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .camera-label {
    position: absolute;
    bottom: 4px;
    left: 4px;
    font-size: 0.75rem;
    color: #00d0a3;
    text-shadow: 0 0 5px #000;
    font-family: 'Share Tech Mono', monospace;
  }
  /* IA Panel */
  #iaPanel {
    display: flex;
    flex-direction: column;
    height: 100%;
  }
  #iaLog {
    flex-grow: 1;
    background:#00221a;
    border: 1px solid #00d0a3;
    border-radius: 6px;
    padding: 0.5rem;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.9rem;
    overflow-y: auto;
    margin-bottom: 0.6rem;
  }
  #iaInput {
    width: 100%;
    padding: 0.5rem;
    font-family: 'Share Tech Mono', monospace;
    font-size: 1rem;
    border: 2px solid #00d0a3;
    border-radius: 4px;
    background: #0f1720;
    color: #00d0a3;
    outline: none;
  }
  /* Access denied */
  #accessDenied {
    color: #ff4d4f;
    font-size: 2rem;
    text-align: center;
    flex-direction: column;
  }
  #accessDenied button {
    margin-top: 1rem;
    background: #ff4d4f;
    color: #08131a;
    border: none;
    padding: 0.8rem 1.5rem;
    font-family: 'Share Tech Mono', monospace;
    font-weight: bold;
    cursor: pointer;
  }
  #accessDenied button:hover {
    background: #cc3a3f;
  }
</style>
</head>
<body>

<div id="loginScreen">
  <h1>FORCE NATIONAL</h1>
  <input type="password" id="passwordInput" placeholder="Digite a senha" autocomplete="off" />
  <button id="loginBtn">Entrar</button>
  <p id="loginMessage" style="color:#ff4d4f; margin-top:1rem; height: 1.2rem;"></p>
</div>

<div id="mainScreen" class="hidden">
  <button id="backBtn" class="hidden">← Voltar</button>
  <div id="folders">
    <div class="folder" data-tool="passwordBreaker" title="Quebrador de Senhas">
      <img src="https://cdn-icons-png.flaticon.com/512/1170/1170576.png" alt="Senha" />
      Quebrador de Senha
    </div>
    <div class="folder" data-tool="ipSatellite" title="IP Satélite">
      <img src="https://cdn-icons-png.flaticon.com/512/753/753345.png" alt="Satélite" />
      IP Satélite
    </div>
    <div class="folder" data-tool="cameras" title="Câmeras">
      <img src="https://cdn-icons-png.flaticon.com/512/1946/1946436.png" alt="Câmera" />
      Câmeras
    </div>
    <div class="folder" data-tool="iaPanel" title="Painel de IA">
      <img src="https://cdn-icons-png.flaticon.com/512/4144/4144874.png" alt="IA" />
      Painel de IA
    </div>
  </div>

  <div id="contentArea"></div>
</div>

<div id="accessDenied" class="hidden">
  <h1>ACESSO NEGADO</h1>
  <p>Seu IP foi detectado e monitorado.</p>
  <button id="resetBtn">Tentar novamente</button>
</div>

<audio id="typeSound" src="https://actions.google.com/sounds/v1/keyboard/keyboard_typing_fast.ogg" preload="auto"></audio>
<audio id="errorSound" src="https://actions.google.com/sounds/v1/cartoon/cartoon_boing.ogg" preload="auto"></audio>
<audio id="alertSound" src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" preload="auto"></audio>

<script>
  (() => {
    const correctPassword = "AMW2022";
    let attempts = 0;
    const maxAttempts = 3;

    const loginScreen = document.getElementById('loginScreen');
    const mainScreen = document.getElementById('mainScreen');
    const accessDenied = document.getElementById('accessDenied');
    const passwordInput = document.getElementById('passwordInput');
    const loginBtn = document.getElementById('loginBtn');
    const loginMessage = document.getElementById('loginMessage');
    const resetBtn = document.getElementById('resetBtn');

    const folders = document.querySelectorAll('.folder');
    const contentArea = document.getElementById('contentArea');
    const backBtn = document.getElementById('backBtn');

    // Sounds
    const typeSound = document.getElementById('typeSound');
    const errorSound = document.getElementById('errorSound');
    const alertSound = document.getElementById('alertSound');

    // Focus input on load
    passwordInput.focus();

    // Login check
    function checkPassword() {
      const val = passwordInput.value.trim();
      if (val === correctPassword) {
        attempts = 0;
        loginMessage.textContent = "";
        loginScreen.classList.add('hidden');
        mainScreen.classList.remove('hidden');
        passwordInput.value = "";
        passwordInput.blur();
      } else {
        attempts++;
        errorSound.play();
        loginMessage.textContent = `Senha incorreta. Tentativa ${attempts} de ${maxAttempts}.`;
        if (attempts >= maxAttempts) {
          // Lock access
          loginScreen.classList.add('hidden');
          accessDenied.classList.remove('hidden');
          alertSound.play();
        }
      }
    }

    loginBtn.addEventListener('click', checkPassword);
    passwordInput.addEventListener('keydown', e => {
      typeSound.play();
      if (e.key === 'Enter') {
        checkPassword();
      }
    });

    resetBtn.addEventListener('click', () => {
      attempts = 0;
      accessDenied.classList.add('hidden');
      loginScreen.classList.remove('hidden');
      loginMessage.textContent = "";
      passwordInput.value = "";
      passwordInput.focus();
    });

    // Navigation & folder tools

    backBtn.addEventListener('click', () => {
      backBtn.classList.add('hidden');
      contentArea.innerHTML = "";
      mainScreen.querySelector('#folders').classList.remove('hidden');
    });

    folders.forEach(folder => {
      folder.addEventListener('click', () => {
        const tool = folder.dataset.tool;
        mainScreen.querySelector('#folders').classList.add('hidden');
        backBtn.classList.remove('hidden');
        loadTool(tool);
      });
    });

    // Tool loaders

    function loadTool(tool) {
      contentArea.innerHTML = "";
      switch(tool) {
        case 'passwordBreaker':
          loadPasswordBreaker();
          break;
        case 'ipSatellite':
          loadIpSatellite();
          break;
        case 'cameras':
          loadCameras();
          break;
        case 'iaPanel':
          loadIaPanel();
          break;
        default:
          contentArea.textContent = "Ferramenta não implementada.";
      }
    }

    // Quebrador de senha simulado
    function loadPasswordBreaker() {
      contentArea.innerHTML = `
        <h2>Quebrador de Senha</h2>
        <p>Iniciando ataque de força bruta...</p>
        <div class="loading-bar"><div class="loading-anim-1"></div></div>
        <div class="loading-bar"><div class="loading-anim-2"></div></div>
        <div class="loading-bar"><div class="loading-anim-3"></div></div>
        <p id="brokenPassword" style="font-weight:bold; margin-top:1rem;">Aguarde...</p>
        <button id="startBreakBtn" style="background:#00d0a3; color:#08131a; border:none; padding:0.5rem 1rem; cursor:pointer; font-family:'Share Tech Mono', monospace; margin-top:1rem;">Iniciar Ataque</button>
      `;

      const startBtn = document.getElementById('startBreakBtn');
      const brokenPass = document.getElementById('brokenPassword');

      startBtn.addEventListener('click', () => {
        startBtn.disabled = true;
        brokenPass.textContent = "Quebrando senha...";
        let progress = 0;
        const fakePasswords = ["123456", "password", "qwerty", "letmein", "AMW2022", "admin123"];
        const interval = setInterval(() => {
          progress++;
          const pw = fakePasswords[Math.floor(Math.random() * fakePasswords.length)];
          brokenPass.textContent = `Testando senha: ${pw}`;
          if (progress > 20) {
            clearInterval(interval);
            brokenPass.textContent = `Senha quebrada com sucesso: AMW2022`;
            startBtn.disabled = false;
          }
        }, 200);
      });
    }

    // IP Satélite falso
    function loadIpSatellite() {
      contentArea.innerHTML = `
        <h2>IP Satélite</h2>
        <p>Rastreamento do IP em tempo real (simulado)...</p>
        <div id="mapFake"></div>
      `;
      const mapFake = document.getElementById('mapFake');

      // Add some satellite dots randomly positioned
      const satellites = [
        {top:"20%", left:"35%", label:"Satélite A"},
        {top:"45%", left:"60%", label:"Satélite B"},
        {top:"70%", left:"25%", label:"Satélite C"},
        {top:"30%", left:"80%", label:"Satélite D"},
      ];

      satellites.forEach(sat => {
        const dot = document.createElement('div');
        dot.classList.add('satellite-dot');
        dot.style.top = sat.top;
        dot.style.left = sat.left;
        mapFake.appendChild(dot);

        const label = document.createElement('div');
        label.classList.add('satellite-label');
        label.style.top = `calc(${sat.top} + 18px)`;
        label.style.left = sat.left;
        label.textContent = sat.label;
        mapFake.appendChild(label);
      });
    }

    // Câmeras falsas
    function loadCameras() {
      contentArea.innerHTML = `
        <h2>Câmeras</h2>
        <div id="cameraGrid"></div>
      `;

      const cameraGrid = document.getElementById('cameraGrid');
      // Use placeholders with static images (fake)
      const cams = [
        {label: "Entrada Principal", src:"https://images.unsplash.com/photo-1501594907352-04cda38ebc29?auto=format&fit=crop&w=400&q=80"},
        {label: "Corredor Lateral", src:"https://images.unsplash.com/photo-1494526585095-c41746248156?auto=format&fit=crop&w=400&q=80"},
        {label: "Estacionamento", src:"https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80"},
        {label: "Entrada Traseira", src:"https://images.unsplash.com/photo-1486308510493-cb7b1f74c3f9?auto=format&fit=crop&w=400&q=80"},
        {label: "Sala Servidores", src:"https://images.unsplash.com/photo-1535223289827-42f1e9919769?auto=format&fit=crop&w=400&q=80"},
        {label: "Cor
        
