<template>
<div id="p5Canvas"></div>
<HelloWorld v-for="arr in ar"
:key=arr.id
:image-y=arr.loc.i.y
:image-x=arr.loc.i.x
:label-y=arr.loc.l.y
:label-x=arr.loc.l.x
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
const locations = []
const reverseLocations = []
const width = window.innerWidth
const height = window.innerHeight
const boxes = 100
function findAlocation () {
  const r = parseInt(Math.random() * locations.length)
  const value = { i: locations[r], l: reverseLocations[r] }
  locations.splice(r, 1)
  reverseLocations.splice(r, 1)
  return value
}

function locationConstructor () {
  const w = width
  const h = height
  const sqrt = (w * h) / boxes
  const diff = Math.sqrt(sqrt)
  for (let i = 0; i < w; i += diff) {
    for (let y = 0; y < h; y += diff) {
      locations.push({ x: i, y: y })
    }
  }
}

function reverselocationConstructor () {
  const w = width
  const h = height
  const sqrt = (w * h) / boxes
  const diff = Math.sqrt(sqrt)
  for (let i = w; i > 0; i -= diff) {
    for (let y = h; y > 0; y -= diff) {
      reverseLocations.push({ x: i, y: y })
    }
  }
}
reverselocationConstructor()
locationConstructor()

console.log(reverseLocations, locations)
const ar = reactive(
  [{ id: 0, label: 'Armaan', loc: findAlocation(), image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png') },
    { id: 1, label: 'Aaryan', loc: findAlocation(), image1: require('@/assets/2/1.png'), image2: require('@/assets/2/2.png') },
    { id: 2, label: 'Papa', loc: findAlocation(), image1: require('@/assets/3/1.png'), image2: require('@/assets/3/2.png') },
    { id: 3, label: 'Muma', loc: findAlocation(), image1: require('@/assets/4/1.png'), image2: require('@/assets/4/2.png') }
  ])

const sketch = (s) => {
  s.setup = () => {
    s.createCanvas(window.innerWidth, window.innerHeight)
  }
  s.draw = () => {
    // const w = width
    // const h = height
    // const sqrt = (w * h) / 400
    // const diff = Math.sqrt(sqrt)
    s.background(200)
    s.strokeWeight(2)
    s.line(s.mouseX, s.mouseY, width - s.mouseX, height - s.mouseY)
    s.fill(200)
    s.strokeWeight(0)
    s.circle(s.mouseX, s.mouseY, 50)
    s.circle(width - s.mouseX, height - s.mouseY, 50)
    // for (let i = 0; i < w; i += diff) {
    //   for (let y = 0; y < h; y += diff) {
    //     s.strokeWeight(0.5)
    //     s.stroke(100)
    //     s.line(i, 0, i, h)
    //     s.line(0, y, w, y)
    //   }
    // }
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
  valuex.value = width - event.pageX
  valuey.value = height - event.pageY
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
