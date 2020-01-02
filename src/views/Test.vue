<template>
  <div class="f2">
    <h1>This is an F2 page</h1>
    <div class="container">
      <canvas id="container" width=375 height=260 ></canvas>
    </div>
  </div>
</template>

<script>
import F2 from '@antv/f2/lib/index'
console.log(F2.version)

const Gesture = require('@antv/f2/lib/plugin/gesture');
const ScrollBar = require('@antv/f2/lib/plugin/scroll-bar');

require('@antv/f2/lib/interaction');

const X = 'name'
const Y = 'value'
const XY = `${X}*${Y}`

export default {
  data () {
    return {
      data: []
    }
  },
  created () {
  },
  mounted () {
    // const data = [
    //   { x: 0, y: 1 },
    //   { x: 1, y: 2 },
    //   { x: 2, y: 3 },
    //   { x: 3, y: 4 },
    //   { x: 4, y: 5 },
    //   { x: 5, y: 6 }
    // ];
    // const data = [
    //   { no: 0, a: 'a', b: 20 },
    //   { no: 1, a: 'b', b: 12 },
    //   { no: 2, a: 'c', b: 8 },
    //   { no: 3, a: 'a', b: 20 },
    //   { no: 4, a: 'b', b: 12 },
    //   { no: 5, a: 'c', b: 8 },
    //   { no: 6, a: 'a', b: 20 },
    //   { no: 7, a: 'b', b: 12 },
    //   { no: 8, a: 'c', b: 8 },
    // ];
    const data = [
      { name: "A1", value: 23 },
      { name: "A2", value: 13 },
      { name: "A3", value: 20 },
      { name: "A4", value: 23 },
      { name: "A5", value: 23 },
      { name: "A6", value: 23 },
      { name: "A7", value: 23 },
      { name: "A8", value: 23 },
      { name: "A9", value: 23 },
      { name: "A10", value: 23 },
      { name: "A11", value: 23 },
      { name: "A12", value: 23 },
      { name: "A13", value: 23 },
      { name: "A14", value: 23 },
      { name: "A15", value: 23 },
      { name: "B1", value: 3 },
      { name: "B2", value: 13 },
      { name: "B3", value: 10 },
      { name: "B4", value: 33 },
      { name: "B5", value: 23 },
      { name: "B6", value: 43 },
      { name: "B7", value: 13 },
      { name: "B8", value: 3 },
      { name: "B9", value: 23 },
      { name: "B10", value: 23 },
      { name: "B11", value: 23 },
      { name: "B12", value: 23 },
      { name: "B13", value: 23 },
      { name: "B14", value: 23 },
      { name: "B15", value: 23 }
    ];

    const chart = new F2.Chart({
      id: 'container',
      pixelRatio: window.devicePixelRatio, // 指定分辨率
      plugins: ScrollBar
    });

    chart.source(data);

    chart.scale(X, {
      type: 'cat',
      // formatter(text) {
      //   return text + '分类'
      // },
      // min: 'B6',
      // max: 'B10',
      // tickCount: 4
      // values: [ 3, 4, 5 ],
    });

    chart.scale(Y, {
      alias: '数值',
      formatter(text) {
        return text + '指数'
      },
      // ticks: [0, 5, 10, 50, 100],
      // tickCount: 3
    })

    chart
      .interval()
      .position(XY)
      .color('#FA692C')
      .style(XY, {
        radius(val) {
          return [10,10,0,0]
        }
      })

    chart.scrollBar({
      mode: 'x',
      xStyle: {
        backgroundColor: '#e8e8e8',
        fillerColor: '#808080',
        offsetY: -2
      }
    });

    chart.interaction('pan');

    chart.render()
  }
}
</script>

<style lang="stylus" scoped>

</style>
