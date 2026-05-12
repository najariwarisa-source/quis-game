```html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DATA STRIKE</title>

<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700&family=Rajdhani:wght@400;500;700&display=swap" rel="stylesheet">

<style>

*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}

body{
  font-family:'Rajdhani',sans-serif;
  background:#0f1722;
  color:white;
  overflow-x:hidden;
}

/* LOADING */

#loadingScreen{
  position:fixed;
  inset:0;
  background:#0f1722;
  z-index:9999;
  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction:column;
}

#loadingScreen h1{
  font-family:'Orbitron';
  color:#ff4655;
  font-size:3rem;
}

.loading-bar{
  width:260px;
  height:8px;
  background:#222;
  margin-top:20px;
  border-radius:20px;
  overflow:hidden;
}

.loading-fill{
  height:100%;
  width:0%;
  background:#ff4655;
}

/* HERO */

.hero{
  min-height:100vh;

  background:
  linear-gradient(rgba(0,0,0,.55),rgba(0,0,0,.8)),
  url('https://images.unsplash.com/photo-1511512578047-dfb367046420?q=80&w=1920&auto=format&fit=crop') center/cover no-repeat;

  display:flex;
  justify-content:center;
  align-items:center;
  text-align:center;
  position:relative;
  padding:20px;
}

.hero-content{
  max-width:800px;
}

.hero h1{
  font-size:5rem;
  font-family:'Orbitron';
  color:white;
  text-shadow:0 0 25px #ff4655;
}

.hero p{
  margin-top:15px;
  color:#ddd;
  font-size:1.3rem;
  letter-spacing:2px;
}

.hero button{
  margin-top:30px;
}

/* CURVE */

.curve{
  position:absolute;
  bottom:-1px;
  width:100%;
  height:120px;
  background:#151b26;
  border-radius:100% 100% 0 0;
}

/* SECTIONS */

.screen{
  display:none;
  min-height:100vh;
  justify-content:center;
  align-items:center;
  padding:30px;
  background:#151b26;
}

.active{
  display:flex;
}

/* CARD */

.card{
  width:100%;
  max-width:900px;

  background:rgba(255,255,255,.05);

  backdrop-filter:blur(14px);

  border:1px solid rgba(255,255,255,.08);

  border-radius:25px;

  padding:40px;

  box-shadow:0 0 40px rgba(255,70,85,.25);
}

/* PROFILE */

.profile-box{
  max-width:450px;
}

.title{
  text-align:center;
  margin-bottom:30px;
}

.title h2{
  font-family:'Orbitron';
  font-size:2rem;
  color:#ff4655;
}

.input-group{
  margin-bottom:20px;
}

label{
  display:block;
  margin-bottom:8px;
  color:#ff9ca5;
}

input{
  width:100%;
  padding:15px;
  border:none;
  border-radius:14px;
  background:#0f1722;
  color:white;
  font-size:1rem;
  border:1px solid rgba(255,255,255,.08);
}

/* BUTTON */

button{
  width:100%;
  padding:16px;
  border:none;
  border-radius:14px;
  background:#ff4655;
  color:white;
  font-family:'Orbitron';
  cursor:pointer;
  transition:.3s;
  font-size:1rem;
}

button:hover{
  transform:translateY(-2px);
  background:#ff6572;
}

/* QUIZ */

.top-bar{
  display:flex;
  justify-content:space-between;
  margin-bottom:25px;
  gap:10px;
  flex-wrap:wrap;
}

.badge{
  background:rgba(255,70,85,.12);
  border:1px solid #ff4655;
  padding:10px 18px;
  border-radius:14px;
}

.question{
  font-size:1.8rem;
  margin-bottom:25px;
}

.answers{
  display:grid;
  gap:15px;
}

.answer-btn{
  background:#1a2230;
  border:1px solid #2e394d;
  text-align:left;
}

.answer-btn:hover{
  border-color:#ff4655;
}

.correct{
  background:#1e8b4f !important;
}

.wrong{
  background:#c53030 !important;
}

/* RESULT */

.result-box{
  text-align:center;
}

.score{
  font-size:5rem;
  color:#ff4655;
  font-family:'Orbitron';
  margin:20px 0;
}

.rank{
  font-size:2rem;
  color:gold;
  margin-bottom:20px;
  font-family:'Orbitron';
}

/* LEADERBOARD */

table{
  width:100%;
  margin-top:20px;
  border-collapse:collapse;
}

th,td{
  padding:14px;
  border-bottom:1px solid rgba(255,255,255,.08);
  text-align:center;
}

th{
  color:#ff4655;
  font-family:'Orbitron';
}

@media(max-width:768px){

.hero h1{
  font-size:3rem;
}

.question{
  font-size:1.2rem;
}

.score{
  font-size:3rem;
}

}

</style>
</head>
<body>

<!-- LOADING -->

<div id="loadingScreen">
  <h1>DATA STRIKE</h1>

  <p style="margin-top:10px;color:#ccc">
    Initializing Mission...
  </p>

  <div class="loading-bar">
    <div class="loading-fill" id="loadingFill"></div>
  </div>
</div>

<!-- HERO -->

<section class="hero" id="heroSection">

  <div class="hero-content">

    <h1>DATA STRIKE</h1>

    <p>Genuine Storytelling Project</p>

    <button onclick="openProfile()">
      START MISSION
    </button>

  </div>

  <div class="curve"></div>

</section>

<!-- PROFILE -->

<section class="screen" id="profileScreen">

  <div class="card profile-box">

    <div class="title">
      <h2>PLAYER PROFILE</h2>
    </div>

    <div class="input-group">
      <label>Nama Lengkap</label>
      <input type="text" id="name">
    </div>

    <div class="input-group">
      <label>Nickname</label>
      <input type="text" id="nickname">
    </div>

    <button onclick="startQuiz()">
      ENTER GAME
    </button>

  </div>

</section>

<!-- QUIZ -->

<section class="screen" id="quizScreen">

  <div class="card">

    <div class="top-bar">

      <div class="badge" id="playerDisplay">
        PLAYER
      </div>

      <div class="badge">
        SCORE :
        <span id="score">0</span>
      </div>

      <div class="badge">
        ⏱️
        <span id="timer">20</span>s
      </div>

    </div>

    <h2 class="question" id="questionText"></h2>

    <div class="answers" id="answersContainer"></div>

  </div>

</section>

<!-- RESULT -->

<section class="screen" id="resultScreen">

  <div class="card result-box">

    <h1>MISSION COMPLETE</h1>

    <div class="score" id="finalScore">
      0
    </div>

    <div class="rank" id="rankText">
      IRON
    </div>

    <p id="playerFinal"></p>

    <table>

      <thead>
        <tr>
          <th>Rank</th>
          <th>Player</th>
          <th>Score</th>
          <th>Tier</th>
        </tr>
      </thead>

      <tbody id="leaderboardBody"></tbody>

    </table>

  </div>

</section>

<script>

const questions = [

{
  question:'Analisis data berarti ...',
  answers:[
    'Menggambar grafik tanpa data',
    'Mengolah data agar bermanfaat',
    'Menghapus data mentah',
    'Menyimpan data'
  ],
  correct:1
},

{
  question:'Tujuan visualisasi data adalah ...',
  answers:[
    'Membuat data sulit',
    'Mempermudah memahami informasi',
    'Menghapus data',
    'Memenuhi memori'
  ],
  correct:1
},

{
  question:'Data mentah perlu diolah karena ...',
  answers:[
    'Tidak langsung memberi informasi',
    'Terlalu bagus',
    'Sudah sempurna',
    'Tidak penting'
  ],
  correct:0
},

{
  question:'Grafik batang digunakan untuk ...',
  answers:[
    'Menampilkan perbandingan',
    'Memasak data',
    'Menghapus file',
    'Menggambar anime'
  ],
  correct:0
},

{
  question:'Contoh analisis data di sekolah adalah ...',
  answers:[
    'Bermain game',
    'Menghitung rata-rata nilai',
    'Tidur',
    'Menggambar'
  ],
  correct:1
}

];

let currentQuestion = 0;
let score = 0;
let timer;
let timeLeft = 20;
let nickname = '';

function openProfile(){

  document.getElementById('heroSection').style.display='none';

  document.getElementById('profileScreen')
  .classList.add('active');

}

function startQuiz(){

  const name =
  document.getElementById('name').value;

  nickname =
  document.getElementById('nickname').value;

  if(!name || !nickname){

    alert('Isi data terlebih dahulu!');
    return;

  }

  document.getElementById('profileScreen')
  .classList.remove('active');

  document.getElementById('quizScreen')
  .classList.add('active');

  document.getElementById('playerDisplay')
  .innerText = nickname;

  loadQuestion();

}

function loadQuestion(){

  clearInterval(timer);

  timeLeft = 20;

  document.getElementById('timer')
  .innerText = timeLeft;

  const q = questions[currentQuestion];

  document.getElementById('questionText')
  .innerText = q.question;

  const container =
  document.getElementById('answersContainer');

  container.innerHTML='';

  q.answers.forEach((answer,index)=>{

    const btn =
    document.createElement('button');

    btn.classList.add('answer-btn');

    btn.innerHTML =
    `<strong>${String.fromCharCode(65+index)}.</strong> ${answer}`;

    btn.onclick = ()=> selectAnswer(index,btn);

    container.appendChild(btn);

  });

  timer = setInterval(()=>{

    timeLeft--;

    document.getElementById('timer')
    .innerText = timeLeft;

    if(timeLeft <= 0){

      clearInterval(timer);

      nextQuestion();

    }

  },1000);

}

function selectAnswer(index,btn){

  clearInterval(timer);

  const q = questions[currentQuestion];

  const buttons =
  document.querySelectorAll('.answer-btn');

  buttons.forEach(b=>b.disabled=true);

  if(index === q.correct){

    btn.classList.add('correct');

    score += 20;

    document.getElementById('score')
    .innerText = score;

  }else{

    btn.classList.add('wrong');

    buttons[q.correct]
    .classList.add('correct');

  }

  setTimeout(()=>{
    nextQuestion();
  },1200);

}

function nextQuestion(){

  currentQuestion++;

  if(currentQuestion < questions.length){

    loadQuestion();

  }else{

    finishQuiz();

  }

}

function getRank(score){

  if(score >= 100) return 'RADIANT';
  if(score >= 80) return 'PLATINUM';
  if(score >= 60) return 'GOLD';
  if(score >= 40) return 'SILVER';

  return 'IRON';

}

function finishQuiz(){

  document.getElementById('quizScreen')
  .classList.remove('active');

  document.getElementById('resultScreen')
  .classList.add('active');

  document.getElementById('finalScore')
  .innerText = score;

  document.getElementById('rankText')
  .innerText = getRank(score);

  document.getElementById('playerFinal')
  .innerHTML =
  `<strong>${nickname}</strong> berhasil menyelesaikan mission`;

  saveLeaderboard();

}

function saveLeaderboard(){

  let leaderboard =
  JSON.parse(localStorage.getItem('leaderboard')) || [];

  leaderboard.push({

    nickname:nickname,
    score:score,
    rank:getRank(score)

  });

  leaderboard.sort((a,b)=>b.score-a.score);

  localStorage.setItem(
    'leaderboard',
    JSON.stringify(leaderboard)
  );

  renderLeaderboard();

}

function renderLeaderboard(){

  let leaderboard =
  JSON.parse(localStorage.getItem('leaderboard')) || [];

  const body =
  document.getElementById('leaderboardBody');

  body.innerHTML='';

  leaderboard.forEach((player,index)=>{

    body.innerHTML += `

    <tr>
      <td>#${index+1}</td>
      <td>${player.nickname}</td>
      <td>${player.score}</td>
      <td>${player.rank}</td>
    </tr>

    `;

  });

}

window.onload = ()=>{

  renderLeaderboard();

  let progress = 0;

  const fill =
  document.getElementById('loadingFill');

  const interval =
  setInterval(()=>{

    progress += 10;

    fill.style.width = progress + '%';

    if(progress >= 100){

      clearInterval(interval);

      setTimeout(()=>{

        document.getElementById('loadingScreen')
        .style.display='none';

      },500);

    }

  },120);

}

</script>

</body>
</html>
```
