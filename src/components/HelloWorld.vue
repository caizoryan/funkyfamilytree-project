<template>
<div class="image-box" v-bind:style="{
  left: props.imageX + 'px',
  top: props.imageY + 'px'
}" @mouseover="hover = true" @mouseleave="hover = false">

<img v-if="hover == false" :src=props.image1 alt="">
<img v-if="hover" :src=props.image2 alt="">

</div>

<div class="label" v-bind:style="{
  left: props.labelX - 100 + 'px',
  top: props.labelY - 100 + 'px'
}" @mouseover="hover = true" @mouseleave="hover = false">
<h1>{{ props.label }}</h1>
</div>
</template>

<script setup>
import { defineProps, onMounted, ref } from 'vue'
const hover = ref(false)
const props = defineProps(['imageX', 'imageY', 'labelX', 'labelY', 'image1', 'image2', 'label', 'id'])

onMounted(() => {
  const id = props.id
  console.log(id)
})

</script>

<style scoped lang="scss">

.image-box{
  width: 100px;
  height: 100px;
  background-color: rgba(177, 177, 177, 0);
  border: 1px solid black;
  position: absolute;
  img{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
}

.label{
  width: 100px;
  height: 100px;
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

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
