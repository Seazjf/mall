<template>
  <div class="checkbutton" :class={chick:isChecked}>
    <img src="~assets/img/cart/tick.svg">
  </div>
</template>

<script>
  export default {
    name: "CheckButton",
    data() {
      return {
        all:null
      }
    },
    props: {
      isChecked: {
        type:Boolean,
        default: true

      }
    },


  }
</script>

<style scoped>
  .checkbutton {

    border-radius: 50%;
    border: 2px solid #666;
  }
  .chick {
    background-color: red;
    border-color: red;
  }
</style>
