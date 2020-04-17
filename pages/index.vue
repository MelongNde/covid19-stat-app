<template>
  <div>
    <v-carousel
      :show-arrows="true"
      cycle
      height="100%"
      width="100%"
      hide-delimiter-background
      show-arrows-on-hover>
      <v-carousel-item
        v-for="(item,i) in items"
        :key="i"
        :src="item.src"
      ></v-carousel-item>
    </v-carousel>
  </div>
</template>

<script>
import img1 from '../assets/img1.jpg'
import img2 from '../assets/img2.jpg'
import img3 from '../assets/img3.png'
import img4 from '../assets/img.png'
  export default {
    data () {
      return {
        items: [
          {
            src: img1,
          },
          {
            src: img2,
          },
          {
            src: img3,
          },
          {
            src: img4,
          },
        ],
      }
    },
  }
</script>
