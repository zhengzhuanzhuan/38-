<template>
  <div class="bg">
    <div id="screen" class="screen">
      <div class="header">
        <div class="header-title">
            <p>奇点云AI体验馆运营平台</p>
        </div>
        <div class="header-time">
          <p>{{ date }}</p>
          <p>{{ time }}</p>
        </div>
      </div>
      <div class="left">
           <div class="left-top">
              <left-coupon :visitorsCnt = 'visitorsCnt' ></left-coupon>
           </div>
           <div class="left-middle">
               <title-bar label="买家占比"></title-bar>
               <div class="chartsPie">
                  <div class="chart-wrap">
                    <left-scale :sexCount = 'sexCount'></left-scale>
                  </div>
                  <div class="chart-wrap">
                    <left-customer :customerCount = 'customerCount'></left-customer>
                  </div>
               </div>
           </div>
            <div class="left-bottom">
               <title-bar label="买家年龄段分布"></title-bar>
                <div class="chart-wrap">
                    <left-state :consumerAge='consumerAge'></left-state>
                </div>
           </div>
      </div>
      <div class="middle">
        <div class="middle-top" >
            <middle-swiper></middle-swiper>
        </div>
        <div class="middle-bottom">
           <title-bar label="销售转化"></title-bar>
                <div class="chart-wrap">
                    <middle-change :change= 'change'></middle-change>
                </div>
        </div>
      </div>
      <div class="right">
           <div class="right-top">
             <title-bar label="热销品类"></title-bar>
              <right-sort :sortData = 'sortData' :category_name = 'category_name' :sell_amount = 'sell_amount' :sell_count = 'sell_count'></right-sort>
           </div>
           <div class="right-middle">
               <title-bar label="热销单品"></title-bar>
               <right-single :rightSingle = 'rightSingle' :first_sell = 'first_sell' :notNull = 'notNull'></right-single>
           </div>
            <div class="right-bottom">
               <title-bar label="店铺明细"></title-bar>
               <right-detail :swiperData = 'swiperData' :shopSale = 'shopSale'></right-detail>
           </div>
      </div>
    </div>
  </div>
</template>

<script>
import setScreen from '@/utils/setScreen'
import common from '../utils/common.js'
import titleBar from './titleBar/index.vue'
import leftCoupon from './leftCoupon/index.vue'
import leftScale from './leftScale/index.vue'
import leftState from './leftState/index.vue'
import leftCustomer from './leftCustomer/index'
import rightSort from './rightSort/index.vue'
import rightDetail from './rightDetail/index.vue'
import rightSingle from './rightSingle/index.vue'
import middleChange from './middleChange/index.vue'
import middleSwiper from './middleSwiper/index.vue'
import urls from './../utils/api.js'

export default {
  name: 'HelloWorld',
  data () {
    return {
      consumerAge: [],
      sexCount: [],
      customerCount: [],
      visitorsCnt: [],
      date: '',
      time: '',
      sortData: 0,
      notNull: 0,
      dateTimer: null,
      first_sell: [],
      category_name: [],
      sell_amount: [],
      sell_count: [],
      axiosTimeOut: null,
      shopSale: 0,
      total: [
        {
          used_coupon_order_count: '0',
          coupon_used_rate: '0',
          draw_coupon: '0',
          with_coupon_rate: '0',
          without_coupon_rate: '0'
        }
      ],
      change: [
        {
          visitors_count: '',
          v_order_count: '',
          v_payed_count: '',
          v_repurchase_count: '',
          v_order_rate: '',
          v_payed_rate: '',
          v_repurchase_rate: ''
        }
      ],
      swiperData: [],
      swiperDataOld: [],
      rightSingle: [],
      reloadTimer: null
    }
  },
  components: {
    titleBar,
    leftCoupon,
    leftScale,
    leftCustomer,
    rightSort,
    rightDetail,
    rightSingle,
    leftState,
    middleChange,
    middleSwiper
  },
  methods: {
    getDate () {
      let _date = new Date()
      this.date = common.dateParse(_date, 'yyyy年MM月d日')
      this.time = common.dateParse(_date, 'hh:mm:ss')
    },
    timer () {
      this.getDate()
      clearInterval(this.dateTimer)
      this.dateTimer = setInterval(this.getDate, 1000)
    },

    // 优惠券状态，柱状图
    // axiosCouponTrendDay () {
    //   this.$axios
    //     .get(urls.baseURL + urls.coupon_trend_day)
    //     .then(res => {
    //       let data = res.data
    //       if (data.code === 0) {
    //         let record = data.data
    //         this.draw_coupon_count = []
    //         this.used_coupon_count = []
    //         this.dt_date = []
    //         for (let i = 0; i < record.length; i++) {
    //           this.draw_coupon_count.push(record[i].draw_coupon_count)
    //           this.used_coupon_count.push(record[i].used_coupon_count)
    //           this.dt_date.push(record[i].dt_date.slice(4, -2) + '-' + record[i].dt_date.slice(-2))
    //           // console.log(this.dt_date)
    //         }
    //       }
    //     })
    //     .catch(error => {
    //       console.log(error)
    //     })
    // },
    // 当日优惠券使用情况
    // axiosCouponToday () {
    //   this.$axios
    //     .get(urls.baseURL + urls.coupon_today)
    //     .then(res => {
    //       let data = res.data
    //       if (data.code === 0) {
    //         let record = data.data
    //         this.total[0].used_coupon_order_count =
    //           record.used_coupon_order_count
    //         this.total[0].coupon_used_rate =
    //           (record.coupon_used_rate * 100).toFixed(2) + '%'
    //         this.total[0].draw_coupon = record.draw_coupon_count
    //         this.total[0].with_coupon_rate =
    //           (record.with_coupon_rate * 100).toFixed(1) + '%'
    //         this.total[0].without_coupon_rate =
    //           (record.without_coupon_rate * 100).toFixed(1) + '%'
    //       }
    //     })
    //     .catch(error => {
    //       console.log(error)
    //     })
    // },
    // 销售转化
    axiosSalesTransToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_trans_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.change[0].visitors_count = record.visitors_count
            this.change[0].v_order_count = record.v_order_count
            this.change[0].v_payed_count = record.v_payed_count
            this.change[0].v_repurchase_count = record.v_repurchase_count
            this.change[0].v_order_rate = record.v_order_rate
            this.change[0].v_payed_rate = record.v_payed_rate
            this.change[0].v_repurchase_rate = record.v_repurchase_rate
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 店铺明细
    axiosSalesStoreToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_store_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            // console.log(record, '上面的')
            this.shopSale = record.length
            this.swiperDataOld = []
            for (var i = 0; i < record.length; i++) {
              this.swiperDataOld.push({
                sell_amount: record[i].sell_amount.toFixed(2),
                sell_count: record[i].sell_count,
                store_name: record[i].store_name,
                store_url: record[i].store_url
              })
            }
            this.axiosSalesCategory()
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 店铺明细之热销品类
    axiosSalesCategory () {
      this.$axios
        .get(urls.baseURL + urls.sales_category)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            for (var i = 0; i < record.length; i++) {
              this.swiperDataOld[i].category_name = record[i].category_name
            }
            this.swiperData = this.swiperDataOld
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 热销单品
    axiosSalesGoodsToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_goods_today)
        .then(res => {
          let data = res.data
          this.notNull = data.data.length
          if (data.code === 0) {
            let record = data.data
            this.rightSingle = []
            for (let i = 0; i < record.length; i++) {
              this.rightSingle.push({
                rank_number: record[i].rank_number,
                goods_name: record[i].goods_name,
                sell_amount: record[i].sell_amount
              })
              this.first_sell.push(record[i].sell_amount)
            }
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 热销品类
    axiosSalesClassToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_class_today)
        .then(res => {
          let hotSales = res.data
          if (hotSales.code === 0) {
            let record = hotSales.data
            this.sortData = record.length
            this.category_name = []
            this.sell_amount = []
            this.sell_count = []
            for (let i = 0; i < record.length; i++) {
              this.category_name.push(record[i].category_name)
              this.sell_amount.push(record[i].sell_amount.toFixed(2))
              this.sell_count.push(record[i].sell_count)
            }
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 扫脸人数
    axiosVisitorsCnt () {
      this.$axios
        .get(urls.baseURL + urls.visitors_cnt)
        .then(res => {
          if (res.data.code === 0) {
            this.visitorsCnt = res.data.data
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 买家占比
    axiosConsumerProportion () {
      this.$axios.get(urls.baseURL + urls.consumer_proportion).then(res => {
        if (res.data.code === 0) {
          let record = res.data.data
          this.customerCount = []
          this.sexCount = []
          this.customerCount.push({
            new_consumer_proportion: record.new_consumer_proportion,
            old_consumer_proportion: record.old_consumer_proportion
          })
          this.sexCount.push({
            female_consumer_proportion: record.female_consumer_proportion,
            male_consumer_proportion: record.male_consumer_proportion
          })
        }
      }).catch(error => {
        console.log(error)
      })
    },
    // 买家年龄段占比
    axiosConsumerAgeProportion () {
      this.$axios.get(urls.baseURL + urls.consumer_age_proportion).then(res => {
        if (res.data.code === 0) {
          let record = res.data.data
          delete record.dt_date
          for (let key in record) {
            record[key] = (record[key] * 100).toFixed(1)
          }
          this.consumerAge = Object.values(record)
        }
      }).catch(error => {
        console.log(error)
      })
    },
    axiosAll () {
      // this.axiosCouponTrendDay()
      // this.axiosCouponToday()
      this.axiosSalesTransToday()
      this.axiosSalesStoreToday()
      // this.axiosSalesCategory()
      this.axiosSalesGoodsToday()
      this.axiosSalesClassToday()
      this.axiosVisitorsCnt()
      this.axiosConsumerProportion()
      this.axiosConsumerAgeProportion()
    }
  },
  mounted () {
    setScreen(1920, 1080, 'screen')
    this.timer()
    this.axiosAll()
    clearInterval(this.axiosTimeOut)
    this.axiosTimeOut = setInterval(() => {
      this.axiosAll()
    }, 3000)

    clearInterval(this.reloadTimer)

    this.reloadTimer = setInterval(() => {
      let d = new Date()
      if (
        (d.getHours() === 9 && d.getMinutes() === 0) ||
        (d.getHours() === 15 && d.getMinutes() === 0) ||
        (d.getHours() === 19 && d.getMinutes() === 0)
      ) {
        location.reload()
      }
    }, 60000)
  },
  destroyed () {
    clearInterval(this.axiosTimeOut)
    clearInterval(this.reloadTimer)
  }
}
</script>

<style lang="less" scoped>
.bg {
  width: 1920px;
  height: 1080px;
  overflow: hidden;
  .screen {
    overflow: hidden;
    width: 100%;
    height: 100%;
    padding: 20px 20px 0 20px;
    /*background-image: radial-gradient(circle at 47% 43%, #0e1f71, #0d093d);*/
    .header {
      margin-bottom: 30px;
      &-title {
        font-size: 54px;
        font-weight: 600;
        text-align: center;
        letter-spacing: 12px;
        font-family: PingFangSC;
        background-clip: text;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-image: linear-gradient(to left, #00bbff 0%, #00ffff);
      }
      &-time {
        text-align: right;
        position: absolute;
        right: 40px;
        top: 27px;
        color: #00bbff;
        font-family: DINAlternate;
        font-size: 26px;
        font-weight: bold;
      }
    }
    .left {
      float: left;
      width: 515px;
      span {
        float: right;
        color: #fff;
        opacity: 0.3;
        font-size: 14px;
      }
      & > div {
        height: 300px;
        margin-bottom: 20px;
        padding: 20px 24px;
        background-image: url('../public/img/small.png');
        // background-image: radial-gradient(circle at 47% 0, #6478a3, rgba(19, 54, 102, 0.1));
      }
      .left-top {
        padding: 20px;
      }
      .left-middle {
        .chartsPie {
          display: flex;
          .chart-wrap {
            width: 50%;
            height: 210px!important;
            // margin-top: 10px;
          }
        }
      }
    }
    .right {
      float: left;
      width: 515px;
      & > div {
        height: 300px;
        margin-bottom: 20px;
        padding: 20px 24px;
        background-image: url('../public/img/small.png');
        // background-image: radial-gradient(circle at 47% 0, #6478a3, rgba(19, 54, 102, 0.1));
      }
    }
    .chart-wrap {
      width: 100%;
      height: 250px;
      margin-top: 20px;
    }
    .middle {
      float: left;
      width: 810px;
      margin: 0 20px;
      &-top {
        height: 620px;
        margin-bottom: 20px;
        background-image: url('../public/img/middle_top.png');
      }
      &-bottom {
        height: 300px;
        padding: 20px 24px;
        background-image: url('../public/img/middle-bottom.png');
        .chart-wrap {
          width: 100%;
          height: 290px;
          line-height: 290px;
          // margin-top: 20px;
        }
      }
    }
  }
}
</style>
