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
      width: 100%; max-width: 900px; background: rgba(20, 20, 20, 0.95);
      border: 2px solid #ff4655; border-radius: 15px; padding: 30px;
      box-shadow: 0 0 30px rgba(255,70,85,0.4); position: relative;
    }
    h1 { font-family: 'Orbitron', sans-serif; font-size: 2.5rem; color: #ff4655; text-align: center; margin-bottom: 5px; letter-spacing: 3px; }
    .subtitle { text-align: center; margin-bottom: 25px; color: #aaa; font-size: 1rem; text-transform: uppercase; }
    
    /* INPUT & BUTTON */
    .input-group { margin-bottom: 15px; }
    label { display: block; margin-bottom: 5px; color: #ff4655; font-weight: bold; }
    input { width: 100%; padding: 12px; border-radius: 5px; border: 1px solid #333; background: #111; color: white; font-size: 1rem; }
    button {
      width: 100%; padding: 15px; border: none; background: #ff4655; color: white; 
      font-weight: bold; cursor: pointer; transition: 0.3s; font-family: 'Orbitron', sans-serif; margin-top: 10px;
    }
    button:hover { background: #ff6d79; filter: brightness(1.2); }

    /* LEADERBOARD TABLE */
    .leaderboard-container { margin-top: 20px; background: rgba(255,255,255,0.05); border-radius: 10px; padding: 15px; }
    table { width: 100%; border-collapse: collapse; margin-top: 10px; font-size: 0.9rem; }
    th { color: #ff4655; text-transform: uppercase; padding: 10px; border-bottom: 2px solid #ff4655; }
    td { padding: 10px; text-align: center; border-bottom: 1px solid #333; }
    .rank-tag { font-weight: bold; padding: 2px 8px; border-radius: 4px; font-size: 0.8rem; }
    
    /* QUIZ UI */
    .top-bar { display: flex; justify-content: space-between; margin-bottom: 20px; background: #161f2b; padding: 10px; border-radius: 8px; }
    .question { font-size: 1.5rem; margin-bottom: 20px; font-weight: 600; }
    .answers { display: grid; gap: 10px; }
    .answer-btn { text-align: left; background: #1f2933; border: 1px solid #333; padding: 15px; color: white; cursor: pointer; border-radius: 5px; }
    .answer-btn:hover { border-color: #ff4655; }
    .correct { background: #1f8f4d !important; }
    .wrong { background: #c53030 !important; }

    /* LOADING BAR */
    #loadingScreen { position:fixed; inset:0; background:#0f1923; display:flex; flex-direction:column; justify-content:center; align-items:center; z-index:9999; }
    .bar-bg { width: 200px; height: 5px; background: #222; margin-top: 15px; overflow: hidden; }
    .bar-fill { height: 100%; width: 0%; background: #ff4655; transition: 0.1s; }
  </style>
</head>
<body>

  <audio id="bgMusic" loop>
    <source src="https://cdn.pixabay.com/audio/2023/02/28/audio_5504c0a7c9.mp3" type="audio/mpeg">
  </audio>

  <div id="loadingScreen">
    <h1 style="font-size: 1.5rem;">INITIALIZING MISSION</h1>
    <div class="bar-bg"><div id="loadingBar" class="bar-fill"></div></div>
  </div>

  <section class="screen active" id="loginScreen">
    <div class="container">
      <h1>DATA STRIKE</h1>
      <p class="subtitle">Ranked Analysis Division</p>

      <div class="leaderboard-container">
        <h3 style="font-family: Orbitron; text-align: center; font-size: 0.9rem;">🏆 GLOBAL PLAYER RANKING</h3>
        <table>
          <thead>
            <tr><th>Rank</th><th>Agent</th><th>Score</th><th>Tier</th></tr>
          </thead>
          <tbody id="frontLeaderboard">
            </tbody>
        </table>
      </div>

      <div style="margin-top: 25px;">
        <div class="input-group">
          <label>NAMA LENGKAP</label>
          <input type="text" id="name" placeholder="Agent Name..." />
        </div>
        <div class="input-group">
          <label>NICKNAME</label>
          <input type="text" id="nickname" placeholder="Callsign..." />
        </div>
        <button onclick="startQuiz()">START MISSION</button>
      </div>
      <p style="font-size: 0.7rem; text-align: center; margin-top: 10px; color: #666;">Klik tombol di atas untuk mengaktifkan audio misi.</p>
    </div>
  </section>

  <section class="screen" id="quizScreen">
    <div class="container">
      <div class="top-bar">
        <div id="playerDisplay" style="color: #ff4655; font-weight: bold;">AGENT: -</div>
        <div>SCORE: <span id="score">0</span></div>
        <div style="color: #ff4655;">⏱️ <span id="timer">20</span>s</div>
      </div>
      <div class="question-box">
        <div id="questionNumber" style="color: #ff4655; font-family: Orbitron; margin-bottom: 10px;">MISSION 01</div>
        <div id="questionText" class="question">Memuat pertanyaan...</div>
        <div id="answersContainer" class="answers"></div>
      </div>
    </div>
  </section>

  <section class="screen" id="resultScreen">
    <div class="container" style="text-align: center;">
      <h1 style="font-size: 1.8rem;">MISSION COMPLETE</h1>
      <div id="finalScore" style="font-size: 4rem; color: #ff4655; font-family: Orbitron;">0</div>
      <div id="rankText" style="font-size: 1.5rem; color: gold; font-family: Orbitron; margin-bottom: 20px;">IRON</div>
      
      <div class="leaderboard-container">
        <h3 style="font-family: Orbitron;">GLOBAL LEADERBOARD</h3>
        <table>
          <thead><tr><th>#</th><th>Agent</th><th>Score</th><th>Tier</th></tr></thead>
          <tbody id="endLeaderboard"></tbody>
        </table>
      </div>
      <button onclick="location.reload()" style="margin-top: 20px; background: #111; border: 1px solid #ff4655;">RETURN TO MENU</button>
    </div>
  </section>

<script>
  const questions = [
    { q: 'Analisis data berarti ...', a: ['Menggambar grafik tanpa data', 'Mengolah data agar lebih bermanfaat', 'Menghapus data mentah', 'Menyimpan data'], c: 1 },
    { q: 'Jenis grafik untuk visualisasi data adalah ...', a: ['Pohon dan bunga', 'Kolom, garis, pai, batang', 'Tabel dan kursi', 'Jembatan'], c: 1 },
    { q: 'Tujuan peringkasan data adalah ...', a: ['Menambah jumlah data', 'Membuat rumit', 'Menampilkan info penting saja', 'Menghapus data'], c: 2 },
    { q: 'Data mentah perlu diolah karena ...', a: ['Sulit dibawa', 'Tidak bisa dibaca komputer', 'Tidak langsung memberi info', 'Selalu salah'], c: 2 },
    { q: 'Contoh analisis data di sekolah ...', a: ['Gambar papan tulis', 'Menghitung rata-rata nilai', 'Main game', 'Hitung kursi'], c: 1 }
  ];

  let currentQuestion = 0;
  let score = 0;
  let timer;
  let timeLeft = 20;
  let nickname = '';

  // FUNGSI MUSIK (Auto-play saat klik pertama)
  function playMusic() {
    const music = document.getElementById('bgMusic');
    music.volume = 0.3;
    music.play().catch(() => console.log("Audio waiting for interaction"));
  }

  function startQuiz() {
    playMusic(); // Memulai musik saat tombol start ditekan
    const name = document.getElementById('name').value.trim();
    nickname = document.getElementById('nickname').value.trim();

    if(!name || !nickname) return alert("Agent! Identitas diperlukan sebelum misi dimulai.");
    
    document.getElementById('playerDisplay').innerText = `AGENT: ${nickname.toUpperCase()}`;
    showScreen('quizScreen');
    loadQuestion();
  }

  function showScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  function loadQuestion() {
    clearInterval(timer);
    timeLeft = 20;
    document.getElementById('timer').innerText = timeLeft;

    const data = questions[currentQuestion];
    document.getElementById('questionNumber').innerText = `MISSION ${String(currentQuestion + 1).padStart(2, '0')}`;
    document.getElementById('questionText').innerText = data.q;

    const container = document.getElementById('answersContainer');
    container.innerHTML = '';

    data.a.forEach((text, i) => {
      const btn = document.createElement('button');
      btn.className = 'answer-btn';
      btn.innerHTML = `<strong>${String.fromCharCode(65+i)}.</strong> ${text}`;
      btn.onclick = () => {
        clearInterval(timer);
        if(i === data.c) {
          btn.classList.add('correct');
          score += 20;
          document.getElementById('score').innerText = score;
        } else {
          btn.classList.add('wrong');
          container.children[data.c].classList.add('correct');
        }
        setTimeout(nextQuestion, 1200);
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
    const rank = score >= 100 ? 'RADIANT' : score >= 80 ? 'PLATINUM' : score >= 60 ? 'GOLD' : score >= 40 ? 'SILVER' : 'IRON';
    document.getElementById('finalScore').innerText = score;
    document.getElementById('rankText').innerText = rank;

    saveData(nickname, score, rank);
    loadLeaderboards();
    showScreen('resultScreen');
  }

  function saveData(nick, sc, rnk) {
    let lb = JSON.parse(localStorage.getItem('strikeLeaderboard')) || [];
    lb.push({ name: nick, score: sc, rank: rnk });
    lb.sort((a,b) => b.score - a.score);
    localStorage.setItem('strikeLeaderboard', JSON.stringify(lb.slice(0, 10))); // Ambil top 10
  }

  function loadLeaderboards() {
    const lb = JSON.parse(localStorage.getItem('strikeLeaderboard')) || [];
    const html = lb.map((p, i) => `
      <tr>
        <td>#${i+1}</td>
        <td>${p.name}</td>
        <td>${p.score}</td>
        <td><span class="rank-tag" style="background:${p.score >= 80 ? '#1f8f4d' : '#444'}">${p.rank}</span></td>
      </tr>
    `).join('');
    
    document.getElementById('frontLeaderboard').innerHTML = html || '<tr><td colspan="4">No Data Recorded</td></tr>';
    document.getElementById('endLeaderboard').innerHTML = html;
  }

  // Loading Bar Logic
  window.onload = () => {
    loadLeaderboards();
    let prog = 0;
    const inv = setInterval(() => {
      prog += 5;
      document.getElementById('loadingBar').style.width = prog + '%';
      if(prog >= 100) {
        clearInterval(inv);
        document.getElementById('loadingScreen').style.opacity = '0';
        setTimeout(() => document.getElementById('loadingScreen').style.display = 'none', 500);
      }
    }, 50);
  };
</script>
</body>
</html>
