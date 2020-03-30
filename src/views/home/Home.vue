<template>
  <div id="home">
    <nav-bar class="nav-bar"><div slot="center">购物街</div></nav-bar>
     <tab-control :titles="['流行','新款','精选']" @tabClick='tabClick' ref="tabControl" class="tab-control" v-show='isTabFixed'/>
    <scroll class="content"  ref="scroll" :probe-type='3' @scroll="contentScroll" :pull-up-load="true" 
     @pullingUp="loadMore">
          <home-swiper :banners="banners" @swiperImageLoad='swiperImageLoad'></home-swiper>
        <recommend-view :recommends='recommends'/>
        <feature-view/>
       <tab-control :titles="['流行','新款','精选']" @tabClick='tabClick' ref="tabControl"/>
      <good-list :goods="showGoodsList"/>
    </scroll>
   <back-top @click.native="backTop" v-show="isshow"/>
  </div>
     
  
 
</template>

<script>
  import NavBar from 'common/navbar/NavBar'
  import HomeSwiper from './childComps/HomeSwiper' 
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  import TabControl from 'content/tabControl/TabControl' 
  import GoodList from 'components/content/goods/GoodsList' 
  import Scroll from 'common/scroll/Scroll'
  import BackTop from 'content/backTop/BackTop'  
  import {getHomeMultidata,getHomeGoods} from "network/home"

  export default {
		name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodList,
      Scroll,
      BackTop

   
    },
    data() {
		  return {
		    banners: [],
        recommends: [],
        goods:{
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        }, 
        currenttype:'pop',
        isshow:false,
        tabOffsetTop: 0, 
        isTabFixed: false,
        saveY:0

        
    
      }
    },
    created(){
     
      // 1.请求多个数据
      this.getHomeMultidata()
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell') 
   
     
     
    },
    mounted(){
     
      

    },

    methods:{
      tabClick(index){
       switch(index){
         case 0:
           this.currenttype='pop'
           break
         case 1:
           this.currenttype='new'
           break
         case 2:
           this.currenttype='sell'
       }

      },

      // 网路请求相关
      getHomeMultidata(){
         getHomeMultidata().then(res=>{
       
        this.banners=res.data.banner.list;
        this.recommends=res.data.recommend.list;
      }) 
      },
      getHomeGoods(type){
        const page=this.goods[type].page + 1
          getHomeGoods(type,page).then(res=>{
            this.goods[type].list.push(...res.data.list) //解构赋值
            this.goods[type].page+=1

            this.$refs.scroll.finishPullUp() 
      })
      },
      backTop() {
        this.$refs.scroll.scrollTo(0,0)
      },
      contentScroll(position){
       this.isshow=-position.y>1000

       this.isTabFixed=(-position.y)>this.tabOffsetTop
        
    },
      loadMore() {
        // this.getHomeProducts(this.currentType)
        this.getHomeGoods(this.currenttype)
        this.$refs.scroll.scroll.refresh()
      },
      swiperImageLoad(){
           this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
      },
    
    },
    computed: {
		  showGoodsList() {
		    return this.goods[this.currenttype].list
      }
    },
    activated(){
      this.$refs.scroll.scrollTo(0,this.saveY,0)
      this.$refs.scroll.refresh()
    },
    deactivated(){
      this.saveY=this.$refs.scroll.getscrolly()
    }  
   
	}
</script>

<style scoped>
  #home {
    /*position: relative;*/
    height: 100vh; 
  }

  .nav-bar {
    background-color: var(--color-tint);
    font-weight: 700;
    color: #fff;
  

  }

  .content {
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
.tab-control{
  position: relative; 
  z-index:9;
}

</style>
