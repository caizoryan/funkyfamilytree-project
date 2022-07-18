<template>

<div class="debugOppReal" :style="{
    top: props.y2 + 'px',
    left: props.x2 + 'px'
}"></div>
<div class="debugOpp" :style="{
    top: props.y2 - 100 + 'px',
    left: props.x2 - 100 + 'px'
}"></div>
<div class="debug" :style="{
    top: props.y1 + 'px',
    left: props.x1 + 'px'
}"></div>
</template>

<script setup>
import { defineProps } from 'vue'
const props = defineProps(['x1', 'y1', 'x2', 'y2'])
</script>

<style scoped lang="scss">
.debug{
    position: absolute;
    width: 100px;
    height: 100px;
    border: 2px white solid;
}

.debugOpp{
    position: absolute;
    width: 100px;
    height: 100px;
    // border: 2px rgb(103, 103, 103) solid;
}

.debugOppReal{
    position: absolute;
    width: 100px;
    height: 100px;
    border: 2px rgb(172, 45, 62) solid;
}
</style>
