<template>
  <h1>X & O Game</h1>
    <div id="game-board">
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
        <div class="cell" onclick="makeMove(this)"></div>
    </div>
    <div id="message">Player's Turn</div>
    <div id="score">
        <p>Player Wins: <span id="playerScore">0</span></p>
        <p>Computer Wins: <span id="computerScore">0</span></p>
      <!-- Add a button for starting a new round -->
<button id="newRoundButton" onclick="startNewRound()">New Round</button>

    </div>
</template>

<script>
const cells = document.querySelectorAll('.cell');
const message = document.getElementById('message');
const playerScoreDisplay = document.getElementById('playerScore');
const computerScoreDisplay = document.getElementById('computerScore');

let currentPlayer = 'X';
let playerScore = 0;
let computerScore = 0;
let gameBoard = ['', '', '', '', '', '', '', '', ''];
let gameOver = false;

function makeMove(cell) {
    const index = Array.from(cells).indexOf(cell);

    if (gameBoard[index] === '' && !gameOver) {
        gameBoard[index] = currentPlayer;
        cell.textContent = currentPlayer;
        cell.style.pointerEvents = 'none';

        if (checkWin()) {
            message.textContent = `${currentPlayer} wins!`;
            if (currentPlayer === 'X') {
                playerScore++;
                playerScoreDisplay.textContent = playerScore;
            } else {
                computerScore++;
                computerScoreDisplay.textContent = computerScore;
            }
            gameOver = true;
        } else if (gameBoard.indexOf('') === -1) {
            message.textContent = "It's a draw!";
            gameOver = true;
        } else {
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            message.textContent = `${currentPlayer}'s Turn`;
            if (currentPlayer === 'O') {
                setTimeout(computerMove, 500); // Delay for computer's move
            }
        }
    }
}

function computerMove() {
    if (!gameOver) {
        let index;
        do {
            index = Math.floor(Math.random() * 9);
        } while (gameBoard[index] !== '');

        makeMove(cells[index]);
    }
}

function checkWin() {
    const winPatterns = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
    ];

    for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[a] === gameBoard[c]) {
            cells[a].style.backgroundColor = 'lightgreen';
            cells[b].style.backgroundColor = 'lightgreen';
            cells[c].style.backgroundColor = 'lightgreen';
            return true;
        }
    }
    return false;
}

for (const cell of cells) {
    cell.addEventListener('click', () => makeMove(cell));
}

message.textContent = `${currentPlayer}'s Turn`;

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style CSS>
body {
    font-family: Arial, sans-serif;
    text-align: center;
}

h1 {
    color: #333;
}

#game-board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    gap: 10px;
    margin: 20px auto;
    max-width: 330px;
}

.cell {
    width: 100px;
    height: 100px;
    font-size: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #eee;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}

.cell:hover {
    background-color: #ccc;
}

#message {
    font-size: 1.2rem;
    margin-top: 10px;
}

#score {
    font-size: 1.2rem;
    margin-top: 20px;
}

#score p {
    margin: 5px;
}
</style>
