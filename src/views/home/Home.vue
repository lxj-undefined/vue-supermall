<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物商城</div></nav-bar>

    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control class="tab-control" :titles="['流行','新款','精选']"
                   @tabClick="tabClick"></tab-control>
      <good-list :goods="showGoods"></good-list>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
  //import someComponent from './someComponent'

  //公共组件
  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodList from '../../components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'
  //import GoodsList from "../../components/content/goods/GoodsList";

  //子组件
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from  './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import { getHomeMultidata, getHomeGoods} from 'network/home'

  //import GoodsList from "../../components/content/goods/GoodsList.vue";

/*
  import Swiper from 'components/common/swiper/Swiper'
  import SwiperItem from 'components/common/swiper/SwiperItem'
*/


  export default {
    name: "Home",
    components: {
      //GoodsList,
      NavBar,
      TabControl,
      GoodList,
      Scroll,
      BackTop,
      HomeSwiper,
      RecommendView,
      FeatureView
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop' : {page: 0, list: []},
          'new' : {page: 0, list: []},
          'sell' : {page: 0, list: []},
        },
        currentType: 'pop',
        isShowBackTop: false
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    created() {
      //1.请求多个数据
    /*  getHomeMultidata().then(res => {
        //console.log(res);
        //console.log(res.data.banner.list);

        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;

      })*/
      this.getHomeMultidata();

      //2.请求商品数据
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
    methods: {
      /**
       *事件监听相关方法
       */
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell';
            break;
        }
        },
      backClick() {
        this.$refs.scroll.scrollTo(0,0,500)
      },
      contentScroll(position) {
        this.isShowBackTop = (-position.y)>1000
      },
      loadMore() {
        this.getHomeGoods(this.currentType)

        //调用refresh()重新计算可滚动区域
        this.$refs.scroll.scroll.refresh()
      },

      /**
       * 网络请求相关的方法
       */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          //console.log(res);
          //console.log(res.data.banner.list);

          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;

        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1;
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1

          this.$refs.scroll.finishPullUp()

        })
      }
    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
    height: 100vh;
    position: relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }
  .tab-control {
    position:sticky;
    top: 44px;
    z-index: 9;
  }

  .content {
    overflow: hidden;

    position: absolute;
    top:44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
 /* .content {
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top: 44px;
  }*/
</style>