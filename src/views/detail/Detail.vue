<template>
  <div id="details">
    <detail-nav-bar class="detailsnavbar"  @NavBary="navbary"></detail-nav-bar>
    <scroll class="content" ref="scroll" @probeclick="probescroll"
            :probeType="3">
      <detail-swiper :top-images="topImage"></detail-swiper>
      <DetailBasInfo :goods="goods"></DetailBasInfo>
      <DetailShopInfo :shop="shop"></DetailShopInfo>
      <DetailGoodsInfo :detailInfo="detailInfo" @imgLoad="imgLoadInfo"></DetailGoodsInfo>
      <DetailParamInfo ref="param" :paramInfo="paramInfo"></DetailParamInfo>
      <DetailCommentInfo ref="comment" :commentInfo="commentInfo"></DetailCommentInfo>
      <GoodsList ref="recommend" :goods="recommends"></GoodsList>
    </scroll>
    <BackTop @click.native="backClick" v-show="probes"></BackTop>
    <DetailBottomRar @addCart="addCartclick"></DetailBottomRar>
  </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBasInfo from './childComps/DetailBasInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import DetailGoodsInfo from './childComps/DetailGoodsInfo'
import DetailParamInfo from './childComps/DetailParamInfo'
import DetailCommentInfo from './childComps/DetailCommentInfo'
import DetailBottomRar from './childComps/DetailBottomRar'

import Scroll from 'components/common/scroll/Scroll'
import GoodsList from 'components/content/goods/GoodsList'
import BackTop from 'components/content/backtop/BackTop'


import {getDetail,Goods,Shop,GoodsParam,getRecommend} from 'network/detail'

import {backTopMixin} from 'common/mixin'
  export default {
    name: "Detall",
    data() {
      return {
        iid:null,
        topImage: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: [],
        themeTops:[],
        themeTopsY:null,
        probes:false

      }
    },
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBasInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      GoodsList,
      DetailBottomRar,
      BackTop


    },
    created() {
      this.iid = this.$route.params.iid

      getDetail(this.iid).then(res => {
        // console.log(res);
        const data = res.result
        this.topImage = data.itemInfo.topImages
        this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
        this.shop = new Shop(data.shopInfo)
        this.detailInfo = data.detailInfo
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule);
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0]
        }

    })
      getRecommend().then(res => {
        console.log(res);
        this.recommends = res.data.list
      })

     this.themeTopsY =  this.debouce(() => {
       this.themeTops = []
       this.themeTops.push(0)
       this.themeTops.push(-this.$refs.param.$el.offsetTop)
       this.themeTops.push(-this.$refs.comment.$el.offsetTop)
       this.themeTops.push(-this.$refs.recommend.$el.offsetTop)
       console.log(this.themeTops);
      })
    },
    mounted() {


    },
    mixin:[],
    methods: {
      imgLoadInfo() {
        this.$refs.scroll.refresh()
        this.themeTopsY()

      },
      navbary(index) {
        this.$refs.scroll.scrollTo(0,this.themeTops[index],100)
      },
      debouce(func,delay) {

        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this,args)
          },delay)
        }
      },
      probescroll(position) {
        this.probes= (-position.y) > 1000

      },
      backClick() {
        this.$refs.scroll.scrollTo(0,0)
      },
      addCartclick() {
        const product = {}
        product.image = this.topImage[0];
        product.titles = this.goods.title;
        product.desc = this.goods.desc;
        product.price = this.goods.realPrice;
        product.iid = this.iid;

        // this.$store.commit('addCart',product)
        this.$store.dispatch('addCart',product).then(res => {
          this.$toast.show(res,2000)
          console.log(res);
        })
      }
    }
  }
</script>

<style scoped>
 #details {
   position: relative;
   z-index: 10;
   background-color: #fff;
   height: 100vh;
 }
  .content {
    position: absolute;
    top: 44px;
    bottom: 60px;

  }
  .detailsnavbar {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
</style>
