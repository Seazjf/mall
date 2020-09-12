<template >
  <div id="home">
    <NavBar class="nav-home"><div slot="center">购物街</div></NavBar>\
    <TabControl class="fixed"
                :title="['流行','新款','精选']"
                @Tabclick="Tabindex"
                ref="tabControl1"
                v-show="isTabFixed"

    ></TabControl>
    <Scroll class="content"
            ref="scroll"
            @probeclick="probescroll"
            :probeType="3"
            :pullUpLoad="true"
            @pullingUp="loadMore"

    >
      <HomeSwiper :banners="banners" @swiperImageLoad="swiperImageLoad"></HomeSwiper>
      <RecommendView :recommends="recommends"></RecommendView>
      <FeatureView></FeatureView>
      <TabControl
                  :title="['流行','新款','精选']"
                  @Tabclick="Tabindex"
                  ref="tabControl2"
      ></TabControl>
      <GoodsList :goods="showGoods"></GoodsList>
    </Scroll >
    <BackTop @click.native="backClick" v-show="probes"></BackTop>
  </div>
</template>

<script>

  import HomeSwiper from './chuildComps/HomeSwiper'
  import RecommendView from './chuildComps/RecommendView'
  import FeatureView from './chuildComps/FeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backtop/BackTop'

  import {getHomeMultidata,getHomeGoods} from 'network/home'
  import {debouce} from "common/utils";


  export default {
    name: "Home",
    components:{
      HomeSwiper,
      RecommendView,
      FeatureView,

      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data() {
      return {
         banners:[],
        recommends:[],
        goods: {
          'pop': {page: 0,list: []},
          'new': {page: 0,list: []},
          'sell': {page: 0,list: []}
        },
        currentType:'pop',
        probes:false,
        tabOffsetTop: 0,
        isTabFixed:false,
        saveY: 0
       }
     },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    activated() {
      this.$refs.scroll.scrollTo(0,this.saveY,3)
      this.$refs.scroll.refresh()
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
      console.log(this.saveY);
    },
    created() {
      this.getHomeMultidata();

      this.getHomeGoods('pop');

      this.getHomeGoods('new');

      this.getHomeGoods('sell');



    },
    mounted() {
      const refresh =this.debouce(this.$refs.scroll.refresh,100)
      this.$bus.$on('ItemImgLoad',() => {
        refresh()
      })

    },
    methods: {
      /**
       * 事件监听
       */
      debouce(func,delay) {

        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this,args)
          },delay)
        }
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
      },

      backClick() {
        this.$refs.scroll.scrollTo(0,0)
      },
      probescroll(position) {
        this.probes= (-position.y) > 1000
        this.isTabFixed= (-position.y) > this.tabOffsetTop
      },
      Tabindex(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.tabControl1.currentIndex = index
        this.$refs.tabControl2.currentIndex = index

      },
      swiperImageLoad() {
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
      },
      /**
       * 网络请求函数
       */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
          console.log(res);
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type,page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
      }
    }

  }
</script>

<style scoped>
  #home{

    height: 100vh;


  }
 .nav-home {
  background-color:var(--color-tint);
   color: #fff;
   font-weight: 700;


   position: fixed;
   left: 0;
   right: 0;
   top: 0;
   z-index: 9;
  }
  /*.tab-control{*/
    /*position: sticky;*/
    /*top: 44px;*/
    /*background-color: #fff;*/
    /*z-index: 9;*/
  /*}*/
  /*.content {*/
    /*height: calc(100% - 93px);*/
    /*overflow: hidden;*/
    /*margin-top: 44px;*/
  /*}*/
  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;

  }
  .fixed {
    position: relative;
    z-index: 9;
    top: 22px;

  }

</style>
