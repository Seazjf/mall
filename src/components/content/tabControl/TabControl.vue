<template>
  <div class="tab-control">
    <div v-for="(item,index) in title" class="tab-control-item"
         :class="{active:index === currentIndex }" @click="itemclick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props: {
      title: {
        type:Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    methods: {
      itemclick(index) {
        this.currentIndex = index
        this.$emit('Tabclick',index)
        console.log('tbaclick');
      }
    }
  }
</script>

<style scoped>
 .tab-control{
   display: flex;
   width: 100%;
   text-align: center;
   font-size: 15px;
   background-color: #fff;
 }
 .tab-control-item{
   flex: 1;
   height: 40px;
   line-height: 40px;

 }
 .tab-control-item span {
   padding: 5px;
 }
 .active{
   color: var(--color-high-text);
 }
 .active span {
   border-bottom: 3px solid var(--color-tint);
 }
</style>
