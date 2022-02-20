<template>
  <div>
    <fieldset>
      <legend>都道府県</legend>
      <ul>
        <li v-for="(item, i) in series" :key="i">
          <label><input v-model="item.visible" type="checkbox" @change="toggleCheckbox(item)" />{{ item.name }}</label>
        </li>
      </ul>
    </fieldset>
    <fieldset>
      <legend>グラフ</legend>
      <div ref="chart"/>
    </fieldset>
  </div>
</template>

<script>
import Highcharts from 'highcharts'

export default {
  name: 'PrefChart',
  props: {
    poplationType: {
      type: String,
      default: 'total',
    },
  },
  data() {
    return {
      prefList:null,
      popuList:null,
      series: [],
      prefCodes: {},
      lineData: null,
    }
  },
  methods: {
    // 都道府県データの取得
    async getPrefData() {
      const ret = {
        prefCodes: {},
        series: [],
      }
      const url = 'api/v1/prefectures'
      const resData = await this.$resas.get(url)
      for (let i = 0; i < resData.data.result.length; i++) {
        ret.prefCodes[resData.data.result[i].prefName] = resData.data.result[i].prefCode
        const item = {
          name: resData.data.result[i].prefName,
        }
        ret.series.push(item)
      }
      return ret
    },
    // 人口データ取得
    async getPopuData(prefCode) {
      const ret = {
        data: [],
      }
      const url = 'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear'
      const resData = await this.$resas.get(url + '?cityCode=-&prefCode=' + prefCode)
      const popu = resData.data.result.data[0].data
      const values = popu.map((d) => d.value)
      ret.data = values
      return ret
    },
    // グラフ描画
    createChart(series) {
      this.lineData = {
        title: {
          text: '都道府県別総人口推移',
        },
        subtitle: {
          text: 'Source: https://opendata.resas-portal.go.jp/',
        },
        xAxis: {
          title: {
            text: '年度',
            align: 'high',
          },
          accessibility: {
            rangeDescription: '年度',
          },
        },
        yAxis: {
          title: {
            text: '人口',
            align: 'high',
            rotation: 0,
          },
        },
        legend: {
          layout: 'vertical',
          align: 'right',
          verticalAlign: 'top',
        },
        plotOptions: {
          series: {
            label: {
              connectorAllowed: false,
            },
            pointStart: 1960,
            pointInterval: 5,
          },
        },
        series,
        responsive: {
          rules: [
            {
              condition: {
                maxWidth: 500,
              },
              chartOptions: {
                legend: {
                  layout: 'horizontal',
                  align: 'center',
                  verticalAlign: 'bottom',
                },
              },
            },
          ],
        },
      }

      const chart = Highcharts.chart(this.$refs.chart, this.lineData)
      this.series = chart.series

      for (let i = 0; i < chart.series.length; i++) {
        chart.series[i].update({ visible: false, showInLegend: false }, true)
      }
    },
    // チェックボックス切り替え
    toggleCheckbox(item) {
      const prefCode = this.prefCodes[item.name]
      this.getPopuData(prefCode).then((d) => {
        item.setData(d.data, false)
        item.update({
          visible: item.visible,
          showInLegend: item.visible,
        })
      })
    },
  },
  created() {
    this.getPrefData(this.$props.poplationType, this.$props.prefectures).then((prefData) => {
      this.prefCodes = prefData.prefCodes
      this.createChart(prefData.series)
    })
  },
}
</script>

<style>
ul {
  list-style: none;
}
ul li {
  display: inline-block;
  width: 10%;
  min-width: 100px;
}
</style>
