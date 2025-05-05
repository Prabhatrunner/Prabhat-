<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Prabhat Runner Game</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: linear-gradient(#87CEEB, #ffffff);
      font-family: sans-serif;
    }
    #game {
      position: relative;
      width: 100vw;
      height: 100vh;
      background: #d0f4f7;
    }
    #prabhat {
      position: absolute;
      bottom: 0;
      left: 50px;
      width: 50px;
      height: 50px;
      background: url('https://i.imgur.com/1XfP9dT.png') no-repeat center/contain;
    }
    .obstacle {
      position: absolute;
      bottom: 0;
      width: 40px;
      height: 60px;
      background-color: #444;
      animation: moveObstacle 3s linear forwards;
    }
    @keyframes moveObstacle {
      0% { left: 100vw; }
      100% { left: -50px; }
    }
    #score {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 24px;
      color: #333;
      background: rgba(255, 255, 255, 0.7);
      padding: 10px;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <div id="game">
    <div id="score">Score: 0</div>
    <div id="prabhat"></div>
  </div>
  <audio id="bg-music" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" loop autoplay></audio>
  <script>
    const prabhat = document.getElementById("prabhat");
    const game = document.getElementById("game");
    const scoreDisplay = document.getElementById("score");
    let isJumping = false;
    let score = 0;
    let speed = 3000;

    function jump() {
      if (isJumping) return;
      isJumping = true;
      let up = 0;
      const jumpInterval = setInterval(() => {
        if (up >= 120) {
          clearInterval(jumpInterval);
          const downInterval = setInterval(() => {
            if (up <= 0) {
              clearInterval(downInterval);
              isJumping = false;
            } else {
              up -= 4;
              prabhat.style.bottom = up + "px";
            }
          }, 20);
        } else {
          up += 4;
          prabhat.style.bottom = up + "px";
        }
      }, 20);
    }

    function createObstacle() {
      const obstacle = document.createElement("div");
      obstacle.classList.add("obstacle");
      obstacle.style.animationDuration = speed / 1000 + "s";
      game.appendChild(obstacle);

      const moveInterval = setInterval(() => {
        const prabhatRect = prabhat.getBoundingClientRect();
        const obsRect = obstacle.getBoundingClientRect();

        if (
          obsRect.left < prabhatRect.right &&
          obsRect.right > prabhatRect.left &&
          obsRect.bottom > prabhatRect.top
        ) {
          alert("Game Over, Prabhat! Your score: " + score);
          location.reload();
        }
      }, 10);

      setTimeout(() => {
        if (game.contains(obstacle)) {
          game.removeChild(obstacle);
          clearInterval(moveInterval);
          score += 1;
          scoreDisplay.textContent = "Score: " + score;

          // Speed increase
          if (score % 5 === 0 && speed > 1000) {
            speed -= 200;
          }
        }
      }, speed);
    }

    document.addEventListener("keydown", (e) => {
      if (e.code === "Space") {
        jump();
      }
    });

    setInterval(createObstacle, speed);

    const music = document.getElementById("bg-music");
    music.volume = 0.5;
  </script>
</body>
</html>
