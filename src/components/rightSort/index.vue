<template>
<div>
  <div class= 'pic' v-if="sortData === 0">
       <img src="../../public/img/notData.png" alt="">
      <span>暂无数据</span>
    </div>
  <div class= 'screen_sort' v-else>
        <div class="logo-pic">
          <div class="logo" v-for="(item,index) in category_name" :key="index">
              <img :class="{ 'logo-active': logoStatus === index}" :src="hotSales[index]" alt="">
              <p :class="{ 'p-active': logoStatus === index}">{{ item }}</p>
          </div>
        </div>
       <div class="word">
          <span>销售额(元)</span>
          <span>销售量(件)</span>
      </div>
      <div class="count">
            <div class="total">
                <span :class="{ 'test': logoStatus > 0 }"  v-for = '(value, index) in sell_amount' :key="index"  v-show="logoStatus === index" >{{ value }}</span>
            </div>
            <div class="total">
                <span :class="{ 'test': logoStatus > 0 }" v-for = '(value, index) in sell_count' :key="index" v-show="logoStatus === index">{{ value }}</span>
            </div>
      </div>
   </div>
</div>

</template>
<style lang="less" scoped>
.pic {
  text-align: center;
  margin-top: 30px;
  span {
    display: block;
    margin-top: 20px;
    text-align: center;
    opacity: 0.8;
    color: #fff;
    font-family: PingFangSC;
    font-size: 16px;
    letter-spacing: 0.8px;
    letter-spacing: 0.8px;
  }
  img {
    width: 220px;
    vertical-align: top;
  }
}
.logo-pic {
  height: 150px;
  margin-top: 10px;
  display: flex;
  justify-content: center;
  width: 100%;
  .logo {
    float: left;
    text-align: center;
    img {
      width: 80px;
      height: 80px;
      opacity: 0.5;
    }
    .logo-active {
      width: 100px;
      height: 100px;
      opacity: 1;
    }
    .p-active {
      opacity: 1;
    }
    p {
      color: #ffffff;
      margin: 0px 20px 0px;
      font-family: PingFangSC;
      font-size: 18px;
      font-weight: 600;
      opacity: 0.5;
    }
  }
}
.screen_sort {
  text-align: center;
}
.word {
  padding-top: 15px;
  span {
    color: #ccc;
    padding: 10px 75px;
  }
  border-top: 1px solid #ccc;
}
.count {
  .total {
    float: left;
    width: 100px;
    height: 50px;
    // background-color: #fff;
    margin: 0px 66px;
  }
  span {
    font-size: 25px;
    text-align: center;
    color: white;
    display: inline-block;
    margin: 5px 0px 0 10px;
  }
}
.test {
  // 花费的时间和延时
  animation: test 0.5s 4.5s forwards;
}
@keyframes test {
  from {
    font-size: 25px;
    color: white;
  }
  to {
    font-size: 22px;
    color: rgba(255, 255, 255, 0.6);
  }
}
</style>
<script>
import oneSales from '@/public/img/one.png'
import twoSales from '@/public/img/two.png'
import threeSales from '@/public/img/three.png'

export default {
  // props: ['rightSort'],
  props: ['category_name', 'sell_amount', 'sell_count', 'sortData'],
  data () {
    return {
      logoStatus: 0,
      logoActive: null,
      hotSales: [oneSales, twoSales, threeSales]
    }
  },
  mounted () {
    clearInterval(this.logoActive)
    if (this.category_name.length === 1) {

    }
    this.logoActive = setInterval(() => {
      if (this.logoStatus < this.category_name.length - 1) {
        this.logoStatus++
      } else {
        this.logoStatus = 0
      }
    }, 5000)
  }
}
</script>
