# slot-machine
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>嚇鼠了神敲敲遊戲系統</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", sans-serif;
      text-align: center;
      background-color: #fef9e7;
      margin: 0;
      padding: 0;
    }

    h1 {
      background-color: #ff6f61;
      color: white;
      padding: 20px;
      margin: 0;
      font-size: 32px;
    }

    #game-area {
      margin: 30px auto;
      max-width: 400px;
    }

    #score, #timer {
      font-size: 20px;
      margin: 10px;
    }

    #grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-top: 20px;
    }

    .hole {
      width: 100px;
      height: 100px;
      background-color: #dcdcdc;
      border-radius: 10px;
      position: relative;
      cursor: pointer;
      overflow: hidden;
    }

    .mole {
      width: 80px;
      height: 80px;
      position: absolute;
      top: 10px;
      left: 10px;
      display: none;
    }

    .mole.show {
      display: block;
      animation: popUp 0.2s ease;
    }

    .hammer {
      position: absolute;
      width: 60px;
      height: 60px;
      top: -30px;
      left: 20px;
      display: none;
      pointer-events: none;
      z-index: 10;
    }

    .hammer.show {
      display: block;
      animation: hit 0.3s ease;
    }

    @keyframes popUp {
      from {
        transform: scale(0.5);
        opacity: 0;
      }
      to {
        transform: scale(1);
        opacity: 1;
      }
    }

    @keyframes hit {
      0% {
        transform: rotate(0deg) translateY(-10px);
        opacity: 1;
      }
      100% {
        transform: rotate(45deg) translateY(10px);
        opacity: 0;
      }
    }

    button {
      padding: 10px 20px;
      font-size: 18px;
      background-color: #28a745;
      color: white;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }

    button:hover {
      background-color: #218838;
    }
  </style>
</head>
<body>

  <h1>嚇鼠了神敲敲遊戲系統</h1>

  <div id="game-area">
    <div>
      <button onclick="startGame()">開始遊戲</button>
    </div>
    <div id="score">分數：0</div>
    <div id="timer">剩餘時間：30 秒</div>
    <div id="grid"></div>
  </div>

  <script>
    const grid = document.getElementById("grid");
    const scoreDisplay = document.getElementById("score");
    const timerDisplay = document.getElementById("timer");

    let score = 0;
    let timeLeft = 30;
    let moleInterval;
    let countdownInterval;
    let moles = [];

    // 建立 9 個地鼠洞
    for (let i = 0; i < 9; i++) {
      const hole = document.createElement("div");
      hole.classList.add("hole");

      const mole = document.createElement("img");
      mole.src = "image/gopher.png";
      mole.classList.add("mole");

      const hammer = document.createElement("img");
      hammer.src = "image/hammer.png";
      hammer.classList.add("hammer");

      mole.addEventListener("click", () => {
        if (mole.classList.contains("show")) {
          score++;
          scoreDisplay.textContent = `分數：${score}`;
          mole.classList.remove("show");

          // 顯示敲擊槌子動畫
          hammer.classList.add("show");
          setTimeout(() => {
            hammer.classList.remove("show");
          }, 300);
        }
      });

      hole.appendChild(mole);
      hole.appendChild(hammer);
      grid.appendChild(hole);
      moles.push(mole);
    }

    function randomMole() {
      moles.forEach(m => m.classList.remove("show"));
      const index = Math.floor(Math.random() * moles.length);
      moles[index].classList.add("show");
    }

    function startGame() {
      score = 0;
      timeLeft = 30;
      scoreDisplay.textContent = "分數：0";
      timerDisplay.textContent = "剩餘時間：30 秒";

      moleInterval = setInterval(randomMole, 700);
      countdownInterval = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = `剩餘時間：${timeLeft} 秒`;

        if (timeLeft <= 0) {
          clearInterval(moleInterval);
          clearInterval(countdownInterval);
          moles.forEach(m => m.classList.remove("show"));
          alert(`時間到！你的得分是：${score} 分`);
        }
      }, 1000);
    }
  </script>

</body>
</html>
