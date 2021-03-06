<template>
  <div class="board">
    <button @click="reset" class="btn btn-gray">Reset</button>
    <canvas id="game" @click="shoot($event)"></canvas>
  </div>
</template>

<script lang="ts">
import Ball from '../classes/Ball'

export default {
  data() {
    return {
      nbBalls: 25,
      canvas: {} as HTMLCanvasElement,
      context: {} as CanvasRenderingContext2D,
      width: 0,
      height: 0,
      balls: [] as Array<Ball>,
      animation: 0
    }
  },
  mounted() {
    this.canvas = document.getElementById("game") as HTMLCanvasElement
    this.context = this.canvas.getContext('2d') as CanvasRenderingContext2D
    this.width = this.canvas.width = window.innerWidth
    this.height = this.canvas.height = window.innerHeight - 64

    this.initAllBalls()
    this.loop()
  },
  methods: {
    initAllBalls() {
      while (this.balls.length < this.nbBalls) {
        let size = this.random(10,20)
        let ball = new Ball(
          this.random(0 + size, this.width - size),
          this.random(0 + size, this.height - size),
          this.random(-7,7),
          this.random(-7,7),
          'rgb(' + this.random(0,255) + ',' + this.random(0,255) + ',' + this.random(0,255) +')',
          size
        )

        this.balls.push(ball)
      }
    },
    loop() {
      this.context.fillStyle = 'rgba(0, 0, 0, 0.25)'
      this.context.fillRect(0, 0, this.width, this.height)
      let it = this.balls.length
      while(it--) {
        this.drawBall(this.balls[it])
        this.balls[it].update(this.width, this.height)
      }

      this.animation = requestAnimationFrame(this.loop)
    },
    drawBall(ball: Ball) {
      this.context.beginPath()
      this.context.fillStyle = ball.color
      this.context.arc(ball.x, ball.y, ball.size, 0, 2 * Math.PI)
      this.context.fill();
    },
    random(min: number, max: number): number {
      var num = Math.floor(Math.random() * (max - min + 1)) + min
      return num
    },
    reset() {
      cancelAnimationFrame(this.animation)
      this.balls = []
      this.initAllBalls()
      this.loop()
    },
    shoot(event: MouseEvent) {
      const offsetX = this.canvas.offsetLeft
      const offsetY = this.canvas.offsetTop
      const clickX = event.clientX - offsetX
      const clickY = event.clientY - (offsetY + 64)

      let it = this.balls.length
      while(it--) {
        const dx = this.balls[it].x - clickX
        const dy = this.balls[it].y - clickY
        if (dx * dx + dy * dy <= this.balls[it].size * this.balls[it].size) {
          let itDel = this.balls.length
          const ballsArray = []
          while(itDel--) {
            if (itDel !== it) {
              ballsArray.push(this.balls[itDel])
            }
          }
          this.balls = ballsArray
        }
      }

      return false
    }
  }
}
</script>

<style scoped>
  .board {
    @apply relative
  }

  .board canvas {
    @apply cursor-move;
  }

  .board .btn {
    @apply font-bold py-2 px-4 rounded absolute right-2 -top-14;
  }

  .board .btn-gray {
    @apply bg-gray-500 text-white;
  }

  .board .btn-gray:hover {
    @apply bg-gray-700;
  }
</style>
