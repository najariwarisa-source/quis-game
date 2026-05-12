<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DATA STRIKE // LIGHT PROTOCOL</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Rajdhani:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --v-red: #ff4655;
      --v-dark: #0f1923;
      --v-white: #ece8e1;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'Rajdhani', sans-serif;
      background: var(--v-dark);
      /* Menggunakan gradient statis agar ringan saat di-scroll */
      background: linear-gradient(135deg, #0f1923 0%, #1a252f 100%);
      color: var(--v-white);
      height: 100vh;
      overflow: hidden;
    }

    .screen {
      min-height: 100vh;
      display: none;
      justify-content: center;
      align-items: center;
      padding: 20px;
    }

    .active { display: flex; }

    .container {
      width: 100%;
      max-width: 800px;
      background: rgba(15, 25, 35, 0.95);
      border: 1px solid rgba(255, 70, 85, 0.4);
      border-left: 4px solid var(--v-red);
      padding: 35px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    }

    h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 3rem;
      color: var(--v-red);
      letter-spacing: 4px;
      text-align: center;
      margin-bottom: 20px;
    }

    /* Tabel Rank - Dioptimasi */
    .leaderboard-container {
      margin-bottom: 25px;
      border: 1px solid rgba(255, 70, 85, 0.2);
      max-height: 200px;
      overflow-y: auto;
    }

    table { width: 100%; border-collapse: collapse; }
    
    th {
      background: var(--v-red);
      color: var(--v-dark);
      font-family: 'Orbitron', sans-serif;
      padding: 8px;
      font-size: 0.7rem;
      position: sticky;
      top: 0;
    }

    td {
      padding: 10px;
      text-align: center;
      border-bottom: 1px solid rgba(255, 70, 85, 0.1);
      font-family: 'Orbitron', sans-serif;
      font-size: 0.8rem;
    }

    /* Form & UI */
    input {
      width: 100%;
      padding: 14px;
      background: #000;
      border: 1px solid #333;
      color: white;
      font-family: 'Orbitron', sans-serif;
      margin-bottom: 15px;
    }

    button {
      width: 100%;
      padding: 16px;
      background: var(--v-red);
      color: white;
      border: none;
      font-family: 'Orbitron', sans-serif;
      font-weight: bold;
      cursor: pointer;
      clip-path: polygon(0 0, 100% 0, 100% 80%, 96% 100%, 0 100%);
      transition: 0.2s;
    }

    button:hover { background: #ff5d6a; }

    /* Ringan tanpa animasi berat */
    #loadingScreen {
      position: fixed; inset: 0; background: var(--v-dark);
      display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 100;
    }
    .loader-bar { width: 200px; height: 2px; background: #222; margin-top: 15px; }
    .loader-fill { height: 100%; background: var(--v-red); width: 0%; }
  </style>
</head>
<body>

  <audio id="bgMusic" loop src="https://cdn.pixabay.com/audio/2023/02/28/audio_5504c0a7c9.mp3"></audio>

  <div id="loadingScreen">
    <h2 style="font-family: Orbitron; color: var(--v-red); font-size: 1rem;">BOOTING SYSTEM...</h2>
    <div class="loader-bar"><div id="loadingBar" class="loader-fill"></div></div>
  </div>

  <section class="screen active" id="loginScreen">
    <div class="container">
      <h1>DATA STRIKE</h1>
      
      <div class="leaderboard-container">
        <table>
          <thead>
            <tr><th>RANK</th><th>AGENT</th><th>SCORE</th><th>TIER</th></tr>
          </thead>
          <tbody id="frontLeaderboard"></tbody>
        </table>
      </div>

      <input type="text" id="nickname" placeholder="AGENT NICKNAME..." maxlength="12">
      <button onclick="startMission()">START MISSION</button>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
      <div style="display:flex; justify-content: space-between; color: var(--v-red); font-family: Orbitron; margin-bottom: 20px;">
        <div id="agentDisplay">AGENT: -</div>
        <div>TIME: <span id="timer">20</span></div>
      </div>
      <h2 id="questionText" style="margin-bottom: 20px;"></h2>
      <div id="answersContainer" style="display: grid; gap: 10px;"></div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align:center;">
      <h1 style="font-size: 2rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 5rem; color: var(--v-red); font-family: Orbitron;">0</div>
      <div id="rankTier" style="font-family: Orbitron; margin-bottom: 20px;">IRON</div>
      <button onclick="location.reload()">RE-DEPLOY</button>
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
      tbody.innerHTML = '<tr><td colspan="4">AWAITING AGENT DATA...</td></tr>';
      return;
    }
    tbody.innerHTML = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td>${p.name.toUpperCase()}</td>
        <td>${p.score}</td>
        <td style="color:var(--v-red)">${p.tier}</td>
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
      btn.style.background = "#161f2b";
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

  window.onload = () => {
    loadLeaderboard();
    let prog = 0;
    const bar = document.getElementById('loadingBar');
    const interval = setInterval(() => {
      prog += 10;
      bar.style.width = prog + '%';
      if(prog >= 100) {
        clearInterval(interval);
        setTimeout(() => document.getElementById('loadingScreen').style.display = 'none', 100);
      }
    }, 40);
  };
</script>
</body>
</html>
