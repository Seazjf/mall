<template>
  <div>
    <NavBar >
      <div slot="left" class="titleimg" @click="backclick">
        <img src="~assets/img/common/back.svg"/>
      </div>
      <div slot="center" class="title">
        <div v-for="(item,index) in titles"
             class="titleitem"
             :class="{active: index === currentIndex}"
             @click="titleclick(index)"
        >
          {{item}}
        </div>
      </div>
    </NavBar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  export default {
    name: "DetailNavBar",
    data() {
      return {
       titles: ['商品','参数','评论','推荐'],
        currentIndex: 0
      }
    },
    methods: {
      titleclick(index) {
        this.currentIndex = index
        this.$emit('NavBary',index)
      },
      backclick() {
        this.$router.back()
      }
    },
    components: {
      NavBar
    }

  }
</script>

<style scoped>
  .title {
   display: flex;
    font-size: 13px;
 }
  .titleitem {
    flex: 1;
  }
  .active {
    color: var(--color-high-text);
  }
  .titleimg {
    margin-top: 8px;
  }
</style>
