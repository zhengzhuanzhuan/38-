<template>
 <div>
    <div class= 'pic' v-if="notNull === 0">
       <img src="../../public/img/notData.png" alt="">
      <span>暂无数据</span>
    </div>
   <div v-else>
     <p>销售额(元)</p>
     <ul>
     <li class="first" v-for="(item,index) in rightSingle" :key = 'index'>
       <strong>{{ item.rank_number }}</strong>
         <span>{{ substrStr(item.goods_name, 17) }}</span>
         <i>{{ item.sell_amount }}</i>
         <em :style="{ width: ((item.sell_amount/first_sell[0])*100).toFixed(2) + '%' }"></em>
     </li>
   </ul>
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
p {
  height: 30px;
  opacity: 0.8;
  color: #fff;
  font-family: PingFangSC;
  font-size: 14px;
  letter-spacing: 0.8px;
  text-align: right;
}
ul {
  list-style: none;
   overflow: hidden;
    height: 212px;
  li {
    width: 472px;
    height: 35px;
    line-height: 35px;
    font-family: PingFangSC;
    font-size: 16px;
    font-weight: 600;
    letter-spacing: 2.3px;
    color: #ffffff;
    span {
      margin-left: 5px;
    }
    strong {
      color: #2e9dff;
    }
    i {
      float: right;
      font-style: normal;
      margin-right: 25px;
    }
    em {
      display: block;
      height: 2px;
      background-image: linear-gradient(to right, #33e6ef, #2e9dff);
    }
  }
  li:first-child {
    strong {
      color: #ee3c7e;
    }
    em {
      background-image: linear-gradient(to right, #ee3c7e, #fac196);
    }
  }
  li:nth-child(odd) {
    background-image: linear-gradient(to left, #9ed0fc, rgba(1, 31, 67, 0));
  }
}
</style>
<script>
export default {
  name: 'hello-world',
  props: ['rightSingle', 'first_sell', 'notNull'],
  data () {
    return {
    }
  },
  methods: {
    substrStr (val, num) {
      let n = num * 1
      if (val === null) {
        return ''
      }
      if (val.length > n) {
        return val.substr(0, n) + '...'
      } else {
        return val
      }
    },
    axiosAll () {
    }
  },
  mounted () {
  }
}
</script>
