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
  props: ['titleSale', 'count'],
  data () {
    return {
      polar: {
        title: {
          text: '销售额(元)',
          textStyle: {
            color: '#707192',
            fontSize: 12
          },
          left: '6%',
          textAlign: 'left'
        },
        grid: {
          y: 30,
          left: '7%',
          right: '2%'
          // borderWidth: 1
        },
        xAxis: {
          type: 'category',
          data: [],
          boundaryGap: false,
          // 控制网格线是否显示
          splitLine: {
            show: true,
            interval: 'auto',
            lineStyle: {
              color: ['#2A3675']
            }
          },
          // 刻度线
          axisTick: {
            show: false
          },
          // x轴坐标和字体颜色
          axisLine: {
            lineStyle: {
              color: '#2A3675'
            }
          },
          axisLabel: {
            margin: 10,
            interval: 3,
            textStyle: {
              fontSize: 14,
              color: 'rgba(255,255,255,0.4)'
            }
          }
        },
        yAxis: {
          type: 'value',
          // max: '500',
          // 控制网格是否显示
          splitLine: {
            lineStyle: {
              color: ['#2A3675']
            }
          },
          axisTick: {
            show: false
          },
          // y轴坐标和字体颜色
          axisLine: {
            lineStyle: {
              color: '#2A3675'
            }
          },
          axisLabel: {
            margin: 10,
            textStyle: {
              fontSize: 14,
              color: 'rgba(255,255,255,0.4)'
            }
          }
        },
        series: [
          {
            name: '昨日',
            type: 'line',
            smooth: true,
            showSymbol: false,
            symbol: 'circle',
            symbolSize: 6,
            data: [],
            // 线下方区域的颜色
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: '#ee3c7e' },
                  { offset: 1, color: 'rgba(250, 193, 150, 0.04)' }
                ])
              }
            },
            // 线的颜色
            itemStyle: {
              normal: {
                color: {
                  colorStops: [
                    {
                      offset: 0,
                      color: '#ee3c7e'
                    },
                    {
                      offset: 1,
                      color: '#fac196'
                    }
                  ]
                }
              }
            },
            lineStyle: {
              normal: {
                width: 3
              }
            }
          }
        ]
      }
    }
  },
  watch: {
    titleSale: {
      handler: function (val, oldval) {
        this.polar.xAxis.data = val.title[5].xdata
        this.polar.series[0].data = val.title[4].titleSale
      },
      deep: true
    },
    count: {
      handler: function (val, oldval) {
        if (val === 0) {
          this.polar.xAxis.axisLabel.interval = 0
        } else {
          this.polar.xAxis.axisLabel.interval = 3
        }
      },
      deep: true
    }
  }
}
</script>
