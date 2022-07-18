<template>
<div id="p5Canvas"></div>
<debugBro v-for="rum in debug"
:key=rum.id
:x1=rum.im.x
:x2=rum.la.x
:y1=rum.im.y
:y2=rum.la.y
/>
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
import debugBro from './components/debugBro.vue'
const locations = []
const reverseLocations = []
const assignedLocations = []
const width = window.innerWidth
const height = window.innerHeight
const boxes = 150
const border = 100
// const boxBorder = 10
// const boxSize = 100
function findAlocation () {
  let r = 0
  let ar = []
  let collide = true
  while (collide) {
    ar = checkCollision()
    collide = ar[0]
    r = ar[1]
  }
  assignedLocations.push({ x: parseInt(locations[r].x), y: parseInt(locations[r].y) })
  assignedLocations.push({ x: parseInt(reverseLocations[r].x), y: parseInt(reverseLocations[r].y) })
  const value = { i: locations[r], l: reverseLocations[r] }
  locations.splice(r, 1)
  reverseLocations.splice(r, 1)
  return value
}
function checkCollision () {
  const r = parseInt(Math.random() * locations.length)
  let collision = false
  for (let i = 0; i < assignedLocations.length; i++) {
    // check if colliding
    // check if locations is colliding with all
    if (locations[r].x + 100 >= assignedLocations[i].x &&
       locations[r].x <= assignedLocations[i].x + 100 &&
       locations[r].y + 100 >= assignedLocations[i].y &&
       locations[r].y <= assignedLocations[i].y + 100) {
      collision = true
    }
    if (reverseLocations[r].x + 100 >= assignedLocations[i].x &&
       reverseLocations[r].x <= assignedLocations[i].x + 100 &&
       reverseLocations[r].y + 100 >= assignedLocations[i].y &&
       reverseLocations[r].y <= assignedLocations[i].y + 100) {
      collision = true
    }
    // check if reverseLocations is collidiing with all
    // return true or false
    // return the value
  }
  return [collision, r]
}
function locationConstructor () {
  const w = width
  const h = height
  const sqrt = (w * h) / boxes
  const diff = Math.sqrt(sqrt)
  for (let i = 0; i < w; i += diff) {
    for (let y = 0; y < h; y += diff) {
      locations.push({ x: parseInt(i), y: parseInt(y) })
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
      reverseLocations.push({ x: parseInt(i) - 100, y: parseInt(y) - 100 })
    }
  }
}
reverselocationConstructor()
locationConstructor()
function cleanUp () {
  const indexToRemove = []
  for (let i = 0; i < locations.length; i++) {
    const x = locations[i].x
    const y = locations[i].y
    if (x < border) {
      indexToRemove.push(i)
    }
    if (y < border) {
      indexToRemove.push(i)
    }
    if (x > (width - border)) {
      indexToRemove.push(i)
    }
    if (y > (height - border)) {
      indexToRemove.push(i)
    }
  }
  const indexFixer = (value, index, self) => {
    return self.indexOf(value) === index
  }
  const unique = indexToRemove.filter(indexFixer)
  for (let i = 0; i < unique.length; i++) {
    locations.splice(unique[i] - i, 1)
    reverseLocations.splice(unique[i] - i, 1)
  }
}
cleanUp()
const ar = reactive(
  [{ id: 0, label: 'Armaan', loc: findAlocation(), image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png') },
    { id: 1, label: 'Aaryan', loc: findAlocation(), image1: require('@/assets/2/1.png'), image2: require('@/assets/2/2.png') },
    { id: 2, label: 'Papa', loc: findAlocation(), image1: require('@/assets/3/1.png'), image2: require('@/assets/3/2.png') },
    { id: 3, label: 'Muma', loc: findAlocation(), image1: require('@/assets/4/1.png'), image2: require('@/assets/4/2.png') },
    { id: 0, label: 'Armaan', loc: findAlocation(), image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png') },
    { id: 1, label: 'Aaryan', loc: findAlocation(), image1: require('@/assets/2/1.png'), image2: require('@/assets/2/2.png') },
    { id: 2, label: 'Papa', loc: findAlocation(), image1: require('@/assets/3/1.png'), image2: require('@/assets/3/2.png') },
    { id: 3, label: 'Muma', loc: findAlocation(), image1: require('@/assets/4/1.png'), image2: require('@/assets/4/2.png') },
    { id: 0, label: 'Armaan', loc: findAlocation(), image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png') },
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
    s.strokeWeight(2)
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

console.log(assignedLocations)
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

const debug = []

for (let i = 0; i < locations.length; i++) {
  debug.push({ id: i, im: locations[i], la: reverseLocations[i] })
}
console.log(debug)
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
