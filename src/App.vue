<script setup>
import { ref, watch } from 'vue'
import Slider from './components/Slider.vue'

const images = ref([
  '/1.png',
  '/2.png',
  '/3.png',
  '/4.png',
])
const c = ref(0)
const displayImage = (current) => {
    c.value = current
    console.log(current,   'FGGE', c)
}

watch(c, () => {
  
})
</script>

<template>
  <div class="container">
    <Slider :images="images" @current="displayImage($event)" />

    <div>
     <img :src="[images[c.value] ? images[c.value] : '/1.png' ]" alt="">
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  display: flex;
  gap: 20px;
}
</style>
