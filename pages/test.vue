<template>
  <div>
    <highcharts :options="lineData" />
    <button @click="changeLine({ type: 'type1' })">Type1</button>
    <button @click="changeLine({ type: 'type2' })">Type2</button>
    <button @click="changeLine({ type: 'type3' })">Type3</button>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue';

const months = ['2019/5', '2019/4', '2019/3', '2019/2', '2019/1', '2018/11', '2018/10', '2018/12'];

const categories = {
  type1: {
    name: 'Type1',
    data: [4, 6, 8, 5, 7, 9, 8, 10],
  },
  type2: {
    name: 'Type2',
    data: [5, 2, 4, 3, 6, 3, 5, 7],
  },
  type3: {
    name: 'Type3',
    data: [10, 9, 6, 8, 3, 5, 3, 1],
  },
};

export default {
  components: {
    highcharts: Chart,
  },
  data() {
    return {
      lineData: null,
    };
  },
  created() {
    this.setLineData(this.defaultLineOptions());
  },
  methods: {
    defaultLineOptions() {
      return {
        type: 'type1',
      };
    },
    dataOptions(type, options) {
      const selected = type === options.type;
      const color = selected ? '#0066FF' : '#CDCDCD';
      return {
        color,
        data: categories[type].data,
        lineWidth: selected ? 3 : 1,
        marker: {
          enabled: true,
          fillColor: color,
          radius: 6,
          symbol: 'circle',
        },
        name: categories[type].name,
        showInLegend: selected,
        zIndex: selected ? 1 : 0,
      };
    },
    setLineData(lineOptions) {
      this.lineData = {
        chart: {
          backgroundColor: 'transparent',
          borderColor: '#EEEEEEE',
          height: '230px',
          spacingRight: 20,
          type: 'line',
        },
        title: {
          align: 'left',
          margin: 24,
          style: {
            fontSize: '16px',
          },
          text: 'Title',
          x: 0,
        },
        legend: {
          align: 'top',
          itemStyle: {
            color: '#0066FF',
          },
          layout: 'vertical',
          symbolWidth: 40,
          verticalAlign: 'top',
          x: 72,
          y: -8,
          useHTML: true,
          enabled: true,
          navigation: {
            enabled: false,
          },
        },
        plotOptions: {
          series: {
            events: {
              legendItemClick() {
                return false;
              },
            },
          },
        },
        tooltip: {
          shared: true,
          useHTML: true,
          formatter() {
            return this.points.reduce((s, point) => {
              return `${s}<br/>
                <i style="
                  background-color:${point.color};
                  border-radius:50%;
                  display: inline-block;
                  height:6px;
                  margin-right:4px;
                  width:6px;"
                ></i>${point.series.name}: <b>${point.y}</b>`;
            }, `<b>${months[this.x]}</b>`);
          },
        },
        credits: {
          enabled: false,
        },
        xAxis: {
          endOnTick: true,
          labels: {
            style: {
              fontSize: '12px',
            },
            rotation: -45,
            x: 16,
            formatter() {
              return months[this.value];
            },
          },
          lineColor: '#CCCCCC',
          lineWidth: 1,
          maxPadding: 0,
          minPadding: 0,
          startOnTick: true,
          tickLength: 0,
          tickInterval: 1,
        },
        yAxis: {
          gridLineColor: '#CCCCCC',
          labels: {
            style: {
              fontSize: '12px',
            },
            x: -20,
          },
          lineColor: '#CCCCCC',
          lineWidth: 1,
          max: 10,
          min: 1,
          minPadding: 0,
          tickInterval: 1,
          title: {
            text: '',
          },
        },
        series: [
          this.dataOptions('type1', lineOptions),
          this.dataOptions('type2', lineOptions),
          this.dataOptions('type3', lineOptions),
        ],
      };
    },
    changeLine(lineOptions) {
      this.lineData.series = [
        this.dataOptions('type1', lineOptions),
        this.dataOptions('type2', lineOptions),
        this.dataOptions('type3', lineOptions),
      ];
    },
  },
};
</script>