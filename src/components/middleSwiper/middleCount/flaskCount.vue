<template>
  <div>
    <div v-for="(item, index) in numList" class="num">
      <div v-if="item===','||item==='.'" class="point">{{item}}</div>
      <swiper-inner v-else :propData="item"></swiper-inner>
    </div>
  </div>
</template>
<style lang="less" scoped>
.num{
  float: left;
  .point{
    color:white;
    font-size: 30px;
    margin-right: 20px;
  }
}

</style>
<script>
import swiperInner from './swiperInner'
export default {
  components: {
    swiperInner
  },
  props: ['propCount'],
  data () {
    return {
      timeOut: null,
      numList: [],
      marginLeft: 0
    }
  },
  watch: {
    propCount: {
      handler: function (val, oldval) {
        this.numList = this.propCount.toString().split('')
      },
      deep: true
    }
  },
  mounted () {
  }
}
</script>
