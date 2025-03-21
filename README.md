<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flappy Bird Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="website-container">
        <h1>Welcome to Flappy Bird Game!</h1>
        <div class="game-container">
            <div id="bird"></div>
            <div id="score">Score: 0</div>
        </div>
        <p>Press SPACE to flap the bird and avoid the poles!</p>
    </div>
    <script src="script.js"></script>
</body>
</html>

/* General Website Styling */
body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
    flex-direction: column;
}

.website-container {
    text-align: center;
}

h1 {
    color: #333;
    margin-bottom: 20px;
}

p {
    color: #555;
    margin-top: 20px;
}

/* Game Styling */
.game-container {
    position: relative;
    width: 400px;
    height: 600px;
    background-color: #70c5ce;
    border: 2px solid #000;
    overflow: hidden;
}

#bird {
    position: absolute;
    top: 50%;
    left: 50px;
    width: 40px;
    height: 40px;
    background-color: yellow;
    border-radius: 50%;
}

.pole {
    position: absolute;
    width: 60px;
    background-color: green;
}

#score {
    position: absolute;
    top: 10px;
    left: 10px;
    font-size: 24px;
    color: white;
    z-index: 10;
}
const bird = document.getElementById("bird");
const gameContainer = document.querySelector(".game-container");
const scoreElement = document.getElementById("score");

let birdTop = 250;
let gravity = 2;
let score = 0;
let gameInterval;
let poleInterval;

// Start the game
function startGame() {
    birdTop = 250;
    score = 0;
    scoreElement.innerText = "Score: " + score;
    bird.style.top = birdTop + "px";

    clearInterval(gameInterval);
    clearInterval(poleInterval);

    gameInterval = setInterval(applyGravity, 20);
    poleInterval = setInterval(createPole, 2000);
}

// Apply gravity to the bird
function applyGravity() {
    birdTop += gravity;
    bird.style.top = birdTop + "px";

    // Check if bird hits the ground or flies out of the screen
    if (birdTop > 560 || birdTop < 0) {
        endGame();
    }
}

// Create poles
function createPole() {
    const poleGap = 150;
    const poleHeight = Math.floor(Math.random() * 300) + 100;

    const topPole = document.createElement("div");
    topPole.classList.add("pole");
    topPole.style.height = poleHeight + "px";
    topPole.style.top = "0";
    topPole.style.left = "400px";

    const bottomPole = document.createElement("div");
    bottomPole.classList.add("pole");
    bottomPole.style.height = (600 - poleHeight - poleGap) + "px";
    bottomPole.style.bottom = "0";
    bottomPole.style.left = "400px";

    gameContainer.appendChild(topPole);
    gameContainer.appendChild(bottomPole);

    let polePosition = 400;

    const poleMoveInterval = setInterval(() => {
        polePosition -= 5;
        topPole.style.left = polePosition + "px";
        bottomPole.style.left = polePosition + "px";

        // Remove poles when they go off-screen
        if (polePosition < -60) {
            clearInterval(poleMoveInterval);
            gameContainer.removeChild(topPole);
            gameContainer.removeChild(bottomPole);
        }

        // Increase score when the bird passes a pole
        if (polePosition === 50) {
            score++;
            scoreElement.innerText = "Score: " + score;
        }

        // Check for collision
        if (polePosition < 90 && polePosition > 50 && (birdTop < poleHeight || birdTop > poleHeight + poleGap - 40)) {
            endGame();
        }
    }, 20);
}

// End the game
function endGame() {
    clearInterval(gameInterval);
    clearInterval(poleInterval);
    alert("Game Over! Your score is: " + score);
    startGame();
}

// Flap the bird when spacebar is pressed
document.addEventListener("keydown", function (e) {
    if (e.code === "Space") {
        birdTop -= 60;
        bird.style.top = birdTop + "px";
    }
});

// Start the game when the page loads
startGame();
