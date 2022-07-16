<template>
<HelloWorld
:top=ar.top
:left=ar.left
:bottom=ar.top
:right=ar.left
/>
</template>

<script setup>
import { reactive } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
const ar = reactive({ id: 0, top: Math.random() * window.innerHeight, left: Math.random() * window.innerWidth })

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
