<template>
  <nav-bar class="detail-nav">
    <img slot="left" class="back" @click="backClick" src="~assets/img/common/back.svg">
    <div class="title" slot="center">
      <div class="title-item" v-for="(item,index) in titles" :class="{active:index===currentIndex}" @click="titleClick(index)">
          {{item}}
      </div>
    </div>
  </nav-bar>
</template>

<script>
  import NavBar from 'common/navbar/NavBar'

	export default {
		name: "DetailNavBar",
    components: {
		  NavBar
    },
    data(){
        return {
          titles:['商品', '参数', '评论', '推荐'],
          currentIndex:0
        }
    },
    methods: {
      itemClick: function (index) {
        this.$emit('itemClick', index)
      },
      backClick() {
        this.$router.back()
      },
      titleClick(index){
          this.currentIndex=index
      }
    }
	}
</script>

<style scoped>
  .detail-nav {
    background-color: #fff;
    font-weight: normal;
  }

  .title {
    display: flex;
    padding: 0 20px;
    font-size: 14px;
  }

  .title-item {
    flex: 1;
  }

  .title-item.active {
    color: var(--color-high-text);
  }

  .back {
    margin-top: 12px;
  }
</style>
