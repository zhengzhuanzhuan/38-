<template>
 <div>
    <div class="total">
       <p>今日扫脸人次</p>
       <span>{{visitorsCnt.visitors_cnt_today}}</span>
    </div>
      <div class="use">
          <div class="word">
                <p>昨日扫脸人次</p>
                <span>{{ visitorsCnt.visitors_cnt_1d }}</span>
            </div>
            <div class="count">
              <p>平均扫脸识别速度</p>
                <span>{{ faceRate[faceId] }}秒</span>
            </div>
      </div>
 </div>
</template>
<style lang="less" scoped>
div {
  color: #fff;
  position: relative;
  .total {
    text-align: center;
    p {
      font-family: PingFangSC;
      font-size: 20px;
      font-weight: 600;
      padding: 20px 0 6px;
      letter-spacing: 1.4px;
    }
    span {
      color: #00ffff;
      font-family: DINAlternate;
      font-size: 60px;
      font-weight: bold;
    }
  }
  .use {
    height: 120px;
    width: 100%;
    position: absolute;
    top: 140px;
    background-color: #2a3675;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    .word {
      margin-right: 80px;
    }
    .count {
      margin-left: 80px;
    }
    p {
      opacity: 0.8;
      font-family: PingFangSC;
      font-size: 18px;
      letter-spacing: 1.3px;
      margin-bottom: 5px;
    }
    // span {
    //   display: inline-block;
    //   padding: 23px 80px 15px 90px;
    // }
    span {
      font-family: DINAlternate;
      font-size: 30px;
      font-weight: bold;
      letter-spacing: 1.6px;
    }
  }
}
</style>
<script>
export default {
  props: ['visitorsCnt'],
  data () {
    return {
      faceRate: ['0.1', '0.15', '0.2'],
      faceId: 0
    }
  },
  methods: {},
  mounted () {
    clearInterval(this.IntervalFaceRate)
    this.IntervalFaceRate = setInterval(() => {
      if (this.faceId === 2) {
        this.faceId = 0
      } else if (this.faceId < 2) {
        this.faceId = this.faceId + 1
      }
    }, 3600000)
  },
  destroyed () {
    clearInterval(this.IntervalFaceRate)
  }
}
</script>
