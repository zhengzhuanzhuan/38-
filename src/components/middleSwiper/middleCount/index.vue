<template>
 <div class="main">
   <h3>{{ slide.title[0].name }}</h3>
      <div class="pic">
        <!-- <div class="float" v-for="(item,index) in slide.title[0].value" :key="index">
          <span v-if="item==='.'||item===','">
              {{item}}
          </span> -->
            <flask-count :propCount = 'slide.title[0].value'></flask-count>
          <!-- <li v-else>{{item}}</li> -->
           <!-- <swiper :options="swiperOption" ref="mySwiper">
             <swiper-slide  v-for="i in propTest" >{{ i }}</swiper-slide>
           </swiper> -->
        <!-- </div> -->
     </div>
     <div class="use">
       <div class="sales">
            <p>{{ slide.title[1].name}}</p>
            <span>{{slide.title[1].value}}</span>
       </div>
       <div class="orders">
          <p>{{ slide.title[2].name}}</p>
          <span>{{slide.title[2].value}}</span>
       </div>
        <div class="price">
          <p>{{ slide.title[3].name}} </p>
          <span>{{slide.title[3].value}}</span>
       </div>
     </div>
 </div>
</template>
<style lang="less" scoped>
h3 {
  font-family: PingFangSC;
  font-size: 28px;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-align: center;
  color: #fff;
  padding-bottom: 15px;
}
.pic {
  width: 100%;
  height: 135px;
  line-height: 135px;
  margin: 0 auto;
  text-align: center;
  display: flex;
  justify-content:center;
  align-items:center;
}
.use {
  height: 120px;
  padding-top: 30px;
  background-color: #2a3675;
  display: flex;
  justify-content: center;
  text-align: center;
  color: #fff;
p {
   font-size: 18px;
  letter-spacing: 1.3px;
  font-family: PingFangSC;
}
span {
    font-size: 30px;
    font-weight: bold;
    letter-spacing: 1.6px;
    font-family: DINAlternate;
}
 .orders {
   margin: 0 120px;
 }
}
</style>
<script>
import flaskCount from './flaskCount'
export default {
  props: [ 'slide' ],
  data () {
    return {
    }
  },
  components: {
    flaskCount
  },
  mounted () {
  }
}
</script>
