<template>
  <div>
    <Scroll class="content">
      <CartListItem v-for="(item,index) in cartList" :key="index" :Product="item"></CartListItem>
    </Scroll>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'
  import Scroll from 'components/common/scroll/Scroll'

  import CartListItem from './CartListItem'

  export default {
    name: "CartList",
    computed: {
      ...mapGetters(['cartList'])
    },
    components: {
      Scroll,
      CartListItem
    }

  }
</script>

<style scoped>
  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 93px;
    left: 0;
    right: 0;
  }
</style>
