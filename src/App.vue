<template>
<div id="p5Canvas"></div>
<HelloWorld v-for="arr in ar"
:key=arr.id
:image-y=arr.top
:image-x=arr.left
:label-y=arr.top
:label-x=arr.left
:image1=arr.image1
:image2=arr.image2
:label=arr.label
:id=arr.id
/>

<div id="cursor" :style="{
  transform: 'translate(' + valuex + 'px,' + valuey + 'px)'
}"><img src="@/assets/cursor.png" alt=""></div>

</template>

<script setup>
import P5 from 'p5'
import { ref, reactive, onBeforeMount, onMounted } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

const ar = reactive(
  [{ id: 0, label: 'Armaan', top: Math.random() * window.innerHeight, left: Math.random() * window.innerWidth, image1: require('@/assets/1/1.jpeg'), image2: require('@/assets/1/2.jpeg') },
    { id: 1, label: 'Aaryan', top: 200, left: 900, image1: require('@/assets/2/1.jpeg'), image2: require('@/assets/2/2.jpeg') }
  ])

const sketch = (s) => {
  s.setup = () => {
    s.createCanvas(window.innerWidth, window.innerHeight)
  }
  s.draw = () => {
    s.background(200)
    s.strokeWeight(1)
    s.line(s.mouseX, s.mouseY, window.innerWidth - s.mouseX, window.innerHeight - s.mouseY)
    s.fill(200)
    s.strokeWeight(0)
    s.circle(s.mouseX, s.mouseY, 50)
    s.circle(window.innerWidth - s.mouseX, window.innerHeight - s.mouseY, 50)
  }
}

let d

onBeforeMount(() => {
  d = new P5(sketch, 'p5Canvas')
})

onMounted(() => {
  window.addEventListener('mousemove', update)
  d.background('red')
})
const valuex = ref(0)
const valuey = ref(0)
function update (event) {
  valuex.value = window.innerWidth - event.pageX
  valuey.value = window.innerHeight - event.pageY
}
</script>

<style lang="scss">

#p5Canvas{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#cursor{
  width: 25px;
  height: 25px;
  position: absolute;
  top: 0;
  left: 0;
  img{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
}
</style>
