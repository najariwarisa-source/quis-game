<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DATA STRIKE // PROTOCOL</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Rajdhani:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --v-red: #ff4655;
      --v-dark: #0f1923;
      --v-white: #ece8e1;
      --v-blue: #111e2a;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'Rajdhani', sans-serif;
      background: var(--v-dark);
      background-image: 
        linear-gradient(rgba(15, 25, 35, 0.85), rgba(15, 25, 35, 0.95)),
        url('https://images.unsplash.com/photo-1550745165-9bc0b252726f?q=80&w=2070&auto=format&fit=crop');
      background-size: cover;
      background-position: center;
      color: var(--v-white);
      height: 100vh;
      overflow: hidden;
    }

    /* Efek Garis Layar (Scanline) */
    body::after {
      content: " ";
      position: fixed;
      top: 0; left: 0; bottom: 0; right: 0;
      background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.1) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.03), rgba(0, 255, 0, 0.01), rgba(0, 0, 255, 0.03));
      z-index: 10;
      background-size: 100% 3px, 3px 100%;
      pointer-events: none;
    }

    .screen {
      min-height: 100vh;
      display: none;
      justify-content: center;
      align-items: center;
      padding: 20px;
      position: relative;
      z-index: 5;
    }

    .active { display: flex; }

    .container {
      width: 100%;
      max-width: 900px;
      background: rgba(15, 25, 35, 0.8);
      backdrop-filter: blur(15px);
      border: 1px solid rgba(255, 70, 85, 0.3);
      border-left: 4px solid var(--v-red);
      padding: 40px;
      position: relative;
      box-shadow: 0 0 50px rgba(0,0,0,0.8);
    }

    h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 3.5rem;
      color: var(--v-red);
      letter-spacing: 5px;
      text-transform: uppercase;
      margin-bottom: 5px;
      filter: drop-shadow(0 0 10px rgba(255, 70, 85, 0.4));
    }

    /* Tabel Rank Neon */
    .leaderboard-container {
      margin: 25px 0;
      border: 1px solid rgba(255, 70, 85, 0.2);
      background: rgba(0, 0, 0, 0.3);
      max-height: 220px;
      overflow-y: auto;
    }

    table { width: 100%; border-collapse: collapse; }
    
    th {
      background: var(--v-red);
      color: var(--v-dark);
      font-family: 'Orbitron', sans-serif;
      padding: 10px;
      font-size: 0.7rem;
      position: sticky;
      top: 0;
    }

    td {
      padding: 12px;
      text-align: center;
      border-bottom: 1px solid rgba(255, 70, 85, 0.1);
      font-family: 'Orbitron', sans-serif;
      font-size: 0.85rem;
      color: var(--v-white);
    }

    tr:hover { background: rgba(255, 70, 85, 0.1); }
    .my-rank { color: var(--v-red); font-weight: bold; }

    /* Input & Button */
    input {
      width: 100%;
      padding: 15px;
      background: rgba(0, 0, 0, 0.6);
      border: 1px solid #333;
      color: var(--v-white);
      font-family: 'Orbitron', sans-serif;
      margin-bottom: 15px;
    }

    input:focus { border-color: var(--v-red); outline: none; }

    button {
      width: 100%;
      padding: 18px;
      background: var(--v-red);
      color: white;
      border: none;
      font-family: 'Orbitron', sans-serif;
      font-weight: bold;
      cursor: pointer;
      clip-path: polygon(0 0, 100% 0, 100% 75%, 95% 100%, 0 100%);
      transition: 0.2s;
    }

    button:hover { background: white; color: var(--v-dark); }

    /* Loading Bar */
    #loadingScreen {
      position: fixed; inset: 0; background: var(--v-dark);
      display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 100;
    }
    .loader-bar { width: 250px; height: 2px; background: #222; margin-top: 20px; }
    .loader-fill { height: 100%; background: var(--v-red); width: 0%; box-shadow: 0 0 15px var(--v-red); }

    /* Quiz Styling */
    .answer-grid { display: grid; gap: 10px; margin-top: 20px; }
    .answer-btn {
      text-align: left; background: rgba(255,255,255,0.05);
      border: 1px solid rgba(255,255,255,0.1); padding: 15px;
      font-family: 'Rajdhani', sans-serif; font-size: 1.1rem;
    }
    .answer-btn:hover { border-color: var(--v-red); background: rgba(255,70,85,0.1); }
  </style>
</head>
<body>

  <audio id="bgMusic" loop src="https://cdn.pixabay.com/audio/2023/02/28/audio_5504c0a7c9.mp3"></audio>

  <div id="loadingScreen">
    <h2 style="font-family: Orbitron; letter-spacing: 5px; color: var(--v-red);">INITIALIZING...</h2>
    <div class="loader-bar"><div id="loadingBar" class="loader-fill"></div></div>
  </div>

  <section class="screen active" id="loginScreen">
    <div class="container">
      <div style="font-family: Orbitron; font-size: 0.7rem; color: var(--v-red); margin-bottom: 10px;">// SYSTEM ONLINE</div>
      <h1>DATA STRIKE</h1>
      <p style="color: #666; letter-spacing: 3px; font-size: 0.8rem; margin-bottom: 20px;">RANKED ANALYSIS PROTOCOL</p>

      <div class="leaderboard-container">
        <table>
          <thead>
            <tr><th>RANK</th><th>AGENT</th><th>SCORE</th><th>TIER</th></tr>
          </thead>
          <tbody id="frontLeaderboard">
            </tbody>
        </table>
      </div>

      <input type="text" id="nickname" placeholder="ENTER CALLSIGN..." maxlength="15">
      <button onclick="startMission()">INITIATE MISSION</button>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
      <div style="display:flex; justify-content: space-between; align-items:center; border-bottom: 1px solid var(--v-red); padding-bottom: 10px; margin-bottom: 20px;">
        <div id="agentDisplay" style="font-family: Orbitron; color: var(--v-red);">AGENT: UNKNOWN</div>
        <div id="timer" style="font-family: Orbitron; font-size: 1.5rem; color: var(--v-red);">20</div>
      </div>
      <h2 id="questionText" style="font-size: 1.8rem; margin-bottom: 20px;"></h2>
      <div id="answersContainer" class="answer-grid"></div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align:center;">
      <h1 style="font-size: 2rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 6rem; color: var(--v-red); font-family: Orbitron; margin: 10px 0;">0</div>
      <div id="rankTier" style="font-family: Orbitron; font-size: 1.5rem; color: white; margin-bottom: 20px;">IRON</div>
      <button onclick="location.reload()">RE-DEPLOY AGENT</button>
    </div>
  </section>

<script>
  let currentQ = 0;
  let score = 0;
  let nickname = '';
  let timer;

  const questions = [
    { q: 'PENGOLAHAN DATA MENTAH DISEBUT...', a: ['VISUALISASI', 'ANALISIS DATA', 'CODING', 'GAMING'], c: 1 },
    { q: 'GRAFIK TERBAIK UNTUK TREN WAKTU ADALAH...', a: ['PIE CHART', 'LINE CHART', 'BAR CHART', 'MAP'], c: 1 },
    { q: 'TUJUAN UTAMA ANALISIS DATA ADALAH...', a: ['MENGHAPUS DATA', 'MENCARI WAWASAN', 'MEMBUAT GAME', 'MENYIMPAN FOTO'], c: 1 }
  ];

  function loadLeaderboard() {
    const lb = JSON.parse(localStorage.getItem('globalRank')) || [];
    const tbody = document.getElementById('frontLeaderboard');
    if(lb.length === 0) {
      tbody.innerHTML = '<tr><td colspan="4" style="color:#444">NO AGENT RECORDED</td></tr>';
      return;
    }
    tbody.innerHTML = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td>${p.name.toUpperCase()}</td>
        <td>${p.score}</td>
        <td style="color:${p.score >= 90 ? 'gold' : 'var(--v-red)'}">${p.tier}</td>
      </tr>
    `).join('');
  }

  function startMission() {
    const val = document.getElementById('nickname').value.trim();
    if(!val) return alert("ACCESS DENIED: NICKNAME REQUIRED");
    nickname = val;
    document.getElementById('bgMusic').play();
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
      btn.className = 'answer-btn';
      btn.innerText = ans;
      btn.onclick = () => {
        if(i === data.c) score += 33;
        nextQ();
      };
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

    // Save to LocalStorage (Global Perangkat)
    let lb = JSON.parse(localStorage.getItem('globalRank')) || [];
    lb.push({ name: nickname, score: score, tier: tier });
    lb.sort((a,b) => b.score - a.score);
    localStorage.setItem('globalRank', JSON.stringify(lb.slice(0, 10)));

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
        setTimeout(() => document.getElementById('loadingScreen').style.display = 'none', 300);
      }
    }, 50);
  };
</script>
</body>
</html>
