<template>
  <!--ref绑定在组件中，通过this.$refw.refname获取到的是一个组件对象-->
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  //import someComponent from './someComponent'
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
        scroll: null
      }
    },
    mounted() {
      //创建BScroll对象
      this.scroll = new BScroll(this.$refs.wrapper,{
        click: true,//不写这个就点不了
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      });

      //2.监听滚动的位置
      this.scroll.on('scroll', (position) => {
        this.$emit('scroll',position)
      })

      //3.监听上拉事件
      this.scroll.on('pullingUp',() =>{
        this.$emit('pullingUp')
      })


      //回到顶部
      this.scroll.scrollTo(0, 0)
    },
    methods: {
      scrollTo(x, y, time=300) {
        this.scroll.scrollTo(x, y ,time)
      },
      finishPullUp() {
        this.scroll.finishPullUp()
      }
    }
  }
</script>

<style scoped>

</style>