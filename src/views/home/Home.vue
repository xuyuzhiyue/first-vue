<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <scroll class="content">
      <home-swiper :banners="banners"/>
      <recommend-view :recommends="recommends"/>
      <feature-view></feature-view>
      <tab-control class="tab-control" :titles="['青春','往事','拥有']" @tabClick="tabClick"></tab-control>
      <good-list :goods="showGoods"></good-list>
    </scroll>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar';
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/Feature'
  import  TabControl from 'components/content/tabControl/TabControl'
  import GoodList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'

  import {getHomeMultidata,getHomeGoods} from "network/home";


  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodList,
      Scroll
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods:{
          // 'pop':{page:1,list:[]},
          // 'news':{page:1,list:[]},
          // 'sell':{page:1,list:[]},
         page:1,list:[]
        }
      }
    },
    created() {
      // 1.请求多个数据
      this.getHomeMultidata()
      this.getHomeGoods()
      //   this.getHomeGoods('pop')
      // this.getHomeGoods('new')
      // this.getHomeGoods('sell')
    },
    computed:{
      showGoods(){
        return this.goods.list
      }
    },
    methods:{
      tabClick(index){
        console.log(index);
      },
      getHomeMultidata(){
        getHomeMultidata().then(res => {
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(){
        getHomeGoods(1).then(res=>{
          console.log(res);
          // this.goods.list.push(...res.result)
          this.goods.list =res.result
          console.log(this.goods.list);
        })
      }
    }
  }
</script>

<style scoped>
   #home{
     padding-top: 44px;
     height: 100vh;
     position: relative;
   }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    z-index: 9;
  }
  .tab-control{
    position: sticky;
    top: 44px;
  }
  .content{
    /*height: 460px;*/
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
  }
</style>
