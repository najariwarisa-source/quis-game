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
      <p style="color: #
