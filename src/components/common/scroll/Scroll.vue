<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        scroll: null,

      }
    },
    mounted() {
      //创建BSscroll对象
      this.scroll = new BScroll(this.$refs.wrapper,{
        click: true,
        probeType: this.probeType,
        pullUpLoad:this.pullUpLoad
      })
      //监听滚动
      this.scroll.on('scroll',(position) => {
        this.$emit('probeclick',position)
      })
      //监听上拉加载
      this.scroll.on('pullingUp',() => {
        this.$emit('pullingUp')
        console.log('上拉加载');

      })
    },
    methods:{
      scrollTo(x,y,time=300) {
        this.scroll.scrollTo(x,y,time)
      },
      //刷新再一次加载
      finishPullUp() {
        this.scroll.finishPullUp()
      },
      //图片高度刷新
      refresh() {
        this.scroll.refresh()


      },
      getScrollY() {
        return this.scroll ? this.scroll.y : 0
      }
    }

  }
</script>

<style scoped>

</style>
