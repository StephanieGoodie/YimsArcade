<template>
  <div id="game-container">
    <div id="bird" :style="{ top: birdY + 'px' }"></div>
    <div
      class="pipe"
      v-for="(pipe, index) in pipes"
      :key="index"
      :style="pipeStyle(index)"
    ></div>
    <button @click="startGame">Start</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      birdY: 150,
      velocity: 0,
      gravity: 1,
      pipes: [],
      gameInterval: null,
    };
  },
  methods: {
    birdStyle() {
      return { transform: `translate(100px, ${this.birdY}px)` };
    },
    pipeStyle(index) {
      const pipe = this.pipes[index];
      return {
        left: `${pipe.x}px`,
        height: `${pipe.height}px`,
      };
    },
    updateBirdPosition() {
      this.birdY += this.velocity;
      this.velocity += this.gravity;
    },
    updatePipes() {
      for (let i = 0; i < this.pipes.length; i++) {
        this.pipes[i].x -= 2; // Adjust the speed of pipes here
        if (this.pipes[i].x < -100) {
          this.pipes.splice(i, 1);
        }
      }
      if (this.pipes.length < 3 && Math.random() < 0.02) {
        this.pipes.push({ x: 400, height: Math.floor(Math.random() * 150) + 100 });
      }
    },
    checkCollision() {
      const birdTop = this.birdY;
      const birdBottom = this.birdY + 50;
      const birdLeft = 100;
      const birdRight = 150;

      for (let i = 0; i < this.pipes.length; i++) {
        const pipe = this.pipes[i];
        const pipeTop = 0;
        const pipeBottom = pipe.height;
        const pipeLeft = pipe.x;
        const pipeRight = pipe.x + 100;

        if (
          birdBottom > pipeTop &&
          birdTop < pipeBottom &&
          birdRight > pipeLeft &&
          birdLeft < pipeRight
        ) {
          this.endGame();
        }
      }
    },
    startGame() {
      if (this.gameInterval === null) {
        this.birdY = 150;
        this.pipes = [];
        this.velocity = 0;
        this.gameInterval = setInterval(() => {
          this.updateBirdPosition();
          this.updatePipes();
          this.checkCollision();
        }, 20);
      }
    },
    endGame() {
      clearInterval(this.gameInterval);
      this.gameInterval = null;
    },
  },
};
</script>

<style scoped>
#game-container {
  width: 400px;
  height: 300px;
  position: relative;
  overflow: hidden;
  background-color: #87ceeb; /* Sky blue */
  margin: 0 auto;
  border: 2px solid #000;
}

#bird {
  width: 50px;
  height: 50px;
  background-color: #ff5733; /* Reddish-Orange */
  position: absolute;
}

.pipe {
  width: 100px;
  background-color: #228b22; /* Forest green */
  position: absolute;
  bottom: 0;
}
</style>
