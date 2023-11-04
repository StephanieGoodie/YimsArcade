<template>
  <div>
    <h1>X & O Game</h1>
    <div id="game-board">
      <div class="cell" v-for="(cell, index) in gameBoard" :key="index" @click="makeMove(index)">
        {{ cell }}
      </div>
    </div>
    <div id="message">{{ message }}</div>
    <div id="score">
      <p>Player Wins: <span id="playerScore">{{ playerScore }}</span></p>
      <p>Computer Wins: <span id="computerScore">{{ computerScore }}</span></p>
      <!-- Add a button for starting a new round -->
      <button id="newRoundButton" @click="startNewRound">New Round</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gameBoard: Array(9).fill(''),
      message: "Player's Turn",
      playerScore: 0,
      computerScore: 0,
      currentPlayer: 'X',
      gameOver: false,
    };
  },
  methods: {
    makeMove(index) {
      if (this.gameBoard[index] === '' && !this.gameOver) {
        this.gameBoard[index] = this.currentPlayer;

        if (this.checkWin()) {
          this.message = `${this.currentPlayer} wins!`;
          if (this.currentPlayer === 'X') {
            this.playerScore++;
          } else {
            this.computerScore++;
          }
          this.gameOver = true;
        } else if (this.gameBoard.indexOf('') === -1) {
          this.message = "It's a draw!";
          this.gameOver = true;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
          this.message = `${this.currentPlayer}'s Turn`;
          if (this.currentPlayer === 'O') {
            setTimeout(this.computerMove, 500); // Delay for the computer's move
          }
        }
      }
    },
    computerMove() {
      if (!this.gameOver) {
        let index;
        do {
          index = Math.floor(Math.random() * 9);
        } while (this.gameBoard[index] !== '');

        this.makeMove(index);
      }
    },
    checkWin() {
      const winPatterns = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6],
      ];

      for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (
          this.gameBoard[a] &&
          this.gameBoard[a] === this.gameBoard[b] &&
          this.gameBoard[a] === this.gameBoard[c]
        ) {
          return true;
        }
      }
      return false;
    },
    startNewRound() {
      this.currentPlayer = 'X';
      this.gameBoard = Array(9).fill('');
      this.gameOver = false;
      this.message = `${this.currentPlayer}'s Turn`;
    },
  },
};
</script>

<style scoped>
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
