<template>
  <div>
    <detail-banner 
      :sightName="sightName"
      :bannerImg="bannerImg"
      :gallaryImgs="gallaryImgs"
      ></detail-banner>
    <detail-header></detail-header>
    <div style="height:2000px"></div>
  </div>
</template>

<script>
  import axios from 'axios'
  import DetailBanner from './components/Banner'
  import DetailHeader from './components/Header'
  export default {
    name: 'Detail',
    data() {
      return {
        sightName: '',
        bannerImg: '',
        gallaryImgs: [],
      }
    },
    components: {
      DetailBanner,
      DetailHeader
    },
    methods: {
      getDetailInfo() {
        axios.get('/api/detail.json', {
          params: {
            id: this.$route.params.id
          }
        }).then(this.handleDetailInfo)
      },
      handleDetailInfo(res) {
        res = res.data
        if (res) {
          const data = res.data
          this.sightName = data.sightName
          this.bannerImg = data.bannerImg
          this.gallaryImgs = data.gallaryImgs
        }
      }
    },
    mounted() {
      this.getDetailInfo()
    }
  }
</script>

<style lang="stylus" scoped>
  .content
    height: 50rem
</style>
