<template>
  <div ref="wrapper" class="wrapper">
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
        default: 1
      },
      data: {
		    type: Array,
        default: () => {
          return []
        }
      },
      pullUpLoad: {
		    type: Boolean,
        default: false
      }
    },
    data() {
		  return {
		    scroll: {}
      }
    },
    mounted() {
		this.scroll=new BScroll(this.$refs.wrapper,{
       click:true,
       probeType:this.probeType,
       pullUpLoad:this.pullUpLoad

    })

    this.scroll.on('scroll',(position)=>{
        this.$emit('scroll',position)
    }),
    this.scroll.on('pullingUp', () => {
         
          this.$emit('pullingUp')
        })
    },
    methods: {
		  __initScroll() {
		    // 1.初始化BScroll对象
		    if (!this.$refs.wrapper) return
        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: true,
          pullUpLoad: this.pullUpLoad
        })

        // 2.将监听事件回调
        this.scroll.on('scroll', pos => {
          this.$emit('scroll', pos)
        })

        // 3.监听上拉到底部
        this.scroll.on('pullingUp', () => {
          console.log('上拉加载');
          this.$emit('pullingUp')
        })
      },
      refresh() {
        this.scroll && this.scroll.refresh && this.scroll.refresh()
      },
      finishPullUp() {
		     this.scroll.finishPullUp()
      },
      scrollTo(x, y, time) {
		    this.scroll.scrollTo(x,y,time)
      },
      getscrolly(){
          return this.scroll?this.scroll.y:0
      },
    },
    watch: {
		  data() {
        setTimeout(this.refresh, 20)
      }
    }
	}
</script>

<style scoped>

</style>
