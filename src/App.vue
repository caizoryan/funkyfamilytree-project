<template>
<div id="p5Canvas"></div>
<NavBar></NavBar>
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
<div  v-if="unlock" class="congratulations">
  <h1>Congratulations!</h1>
  <p>You unlocked the whole family!</p>
  <p>Total Time: {{ time }}</p>
  <button class="close" @click="unlock = false" >x</button>
</div>
<div class="time">{{ time }}</div>
<div v-if="small" class="incSize">
  <h1>Please increase screen size and refresh</h1>
</div>
</template>

<script setup>
import P5 from 'p5'
import { ref, reactive, onBeforeMount, onMounted, provide } from 'vue'
import MembersComponent from './components/membersComponent.vue'
import NavBar from './components/navBar.vue'
const magnifyValue = ref(150)
const locations = []
const reverseLocations = []
const unlock = ref(false)
const width = window.innerWidth
const height = window.innerHeight
const boxes = 1000
const sqrt = (width * height) / (46 * 3)
const boxSize = Math.sqrt(sqrt)
const border = 100
const small = ref(false)
const assignedLocations = [{ x: width / 2 - boxSize, y: height / 2 }, { x: width / 2 + boxSize, y: height / 2 }, { x: width / 2, y: height / 2 + boxSize }, { x: width / 2, y: height / 2 - boxSize }]
const time = ref(0)
const firstclick = ref(true)
// eslint-disable-next-line
var interval
if (width < 800) {
  small.value = true
}

function findAlocation () {
  let value = { i: { x: 0, y: 0 }, l: { x: 0, y: 0 } }
  if (small.value === false) {
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
    value = { i: locations[r], l: reverseLocations[r] }
    locations.splice(r, 1)
    reverseLocations.splice(r, 1)
  }
  return value
}
function checkCollision () {
  const r = parseInt(Math.random() * locations.length)
  let collision = false
  for (let i = 0; i < assignedLocations.length; i++) {
    if (locations[r].x + boxSize >= assignedLocations[i].x &&
       locations[r].x <= assignedLocations[i].x + boxSize &&
       locations[r].y + boxSize >= assignedLocations[i].y &&
       locations[r].y <= assignedLocations[i].y + boxSize) {
      collision = true
    }
    if (reverseLocations[r].x + boxSize >= assignedLocations[i].x &&
       reverseLocations[r].x <= assignedLocations[i].x + boxSize &&
       reverseLocations[r].y + boxSize >= assignedLocations[i].y &&
       reverseLocations[r].y <= assignedLocations[i].y + boxSize) {
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
      reverseLocations.push({ x: parseInt(i) - boxSize, y: parseInt(y) - boxSize })
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
  [{ id: 0, label: 'Armaan', display: true, connections: [2, 3], image1: require('@/assets/1/1.png'), image2: require('@/assets/1/2.png'), loc: findAlocation(), hover: false },
    { id: 1, label: 'Aaryan', display: true, connections: [2, 3], image1: require('@/assets/2/1.png'), image2: require('@/assets/2/2.png'), loc: findAlocation(), hover: false },
    { id: 2, label: 'Papa', display: false, connections: [0, 1, 8, 9], image1: require('@/assets/3/1.png'), image2: require('@/assets/3/2.png'), loc: findAlocation(), hover: false },
    { id: 3, label: 'Mumma', display: false, connections: [0, 1, 5, 6], image1: require('@/assets/4/1.png'), image2: require('@/assets/4/2.png'), loc: findAlocation(), hover: false },
    { id: 4, label: 'Appy Mamu', display: false, connections: [5, 6, 17, 18], image1: require('@/assets/5/1.png'), image2: require('@/assets/5/2.png'), loc: findAlocation(), hover: false },
    { id: 5, label: 'Baba', display: false, connections: [4, 3, 7], image1: require('@/assets/6/1.png'), image2: require('@/assets/6/2.png'), loc: findAlocation(), hover: false },
    { id: 6, label: 'Nani', display: false, connections: [4, 3, 7], image1: require('@/assets/7/1.png'), image2: require('@/assets/7/2.png'), loc: findAlocation(), hover: false },
    { id: 7, label: 'Angai Masi', display: false, connections: [5, 6, 20, 21], image1: require('@/assets/8/1.png'), image2: require('@/assets/8/2.png'), loc: findAlocation(), hover: false },
    { id: 8, label: 'Dadu', display: false, connections: [2, 10, 14], image1: require('@/assets/9/1.png'), image2: require('@/assets/9/2.png'), loc: findAlocation(), hover: false },
    { id: 9, label: 'Dadi', display: false, connections: [2, 10, 14], image1: require('@/assets/10/1.png'), image2: require('@/assets/10/2.png'), loc: findAlocation(), hover: false },
    { id: 10, label: 'Abhishek Chachu', display: false, connections: [11, 12, 8, 9], image1: require('@/assets/11/1.png'), image2: require('@/assets/11/2.png'), loc: findAlocation(), hover: false },
    { id: 11, label: 'Ahaan', display: false, connections: [10, 13], image1: require('@/assets/12/1.png'), image2: require('@/assets/12/2.png'), loc: findAlocation(), hover: false },
    { id: 12, label: 'Adweta', display: false, connections: [10, 13], image1: require('@/assets/13/1.png'), image2: require('@/assets/13/2.png'), loc: findAlocation(), hover: false },
    { id: 13, label: 'Lekhni Chachi', display: false, connections: [11, 12], image1: require('@/assets/14/1.png'), image2: require('@/assets/14/2.png'), loc: findAlocation(), hover: false },
    { id: 14, label: 'Ankit Chachu', display: false, connections: [16, 8, 9], image1: require('@/assets/15/1.png'), image2: require('@/assets/15/2.png'), loc: findAlocation(), hover: false },
    { id: 15, label: 'Meenakshi Chachi', display: false, connections: [16], image1: require('@/assets/16/1.png'), image2: require('@/assets/16/2.png'), loc: findAlocation(), hover: false },
    { id: 16, label: 'Ananya', display: false, connections: [14, 15], image1: require('@/assets/17/1.png'), image2: require('@/assets/17/2.png'), loc: findAlocation(), hover: false },
    { id: 17, label: 'Shanaya', display: false, connections: [4, 19], image1: require('@/assets/18/1.png'), image2: require('@/assets/18/2.png'), loc: findAlocation(), hover: false },
    { id: 18, label: 'Veer', display: false, connections: [4, 19], image1: require('@/assets/19/1.png'), image2: require('@/assets/19/2.png'), loc: findAlocation(), hover: false },
    { id: 19, label: 'Neha Mami', display: false, connections: [17, 18], image1: require('@/assets/20/1.png'), image2: require('@/assets/20/2.png'), loc: findAlocation(), hover: false },
    { id: 20, label: 'Kian', display: false, connections: [22, 7], image1: require('@/assets/21/1.png'), image2: require('@/assets/21/2.png'), loc: findAlocation(), hover: false },
    { id: 21, label: 'Eva', display: false, connections: [22, 7], image1: require('@/assets/22/1.png'), image2: require('@/assets/22/2.png'), loc: findAlocation(), hover: false },
    { id: 22, label: 'Prasann Mamu', display: false, connections: [20, 21], image1: require('@/assets/23/1.png'), image2: require('@/assets/23/2.png'), loc: findAlocation(), hover: false }
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
        s.strokeWeight(1)
        s.stroke(100, 100, 100, 2)
        s.line(i, 0, i, h)
        s.line(0, y, w, y)
      }
    }
    s.strokeWeight(1)
    s.stroke(0)
    for (let i = 0; i < s.array.length; i++) {
      if (s.array[i].hover === true) {
        for (let c = 0; c < s.array[i].connections.length; c++) {
          if (s.array[s.array[i].connections[c]].display === true) {
            s.line(s.array[i].loc.i.x + s.boxSize / 2, s.array[i].loc.i.y + s.boxSize / 2, s.array[s.array[i].connections[c]].loc.i.x + s.boxSize / 2, s.array[s.array[i].connections[c]].loc.i.y + s.boxSize / 2)
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
  d.boxSize = boxSize
})
const valuex = ref(0)
const valuey = ref(0)
function update (event) {
  valuex.value = width - event.pageX
  valuey.value = height - event.pageY
}

function releaseConnections (id) {
  if (firstclick.value === true) {
    firstclick.value = false
    // eslint-disable-next-line
    interval = setInterval(function () {
      time.value++
    }, 1000)
  }
  for (let i = 0; i < ar[id].connections.length; i++) {
    ar[ar[id].connections[i]].display = true
  }
  let d = true
  for (let i = 0; i < ar.length; i++) {
    if (ar[i].display === false) {
      d = false
    }
  }
  if (d === true) {
    clearInterval(interval)
  }
  unlock.value = d
}
function magnify (val) {
  magnifyValue.value = val
}

provide('release', releaseConnections)
provide('ar', ar)
provide('boxSize', boxSize)
provide('magnifyValue', magnifyValue)
provide('magnifyFunction', magnify)
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
}
.congratulations{
  position: absolute;
  left: 38vw;
  top: 40vh;
  width: 25vw;
  height: 30vh;
  display: flex;
  border: 2px solid black;
  flex-direction: column;
  background-color: rgb(200, 200, 200);
  justify-content: center;
  align-items: center;
  .close{
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: rgba(0, 0, 0, 0);
    border: 1.5px solid black;
    }
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

.incSize{
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  margin: auto;
  justify-content: center;
  align-items: center;
  display: flex;
  background-color: rgb(200, 200, 200);
}

.time{
  position: absolute;
  top: 20px;
  left: 20px;
  font-size: 30px;
}
</style>
