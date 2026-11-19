<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mini Car Racing</title>
<style>
body {
  margin: 0;
  background: #111;
  color: white;
  text-align: center;
  font-family: Arial;
}
h2 { color: #00ff99; }
#game {
  width: 300px;
  height: 500px;
  background: #444;
  margin: auto;
  position: relative;
  overflow: hidden;
  border: 4px solid white;
}
.car {
  width: 40px;
  height: 70px;
  position: absolute;
  bottom: 20px;
  left: 130px;
  background: red;
  border-radius: 10px;
}
.enemy {
  width: 40px;
  height: 70px;
  position: absolute;
  background: yellow;
  border-radius: 10px;
}
#score { font-size: 22px; }
button {
  padding: 15px 30px;
  margin: 10px;
  font-size: 20px;
  border-radius: 10px;
}
</style>
</head>
<body>

<h2>🏎️ Mini Car Racing</h2>
<div id="score">Score: 0</div>

<div id="game">
  <div class="car" id="car"></div>
</div>

<button onclick="moveLeft()">⬅️</button>
<button onclick="moveRight()">➡️</button>
<button onclick="restart()">🔄</button>

<script>
const game = document.getElementById("game");
const car = document.getElementById("car");
const scoreText = document.getElementById("score");

let carX = 130;
let score = 0;
let running = true;
let enemies = [];

function moveLeft() {
  if(carX > 10 && running) {
    carX -= 30;
    car.style.left = carX + "px";
  }
}

function moveRight() {
  if(carX < 250 && running) {
    carX += 30;
    car.style.left = carX + "px";
  }
}

document.addEventListener("keydown", function(e) {
  if(e.key === "ArrowLeft") moveLeft();
  if(e.key === "ArrowRight") moveRight();
});

function createEnemy() {
  if(!running) return;

  let enemy = document.createElement("div");
  enemy.className = "enemy";
  enemy.style.left = Math.floor(Math.random() * 250) + "px";
  enemy.style.top = "-70px";
  game.appendChild(enemy);
  enemies.push(enemy);
}

function gameLoop() {
  if(!running) return;

  enemies.forEach((enemy, index) => {
    let y = parseInt(enemy.style.top);
    y += 5;
    enemy.style.top = y + "px";

    if(y > 430 && y < 490) {
      let ex = parseInt(enemy.style.left);
      if(Math.abs(ex - carX) < 40) {
        running = false;
        alert("Game Over! Score: " + score);
      }
    }

    if(y > 500) {
      enemy.remove();
      enemies.splice(index, 1);
      score++;
      scoreText.innerText = "Score: " + score;
    }
  });

  requestAnimationFrame(gameLoop);
}

function restart() {
  location.reload();
}

setInterval(createEnemy, 1000);
gameLoop();
</script>

</body>
</html>
