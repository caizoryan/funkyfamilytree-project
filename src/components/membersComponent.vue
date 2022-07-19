<template>
<div @click="releaseConnections" v-if="props.display" class="image-box" :style="{
  left: props.imageX + 'px',
  top: props.imageY + 'px',
  height: boxSize + 'px',
  width: boxSize + 'px'
}" @mouseover="enter" @mouseleave="leave">

<img v-if="hover == false" :src=props.image1 :style="{
  width: 100 + '%',
  height: 100 + '%'}">
<img v-if="hover" :src=props.image2 :style="{
  width: 120 + '%',
  height: 120 + '%'}" >

</div>

<div @click="releaseConnections" v-if="props.display" class="label" v-bind:style="{
  left: props.labelX + 'px',
  top: props.labelY + 'px',
  height: boxSize + 'px',
  width: boxSize + 'px'
}" @mouseover="enter" @mouseleave="leave">
<h1>{{ props.label }}</h1>
</div>
</template>

<script setup>
import { defineProps, onMounted, ref, inject } from 'vue'
const hover = ref(false)
const props = defineProps(['imageX', 'imageY', 'labelX', 'labelY', 'image1', 'image2', 'label', 'id', 'display'])
const ar = inject('ar')
const release = inject('release')
const display = ref(false)
const boxSize = inject('boxSize')
let id
onMounted(() => {
  id = props.id
  display.value = ar[id].display
})
function releaseConnections () {
  release(id)
}
function enter () {
  hover.value = true
  ar[id].hover = true
}
function leave () {
  hover.value = false
  ar[id].hover = false
}
</script>

<style scoped lang="scss">

.image-box{
  background-color: rgba(177, 177, 177, 0);
  // border: 1px solid black;
  position: absolute;
  img{
    position: absolute;
    top: 0;
    left: 0;
  }
}

.label{
  position: absolute;
  background-color: rgb(200, 200, 200);
  border: 2px solid black;
  display: flex;
  // justify-content: center;
  // align-items: center;
  h1{
    padding-left: 10px;
    color: black;
    font-size: 15px;
  }
}
</style>
