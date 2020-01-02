<template>
  <div class="f2">
    <h1>This is an F2 page</h1>
    <div class="container">
      <div class="f2-tooltip" :style="this.tooltipStyle">
        <span>{{tooltipDate}}</span>
        Web Visits: <b>{{tooltipText}}</b>
      </div>
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
      data: [],
      tooltipDate: '',
      tooltipText: '',
      tooltipStyle: {}
    }
  },
  created () {
  },
  mounted () {
    const canvasPosition = document.getElementById('container').getBoundingClientRect()
    const canvasOffsetTop = canvasPosition.top;
    const canvasOffsetLeft = canvasPosition.left;
    const tooltipEl = document.getElementsByClassName('f2-tooltip');

    const dataPoints = [{
    date: '2015-01-01',
    value: 570
    }, {
    date: '2015-01-05',
    value: 525
    }, {
    date: '2015-01-09',
    value: 560
    }, {
    date: '2015-01-13',
    value: 550
    }, {
    date: '2015-01-17',
    value: 555
    }, {
    date: '2015-01-21',
    value: 580
    }, {
    date: '2015-01-25',
    value: 560
    }, {
    date: '2015-01-29',
    value: 560
    }];

    const chart = new F2.Chart({
    id: 'container',
    pixelRatio: window.devicePixelRatio,
    padding: [ 30, 50, 'auto' ]
    });
    chart.source(dataPoints, {
        date: {
            type: 'timeCat',
            mask: 'D.MMM',
            range: [ 0, 1 ],
            tickCount: 8
        },
        value: {
            tickCount: 5
        }
        });

    chart.axis('date', {
    tickLine: {
        length: 5,
        stroke: '#e8e8e8',
        lineWidth: 1
    }
    });
    chart.axis('value', {
    grid: {
        lineDash: null
    }
    });
    chart.tooltip({
    custom: true,
    showCrosshairs: false,
    onChange: (ev) => {
        const currentData = ev.items[0];
        this.tooltipDate = currentData.origin.date;
        this.tooltipText = currentData.value
        this.tooltipStyle = {
            opacity: 1,
            left: canvasOffsetLeft + currentData.x - 130 / 2 + 'px',
            top: canvasOffsetTop + currentData.y - 40 - 15 + 'px'
        }
    },
    onHide: () => {
        this.tooltipStyle = {
            opacity: 0
        }
    }
    });

    chart.area()
    .position('date*value')
    .shape('smooth')
    .style({
        fillOpacity: 0.85
    });
    chart.render();
  }
}
</script>

<style scoped>
  .chart-wrapper {
    position: relative;
  }
  .f2-tooltip {
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
  .f2-tooltip:after {
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
  .f2-tooltip span {
    display: block;
    color: #fff;
  }
  .f2-tooltip span:nth-child(1) {
    font-size: 11px !important;
  }
  .f2-tooltip span:nth-child(2) {
    font-size: 13px !important;
  }
</style>
