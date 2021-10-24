<template>
  <canvas id="game"></canvas>
</template>

<script lang="ts">
import Ball from '../classes/Ball'

export default {
  data() {
    return {
      canvas: {} as HTMLCanvasElement,
      context: {} as CanvasRenderingContext2D,
      width: 0,
      height: 0,
      balls: [] as Array<Ball>
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
      while (this.balls.length < 25) {
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

      for (let i = 0; i < this.balls.length; i++) {
        this.drawBall(this.balls[i])
        this.balls[i].update(this.width, this.height)
      }

      requestAnimationFrame(this.loop)
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
    }
  }
}
</script>

<style scoped>

</style>
