<template>
   <swiper :options="swiperOption" ref="mySwiper">
      <swiper-slide  v-for="(slide,index) in swiperSlides" :key = 'index'>{{ slide }}</swiper-slide>
    </swiper>
</template>
<style lang="less" scoped>
.swiper-container {
      width: 63px;
      height: 88px;
      border-radius: 6.3px;
      background-image: linear-gradient(to left, #00f2ff, #0080ff);
      box-shadow: 0 20px 20px 0 #0066b0, 0 6px 0 0 #00cfff;
      font-size: 62px;
      color: white;
      line-height: 88px;
      float: left;
      margin-right: 20px;
}
</style>
<script>
export default {
  name: 'carrousel',
  data () {
    return {
      swiperOption: {
        pagination: '.swiper-pagination',
        direction: 'vertical',
        slidesPerView: 1
      },
      swiperSlides: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
  },
  computed: {
    swiper () {
      return this.$refs.mySwiper.swiper
    }
  },
  props: ['propData'],
  watch: {
    propData: {
      handler: function (val, oldval) {
        this.blockCount()
      },
      deep: true
    }
  },
  methods: {
    blockCount () {
      this.swiper.slideTo(this.propData, 1000, false)
    }
  },
  mounted () {
    this.blockCount()
  }
}
</script>
