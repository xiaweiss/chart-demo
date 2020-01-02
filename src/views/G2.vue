<template>
  <div class="g2">
    <h1>This is an G2 page</h1>
    <button @click="add">add</button>
    <button @click="reduce">reduce</button>
    <div class="container">
      <div id="myChart" width="375" height="260"></div>
    </div>
  </div>
</template>

<script>
import G2 from '@antv/g2';


const X = 'label'
const Y = 'min'
const XY = `${X}*${Y}`

let data = [
  { label: '周一', min: 190 },
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

data = data.map((item, index) => {
  item.no = index
  return item
})

export default {
  data () {
    return {
      data
    }
  },
  created () {
  },
  mounted () {
    // Step 1: 创建 Chart 对象
    const chart = new G2.Chart({
      id: 'myChart',
      pixelRatio: window.devicePixelRatio, // 指定分辨率
      padding: [50, 'auto', 'auto'],
      width: 375,
      height: 260
    });
    this.chart = chart

    console.log('data', this.data);


    // Step 2: 载入数据源
    chart.source(this.data, {
      // no: {
      //   min: 0,
      //   max: 7
      // }
    });

    chart.coord('rect')

    // 为多个字段设置列定义
    // chart.scale(Y, {
    //   type: 'linear',
    //   tickCount: 3, // TODO: 换为手动计算 ticks
    //   // NOTE: 也可以在 axis 里配置
    //   // formatter (text) {
    //   //   return text + '分钟'
    //   // }
    // })

    // chart.scale({
    //   x: {
    //     type: 'cat', // 声明 type 字段为分类类型
    //     values: [ 'A', 'B', 'C' ] // 重新显示的值
    //     alias: '类型' // 设置属性的别名
    //   },
    //   y: {
    //     type: 'cat'
    //   }
    // });

    // // X 轴配置
    // chart.axis(X, {
    //   labelOffset: 12,
    //   line: {
    //     lineWidth: 1,
    //     stroke: '#E8E8E8',
    //     top: true, // 展示在最上层
    //   }, // 设置坐标轴线的样式，如果值为 null，则不显示坐标轴线，图形属性'
    // })

    // Y 轴配置
    // chart.axis(Y, {
    // //   labelOffset: 10.5,
    //   position: 'right',
    //   label (text, index, total) {
    //     let _text = ''
    //     if (index) {
    //       _text = text + '分钟'
    //     }
    //     return {
    //       text: _text,
    //       fontSize: 10,
    //       fill: '#B2B2B2',
    //       // textBaseline: 'right',
    //       // textAlign: 'end', // TODO
    //       // fontWeight:
    //       top: true
    //     }
    //   },
    //   grid (text, index, total) {
    //     return {
    //       stroke: '#D8D8D8',
    //       lineDash: [9, 5]
    //     }
    //   }
    // })

    // 图例：关
    // chart.legend(false)

    // Tooltip 配置
    chart.tooltip({
      showTitle: true, // 展示  tooltip 的标题
      layout: 'vertical'
    });

    // G2.Shape.registerShape('interval', 'lineCap', {
    //   getPoints(pointInfo) {
    //     // ...
    //   },
    //   draw(cfg, container) {
    //     // ...
    //     path = this.parsePath(path);
    //     // ...
    //     //
    //     return shape; // 返回最后绘制的 shape
    //   },
    // });

    // const Shape = F2.Shape;
    // Shape.registerShape('interval', 'triangle', {
    //   getPoints: function(cfg) {
    //     const x = cfg.x;
    //     const y = cfg.y;
    //     const y0 = cfg.y0;
    //     const width = cfg.size;
    //     return [
    //       { x: x - width / 2, y: y0 },
    //       { x: x, y: y },
    //       { x: x + width / 2, y: y0 },
    //     ];
    //   },
    //   draw: function(cfg, group) {
    //     const points = this.parsePoints(cfg.points); // 将0-1空间的坐标转换为画布坐标
    //     const polygon = group.addShape('polygon', {
    //       attrs: {
    //         points: [
    //           { x: points[0].x, y: points[0].y },
    //           { x: points[1].x, y: points[1].y },
    //           { x: points[2].x, y: points[2].y },
    //         ],
    //         fill: cfg.color,
    //       },
    //     });
    //     return polygon; // 将自定义Shape返回
    //   },
    // });

    // Step 3：创建图形语法，绘制柱状图，由 genre 和 min 两个属性决定图形位置，label 映射至 x 轴，min 映射至 y 轴
    chart
      .interval()
      // .position('label*min')
      .position(XY)
      // .color('#FA692C')
      .color('l(90) 0:#FEE4C1 0.2:#FC9051 1:#FA692C') // 定义柱状图渐变色
      .style(XY, {
        radius(val) {
          console.log('radius', arguments)
          return [10,10,0,0]
        }
      })

    // Step 4: 渲染图表
    chart.render();

    console.log('chart.get', chart.get('coord'))
    console.log('chart', chart)
  },
  methods: {
    add () {
      console.log('add')
      this.data[0].min = 200
      this.chart.changeData(this.data);
      // this.chart.source(this.data);
      // this.chart.repaint();
    },
    reduce () {
      console.log('reduce')
      this.data[0].min = 20
      // 更新方式 1: 图表数据更新（前后数据结构不发生变化），需要马上更新图表
      this.chart.changeData(this.data);
      // 更新方式 2:
      // this.chart.source(this.data);
      // this.chart.repaint();
    }
  }
}
</script>

<style lang="stylus" scoped>

</style>
