<template>
<div>
   <div class= 'pic' v-if="shopSale === 0">
       <img src="../../public/img/notData.png" alt="">
      <span>暂无数据</span>
  </div>
  <div v-else>
    <swiper :options="swiperOption" ref="mySwiper">
      <swiper-slide  v-for='(item,index) in swiperData' :key="index">
          <div class="detail">
              <div class="logo">
                  <a href="javascript:;"><img :src="item.store_url" alt=""></a>
                  <p>{{ item.store_name }}</p>
              </div>
              <div class="right-show">
                  <div class="sales">
                      <p>销售额(元)</p>
                      <span>{{ item.sell_amount }}</span>
                  </div>
                  <div class="bottom-show">
                      <div class="count">
                          <p>销售件数</p>
                          <span>{{ item.sell_count }}</span>
                      </div>
                      <div class="kind">
                          <p>热销品类</p>
                          <span>{{ item.category_name }}</span>
                      </div>
                  </div>
              </div>
          </div>
      </swiper-slide>
    </swiper>
  </div>
</div>
</template>
<style lang="less" scoped>
.pic {
  margin-top: 30px;
  text-align: center;
  span {
    display: block;
    margin-top: 20px;
    text-align: center;
    opacity: 0.8;
    color: #fff;
    font-family: PingFangSC;
    font-size: 16px;
    letter-spacing: 0.8px;
  }
  img {
    width: 220px;
    vertical-align: top;
  }
}
.detail {
  display: flex;
  .logo {
    margin: 30px 70px 0px 30px;
    width: 25%;
    height: 70%;
    text-align: center;
    a {
      display: inline-block;
      width: 80%;
      height: 80%;
      background-color: #ffffff;
      border-style: solid;
      border-width: 2px;
      border-image-source: linear-gradient(to right, #ff007c, #fa9696);
      border-image-slice: 1;
      img {
        vertical-align: middle;
        width: 100%;
        height: 100%;
      }
    }
    p {
      color: #ffffff;
      margin: 15px 0px;
      font-family: PingFangSC;
      font-size: 18px;
      font-weight: 600;
    }
  }
  .right-show {
    .sales {
      margin: 40px 0 25px;
      span {
        font-family: DINAlternate;
        font-size: 50px;
        color: #ffffff;
      }
      p {
        font-size: 18px;
      }
    }
    .bottom-show {
      display: flex;
      .count {
        margin-right: 67px;
        span {
          font-family: DINAlternate;
          font-size: 36px;
          color: #ffffff;
        }
      }
      .kind {
        text-align: center;
        float: left;
        p {
          padding-bottom: 7px;
        }
        span {
          font-family: PingFangSC;
          font-size: 24px;
          text-align: left;
          color: #ffffff;
        }
      }
    }
    p {
      opacity: 0.5;
      font-family: PingFangSC;
      font-size: 14px;
      letter-spacing: 0.8px;
      margin-bottom: 5px;
      color: #fff;
    }
  }
}
.swiper-container {
  width: 100%;
  height: 100%;
}
</style>
<script>
import nobody from '../../public/img/nobody.jpg'
import brain from '../../public/img/brain.png'
import magicBox from '../../public/img/奇点魔盒.jpg'
import magicCabinet from '../../public/img/奇点魔柜.jpg'
export default {
  name: 'hello-world',
  props: ['swiperData', 'shopSale'],
  data () {
    return {
      swiperOption: {
        autoplay: {
          delay: 10000,
          stopOnLastSlide: false,
          disableOnInteraction: false
        },
        direction: 'horizontal',
        slidesPerView: 1,
        loop: true
      },
      img: [magicCabinet, nobody, magicBox, brain]
    }
  },
  methods: {},
  watch: {
    swiperData: function () {
      // console.log(this.swiperData, 'watch')
    }
  },
  mounted () {}
}
</script>
