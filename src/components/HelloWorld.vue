<template>
<div class="image-box" v-bind:style="{
  top: props.top + 'px',
  left: props.left + 'px'
}"></div>

<div class="opposite" v-bind:style="{
  bottom: props.bottom + 'px',
  right: props.right + 'px'
}"></div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps(['top', 'left', 'bottom', 'right'])

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.image-box{
  width: 200px;
  height: 200px;
  background-color: red;
  position: absolute;
}

.opposite{
  width: 200px;
  height: 200px;
  position: absolute;
  background-color: blue;

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
