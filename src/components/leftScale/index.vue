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
export default {
  props: ['sexCount'],
  name: 'lineChart',
  data () {
    return {
      polar: {
        // legend: {
        //   textStyle: {
        //     fontSize: 10,
        //     color: 'rgb(255,255,255,)'
        //   },
        //   show: true,
        //   data: ['男买家', '女买家']
        // },
        grid: {
          // show: true,
          // left: '7%',
          // right: '2%',
          // // top: '25%',
          // bottom: '50%'
        },
        // title: {
        //   text: '性别',
        //   x: 'center',
        //   y: 'center',
        //   textStyle: {
        //     fontWeight: 'normal',
        //     fontSize: 16,
        //     color: '#fff'
        //   }
        // }
        title: [
          {
            text: '性别',
            x: 'center',
            y: 'center',
            textStyle: {
              fontWeight: 'normal',
              fontSize: 16,
              color: '#fff'
            }
          },
          {
            text: '',
            x: 'center',
            y: 'top',
            textStyle: {
              color: '#fa9696',
              fontSize: 12
            }
          },
          {
            text: '',
            x: 'center',
            y: 'bottom',
            textStyle: {
              color: '#00bbff',
              fontSize: 12
            }
          }
        ],
        series: [
          {
            // name: '访问来源',
            type: 'pie',
            hoverAnimation: false,
            startAngle: '60',
            radius: ['40%', '60%'],
            // labelLine: {
            // show: false
            //   normal: {
            //     length: 15,
            //     length2: 50,
            //     lineStyle: {
            //       width: 2
            //     }
            //   }
            // },
            // label: {
            //   show: false
            //   normal: {
            //     formatter: '{a|{d}%}\n',
            //     padding: [0, -50],
            //     rich: {
            //       a: {
            //         color: '#fff',
            //         fontSize: 14,
            //         lineHeight: 20
            //       },
            //       b: {
            //         fontSize: 16,
            //         lineHeight: 20,
            //         color: '#fff'
            //       }
            //     }
            //   }
            // },
            itemStyle: {
              normal: {
                label: {
                  show: false // 隐藏标示文字
                },
                labelLine: {
                  show: false // 隐藏标示线
                }
              }
            },
            data: [
              {
                value: 0,
                name: '男买家',
                itemStyle: {
                  normal: {
                    // 边框大小和边框颜色
                    // borderWidth: '10',
                    // borderColor: '#0e1f71',
                    color: {
                      colorStops: [
                        {
                          offset: 0,
                          color: '#fa9696'
                        },
                        {
                          offset: 0.3,
                          color: '#fa9696'
                        },
                        {
                          offset: 1,
                          color: '#ff007c'
                        }
                      ]
                    }
                  }
                }
              },
              {
                value: 0,
                name: '女买家',
                itemStyle: {
                  normal: {
                    color: {
                      colorStops: [
                        {
                          offset: 0,
                          color: '#00bbff'
                        },
                        {
                          offset: 0.6,
                          color: '#00ffff'
                        }
                      ]
                    }
                  }
                }
              }
            ]
          }
        ]
      }
    }
  },
  watch: {
    sexCount: {
      handler: function (val, oldval) {
        this.polar.series[0].data[0].value = val[0].male_consumer_proportion
        this.polar.series[0].data[1].value = val[0].female_consumer_proportion
        this.polar.title[1].text = `男买家   ${(
          val[0].male_consumer_proportion * 100
        ).toFixed(2)}%`
        this.polar.title[2].text = `女买家   ${(
          val[0].female_consumer_proportion * 100
        ).toFixed(2)}%`
      },
      deep: true
    }
  }
}
</script>
