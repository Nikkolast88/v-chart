<template>
  <div class="hello">
    <VeMap
      ref="mapChart"
      :settings="chartSettings"
      :geo="chartGeo"
      :series="chartSeries"
      :data="mapData"
    ></VeMap>
  </div>
</template>

<script>
import VeMap from "v-charts/lib/map.common";
import Axios from 'axios';
export default {
  name: "HelloWorld",
  components: {
    VeMap
  },
  props: {
    msg: String
  },
  data() {
    return{
      chartTooltip: {},
      mapData: {
        columns: [],
        rows: []
      },
      chartSettings: {},
      chartSeries: {
        type: 'map'
      },
      chartGeo: {},
      longitude: '',
      latitude: ''
    }
  },
  created() {
    this.initMap()
  },
  methods: {
    initMap() {
      Axios.get('./shenzhen.json').then(res => {
        let shenzhen = res.data
        this.chartTooltip = {
          formatter: function(v) {
            var tpl = []
            tpl.push(v.seriesName + '<br>')
            tpl.push(v.name + '：' + v.value[2])
            return tpl.join('')
          }
        }
        this.mapData = {
          columns: [],
          rows: []
        }
        this.chartSettings = {
          mapOrigin: shenzhen,
          position: 'shenzhen'
        }
        this.chartSeries = [{
          name: 'pm2.5',
          type: 'effectScatter', // 'scatter'
          coordinateSystem: 'geo',
          symbolSize: 10,
          rippleEffect: {
            brushType: 'stroke'
          },
          hoverAnimation: true,
          data: [
            { name: '', value: [this.longitude, this.latitude, 10] }
          ],
          label: {
            normal: {
              formatter: '{b}',
              position: 'right',
              show: true
            },
            emphasis: {
              show: true
            }
          },
          itemStyle: {
            normal: {
              color: '#DDDB3F'
            }
          }
        }]
        this.chartGeo = {
          map: 'shenzhen',
          label: {
            emphasis: {
              show: false
            }
          },
          roam: true,
          itemStyle: {
            normal: {
              areaColor: '#01A9DA',
              borderColor: '#22A0CB'
            },
            emphasis: {
              areaColor: '#0184B3'
            }
          }
        }
      })
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
