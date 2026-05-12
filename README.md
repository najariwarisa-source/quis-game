<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DATA STRIKE // PERFORMANCE MODE</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Rajdhani:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root { --v-red: #ff4655; --v-dark: #0f1923; --v-white: #ece8e1; }
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'Rajdhani', sans-serif;
      background: var(--v-dark);
      background: linear-gradient(180deg, #0f1923 0%, #161f2b 100%);
      color: var(--v-white);
      height: 100vh;
      overflow: hidden;
    }

    .screen { min-height: 100vh; display: none; justify-content: center; align-items: center; padding: 15px; }
    .active { display: flex; }

    .container {
      width: 100%; max-width: 750px;
      background: #111e2a;
      border-left: 4px solid var(--v-red);
      padding: 30px;
      border-radius: 2px;
    }

    h1 { font-family: 'Orbitron', sans-serif; font-size: 2.5rem; color: var(--v-red); text-align: center; margin-bottom: 15px; }

    /* TABEL RANKING MERAH & RAPI */
    .leaderboard-container { margin-bottom: 20px; border: 1px solid #ff465533; max-height: 180px; overflow-y: auto; }
    table { width: 100%; border-collapse: collapse; background: #000; }
    th { background: var(--v-red); color: #000; padding: 10px; font-family: 'Orbitron'; font-size: 0.7rem; }
    td { padding: 12px; text-align: center; border-bottom: 1px solid #333; font-family: 'Orbitron'; font-size: 0.8rem; }

    input { width: 100%; padding: 12px; background: #000; border: 1px solid #ff4655; color: white; margin-bottom: 10px; font-family: 'Orbitron'; }
    button { 
      width: 100%; padding: 15px; background: var(--v-red); color: white; border: none; 
      font-family: 'Orbitron'; font-weight: bold; cursor: pointer; text-transform: uppercase;
    }
    button:hover { filter: brightness(1.2); }

    /* LOADING SCREEN (SANGAT CEPAT) */
    #loadingScreen { position: fixed; inset: 0; background: var(--v-dark); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 1000; }
    .loader-bar { width: 200px; height: 3px; background: #222; margin-top: 10px; }
    .loader-fill { height: 100%; background: var(--v-red); width: 0%; transition: width 0.1s linear; }
  </style>
</head>
<body>

  <audio id="bgMusic" loop src="https://cdn.pixabay.com/audio/2023/02/28/audio_5504c0a7c9.mp3"></audio>

  <div id="loadingScreen">
    <h2 style="font-family: Orbitron; color: var(--v-red); font-size: 0.9rem;">LOADING MISSION...</h2>
    <div class="loader-bar"><div id="loadingBar" class="loader-fill"></div></div>
  </div>

  <section class="screen active" id="loginScreen">
    <div class="container">
      <h1>DATA STRIKE</h1>
      <div class="leaderboard-container">
        <table>
          <thead><tr><th>#</th><th>AGENT</th><th>SCORE</th><th>TIER</th></tr></thead>
          <tbody id="frontLeaderboard"></tbody>
        </table>
      </div>
      <input type="text" id="nickname" placeholder="ENTER CALLSIGN..." maxlength="12">
      <button onclick="startMission()">START MISSION</button>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
      <div style="display:flex; justify-content: space-between; border-bottom: 1px solid var(--v-red); padding-bottom: 10px; margin-bottom: 20px;">
        <div id="agentDisplay" style="color:var(--v-red); font-family:Orbitron;">AGENT: -</div>
        <div style="color:var(--v-red); font-family:Orbitron;">TIME: <span id="timer">20</span></div>
      </div>
      <h2 id="questionText" style="margin-bottom: 25px; font-size: 1.4rem;"></h2>
      <div id="answersContainer" style="display: grid; gap: 10px;"></div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align: center;">
      <h1 style="font-size: 1.5rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 5rem; color: var(--v-red); font-family: Orbitron;">0</div>
      <div id="rankTier" style="font-family: Orbitron; margin-bottom: 25px; color:#aaa;">IRON</div>
      <button onclick="location.reload()">RE-DEPLOY AGENT</button>
    </div>
  </section>

<script>
  let currentQ = 0, score = 0, nickname = '', timer;
  const questions = [
    { q: 'PENGOLAHAN DATA MENTAH DISEBUT...', a: ['VISUALISASI', 'ANALISIS DATA', 'CODING', 'GAMING'], c: 1 },
    { q: 'GRAFIK TERBAIK UNTUK TREN WAKTU ADALAH...', a: ['PIE CHART', 'LINE CHART', 'BAR CHART', 'MAP'], c: 1 },
    { q: 'TUJUAN UTAMA ANALISIS DATA ADALAH...', a: ['HAPUS DATA', 'CARI WAWASAN', 'MAEN GAME', 'SIMPAN FOTO'], c: 1 }
  ];

  function loadLeaderboard() {
    const lb = JSON.parse(localStorage.getItem('globalRank')) || [];
    const tbody = document.getElementById('frontLeaderboard');
    if(lb.length === 0) {
      tbody.innerHTML = '<tr><td colspan="4" style="color:#555; font-size:0.7rem;">AWAITING DATA...</td></tr>';
      return;
    }
    // Perbaikan: Pastikan membungkus setiap data dengan tag <tr> agar muncul sebagai baris
    tbody.innerHTML = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td style="color:#fff">${p.name.toUpperCase()}</td>
        <td style="color:var(--v-red)">${p.score}</td>
        <td>${p.tier}</td>
      </tr>
    `).join('');
  }

  function startMission() {
    const val = document.getElementById('nickname').value.trim();
    if(!val) return alert("NICKNAME REQUIRED");
    nickname = val;
    document.getElementById('bgMusic').play().catch(()=>{});
    document.getElementById('agentDisplay').innerText = `AGENT: ${nickname.toUpperCase()}`;
    showScreen('quizScreen');
    loadQuestion();
  }

  function loadQuestion() {
    clearInterval(timer);
    let timeLeft = 20;
    document.getElementById('timer').innerText = timeLeft;
    const data = questions[currentQ];
    document.getElementById('questionText').innerText = data.q;
    const container = document.getElementById('answersContainer');
    container.innerHTML = '';
    data.a.forEach((ans, i) => {
      const btn = document.createElement('button');
      btn.innerText = ans;
      btn.style.background = "#161f2b"; btn.style.border = "1px solid #333";
      btn.onclick = () => { if(i === data.c) score += 33; nextQ(); };
      container.appendChild(btn);
    });
    timer = setInterval(() => {
      timeLeft--;
      document.getElementById('timer').innerText = timeLeft;
      if(timeLeft <= 0) nextQ();
    }, 1000);
  }

  function nextQ() {
    currentQ++;
    if(currentQ < questions.length) loadQuestion();
    else finishMission();
  }

  function finishMission() {
    clearInterval(timer);
    const tier = score >= 90 ? 'RADIANT' : score >= 60 ? 'GOLD' : 'IRON';
    document.getElementById('finalScore').innerText = score;
    document.getElementById('rankTier').innerText = tier;
    
    let lb = JSON.parse(localStorage.getItem('globalRank')) || [];
    lb.push({ name: nickname, score: score, tier: tier });
    lb.sort((a,b) => b.score - a.score);
    localStorage.setItem('globalRank', JSON.stringify(lb.slice(0, 5)));
    
    showScreen('resultScreen');
  }

  function showScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  // Loading Logic - Super Cepat & Tanpa Bug
  window.addEventListener('load', () => {
    loadLeaderboard();
    let prog = 0;
    const bar = document.getElementById('loadingBar');
    const interval = setInterval(() => {
      prog += 20; // 5 langkah saja langsung 100%
      bar.style.width = prog + '%';
      if(prog >= 100) {
        clearInterval(interval);
        setTimeout(() => {
          document.getElementById('loadingScreen').style.display = 'none';
        }, 150);
      }
    }, 80);
  });
</script>
</body>
</html>
