<template>
  <!-- <HelloWorld msg="Welcome to Your Vue.js Aaaapp"/> -->
  
  <component :is="currentView" />
</template>

<script setup>

import { ref, computed } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import Cube from './components/Cube.vue'
import SCC from './components/SCC.vue'
import BasicWorld from './components/BasicWorld.vue'
import World from './components/World.vue'

    const routes = {
      '/': HelloWorld,
      '/cube': Cube,
      '/scc': SCC,
      '/bworld': BasicWorld,
      '/world': World
    }
    const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})


</script>

<style>
body{
  margin:0;
  overflow: hidden;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
