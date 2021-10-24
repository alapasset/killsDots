<script lang="ts">
import { defineComponent } from 'vue'
import Header from './components/Header.vue'
import Game from './components/Game.vue'

export default defineComponent({
  name: 'App',
  components: { Header, Game },
})
</script>

<template>
  <Header />
  <Game />
</template>

<style>
</style>
