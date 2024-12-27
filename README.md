# Supers-Games
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Catch the Falling Objects</title>
    <link rel="stylesheet" href="styles.css"> <!-- Link to styles.css -->
</head>
<body>
    <div id="game-container">
        <div id="basket"></div>
        <div id="score">Score: 0</div>
        <div id="game-over" style="display: none;">Game Over!</div>
    </div>

    <script src="game.js"></script> <!-- Link to game.js -->
</body>
</html>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
}

#game-container {
    position: relative;
    width: 400px;
    height: 600px;
    background-color: #ffffff;
    border: 2px solid #333;
    overflow: hidden;
}

#basket {
    position: absolute;
    bottom: 0;
    left: 50%;
    width: 80px;
    height: 20px;
    background-color: #008080;
    border-radius: 10px;
    transform: translateX(-50%);
}

.falling-object {
    position: absolute;
    width: 30px;
    height: 30px;
    background-color: red;
    border-radius: 50%;
}

#score {
    position: absolute;
    top: 10px;
    left: 10px;
    color: #333;
    font-size: 18px;
}

#game-over {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 30px;
    color: red;
    font-weight: bold;
}
// Game settings
const basket = document.getElementById('basket');
const gameContainer = document.getElementById('game-container');
const scoreDisplay = document.getElementById('score');
const gameOverDisplay = document.getElementById('game-over');

let score = 0;
let gameOver = false;

// Basket movement
let basketPosition = 150; // Initial position (center)
const basketSpeed = 10; // Speed at which the basket moves

// Create a falling object
function createFallingObject() {
    const object = document.createElement('div');
    object.classList.add('falling-object');
    object.style.left = `${Math.random() * (gameContainer.offsetWidth - 30)}px`; // Random x position
    gameContainer.appendChild(object);

    // Move the falling object
    let objectPosition = 0;
    const objectSpeed = 3 + Math.random() * 3; // Random falling speed

    function fall() {
        if (gameOver) return;

        objectPosition += objectSpeed;
        object.style.top = `${objectPosition}px`;

        // Check if the object reaches the bottom
        if (objectPosition >= gameContainer.offsetHeight - 20) {
            // Check if the object is caught by the basket
            if (Math.abs(parseInt(object.style.left) - basketPosition) < 80) {
                score++;
                scoreDisplay.textContent = `Score: ${score}`;
                object.remove();
            } else {
                gameOver = true;
                gameOverDisplay.style.display = 'block';
            }
        }

        if (!gameOver) {
            requestAnimationFrame(fall);
        }
    }

    fall();
}

// Control the basket with arrow keys
document.addEventListener('keydown', (e) => {
    if (gameOver) return;

    if (e.key === 'ArrowLeft' && basketPosition > 0) {
        basketPosition -= basketSpeed;
    } else if (e.key === 'ArrowRight' && basketPosition < gameContainer.offsetWidth - 80) {
        basketPosition += basketSpeed;
    }

    basket.style.left = `${basketPosition}px`;
});

// Generate falling objects at intervals
function startGame() {
    setInterval(() => {
        if (!gameOver) {
            createFallingObject();
        }
    }, 1000); // Create new object every second
}

// Start the game
startGame();
