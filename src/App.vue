<template>
<div id="p5Canvas"></div>
<!-- <debugBro v-for="rum in debug"
:key=rum.id
:x1=rum.im.x
:x2=rum.la.x
:y1=rum.im.y
:y2=rum.la.y
/> -->
<MembersComponent v-for="arr in ar"
:key=arr.id
:image-y=arr.loc.i.y
:image-x=arr.loc.i.x
:label-y=arr.loc.l.y
:label-x=arr.loc.l.x
:image1=arr.image1
:image2=arr.image2
:label=arr.label
:display=arr.display
:id=arr.id
/>

<div id="cursor" :style="{
  transform: 'translate(' + valuex + 'px,' + valuey + 'px)'
}"><img src="@/assets/cursor.png" alt=""></div>

</template>

<script setup>
import P5 from 'p5'
import { ref, reactive, onBeforeMount, onMounted, provide } from 'vue'
import MembersComponent from './components/membersComponent.vue'
// import debugBro from './components/debugBro.vue'
const locations = []
const reverseLocations = []
const assignedLocations = []
const width = window.innerWidth
const height = window.innerHeight
const boxes = 1000
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
function removeBorderLocations () {
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
reverselocationConstructor()
locationConstructor()
removeBorderLocations()
const ar = reactive(
  [{ id: 0, label: 'Armaan', display: true, connections: [2, 3], image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png'), loc: findAlocation() },
    { id: 1, label: 'Aaryan', display: true, connections: [2, 3], image1: require('@/assets/2/1.png'), image2: require('@/assets/2/2.png'), loc: findAlocation() },
    { id: 2, label: 'Papa', display: false, connections: [0, 1], image1: require('@/assets/3/1.png'), image2: require('@/assets/3/2.png'), loc: findAlocation() },
    { id: 3, label: 'Muma', display: false, connections: [0, 1, 5, 6], image1: require('@/assets/4/1.png'), image2: require('@/assets/4/2.png'), loc: findAlocation() },
    { id: 4, label: 'Appy Mamu', display: false, connections: [5, 6], image1: require('@/assets/5/1.png'), image2: require('@/assets/5/2.png'), loc: findAlocation() },
    { id: 5, label: 'Baba', display: false, connections: [4, 5], image1: require('@/assets/6/1.png'), image2: require('@/assets/6/2.png'), loc: findAlocation() },
    { id: 6, label: 'Nani', display: false, connections: [4, 5], image1: require('@/assets/7/1.png'), image2: require('@/assets/7/2.png'), loc: findAlocation() }
  ])
const sketch = (s) => {
  s.setup = () => {
    s.createCanvas(window.innerWidth, window.innerHeight)
  }
  s.draw = () => {
    const w = width
    const h = height
    const sqrt = (w * h) / 400
    const diff = Math.sqrt(sqrt)
    s.background(200)
    s.strokeWeight(2)
    s.stroke(0)
    s.line(s.mouseX, s.mouseY, width - s.mouseX, height - s.mouseY)
    s.fill(200)
    s.strokeWeight(2)
    s.circle(s.mouseX, s.mouseY, 50)
    s.circle(width - s.mouseX, height - s.mouseY, 50)
    for (let i = 0; i < w; i += diff) {
      for (let y = 0; y < h; y += diff) {
        s.strokeWeight(0.5)
        s.stroke(100, 100, 100, 2)
        s.line(i, 0, i, h)
        s.line(0, y, w, y)
      }
    }
    s.strokeWeight(1)
    s.stroke(80, 80, 80, 20)
    for (let i = 0; i < s.array.length; i++) {
      if (s.array[i].display === true) {
        for (let c = 0; c < s.array[i].connections.length; c++) {
          if (s.array[s.array[i].connections[c]].display === true) {
            s.line(s.array[i].loc.i.x + 50, s.array[i].loc.i.y + 50, s.array[s.array[i].connections[c]].loc.i.x + 50, s.array[s.array[i].connections[c]].loc.i.y + 50)
          }
        }
      }
    }
  }
}
let d
onBeforeMount(() => {
  d = new P5(sketch, 'p5Canvas')
})

onMounted(() => {
  window.addEventListener('mousemove', update)
  console.log(d)
  d.array = ar
})
const valuex = ref(0)
const valuey = ref(0)
function update (event) {
  valuex.value = width - event.pageX
  valuey.value = height - event.pageY
}

// const debug = []

// for (let i = 0; i < locations.length; i++) {
//   debug.push({ id: i, im: locations[i], la: reverseLocations[i] })
// }
// console.log(debug)
function releaseConnections (id) {
  for (let i = 0; i < ar[id].connections.length; i++) {
    ar[ar[id].connections[i]].display = true
    console.log(ar[ar[id].connections[i]].display)
  }
}
provide('release', releaseConnections)
provide('ar', ar)
</script>

<style lang="scss">
*{
  cursor: none;
}
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
    opacity: 0;
  }
}
</style>
