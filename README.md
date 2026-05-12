<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DATA STRIKE // FINAL VERSION</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Rajdhani:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root { --v-red: #ff4655; --v-dark: #0f1923; --v-white: #ece8e1; }
    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'Rajdhani', sans-serif;
      background: var(--v-dark);
      background: radial-gradient(circle, #161f2b 0%, #0f1923 100%);
      color: var(--v-white);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    /* Screen Setup */
    .screen { 
      display: none; 
      justify-content: center; 
      align-items: center; 
      padding: 20px; 
      width: 100%;
      min-height: 100vh;
    }
    .active { display: flex; }

    .container {
      width: 100%; 
      max-width: 700px;
      background: #111e2a;
      border: 1px solid #ff465544;
      border-left: 4px solid var(--v-red);
      padding: 30px;
      /* Memungkinkan scroll di dalam kotak jika konten panjang */
      max-height: 90vh;
      overflow-y: auto; 
    }

    h1 { font-family: 'Orbitron', sans-serif; font-size: 2.2rem; color: var(--v-red); text-align: center; margin-bottom: 15px; }

    /* TABEL RANKING - PERBAIKAN WARNA TULISAN */
    .leaderboard-container { margin-bottom: 20px; border: 1px solid #ff465533; background: #000; }
    table { width: 100%; border-collapse: collapse; }
    th { background: var(--v-red); color: #000; padding: 10px; font-family: 'Orbitron'; font-size: 0.7rem; }
    td { 
      padding: 12px; 
      text-align: center; 
      border-bottom: 1px solid #222; 
      font-family: 'Orbitron'; 
      font-size: 0.8rem; 
      color: #ffffff !important; /* Memaksa warna tulisan jadi putih */
    }

    input { width: 100%; padding: 12px; background: #000; border: 1px solid #ff4655; color: white; margin-bottom: 10px; font-family: 'Orbitron'; }
    button { 
      width: 100%; padding: 15px; background: var(--v-red); color: white; border: none; 
      font-family: 'Orbitron'; font-weight: bold; cursor: pointer; text-transform: uppercase;
    }
    button:hover { background: #fff; color: #000; }

    /* QUIZ BUTTONS */
    .answer-btn {
      width: 100%; padding: 12px; background: #161f2b; border: 1px solid #333;
      color: white; text-align: left; margin-bottom: 8px; cursor: pointer; font-family: 'Rajdhani';
    }
    .answer-btn:hover { border-color: var(--v-red); background: #1a252f; }

    /* LOADING SCREEN */
    #loadingScreen { position: fixed; inset: 0; background: var(--v-dark); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 9999; }
    .loader-bar { width: 180px; height: 2px; background: #222; margin-top: 10px; }
    .loader-fill { height: 100%; background: var(--v-red); width: 0%; transition: width 0.3s ease; }
  </style>
</head>
<body>

  <div id="loadingScreen">
    <h2 style="font-family: Orbitron; color: var(--v-red); font-size: 0.8rem;">SYNCING AGENT DATA...</h2>
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
      <input type="text" id="nickname" placeholder="ENTER AGENT NAME..." maxlength="12">
      <button onclick="startMission()">START MISSION</button>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
      <div style="display:flex; justify-content: space-between; color:var(--v-red); font-family:Orbitron; margin-bottom: 20px;">
        <div id="agentDisplay">AGENT: -</div>
        <div>⏱️ <span id="timer">20</span></div>
      </div>
      <h2 id="questionText" style="margin-bottom: 20px; font-size: 1.3rem;"></h2>
      <div id="answersContainer"></div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align: center;">
      <h1 style="font-size: 1.5rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 5rem; color: var(--v-red); font-family: Orbitron;">0</div>
      <div id="rankTier" style="font-family: Orbitron; margin-bottom: 25px; color:#aaa;">IRON</div>
      <button onclick="location.reload()">RE-DEPLOY</button>
    </div>
  </section>

<script>
  let currentQ = 0, score = 0, nickname = '', timer;
  
  // SOAL DITAMBAH AGAR BANYAK
  const questions = [
    { q: 'Tahap pertama dalam analisis data adalah...', a: ['Visualisasi', 'Pengumpulan Data', 'Penghapusan Data', 'Presentasi'], c: 1 },
    { q: 'Grafik yang cocok untuk melihat kontribusi persentase adalah...', a: ['Line Chart', 'Bar Chart', 'Pie Chart', 'Scatter Plot'], c: 2 },
    { q: 'Software yang sering digunakan untuk analisis data sederhana adalah...', a: ['Photoshop', 'Excel', 'Discord', 'Valorant'], c: 1 },
    { q: 'Data yang berbentuk angka disebut data...', a: ['Kualitatif', 'Kuantitatif', 'Deskriptif', 'Narasi'], c: 1 },
    { q: 'Apa kegunaan Mean dalam statistik?', a: ['Mencari nilai tengah', 'Mencari nilai rata-rata', 'Mencari nilai tersering', 'Mencari nilai tertinggi'], c: 1 },
    { q: 'Visualisasi data bertujuan untuk...', a: ['Memperumit data', 'Mempercantik data saja', 'Memudahkan pemahaman informasi', 'Menyembunyikan fakta'], c: 2 }
  ];

  function loadLeaderboard() {
    const lb = JSON.parse(localStorage.getItem('globalRank')) || [];
    const tbody = document.getElementById('frontLeaderboard');
    if(lb.length === 0) {
      tbody.innerHTML = '<tr><td colspan="4" style="color:#555; padding:10px;">AWAITING DATA...</td></tr>';
      return;
    }
    tbody.innerHTML = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td>${p.name.toUpperCase()}</td>
        <td style="color:var(--v-red)">${p.score}</td>
        <td>${p.tier}</td>
      </tr>
    `).join('');
  }

  function startMission() {
    const val = document.getElementById('nickname').value.trim();
    if(!val) return alert("NICKNAME REQUIRED");
    nickname = val;
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
      btn.onclick = () => { if(i === data.c) score += 16; nextQ(); };
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
    const tier = score >= 80 ? 'RADIANT' : score >= 50 ? 'GOLD' : 'IRON';
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
    setTimeout(() => {
      document.getElementById('loadingBar').style.width = '100%';
      setTimeout(() => document.getElementById('loadingScreen').style.display = 'none', 300);
    }, 200);
  };
</script>
</body>
</html>
