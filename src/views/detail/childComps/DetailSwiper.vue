<template>
  <Swiper class="swiperimages">
    <SwiperItem v-for="item in topImages">
      <img :src="item">
    </SwiperItem>
  </Swiper>
</template>

<script>
  import {Swiper,SwiperItem} from 'components/common/Swiper'

  export default {
    name: "DetailESwiper",
    props: {
      topImages: {
        type: Array,
        default() {
          return []
        }
      }
    },
   components: {
     Swiper,
     SwiperItem
   }
  }
</script>

<style scoped>
 .swiperimages {
   height: 300px;
   overflow: hidden;
 }
</style>
