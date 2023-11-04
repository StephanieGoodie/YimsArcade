<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
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
