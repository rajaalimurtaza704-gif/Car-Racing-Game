<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Car Racing Game</title>
  <style>
    body { margin: 0; overflow: hidden; background: #111; }
    canvas { display: block; margin: auto; background: #333; }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="400" height="600"></canvas>
  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");let car = { x: 180, y: 500, width: 40, height: 80, speed: 5 };
let obstacles = [];
let gameOver = false;
let score = 0;

document.addEventListener("keydown", (e) => {
  if (e.key === "ArrowLeft" && car.x > 0) car.x -= car.speed * 2;
  if (e.key === "ArrowRight" && car.x < canvas.width - car.width) car.x += car.speed * 2;
});

function drawCar() {
  ctx.fillStyle = "#0f0";
  ctx.fillRect(car.x, car.y, car.width, car.height);
}

function drawObstacles() {
  ctx.fillStyle = "#f00";
  obstacles.forEach((obs) => {
    ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
  });
}

function updateObstacles() {
  if (Math.random() < 0.03) {
    obstacles.push({
      x: Math.random() * (canvas.width - 40),
      y: -80,
      width: 40,
      height: 80,
      speed: 3 + Math.random() * 2,
    });
  }
  obstacles.forEach((obs) => (obs.y += obs.speed));
  obstacles = obstacles.filter((obs) => obs.y < canvas.height);
}

function checkCollision() {
  for (let obs of obstacles) {
    if (
      car.x < obs.x + obs.width &&
      car.x + car.width > obs.x &&
      car.y < obs.y + obs.height &&
      car.y + car.height > obs.y
    ) {
      gameOver = true;
    }
  }
}

function drawScore() {
  ctx.fillStyle = "#fff";
  ctx.font = "20px Arial";
  ctx.fillText("Score: " + score, 10, 30);
}

function gameLoop() {
  if (gameOver) {
    ctx.fillStyle = "rgba(0,0,0,0.5)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "#fff";
    ctx.font = "40px Arial";
    ctx.fillText("Game Over", 100, 300);
    return;
  }

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawCar();
  drawObstacles();
  updateObstacles();
  checkCollision();
  drawScore();

  score++;
  requestAnimationFrame(gameLoop);
}

gameLoop();

  </script>
</body>
</html>
