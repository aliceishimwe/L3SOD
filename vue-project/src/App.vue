<template>
  <div class="game">
    <h1>Snake Game 🐍</h1>

    <p>Score: {{ score }}</p>

    <div class="board">
      <div
        v-for="(cell, index) in gridSize"
        :key="index"
        class="cell"
        :class="getClass(index)"
      ></div>
    </div>

    <p v-if="gameOver" class="over">Game Over 💀</p>
    <button @click="resetGame">Restart 🔄</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gridSize: 100, // 10x10 grid
      snake: [22, 21, 20],
      direction: 1,
      food: 50,
      score: 0,
      gameOver: false,
      interval: null
    }
  },

  mounted() {
    window.addEventListener("keydown", this.changeDirection)
    this.startGame()
  },

  methods: {
    startGame() {
      this.interval = setInterval(this.moveSnake, 300)
    },

    moveSnake() {
      if (this.gameOver) return

      let head = this.snake[0]
      let newHead = head + this.direction

      // wall collision
      if (
        newHead < 0 ||
        newHead >= 100 ||
        this.snake.includes(newHead)
      ) {
        this.gameOver = true
        clearInterval(this.interval)
        return
      }

      this.snake.unshift(newHead)

      // eat food
      if (newHead === this.food) {
        this.score++
        this.food = Math.floor(Math.random() * 100)
      } else {
        this.snake.pop()
      }
    },

    changeDirection(e) {
      if (e.key === "ArrowRight") this.direction = 1
      if (e.key === "ArrowLeft") this.direction = -1
      if (e.key === "ArrowUp") this.direction = -10
      if (e.key === "ArrowDown") this.direction = 10
    },

    getClass(index) {
      if (this.snake.includes(index)) return "snake"
      if (this.food === index) return "food"
    },

    resetGame() {
      this.snake = [22, 21, 20]
      this.direction = 1
      this.food = 50
      this.score = 0
      this.gameOver = false
      this.startGame()
    }
  }
}
</script>

<style>
.game {
  text-align: center;
  font-family: Arial;
}

.board {
  display: grid;
  grid-template-columns: repeat(10, 25px);
  justify-content: center;
  margin: 20px;
}

.cell {
  width: 25px;
  height: 25px;
  border: 1px solid #ddd;
}

.snake {
  background: green;
}

.food {
  background: red;
}

.over {
  color: red;
  font-size: 20px;
  font-weight: bold;
}
</style>