<template>
 <!-- <swiper :options="swiperOption" ref="mySwiper">
    <swiper-slide v-for='(item,index) in swiperArray' :key="index">
        <div class="middle-top" >
          <middle-count  :slide="item"></middle-count>
          <div class="chart-wrap">
              <middle-pic :titleSale="item"></middle-pic>
          </div>
        </div>
    </swiper-slide>
  </swiper>   -->
  <div style="width: 100%">
    <div  v-if="swiperActive === 0" class="middle-top" >
      <middle-count  :slide="swiperArray[0]"></middle-count>
      <div class="chart-wrap">
          <middle-pic :titleSale="swiperArray[0]" :count= 1></middle-pic>
      </div>
    </div>
    <div  v-if="swiperActive === 1" class="middle-top" >
      <middle-count  :slide="swiperArray[1]"></middle-count>
      <div class="chart-wrap">
          <middle-pic :titleSale="swiperArray[1]" :count = 0></middle-pic>
      </div>
    </div>
  </div>
</template>
<style lang="less" scoped>
.middle-top {
  height: 620px;
  padding: 20px 24px;
  background-image: url('../../public/img/middle_top.png');
  .chart-wrap {
    width: 100%;
    height: 270px;
    margin-top: 20px;
  }
}
</style>
<script>
import middleCount from './middleCount/index.vue'
import middlePic from './middlePic/index.vue'
import urls from '../../utils/api.js'
import common from '../../utils/common.js'

export default {
  name: 'hello-world',
  data () {
    return {
      swiperActive: 0,
      swiperTimer_: null,
      sale_today: '',
      sale_yesterday: '',
      order_count: '',
      unit_price: '',
      sales_trend_month: [],
      sales_trend_day: [],
      x_month: [],
      x_day: [],
      swiperArray: [
        {
          title: [
            { name: '今日销售额(元)', value: '0' },
            { name: '昨日销售额(元)', value: '0' },
            { name: '今日订单量(笔)', value: '0' },
            { name: '今日客单价(元)', value: '0' },
            { titleSale: 0 },
            { xdata: 0 }
          ]
        },
        {
          title: [
            { name: '本月销售额(元)', value: '0' },
            { name: '上月销售额(元)', value: '0' },
            { name: '本月订单量(笔)', value: '0' },
            { name: '本月客单价(元)', value: '0' },
            { titleSale: 0 },
            { xdata: 0 }
          ]
        }
      ]
    }
  },
  components: {
    middlePic,
    middleCount
  },
  methods: {
    // 昨日销售额
    axiosSalesdaydata () {
      this.$axios
        .get(urls.baseURL + urls.salesdaydata)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.sale_yesterday = common.toInt(record.sell_amount)
            this.swiperArray[0].title[1].value = this.sale_yesterday.slice(0, ((this.sale_yesterday).length - 3))
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 今日销售额包括今日订单量，今日客单价
    axiosSalesToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.sale_today = []
            this.swiperArray[0].title[0].value = common.toInt(record.sell_amount)
            // console.log(this.swiperArray[0].title[0].value)
            this.swiperArray[0].title[2].value = record.order_count
            this.swiperArray[0].title[3].value = record.unit_price.toFixed(2)
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 本月销售额
    axiosSalesMonth () {
      this.$axios
        .get(urls.baseURL + urls.sales_month)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.sales_month = []
            this.sales_month = common.toInt(record.sell_amount)
            this.swiperArray[1].title[0].value = this.sales_month
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 本月订单量
    axiosOrderCountMonth () {
      this.$axios
        .get(urls.baseURL + urls.order_count_month)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.order_count_month = record.order_count
            this.swiperArray[1].title[2].value = this.order_count_month
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 本月客单价
    axiosUnitPriceMonth () {
      this.$axios
        .get(urls.baseURL + urls.unit_price_month)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.unit_price = record.unit_price.toFixed(2)
            this.swiperArray[1].title[3].value = this.unit_price
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 上月销售额
    axiosSalesLmonth () {
      this.$axios
        .get(urls.baseURL + urls.sales_lmonth)
        .then(res => {
          let data = res.data
          // console.log(data)
          if (data.code === 0) {
            let record = data.data
            this.sales_lmonth = common.toInt(record.sell_amount)
            this.swiperArray[1].title[1].value = (this.sales_lmonth).slice(0, ((this.sales_lmonth).length - 3))
            // console.log((this.sales_lmonth).slice(0, ((this.sales_lmonth).length - 3)))
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 最近12个月销售额趋势
    axiosSalesTrendMonth () {
      this.$axios
        .get(urls.baseURL + urls.sales_trend_month)
        .then(res => {
          let data = res.data
          // console.log(data)
          if (data.code === 0) {
            let record = data.data
            // console.log(record)
            this.sales_trend_month = []
            this.x_month = []
            for (var i = 0; i < record.length; i++) {
              this.sales_trend_month.push(record[i].sell_amount)
              this.x_month.push(record[i].dt_date.slice(-2) + '月')
            }
            this.swiperArray[1].title[4].titleSale = this.sales_trend_month
            this.swiperArray[1].title[5].xdata = this.x_month
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 最近30天的销售趋势
    axiosSalesTrendDay () {
      this.$axios
        .get(urls.baseURL + urls.sales_trend_day)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.sales_trend_day = []
            this.x_day = []
            for (var i = 0; i < record.length; i++) {
              this.sales_trend_day.push(record[i].sell_amount)
              this.x_day.push(record[i].dt_date.slice(4, -2) + '-' + record[i].dt_date.slice(-2))
            }
            this.swiperArray[0].title[4].titleSale = this.sales_trend_day
            this.swiperArray[0].title[5].xdata = this.x_day
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    axiosAll () {
      this.axiosSalesdaydata()
      this.axiosSalesToday()
      this.axiosSalesMonth()
      this.axiosSalesLmonth()
      this.axiosSalesTrendMonth()
      this.axiosSalesTrendDay()
      this.axiosUnitPriceMonth()
      this.axiosOrderCountMonth()
    }
  },
  mounted () {
    clearInterval(this.axiosTimeOut)
    this.axiosTimeOut = setInterval(() => {
      this.axiosAll()
    }, 3000)
    clearInterval(this.swiperTimer_)
    this.swiperTimer_ = setInterval(() => {
      if (this.swiperActive < 1) {
        this.swiperActive ++
      } else {
        this.swiperActive = 0
      }
    }, 10000)
  },
  destroyed () {
    clearInterval(this.axiosTimeOut)
    clearInterval(this.swiperTimer_)
  }
}
</script>
