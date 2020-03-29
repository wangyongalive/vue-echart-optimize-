<template>
  <div id="main">
  </div>
</template>

<script>
  import option from "./../../static/options";
  import echarts from 'echarts';
  import axios from "axios";


  export default {
    name: 'Echart',
    data() {
      return {
        options: null,
        timeID: ""
      }
    },
    created() {
      axios.get('https://unpkg.com/echarts@4.1.0/map/json/world.json').then((response) => {
        echarts.registerMap('world', response.data);
      })
    },
    mounted() {
      // use different zlevel for performance
      option.geo.zlevel = 1;
      option.series.forEach((s, index) => {
        s.zlevel = index + 2;
        if (s.effect) {
          s.effect.zlevel = index + option.series.length + 2;
        }
        if (s.rippleEffect) {
          s.rippleEffect.zlevel = index + option.series.length + 2;
        }
      });
      this.options = option;
      let series = option.series;
      this.data2 = series[2].data;
      this.data3 = series[3].data;
      this.draw();

    },
    beforeDestroy() {
      // TODO need dispose echart instance and unbind resize event manually.
      clearInterval(this.timeID);
      this.chart.dispose();
    },
    methods: {
      draw() {
        this.chart = echarts.init(document.getElementById('main'), null, {
          devicePixelRatio: 1
        });
        this.chart.setOption(this.options);
        this.timeID = setInterval(() => {
          this.refreshData();
          this.chart.setOption(this.options);
        }, 5000);
      },
      getRandomData(data) {
        let result = data.slice();
        return result.slice(0, Math.round(Math.random() * (data.length - 1)));
      },
      refreshData() {
        let series = this.options.series;
        series[2].data = this.getRandomData(this.data2);
        series[3].data = this.getRandomData(this.data3);
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #main {
    height: 100%;
  }
</style>
