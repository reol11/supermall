<template>
  <div id="detail">
   <detail-nav-bar/>
   <scroll class="content">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/> 
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info="detailInfo"/>
      <detail-param-info :param-info="paramInfo"/>
   </scroll>
  
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
  import {getDetail,Goods,Shop,GoodsParam} from 'network/detail'

  export default {
    name: "Detail",
    components:{
      Scroll,
     DetailNavBar,
     DetailSwiper,
     DetailBaseInfo,
     DetailShopInfo,
     DetailGoodsInfo,
     DetailParamInfo
    },
    data(){
      return {
        iid:null,
        topImages:[],
        goods:{},
        shop:{},
        detailInfo:{},
        paramInfo: {}, 

      }
    },
  created(){
      this.iid=this.$route.params.iid

      getDetail(this.iid).then(res =>{
         const data=res.result
         this.topImages=data.itemInfo.topImages
        // 获取商品信息
         this.goods=new Goods(data.itemInfo,data.columns, data.shopInfo.services)
         //保存店铺的信息
         this.shop=new Shop(data.shopInfo)
         //保存商品的详情数据
         this.detailInfo = data.detailInfo 

         this.paramInfo =new GoodsParam(data.itemParams.info, data.itemParams.rule)
      })

  },
     
    
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
