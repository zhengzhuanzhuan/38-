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
  props: ['change'],
  data () {
    return {
      polar: {
        title: {
          text: '单位:人数',
          right: '5%',
          textStyle: {
            color: 'rgba(255,255,255,0.5)',
            fontSize: '12'

          }
        },
        grid: {
          left: '3%',
          right: '4%',
          top: '12%',
          bottom: '20%',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            show: true,
            data: ['访客', '下单', '付费', '复购'],
            axisTick: {
              show: false
            },
            axisLabel: {
              margin: 10,
              show: true,
              textStyle: {
                color: '#fff',
                fontSize: 12
              }
            },
            axisLine: {
              lineStyle: {
                color: '#2A3675'
              }
            }
          }
        ],

        yAxis: [
          {
            type: 'value',
            // name: '保障率',
            min: 0,
            axisTick: {
              show: false
            },
            axisLine: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 10,
                color: 'rgba(255,255,255,0.4)'
              }
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
            name: '保障率',
            type: 'bar',
            barWidth: '10%',
            data: [
              {
                value: 0,
                itemStyle: {
                  normal: {
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                      { offset: 0, color: '#00ffde' },
                      { offset: 1, color: '#00a7ff' }
                    ]),
                    barBorderRadius: [50, 50, 0, 0]
                  }
                }
              },
              {
                value: 0,
                itemStyle: {
                  normal: {
                    label: {
                      show: true,
                      position: 'bottom',
                      offset: [-90, -60],
                      textStyle: {
                        color: '#fff',
                        fontSize: 20
                      },
                      formatter: (params) => {
                        if (params.value === 0) {
                          return ''
                        } else {
                          return (this.change[0].v_order_rate * 100).toFixed(1) + '%'
                        }
                      }
                    },
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                      { offset: 0, color: '#00ffde' },
                      { offset: 1, color: '#00a7ff' }
                    ]),
                    barBorderRadius: [50, 50, 0, 0]
                  }
                }
              },
              {
                value: 0,
                itemStyle: {
                  normal: {
                    label: {
                      show: true,
                      position: 'bottom',
                      offset: [-90, -60],
                      textStyle: {
                        color: '#fff',
                        fontSize: 20
                      },
                      formatter: (params) => {
                        // var ss = window.sessionstorage.getItem('num')
                        if (params.value === 0) {
                          return ''
                        } else {
                          return (this.change[0].v_payed_rate * 100).toFixed(1) + '%'
                        }
                      }
                    },
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                      { offset: 0, color: '#00ffde' },
                      { offset: 1, color: '#00a7ff' }
                    ]),
                    barBorderRadius: [50, 50, 0, 0]
                  }
                }
              },
              {
                value: 0,
                itemStyle: {
                  normal: {
                    label: {
                      show: true,
                      position: 'bottom',
                      offset: [-90, -60],
                      textStyle: {
                        color: '#fff',
                        fontSize: 20
                      },
                      formatter: (params) => {
                        if (params.value === 0) {
                          return ''
                        } else {
                          return (this.change[0].v_repurchase_rate * 100).toFixed(1) + '%'
                        }
                      }
                    },
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                      { offset: 0, color: '#00ffde' },
                      { offset: 1, color: '#00a7ff' }
                    ]),
                    barBorderRadius: [50, 50, 0, 0]
                  }
                }
              }
            ]
          },

          {
            type: 'line',
            name: '占比',
            smooth: true,
            showSymbol: false,
            data: [],
            // 线下的颜色
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: '#00FFDE ' },
                  { offset: 1, color: 'rgba(30, 15, 200, 0.05)' }
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
                      color: '#00a7ff'
                    },
                    {
                      offset: 1,
                      color: '#00ffde'
                    }
                  ]
                }
              }
            },
            lineStyle: {
              normal: {
                width: 2
              }
            }
          }
        ]
      }
    }
  },
  watch: {
    change: {
      handler: function (val, oldval) {
        this.polar.series[0].data[0].value = val[0].visitors_count
        this.polar.series[0].data[1].value = val[0].v_order_count
        this.polar.series[0].data[2].value = val[0].v_payed_count
        this.polar.series[0].data[3].value = val[0].v_repurchase_count
        this.polar.series[1].data[0] = val[0].visitors_count
        this.polar.series[1].data[1] = val[0].v_order_count
        this.polar.series[1].data[2] = val[0].v_payed_count
        this.polar.series[1].data[3] = val[0].v_repurchase_count
      },
      deep: true
    }
  }
}
</script>
