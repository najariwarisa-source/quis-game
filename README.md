<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DATA STRIKE - RANKED MISSION</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700&family=Rajdhani:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    /* CSS THEME VALORANT */
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Rajdhani', sans-serif;
      background: #0f1923 url('https://images.unsplash.com/photo-1542751371-adc38448a05e?q=80&w=1920&auto=format&fit=crop') center/cover no-repeat fixed;
      color: white; overflow-x: hidden;
    }
    body::before {
      content: ""; position: fixed; inset: 0;
      background: linear-gradient(rgba(15,25,35,.85), rgba(15,25,35,.92)); z-index: -1;
    }
    .screen { min-height: 100vh; display: none; justify-content: center; align-items: center; padding: 20px; }
    .active { display: flex; }
    .container {
      width: 100%; max-width: 900px; background: rgba(15, 25, 35, 0.9);
      border: 2px solid #ff4655; border-radius: 4px; padding: 30px;
      box-shadow: 0 0 30px rgba(255,70,85,0.3); position: relative;
    }
    h1 { font-family: 'Orbitron', sans-serif; font-size: 2.5rem; color: #ff4655; text-align: center; letter-spacing: 3px; }
    
    /* TABEL RANKING MERAH */
    .leaderboard-container { margin-top: 20px; border: 1px solid rgba(255, 70, 85, 0.3); background: rgba(255, 70, 85, 0.05); }
    table { width: 100%; border-collapse: collapse; color: #ff4655; }
    th { background: #ff4655; color: #0f1923; padding: 10px; font-family: 'Orbitron', sans-serif; font-size: 0.8rem; }
    td { padding: 12px; text-align: center; border-bottom: 1px solid rgba(255, 70, 85, 0.2); font-weight: bold; color: #ece8e1; }
    tr:hover { background: rgba(255, 70, 85, 0.1); }

    /* FORM & BUTTON */
    input { width: 100%; padding: 12px; background: #111; border: 1px solid #ff4655; color: white; margin-top: 10px; font-family: 'Rajdhani'; }
    button {
      width: 100%; padding: 15px; border: none; background: #ff4655; color: white; 
      font-weight: bold; cursor: pointer; transition: 0.2s; font-family: 'Orbitron'; margin-top: 20px;
    }
    button:hover { background: #ece8e1; color: #ff4655; }

    /* LOADING BAR */
    #loadingScreen { position:fixed; inset:0; background:#0f1923; display:flex; flex-direction:column; justify-content:center; align-items:center; z-index:9999; }
    .bar-bg { width: 250px; height: 4px; background: #333; margin-top: 15px; }
    .bar-fill { height: 100%; width: 0%; background: #ff4655; transition: 0.1s; }
  </style>
</head>
<body>

  <audio id="bgMusic" loop>
    <source src="https://cdn.pixabay.com/audio/2023/02/28/audio_5504c0a7c9.mp3" type="audio/mpeg">
  </audio>

  <div id="loadingScreen">
    <h1 style="font-size: 1.2rem;">INITIALIZING MISSION...</h1>
    <div class="bar-bg"><div id="loadingBar" class="bar-fill"></div></div>
  </div>

  <section class="screen active" id="loginScreen">
    <div class="container">
      <h1>DATA STRIKE</h1>
      <p style="text-align:center; color:#aaa; font-size:0.8rem; letter-spacing:2px;">RANKED ANALYSIS DIVISION</p>

      <div class="leaderboard-container">
        <table>
          <thead>
            <tr><th>RANK</th><th>AGENT</th><th>SCORE</th><th>TIER</th></tr>
          </thead>
          <tbody id="frontLeaderboard">
            </tbody>
        </table>
      </div>

      <div style="margin-top: 20px;">
        <label style="color:#ff4655; font-size:0.8rem; font-weight:bold;">IDENTITIY :</label>
        <input type="text" id="nickname" placeholder="CALLSIGN / NICKNAME..." />
        <button onclick="startQuiz()">START MISSION</button>
      </div>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
        <div style="display:flex; justify-content: space-between; border-bottom:1px solid #ff4655; padding-bottom:10px; margin-bottom:20px;">
            <div id="playerDisplay" style="color:#ff4655; font-family:Orbitron;">AGENT: -</div>
            <div style="color:#ff4655; font-family:Orbitron;">⏱️ <span id="timer">20</span>s</div>
        </div>
        <div id="questionText" style="font-size: 1.4rem; margin-bottom: 25px;"></div>
        <div id="answersContainer" style="display:grid; gap:12px;"></div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align: center;">
      <h1 style="font-size: 1.5rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 5rem; color: #ff4655; font-family: Orbitron;">0</div>
      <div id="rankText" style="font-size: 1.5rem; color: #ece8e1; font-family: Orbitron; margin-bottom: 30px;">IRON</div>
      <button onclick="location.reload()">RE-INITIALIZE</button>
    </div>
  </section>

<script>
  let currentQuestion = 0;
  let score = 0;
  let nickname = '';
  let timer;

  const questions = [
    { q: 'Analisis data berarti ...', a: ['Menggambar tanpa data', 'Mengolah data agar bermanfaat', 'Menghapus data', 'Menyimpan data'], c: 1 },
    { q: 'Jenis grafik untuk visualisasi adalah ...', a: ['Pohon', 'Kolom, Garis, Pai', 'Meja', 'Jembatan'], c: 1 },
    { q: 'Peringkasan data bertujuan ...', a: ['Tambah data', 'Buat rumit', 'Ambil info penting', 'Hapus semua'], c: 2 }
  ];

  function loadLeaderboards() {
    const lb = JSON.parse(localStorage.getItem('strikeLeaderboard')) || [];
    const frontBody = document.getElementById('frontLeaderboard');
    
    if (lb.length === 0) {
        frontBody.innerHTML = '<tr><td colspan="4" style="color:#666">NO DATA DETECTED</td></tr>';
        return;
    }

    frontBody.innerHTML = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td>${p.name.toUpperCase()}</td>
        <td>${p.score}</td>
        <td style="color:#ff4655">${p.rank}</td>
      </tr>
    `).join('');
  }

  function startQuiz() {
    const inputNick = document.getElementById('nickname').value.trim();
    if(!inputNick) return alert("Agent! Identitas diperlukan.");
    
    nickname = inputNick;
    document.getElementById('bgMusic').play();
    document.getElementById('playerDisplay').innerText = `AGENT: ${nickname.toUpperCase()}`;
    showScreen('quizScreen');
    loadQuestion();
  }

  function loadQuestion() {
    clearInterval(timer);
    let timeLeft = 20;
    document.getElementById('timer').innerText = timeLeft;

    const data = questions[currentQuestion];
    document.getElementById('questionText').innerText = data.q;
    const container = document.getElementById('answersContainer');
    container.innerHTML = '';

    data.a.forEach((text, i) => {
      const btn = document.createElement('button');
      btn.style.marginTop = "0px";
      btn.style.textAlign = "left";
      btn.style.background = "#161f2b";
      btn.style.border = "1px solid #333";
      btn.innerHTML = text;
      btn.onclick = () => {
        if(i === data.c) score += 33;
        nextQuestion();
      };
      container.appendChild(btn);
    });

    timer = setInterval(() => {
      timeLeft--;
      document.getElementById('timer').innerText = timeLeft;
      if(timeLeft <= 0) nextQuestion();
    }, 1000);
  }

  function nextQuestion() {
    currentQuestion++;
    if(currentQuestion < questions.length) loadQuestion();
    else finishQuiz();
  }

  function finishQuiz() {
    clearInterval(timer);
    const rank = score >= 90 ? 'RADIANT' : score >= 60 ? 'GOLD' : 'IRON';
    document.getElementById('finalScore').innerText = score;
    document.getElementById('rankText').innerText = rank;

    let lb = JSON.parse(localStorage.getItem('strikeLeaderboard')) || [];
    lb.push({ name: nickname, score: score, rank: rank });
    lb.sort((a,b) => b.score - a.score);
    localStorage.setItem('strikeLeaderboard', JSON.stringify(lb.slice(0, 5)));

    showScreen('resultScreen');
  }

  function showScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  window.onload = () => {
    loadLeaderboards();
    let prog = 0;
    const inv = setInterval(() => {
      prog += 10;
      document.getElementById('loadingBar').style.width = prog + '%';
      if(prog >= 100) {
        clearInterval(inv);
        setTimeout(() => document.getElementById('loadingScreen').style.display = 'none', 200);
      }
    }, 20);
  };
</script>
</body>
</html>
