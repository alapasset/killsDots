<template>
  <header>
    <div class="title">
      <canvas id="title-circle"></canvas>
      <h1>Kills Dots</h1>
    </div>
    <div class="menu">
    </div>
  </header>
</template>

<script lang="ts">
export default {
  mounted() {
    this.drawCircle()
  },
  methods: {
    drawCircle: () => {
      const canvas = document.getElementById("title-circle") as HTMLCanvasElement
      const context = canvas?.getContext("2d") as CanvasRenderingContext2D
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 11; j++) {
          context.strokeStyle = 'rgb(125, ' + Math.floor(255 - 100 * i) + ', ' + Math.floor(255 - 100 * j) + ')'
          context.beginPath()
          context.arc(12.5 + j * 25, 12.5 + i * 25, 10, 0, Math.PI * 2, true)
          context.stroke()
        }
      }
    }
  }
}
</script>

<style scoped>
  header {
    @apply flex flex-row h-16 items-center border-b-2;
  }

  header .title {
    @apply flex flex-row pl-4;
  }

  header .title canvas {
    @apply w-48 -mb-8;
  }

  header .title h1 {
    @apply h-16 font-black text-xl;
    line-height: 4rem;
  }

  header .menu {
    @apply flex-grow flex flex-row justify-end pr-4;
  }
</style>
