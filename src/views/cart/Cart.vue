<template>
  <div>
    <nav-bar class="nav-bar">
      <div slot="center">购物车({{cartLength}})</div>
    </nav-bar>
    <!---->
    <cart-list></cart-list>
    <CartButton></CartButton>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'


  import CartList from './childComps/CartList'
  import CartButton from './childComps/CartButton'

  import {mapGetters} from 'vuex'
  export default {
    name: "Cart",
    components: {
      NavBar,
      CartList,
      CartButton

    },
    computed: {
      ...mapGetters(['cartLength','cartList'])
    }

  }
</script>

<style scoped>
 .nav-bar {
   background-color: var(--color-tint);
   color: #fff;
   font-weight: 700;
 }

</style>
