<template>
<div class="bar">
    <div class="content">
        <h1>Magnify</h1>
        <button @click="magnifyOn(true)" :style="{
            color: onStyle.color,
            background: onStyle.background
        }">on</button>
        <button @click="magnifyOn(false)" :style="{
            color: offStyle.color,
            background: offStyle.background
        }">off</button>
    </div>
</div>
</template>

<script setup>
import { ref, inject } from 'vue'
const magnifyFunction = inject('magnifyFunction')
const magnify = ref(true)
const onStyle = ref({ color: 'rgb(255, 255, 255)', background: 'rgb(0, 0, 0)' })
const offStyle = ref({ color: 'rgb(0, 0, 0)', background: 'rgb(255, 255, 255, 0)' })
function magnifyOn (bool) {
  magnify.value = bool
  if (magnify.value === true) {
    onStyle.value = { color: 'rgb(255, 255, 255)', background: 'rgb(0, 0, 0)' }
    offStyle.value = { color: 'rgb(0, 0, 0)', background: 'rgb(255, 255, 255, 0)' }
    magnifyFunction(150)
  } else {
    offStyle.value = { color: 'rgb(255, 255, 255)', background: 'rgb(0, 0, 0)' }
    onStyle.value = { color: 'rgb(0, 0, 0)', background: 'rgb(255, 255, 255, 0)' }
    magnifyFunction(110)
  }
}
</script>

<style scoped lang="scss">
.bar{
    display: flex;
    z-index: 100;
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100px;
    justify-content: flex-end;
    align-items: center;
    .content{
        display: flex;
        align-items: center;
        margin-right: 100px;
        h1{
            font-size: 16px;
            padding-right: 10px;
        }
        button{
            font-size: 14px;
            background-color: rgb(255, 255, 255, 0);
            border: 1px solid black;
            border-radius: 20px;
            margin: 5px;
            width: 40px;
            height: 20px;
            &:hover{
                background-color: black;
                color: white;
            }
        }
     }
}
</style>
