<template>
  <div id="detail">
   <detail-nav-bar @titleClick="titleClick"/>
   <scroll class="content">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/> 
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info="detailInfo"/>
      <detail-param-info :param-info="paramInfo" ref="params"/>
      <detail-comment-info :comment-info="commentInfo" ref="comment"/>
      <goods-list :goods="recommends" ref="recommend"/>
   </scroll>
   <detail-bottom-bar/>
  
  </div>
</template>

<script>
  import Scroll from 'common/scroll/Scroll'
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper' 
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailShopInfo from './childComps/DetailShopInfo'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo'
  import DetailParamInfo from './childComps/DetailParamInfo'
  import DetailCommentInfo from './childComps/DetailCommentInfo' 
  import GoodsList from 'components/content/goods/GoodsList' 
  import DetailBottomBar from './childComps/DetailBottomBar'  
  import {getDetail,Goods,Shop,GoodsParam,getRecommend} from 'network/detail'

  export default {
    name: "Detail",
    components:{
      Scroll,
     DetailNavBar,
     DetailSwiper,
     DetailBaseInfo,
     DetailShopInfo,
     DetailGoodsInfo,
     DetailParamInfo,
     DetailCommentInfo,
     GoodsList,
     DetailBottomBar
    },
    data(){
      return {
        iid:null,
        topImages:[],
        goods:{},
        shop:{},
        detailInfo:{},
        paramInfo: {}, 
        commentInfo: {},
        recommends:[],
        themeTopYs:[],
       

      }
    },
  created(){
      this.iid=this.$route.params.iid

      getDetail(this.iid).then(res =>{
          // console.log(res)
         const data=res.result
         this.topImages=data.itemInfo.topImages
        // 获取商品信息
         this.goods=new Goods(data.itemInfo,data.columns, data.shopInfo.services)
         //保存店铺的信息
         this.shop=new Shop(data.shopInfo)
         //保存商品的详情数据
         this.detailInfo = data.detailInfo 
         //商品参数信息 
         this.paramInfo =new GoodsParam(data.itemParams.info, data.itemParams.rule)
         //商品评论信息
         if(data.rate.cRate !== 0){
            this.commentInfo=data.rate.list[0]
         }

      })
      getRecommend().then(res=>{
          this.recommends=res.data.list
       
      })
     
  },
   methods:{
     titleClick(index){
       console.log(index)
      //  this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
     },
     
   },  
   mounted(){
      // this.themeTopYs=[]
      // this.themeTopYs.push(0)
      // this.themeTopYs.push(this.$refs.params.$el.offsetTop)
      // this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
      // this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
      // console.log(this.themeTopYs)   

      // this.$next(()=>{
      //      this.themeTopYs=[]
      // this.themeTopYs.push(0)
      // this.themeTopYs.push(this.$refs.params.$el.offsetTop)
      // this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
      // this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
      // console.log(this.themeTopYs) 
      // })
   }
    
	}
</script>

<style scoped>
  #detail {
    height: 100vh;
    position: relative;
    z-index: 1;
    background-color: #fff;
  }

  .content {
    position: absolute;
    top: 44px;
    bottom: 60px;
  }

  .back-top {
    position: fixed;
    right: 10px;
    bottom: 65px;
  }
</style>
