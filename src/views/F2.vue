<template>
  <div class="f2">
    <h1>This is an F2 page</h1>
    <button @click="add">add</button>
    <button @click="reduce">reduce</button>
    <div ref="container" class="container">
      <div class="tooltip" :style="this.tooltipStyle">
        <div>学习时长</div>
        <div>{{tooltipText}}</div>
        <div>{{tooltipDate}}</div>
      </div>
      <canvas id="myChart" width="375" height="260"></canvas>
    </div>
  </div>
</template>

<script>
import F2 from '@antv/f2/lib/index';

console.log(F2.version)

const Gesture = require('@antv/f2/lib/plugin/gesture');

require('@antv/f2/lib/interaction');

const X = 'label'
const Y = 'min'
const XY = `${X}*${Y}`

let data = [
  { label: '周一', min: 1 },
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
      data,
      tooltipStyle: {},
      tooltipText: '',
      tooltipDate: '',
      containerRect: {}
    }
  },
  created () {
  },
  mounted () {
    // Step 1: 创建 Chart 对象
    const chart = new F2.Chart({
      id: 'myChart',
      pixelRatio: window.devicePixelRatio, // 指定分辨率
      padding: [50, 'auto', 'auto'],
      plugins: Gesture
    });
    this.chart = chart

    console.log('data', this.data);

    // 加载初始化手势插件
    chart.pluginGesture({
      gesture: {
        // touchstart
        // press
        swipe(data, event) {
          const {points, _originY, x, y} = data[0]
          console.log('swipe', event)
          // alert('swipe')
        }
      },
      hammerOptions: {},
      options: {},
    });

    // Step 2: 载入数据源
    chart.source(this.data);

    chart.coord('rect')

    // NOTE: 要左右滑动时，使用 liner + pan 模式，而且数据前后要各插入1条空数据， swipe 不好用
    // chart.interaction('pan');
    // chart.scale(X, {
    //   type: 'linear',
    //   min: 0,
    //   max: 7
    // })

    chart.scale(X, {
      type: 'cat',
    })

    // 为多个字段设置列定义
    chart.scale(Y, {
      tickCount: 3, // TODO: 换为手动计算 ticks
      // NOTE: 也可以在 axis 里配置
      // formatter (text) {
      //   return text + '分钟'
      // }
    })

    // X 轴配置
    chart.axis(X, {
      labelOffset: 12,
      line: {
        lineWidth: 1,
        stroke: '#E8E8E8',
        top: true, // 展示在最上层
      }, // 设置坐标轴线的样式，如果值为 null，则不显示坐标轴线，图形属性'
      label (text, index, total) {
        return {
          top: true
        }
      }
    })

    // Y 轴配置
    chart.axis(Y, {
      labelOffset: 10.5,
      position: 'right',
      label (text, index, total) {
        let _text = ''
        if (index) {
          _text = text + '分钟'
        }
        return {
          text: _text,
          fontSize: 10,
          fill: '#B2B2B2',
          // textBaseline: 'right',
          // textAlign: 'end', // TODO
          // fontWeight:
          top: true
        }
      },
      grid (text, index, total) {
        return {
          stroke: '#D8D8D8',
          lineDash: [9, 5]
        }
      }
    })

    // 图例：关
    chart.legend(false)

    // Tooltip 配置
    chart.tooltip({
      custom: true,
      layout: 'vertical',
      showCrosshairs: true, // 是否显示辅助线
      crosshairsStyle: {
        stroke: '#E8E8E8',
        lineWidth: 1
      }, // 配置辅助线的样式
      showTooltipMarker: false, // 是否显示辅助背景
      onChange: (ev) => {
        const currentData = ev.items[0];
        console.log('onChange', ev)
        return
        this.tooltipDate = currentData.origin.date;
        this.tooltipText = currentData.value
        this.tooltipStyle = {
            opacity: 1,
            left: canvasOffsetLeft + currentData.x - 130 / 2 + 'px',
            top: canvasOffsetTop + currentData.y - 40 - 15 + 'px'
        }
      },
      onHide: () => {
        console.log('onhide', arguments)
          this.tooltipStyle = {
              opacity: 0
          }
      }
    });

    // 自定义柱状图图形
    F2.Shape.registerShape('interval', 'triangle', {
      getPoints: function(cfg) {
        const {x, y, y0, size} = cfg
        return [
          { x: x - size / 2, y: y0 },
          { x: x - size / 2, y: y },
          { x: x + size / 2, y: y },
          { x: x + size / 2, y: y0 },
        ];
      },
      draw: function(cfg, group) {
        console.log(this)
        console.log('draw', cfg.points, cfg, group)
        const points = this.parsePoints(cfg.points); // 将0-1空间的坐标转换为画布坐标
        console.log('points', points)
        // const polygon = group.addShape('polygon', {
        //   attrs: {
        //     points,
        //     fill: '#1890FF',
        //   },
        // });

        // 这个为框架里绘制路径的方式
        // group.addShape('custom', {
        //   attrs: {
        //     x: points[1].x,
        //     y: points[1].y,
        //     width: points[3].y - points[1].y,
        //     height: points[3].x - points[1].x,
        //     fill: cfg.color,
        //     radius: cfg.radius // [40, 40, 0, 0]
        //   },
        //   createPath(context) {
        //     const { x, y, width, height, radius } = this._attrs.attrs
        //     context = context || this.get('context');
        //     context.beginPath();
        //     if (radius === 0) {
        //       context.rect(x, y, width, height);
        //     } else {
        //       var r = parseRadius(radius);
        //       context.moveTo(x + r.r1, y);
        //       context.lineTo(x + width - r.r2, y);
        //       r.r2 !== 0 && context.arc(x + width - r.r2, y + r.r2, r.r2, -Math.PI / 2, 0);
        //       context.lineTo(x + width, y + height - r.r3);
        //       r.r3 !== 0 && context.arc(x + width - r.r3, y + height - r.r3, r.r3, 0, Math.PI / 2);
        //       context.lineTo(x + r.r4, y + height);
        //       r.r4 !== 0 && context.arc(x + r.r4, y + height - r.r4, r.r4, Math.PI / 2, Math.PI);
        //       context.lineTo(x, y + r.r1);
        //       r.r1 !== 0 && context.arc(x + r.r1, y + r.r1, r.r1, Math.PI, Math.PI * 1.5);
        //       context.closePath();
        //     }
        //   }
        // })

        // 计算 radius，为宽度的一半
        const radius = (points[3].x - points[1].x) / 2

        // 柱状图
        group.addShape('rect', {
          attrs: {
            x: points[1].x,
            y: points[1].y,
            height: points[3].y - points[1].y,
            width: points[3].x - points[1].x,
            fill: cfg.color,
            radius: [radius, radius, 0, 0]
          },
        });
        // 底部的遮罩，需要坐标文字置顶显示
        group.addShape('rect', {
          attrs: {
            x: points[1].x - 1,
            y: points[0].y,
            height: radius,
            width: points[3].x - points[1].x + 2,
            fill: 'white'
          },
        });

        return group;
      },
    });

    // Step 3：创建图形语法，绘制柱状图，由 genre 和 min 两个属性决定图形位置，label 映射至 x 轴，min 映射至 y 轴
    chart
      .interval()
      .position(XY)
      .color('l(90) 0:#FEE4C1 0.2:#FC9051 1:#FA692C') // 定义柱状图渐变色
      .shape('triangle')

    // Step 4: 渲染图表
    chart.render();

    // this.$vm.containerRect.getBoundingClientRect()

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


function parseRadius(radius) {
    var r1 = 0,
        r2 = 0,
        r3 = 0,
        r4 = 0;

    if (Array.isArray(radius)) {
      if (radius.length === 1) {
        r1 = r2 = r3 = r4 = radius[0];
      } else if (radius.length === 2) {
        r1 = r3 = radius[0];
        r2 = r4 = radius[1];
      } else if (radius.length === 3) {
        r1 = radius[0];
        r2 = r4 = radius[1];
        r3 = radius[2];
      } else {
        r1 = radius[0];
        r2 = radius[1];
        r3 = radius[2];
        r4 = radius[3];
      }
    } else {
      r1 = r2 = r3 = r4 = radius;
    }

    return {
      r1: r1,
      r2: r2,
      r3: r3,
      r4: r4
    };
}
</script>

<style scoped>
  .container {
    position: relative;
  }
  .tooltip {
    -moz-box-shadow: 1px 1px 0.5px 0.5px rgba(0, 0, 0, 0.3);
    -webkit-box-shadow: 1px 1px 0.5px 0.5px rgba(0, 0, 0, 0.3);
    box-shadow: 1px 1px 0.5px 0.5px rgba(0, 0, 0, 0.3);
    position: absolute;
    z-index: 99;
    background-color: #1890ff;
    padding: 5px;
    border-radius: 3px;
    text-align: center;
    width: 120px;
    opacity: 0;
  }
  .tooltip:after {
    content: " ";
    width: 0;
    height: 0;
    border-left: 6px solid transparent;
    border-right: 6px solid transparent;
    border-top: 8px solid #1890ff;
    position: absolute;
    left: 50%;
    margin-left: -6px;
    bottom: -8px;
  }
  .tooltip {
    display: block;
    color: #fff;
  }
  .tooltip div:nth-child(1) {
    font-size: 11px !important;
  }
  .tooltip div:nth-child(2) {
    font-size: 13px !important;
  }
</style>
