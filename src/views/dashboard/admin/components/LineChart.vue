<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '350px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chart: null
    }
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val)
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')
      this.setOptions(this.chartData)
    },
    setOptions({ kandidatA, target, kandidatC, kandidatD } = {}) {
      this.chart.setOption({
        xAxis: {
          data: ['Kota Cirebon',
                  'Kab. Cirebon',
                  'Kab. Indramayu',
                  // 'Thu',
                  // 'Fri',
                  // 'Sat',
                  // 'Sun'
                ],
          boundaryGap: false,
          axisTick: {
            show: false
          }
        },
        grid: {
          left: 10,
          right: 10,
          bottom: 20,
          top: 30,
          containLabel: true
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          },
          padding: [5, 10]
        },
        yAxis: {
          axisTick: {
            show: false
          }
        },
        legend: {
          data: ['Kandidat A', 'Target']
        },
        series: [
          {
            name: 'Kandidat A', itemStyle: {
              normal: {
                color: '#40c9c6',
                lineStyle: {
                  color: '#40c9c6',
                  width: 2
                }
              }
            },
            smooth: true,
            type: 'line',
            data: kandidatA,
            animationDuration: 2800,
            animationEasing: 'cubicInOut'
          },
          {
            name: 'Target',
            smooth: true,
            type: 'line',
            itemStyle: {
              normal: {
                color: '#36a3f7',
                lineStyle: {
                  color: '#36a3f7',
                  width: 2
                }
              }
            },
            data: target,
            animationDuration: 2800,
            animationEasing: 'quadraticOut'
          },
          // {
          //   name: 'Kandidat C',
          //   smooth: true,
          //   type: 'line',
          //   itemStyle: {
          //     normal: {
          //       color: '#f4516c',
          //       lineStyle: {
          //         color: '#f4516c',
          //         width: 2
          //       }
          //     }
          //   },
          //   data: kandidatC,
          //   animationDuration: 2800,
          //   animationEasing: 'quadraticOut'
          // },
          // {
          //   name: 'Kandidat D',
          //   smooth: true,
          //   type: 'line',
          //   itemStyle: {
          //     normal: {
          //       color: '#34bfa3',
          //       lineStyle: {
          //         color: '#34bfa3',
          //         width: 2
          //       }
          //     }
          //   },
          //   data: kandidatD,
          //   animationDuration: 2800,
          //   animationEasing: 'quadraticOut'
          // }
        ]
      })
    }
  }
}
</script>
