<template>
    <chart :options="polar" auto-resize></chart>
</template>
<style lang="less" scoped>
.echarts {
  width: 100%;
  height: 100%;
}
</style>
<script>
import echarts from 'echarts'
export default {
  name: 'lineChart',
  props: ['consumerAge'],
  watch: {
    consumerAge: {
      handler: function (val, oldval) {
        this.polar.series[0].data = val
      },
      deep: true
    }
  },
  data () {
    return {
      polar: {
        // legend: {
        //   right: 10,
        //   width: 500,
        //   top: 0,
        //   itemWidth: 10,
        //   itemHeight: 10,
        //   textStyle: {
        //     fontSize: 12,
        //     color: 'rgba(255,255,255,0.4)'
        //   },
        //   data: ['发放数', '使用数']
        // },
        grid: {
          left: '3%',
          right: '4%',
          top: '20%',
          bottom: '10%',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            data: ['不足18', '19-24岁', '25-29岁', '30-34岁', '35-39岁', '39岁以上'],
            axisLine: {
              lineStyle: {
                color: '#2A3675'
              }
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 12,
                color: 'rgba(255,255,255,0.4)'
              }
            }
          }
        ],
        yAxis: [
          {
            type: 'value',
            axisLine: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 14,
                color: 'rgba(255,255,255,0.4)'
              }
            },
            axisTick: {
              show: false
            },
            splitLine: {
              show: true,
              interval: 'auto',
              lineStyle: {
                color: ['#2A3675']
              }
            }
          }
        ],
        series: [
          {
            name: '发放数',
            type: 'bar',
            barWidth: '30%',
            itemStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: '#33e6ef' },
                  { offset: 1, color: '#2e9dff' }
                ]),
                label: {
                  show: true,
                  textStyle: {
                    color: '#fff',
                    fontSize: 18
                  },
                  position: 'top',
                  formatter: '{c}%',
                  padding: 10
                }
              }
            },
            data: []
          }
          // {
          //   name: '使用数',
          //   type: 'bar',
          //   barWidth: '20%',
          //   itemStyle: {
          //     normal: {
          //       color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          //         { offset: 0, color: '#fa9696' },
          //         { offset: 1, color: '#ff007c' }
          //       ])
          //     }
          //   },
          //   data: [50, 30, 20, 60, 70, 50]
          // }
        ]
      }
    }
  }
}
</script>
