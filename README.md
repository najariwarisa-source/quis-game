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
  background:#120f1d;
  color:white;
  overflow-x:hidden;
}

/* =========================
LOADING
========================= */

#loadingScreen{
  position:fixed;
  inset:0;
  background:#0f0f16;
  z-index:99999;

  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction:column;
}

#loadingScreen h1{
  font-family:'Orbitron';
  font-size:3rem;
  color:#ff4655;
  letter-spacing:5px;
}

.loading-bar{
  width:260px;
  height:8px;
  background:#222;
  border-radius:30px;
  margin-top:25px;
  overflow:hidden;
}

.loading-fill{
  width:0%;
  height:100%;
  background:#ff4655;
}

/* =========================
HERO
========================= */

.hero{
  min-height:100vh;

  background:
  linear-gradient(rgba(0,0,0,.45),rgba(0,0,0,.85)),
  url('https://images.unsplash.com/photo-1511512578047-dfb367046420?q=80&w=1920&auto=format&fit=crop')
  center/cover no-repeat;

  position:relative;

  display:flex;
  justify-content:center;
  align-items:center;

  text-align:center;

  overflow:hidden;
}

.hero::before{
  content:'';
  position:absolute;
  inset:0;
  backdrop-filter:blur(3px);
}

.hero-content{
  position:relative;
  z-index:2;
}

.hero h1{
  font-size:6rem;
  font-family:'Orbitron';

  text-shadow:
  0 0 10px #ff4655,
  0 0 30px #ff4655;

  letter-spacing:5px;
}

.hero p{
  margin-top:20px;
  font-size:1.3rem;
  color:#ddd;
  letter-spacing:2px;
}

.hero button{
  margin-top:35px;
}

.character{
  position:absolute;
  bottom:0;
  left:50%;
  transform:translateX(-50%);
  z-index:1;
}

.character img{
  width:430px;

  filter:
  drop-shadow(0 0 30px rgba(255,70,85,.7));
}

/* =========================
CURVE
========================= */

.curve{
  position:absolute;
  bottom:-1px;
  width:100%;
  height:140px;
  background:#1a1525;
  border-radius:100% 100% 0 0;
  z-index:2;
}

/* =========================
SECTION
========================= */

.section{
  padding:120px 20px;
  background:#1a1525;
  text-align:center;
}

.section h2{
  font-family:'Orbitron';
  font-size:3rem;
  margin-bottom:25px;
  color:#ffe7a1;
}

.section p{
  max-width:900px;
  margin:auto;
  line-height:1.8;
  color:#ccc;
  font-size:1.1rem;
}

/* =========================
SCREEN
========================= */

.screen{
  display:none;
  min-height:100vh;
  justify-content:center;
  align-items:center;
  padding:30px;
  background:#15121f;
}

.active{
  display:flex;
}

/* =========================
CARD
========================= */

.card{
  width:100%;
  max-width:900px;

  background:rgba(255,255,255,.05);

  backdrop-filter:blur(15px);

  border:1px solid rgba(255,255,255,.08);

  border-radius:25px;

  padding:40px;

  box-shadow:
  0 0 40px rgba(255,70,85,.15);
}

/* =========================
PROFILE
========================= */

.profile-card{
  max-width:450px;
}

.title{
  text-align:center;
  margin-bottom:30px;
}

.title h2{
  font-family:'Orbitron';
  color:#ff4655;
  font-size:2rem;
}

.input-group{
  margin-bottom:20px;
}

label{
  display:block;
  margin-bottom:10px;
  color:#ff9ca5;
}

input{
  width:100%;
  padding:16px;

  background:#111827;

  border:none;

  border-radius:14px;

  color:white;

  font-size:1rem;

  border:1px solid rgba(255,255,255,.08);
}

/* =========================
BUTTON
========================= */

button{
  width:100%;
  padding:17px;

  border:none;

  border-radius:15px;

  background:#ff4655;

  color:white;

  font-family:'Orbitron';

  cursor:pointer;

  transition:.3s;

  letter-spacing:1px;
}

button:hover{
  transform:translateY(-2px);
  background:#ff6470;
}

/* =========================
QUIZ
========================= */

.top-bar{
  display:flex;
  justify-content:space-between;
  flex-wrap:wrap;
  gap:15px;
  margin-bottom:30px;
}

.badge{
  background:rgba(255,70,85,.1);

  border:1px solid #ff4655;

  padding:12px 18px;

  border-radius:14px;
}

.question-number{
  color:#ff4655;
  margin-bottom:15px;
  font-family:'Orbitron';
}

.question{
  font-size:2rem;
  margin-bottom:30px;
  line-height:1.5;
}

.answers{
  display:grid;
  gap:15px;
}

.answer-btn{
  text-align:left;

  background:#1b2230;

  border:1px solid #2d384d;

  transition:.25s;
}

.answer-btn:hover{
  border-color:#ff4655;
  background:#232d40;
}

.correct{
  background:#1f8f4d !important;
}

.wrong{
  background:#c53030 !important;
}

/* =========================
RESULT
========================= */

.result-box{
  text-align:center;
}

.score{
  font-size:6rem;
  color:#ff4655;
  margin:20px 0;
  font-family:'Orbitron';
}

.rank{
  font-size:2.3rem;
  color:gold;
  margin-bottom:20px;
  font-family:'Orbitron';
}

.final-text{
  color:#ddd;
  margin-bottom:40px;
}

/* =========================
TABLE
========================= */

table{
  width:100%;
  border-collapse:collapse;
  margin-top:25px;
}

th,td{
  padding:15px;
  border-bottom:1px solid rgba(255,255,255,.08);
  text-align:center;
}

th{
  color:#ff4655;
  font-family:'Orbitron';
}

/* =========================
RESPONSIVE
========================= */

@media(max-width:768px){

.hero h1{
  font-size:3rem;
}

.character img{
  width:280px;
}

.question{
  font-size:1.3rem;
}

.score{
  font-size:3rem;
}

.section h2{
  font-size:2rem;
}

}

</style>
</head>
<body>

<!-- ======================
LOADING
====================== -->

<div id="loadingScreen">

  <h1>DATA STRIKE</h1>

  <p style="margin-top:10px;color:#aaa">
    Initializing Mission...
  </p>

  <div class="loading-bar">
    <div class="loading-fill" id="loadingFill"></div>
  </div>

</div>

<!-- ======================
HERO
====================== -->

<section class="hero" id="heroSection">

  <div class="hero-content">

    <h1>DATA STRIKE</h1>

    <p>Genuine Storytelling Project</p>

    <button onclick="openProfile()">
      START MISSION
    </button>

  </div>

  <div class="character">

    <img src="https://i.imgur.com/8Km9tLL.png">

  </div>

  <div class="curve"></div>

</section>

<!-- ======================
VISION
====================== -->

<section class="section">

  <h2>VISION</h2>

  <p>
    Data Strike adalah quiz game interaktif bertema fantasy
    yang menggabungkan analisis data, storytelling,
    dan sistem ranking layaknya game kompetitif modern.
  </p>

</section>

<!-- ======================
PROFILE
====================== -->

<section class="screen" id="profileScreen">

  <div class="card profile-card">

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

<!-- ======================
QUIZ
====================== -->

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

    <div class="question-number" id="questionNumber">
      MISSION 01
    </div>

    <div class="question" id="questionText"></div>

    <div class="answers" id="answersContainer"></div>

  </div>

</section>

<!-- ======================
RESULT
====================== -->

<section class="screen" id="resultScreen">

  <div class="card result-box">

    <h1>MISSION COMPLETE</h1>

    <div class="score" id="finalScore">
      0
    </div>

    <div class="rank" id="rankText">
      IRON
    </div>

    <p class="final-text" id="playerFinal"></p>

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

/* ======================
QUESTIONS
====================== */

const questions = [

{
question:'Analisis data berarti ...',
answers:[
'Menggambar grafik',
'Mengolah data menjadi informasi',
'Menghapus data',
'Menyimpan file'
],
correct:1
},

{
question:'Visualisasi data digunakan untuk ...',
answers:[
'Mempersulit informasi',
'Mempermudah memahami data',
'Menghapus data',
'Menggambar anime'
],
correct:1
},

{
question:'Grafik batang digunakan untuk ...',
answers:[
'Perbandingan data',
'Memasak data',
'Membuat video',
'Menghapus tabel'
],
correct:0
},

{
question:'Data mentah perlu diolah karena ...',
answers:[
'Sudah sempurna',
'Tidak langsung memberi informasi',
'Tidak penting',
'Tidak berguna'
],
correct:1
},

{
question:'Contoh analisis data di sekolah adalah ...',
answers:[
'Bermain game',
'Menggambar',
'Menghitung rata-rata nilai',
'Tidur'
],
correct:2
}

];

/* ======================
VARIABLE
====================== */

let currentQuestion = 0;
let score = 0;
let timer;
let timeLeft = 20;
let nickname = '';

/* ======================
OPEN PROFILE
====================== */

function openProfile(){

window.scrollTo({
top:document.body.scrollHeight,
behavior:'smooth'
});

document.getElementById('profileScreen')
.classList.add('active');

}

/* ======================
START QUIZ
====================== */

function startQuiz(){

const name =
document.getElementById('name').value.trim();

nickname =
document.getElementById('nickname').value.trim();

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

/* ======================
LOAD QUESTION
====================== */

function loadQuestion(){

clearInterval(timer);

timeLeft = 20;

document.getElementById('timer')
.innerText = timeLeft;

const q = questions[currentQuestion];

document.getElementById('questionNumber')
.innerText =
`MISSION ${String(currentQuestion+1).padStart(2,'0')}`;

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

if(timeLeft <= 5){

document.querySelectorAll('.badge')[2]
.style.color = '#ff0000';

}

if(timeLeft <= 0){

clearInterval(timer);

nextQuestion();

}

},1000);

}

/* ======================
SELECT ANSWER
====================== */

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

/* ======================
NEXT QUESTION
====================== */

function nextQuestion(){

currentQuestion++;

if(currentQuestion < questions.length){

loadQuestion();

}else{

finishQuiz();

}

}

/* ======================
RANK
====================== */

function getRank(score){

if(score >= 100) return 'RADIANT';
if(score >= 80) return 'PLATINUM';
if(score >= 60) return 'GOLD';
if(score >= 40) return 'SILVER';

return 'IRON';

}

/* ======================
FINISH
====================== */

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
`Player <strong>${nickname}</strong>
berhasil menyelesaikan mission`;

saveLeaderboard();

}

/* ======================
LEADERBOARD
====================== */

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

/* ======================
LOADING
====================== */

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
