<template>
  <div class="container">
    <button @click="add">add</button>
    <button @click="reduce">reduce</button>
    <div ref="chart" class="chart"></div>
  </div>
</template>

<script>
import Echarts from "echarts";

let data = [
  { label: '周一', min: 5 },
  { label: '周二', min: 30 },
  { label: '周三', min: 50 },
  { label: '周四', min: 63 },
  { label: '周五', min: 70 },
  { label: '周六', min: 90 },
  { label: '周日', min: 70 },
  // { label: '周一a', min: 199 },
  // { label: '周二a', min: 39 },
  // { label: '周三a', min: 54 },
  // { label: '周四a', min: 63 },
  // { label: '周五a', min: 70 },
  // { label: '周六a', min: 90 },
  // { label: '周日a', min: 70 },
  // { label: '周一b', min: 199 },
  // { label: '周二b', min: 39 },
  // { label: '周三b', min: 54 },
  // { label: '周四b', min: 63 },
  // { label: '周五b', min: 70 },
  // { label: '周六b', min: 90 },
  // { label: '周日b', min: 70 },
];

export default {
  data() {
    return {
      data
    };
  },
  mounted() {
    // 基于准备好的dom，初始化echarts实例
    this.chart = Echarts.init(this.$refs.chart, 'light');

    // 指定图表的配置项和数据
    var option = {
      // title: {
      //   text: "ECharts 入门示例"
      // },
      tooltip: {},
      // legend: {
      //   data: ["销量"]
      // },
      xAxis: {
        type: 'category',
        // data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
        // boundaryGap: true,
        axisTick: {
          alignWithLabel: true
        }
      },
      yAxis: {
        show: true,
        type: 'value',
        position: 'right',
        // splitNumber: 3,
        interval: 40,
        min: 0,
        max: 120,
        axisLine: {
          // 坐标轴线和文字配置
          lineStyle: {
            width: 0
          }
        },
        // 坐标轴刻度相关设置
        axisTick: {
          show: false,
        },
        // 坐标轴刻度标签的相关设置
        axisLabel: {
          // margin: 10,
          margin: 42,
          // 使用字符串模板，模板变量为刻度默认标签 {value}
          formatter: '{value}分钟',
          color: '#B2B2B2',
          fontSize: 10,
          align: 'right'
        },
        splitLine: {
          show: true,
          lineStyle: {
            color: '#D8D8D8',
            type: 'dashed'
            // TODO 设置虚线间隔
          }
        }
      },
      dataset: {
        dimensions: ['label', 'min'],
        source: this.data
      },
      series: [
        {
          name: "销量", // 柱状图
          type: "bar",
          // data: [5, 20, 36, 10, 10, 20] // -> data sourcde
          // encode: {x: 0, y: 1}
          itemStyle: {
            color: new Echarts.graphic.LinearGradient(
                0, 0, 0, 1,
                [
                    {offset: 0, color: '#83bff6'},
                    {offset: 0.5, color: '#188df0'},
                    {offset: 1, color: '#188df0'}
                ]
            ),
            // TODO 自定义圆角实现
            barBorderRadius: [200, 200, 0, 0]
          },

        }
      ],
      // dataZoom: {},
      // grid: {},
      // toolbox: {},
      // visualMap: {}
    };

    // 使用刚指定的配置项和数据显示图表。
    this.chart.setOption(option);
    this.chart.on('click', function (params) {
      console.log('click')
    });
    this.chart.on('mousemove', function (params) {
      console.log('mousemove')
    });
  },
  methods: {
    add () {
      console.log('add')
      this.data[0].min = 200
      this.chart.setOption({
        dataset: {
          dimensions: ['label', 'min'],
          source: this.data
        },
        yAxis: {
          interval: 80,
          min: 0,
          max: 240,
        }
      })
    },
    reduce () {
      console.log('reduce')
      this.data[0].min = 20
      this.chart.setOption({
        dataset: {
          dimensions: ['label', 'min'],
          source: this.data
        },
        yAxis: {
          interval: 40,
          min: 0,
          max: 120,
        }
      })
    }
  }
};
</script>

<style scoped>
.chart {
  width: 375px;
  height: 260px;
}
</style>
