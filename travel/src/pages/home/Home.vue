<template>
  <div>
      <home-header></home-header>
      <home-swiper :list="swiperArr"></home-swiper>
      <home-icons :list="iconArr"></home-icons>
      <home-recommend :list="recommendArr"></home-recommend>
      <home-weekend :list="weekendArr"></home-weekend>
  </div>
</template>

<script>
  import HomeHeader from './components/Header'
  import HomeSwiper from './components/Swiper'
  import HomeIcons from './components/Icons'
  import HomeRecommend from './components/Recommend'
  import HomeWeekend from './components/Weekend'
  import axios from 'axios'
  import { mapState } from 'vuex'
  export default {
    name: 'Home',
    data() {
      return {
        swiperArr: [],
        iconArr: [],
        recommendArr: [],
        weekendArr: [],
        lastCity: ''
      }
    },
    components: {
      HomeHeader,
      HomeSwiper,
      HomeIcons,
      HomeRecommend,
      HomeWeekend,
    },
    methods: {
      getHomeInfo() {
        axios.get('/api/index.json').then(this.getHomeInfoSucc)
      },
      getHomeInfoSucc(res) {
        res = res.data
        if (res.data && res.ret) {
          const data = res.data
          this.swiperArr = data.swiperList
          this.iconArr = data.iconList
          this.recommendArr = data.recommendList
          this.weekendArr = data.weekendList
        }
      }
    },
    computed: {
      ...mapState(['city'])
    },
    mounted() {
      this.lastCity = this.city
      this.getHomeInfo()
    },
    activated() { // 加入keepalive后自动添加的一个生命周期钩子
      if(this.lastCity !== this.city){
        this.lastCity = this.city
        this.getHomeInfo()
      }
    }
  }
</script>

<style>
  
</style>
